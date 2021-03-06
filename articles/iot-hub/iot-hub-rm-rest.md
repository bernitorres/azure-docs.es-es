---
title: "Creación de un centro de IoT Hub de Azure mediante la API de REST del proveedor de recursos | Microsoft Docs"
description: "Describe cómo usar la API de REST del proveedor de recursos para crear un centro de IoT Hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 52814ee5-bc10-4abe-9eb2-f8973096c2d8
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: e9185862bd1f15adacf7fd407a6f5165b2b337f5
ms.lasthandoff: 03/10/2017


---
# <a name="create-an-iot-hub-using-the-resource-provider-rest-api-net"></a>Creación de un centro de IoT Hub mediante la API de REST del proveedor de recursos (.NET)
[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Introducción
Puede usar la [API de REST del proveedor de recursos de IoT Hub][lnk-rest-api] para crear y administrar los centros de Azure IoT Hub mediante programación. En este tutorial se muestra cómo usar la API de REST del proveedor de recursos de IoT Hub para crear un IoT Hub desde un programa de C#.

> [!NOTE]
> Azure tiene dos modelos de implementación diferentes para crear y trabajar con recursos: [Azure Resource Manager y clásico](../azure-resource-manager/resource-manager-deployment-model.md).  Este artículo trata sobre el uso del modelo de implementación de Azure Resource Manager.
> 
> 

Para completar este tutorial, necesitará lo siguiente:

* Visual Studio 2015 o Visual Studio 2017.
* Una cuenta de Azure activa. <br/>Si no tiene ninguna, puede crear una [cuenta gratuita][lnk-free-trial] en tan solo unos minutos.
* [Azure PowerShell 1.0][lnk-powershell-install] o posterior.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Preparar su proyecto de Visual Studio
1. En Visual Studio, cree un proyecto de escritorio clásico de Windows de Visual C# usando la plantilla de proyecto **Aplicación de consola (.NET Framework)**. Asigne al proyecto el nombre **CreateIoTHubREST**.
2. En el Explorador de soluciones, haga clic con el botón secundario en su proyecto y luego haga clic en **Administrar paquetes de NuGet**.
3. En el Administrador de paquetes NuGet, active **Incluir versión preliminar** y en la página **Examinar** busque **Microsoft.Azure.Management.ResourceManager**. Seleccione el paquete, haga clic en **Instalar**, en **Revisar cambios**, haga clic en **Aceptar** y, luego, en **Acepto** para aceptar las licencias.
4. En el Administrador de paquetes NuGet, busque **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Haga clic en **Instalar**, en **Revisar cambios**, haga clic en **Aceptar** y, luego, en **Acepto** para aceptar la licencia.
5. En Program.cs, reemplace las instrucciones **using** existentes por el siguiente código:
   
    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
6. En Program.cs, agregue las siguientes variables estáticas reemplazando los valores de marcador de posición. Ha tomado nota de **ApplicationId**, **SubscriptionId**, **TenantId** y **Password** anteriormente en este tutorial. El **nombre de grupo de recursos** es el nombre del grupo de recursos que usará al crear IoT Hub. Puede ser un grupo de recursos existente u otro nuevo. El **nombre del IoT Hub** es el nombre del IoT Hub que se creará, como **MiCentroDeIoT** (tenga en cuenta que este nombre debe ser globalmente único, por lo que debe incluir su nombre o sus iniciales). El **Nombre de la implementación** es un nombre para la implementación, como **Deployment_01**.
   
    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
   
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-resource-provider-rest-api-to-create-an-iot-hub"></a>Uso de la API de REST del proveedor de recursos para crear un centro de IoT hub
Utilice la [API de REST del proveedor de recursos de IoT Hub][lnk-rest-api] para crear un centro de IoT Hub en el grupo de recursos. También puede utilizar la API de REST del proveedor de recursos para efectuar cambios en un centro de IoT Hub existente.

1. Agregue el método siguiente a Program.cs:
   
    ```
    static void CreateIoTHub(string token)
    {
   
    }
    ```
2. Agregue el código siguiente al método **CreateIoTHub**. Este código crea un objeto **HttpClient** con el token de autenticación de los encabezados:
   
    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```
3. Agregue el código siguiente al método **CreateIoTHub**. Este código describe el centro de IoT Hub que se creará y genera una representación JSON. Para ver la lista actualizada de las ubicaciones admitidas por IoT Hub, consulte [Estado de Azure][lnk-status]:
   
    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
   
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```
4. Agregue el código siguiente al método **CreateIoTHub**. Este código envía la solicitud de REST a Azure, comprueba la respuesta y recupera la URL con la que se puede supervisar el estado de la tarea de implementación:
   
    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
   
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
   
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```
5. Agregue el siguiente código al final del método **CreateIoTHub**. Este código usa la dirección **asyncStatusUri** recuperada del paso anterior para esperar a que finalice la implementación:
   
    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```
6. Agregue el siguiente código al final del método **CreateIoTHub**. Este código recupera las claves del centro de IoT Hub que se crea y las imprime en la consola:
   
    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
   
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```

## <a name="complete-and-run-the-application"></a>Completar y ejecutar la aplicación
Ahora puede completar la aplicación llamando al método **CreateIoTHub** antes de compilarla y ejecutarla.

1. Agregue el siguiente código al final del método **Main** :
   
    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
2. Haga clic en **Compilar** y luego en **Compilar solución**. Corrija los errores.
3. Haga clic en **Depurar** y luego en **Iniciar depuración** para ejecutar la aplicación. La ejecución de la implementación puede tardar varios minutos en completarse.
4. Para comprobar que la aplicación ha agregado el nuevo centro de IoT Hub, visite [Azure Portal][lnk-azure-portal] y consulte la lista de recursos. También puede utilizar el cmdlet de PowerShell **Get-AzureRmResource**.

> [!NOTE]
> Esta aplicación de ejemplo agrega un Centro de IoT estándar S1 por el que se le cobrará. Cuando haya terminado, podrá eliminar el centro de IoT Hub a través de [Azure Portal][lnk-azure-portal] o con el cmdlet **Remove-AzureRmResource** de PowerShell.
> 
> 

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha implementado un centro de IoT HOB con la API de REST del proveedor de recursos, es posible que quiera profundizar más en este tema:

* Consulte las funcionalidades de la [API de REST del proveedor de recursos de IoT Hub][lnk-rest-api].
* Para más información sobre las funcionalidades de Azure Resource Manager, consulte [Información general de Azure Resource Manager][lnk-azure-rm-overview].

Para obtener más información sobre cómo desarrollar para IoT Hub, consulte los siguientes artículos:

* [Introducción al SDK de C][lnk-c-sdk]
* [SDK de IoT de Azure][lnk-sdks]

Para explorar aún más las funcionalidades de Centro de IoT, consulte:

* [Simulación de un dispositivo con el SDK de puerta de enlace de IoT][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azureps-cmdlets-docs
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

