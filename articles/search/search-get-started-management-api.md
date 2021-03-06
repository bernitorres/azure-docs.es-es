---
title: "Introducción a la API de REST de administración de Azure Search | Microsoft Docs"
description: "Administre el servicio de Búsqueda de Azure hospedado en la nube mediante una API de REST de administración."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: cd4c41d8-81bd-4609-9a37-e112ddf1f21f
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/17/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 4fe0db2a5fb9b7798d8583721ac35b2e02435d7d
ms.lasthandoff: 03/29/2017


---
# <a name="get-started-with-azure-search-management-rest-api"></a>Introducción a la API de REST de administración de búsqueda de Azure
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [API DE REST](search-get-started-management-api.md)
>
>

La API de administración de REST de Búsqueda de Azure es una alternativa de programación para realizar tareas administrativas en el portal. Las operaciones de administración de servicios incluyen crear o eliminar el servicio, escalar el servicio y administrar las claves. Este tutorial incluye una aplicación cliente de ejemplo que muestra la API de administración de servicios. También incluye los pasos de configuración necesarios para ejecutar el ejemplo en el entorno de desarrollo local.

Para completar este tutorial, necesitará:

* Visual Studio 2012 o 2013
* Descargar la aplicación cliente de ejemplo

Durante la realización del tutorial, se proporcionarán dos servicios: Búsqueda de Azure y Azure Active Directory (AD). Además, creará una aplicación de AD que establece la confianza entre la aplicación cliente y el extremo del Administrador de recursos en Azure.

Para completar este tutorial, deberá tener una cuenta de Azure:

## <a name="download-the-sample-application"></a>Descarga de la aplicación de ejemplo
Este tutorial se basa en una aplicación de consola de Windows escrita en C#, que puede editar y ejecutar en Visual Studio 2012 o 2013

Puede encontrar la aplicación cliente en GitHub en [Azure Search .NET Management API Demo](https://github.com/Azure-Samples/search-dotnet-management-api/) (Demostración de la API de administración de Azure Search .NET).

## <a name="configure-the-application"></a>Configuración de la aplicación
Para poder ejecutar la aplicación de ejemplo, debe habilitar la autenticación de modo que se puedan aceptar solicitudes enviadas desde la aplicación cliente al extremo del Administrador de recursos. El requisito de autenticación se origina con el [Administrador de recursos de Azure](https://msdn.microsoft.com/library/azure/dn790568.aspx), que es la base para todas las operaciones relacionadas con el portal solicitadas a través de una API, incluidas las relacionadas con la administración del servicio Búsqueda. La API de administración de servicios de Búsqueda de Azure es simplemente una extensión del Administrador de recursos de Azure y, por tanto, hereda sus dependencias.  

El Administrador de recursos de Azure requiere el servicio Azure Active Directory como su proveedor de identidades.

Para obtener un token de acceso que permitirá que las solicitudes lleguen al Administrador de recursos, la aplicación cliente incluye un segmento de código que llama a Active Directory. El segmento de código, además de los pasos previos para usarlo, se ha tomado prestado de este artículo: [Autenticación de solicitudes del Administrador de recursos de Azure](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Puede seguir las instrucciones del vínculo anterior o usar los pasos de este documento si prefiere revisar el tutorial paso a paso.

En esta sección, realizaremos las siguientes tareas:

1. Crear un servicio de AD
2. Crear una aplicación de AD
3. Configurar la aplicación AD registrando detalles acerca de la aplicación cliente de ejemplo que descargó
4. Cargar la aplicación cliente de ejemplo con los valores que se usarán para obtener autorización para sus solicitudes

> [!NOTE]
> Estos vínculos proporcionan antecedentes acerca del uso de Azure Active Directory para autenticar solicitudes de cliente para el administrador de recursos: [Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790568.aspx), [Autenticación de solicitudes de Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx) y [Azure Active Directory](http://msdn.microsoft.com/library/azure/jj673460.aspx).
>
>

### <a name="create-an-active-directory-service"></a>Crear un servicio de Active Directory
1. Inicie sesión en el [Portal de Azure](https://manage.windowsazure.com).
2. Desplácese por el panel de navegación izquierdo y haga clic en **Active Directory**.
3. Haga clic en **NUEVO** para abrir **App Services** | **Active Directory**. En este paso, va a crear un nuevo servicio de Active Directory. Este servicio va a hospedar la aplicación de AD para la que definirá unos cuantos pasos desde ahora. Crear un nuevo servicio ayuda a aislar el tutorial de otras aplicaciones que ya se podrían estar hospedando en Azure.
4. Haga clic en **Directorio** | **Creación personalizada**.
5. Escriba un nombre de servicio, el dominio y la ubicación geográfica. El dominio debe ser único. Haga clic en la marca de verificación para crear el servicio.

### <a name="create-a-new-ad-application-for-this-service"></a>Creación de una nueva aplicación de AD para este servicio
1. Seleccione el servicio de Active Directory "SearchTutorial" que acaba de crear.
2. En el menú superior, haga clic en **Aplicaciones**.
3. Haga clic en **Agregar una aplicación**. Una aplicación de AD almacena información acerca de las aplicaciones cliente que la van a usar como proveedor de identidades.  
4. Elija **Agregar una aplicación que mi organización está desarrollando**. Esta opción proporciona la configuración del registro para las aplicaciones que no están publicadas en la Galería de aplicaciones. Puesto que la aplicación cliente no forma parte de la Galería de aplicaciones, esta es la opción más adecuada para este tutorial.
5. Escriba un nombre, como "Administrador de búsqueda de Azure".
6. Elija **Aplicación de cliente nativo** como tipo de aplicación. Esto es correcto para la aplicación de ejemplo, ya que es una aplicación cliente (consola) de Windows, no una aplicación web.
7. En el URI de redirección, escriba "http://localhost/Azure-Search-Manager-App". Este es el URI al que Azure Active Directory redirigirá el agente de usuario en respuesta a una solicitud de autorización OAuth 2.0. El valor no tiene que ser un extremo físico, pero debe ser un URI válido.

    Para los fines de este tutorial, el valor puede ser cualquier cosa, pero todo lo que escriba se convierte en una entrada necesaria para la conexión administrativa en la aplicación de ejemplo.
8. Haga clic en la marca de verificación para crear la aplicación de Active Directory. Debe ver "Aplicación de administrador de Búsqueda de Azure" en el panel de navegación izquierdo.

### <a name="configure-the-ad-application"></a>Configuración de la aplicación de AD
1. Haga clic en la aplicación de AD, "Aplicación de administrador de Búsqueda de Azure", que acaba de crear. Debe ver que aparece en el panel de navegación izquierdo.
2. Haga clic en **Configurar** en el menú de la parte superior.
3. Desplácese hacia abajo hasta los permisos y seleccione la **API de administración de Azure**. En este paso, especifique la API (en este caso, la API del Administrador de recursos de Azure) a la que la aplicación cliente necesita tener acceso, junto con el nivel de acceso que necesita.
4. En Permisos delegados, haga clic en la lista desplegable y seleccione **Acceso a  la administración de Servicios de Azure (vista previa)**.
5. Guarde los cambios.

Mantenga abierta la página de configuración de la aplicación. En el siguiente paso, copiará los valores de esta página y los escribirá en la aplicación de ejemplo.

### <a name="load-the-sample-application-program-with-registration-and-subscription-values"></a>Carga del programa de aplicación de ejemplo con valores de registro y suscripción
En esta sección, modificará la solución en Visual Studio, sustituyendo los valores válidos obtenidos del portal.
Los valores que va a agregar aparecen cerca de la parte superior de Program.cs:

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

Si aún no [ha descargado la aplicación de ejemplo de GitHub](https://github.com/Azure-Samples/search-dotnet-management-api/), la necesitará en este paso.

1. Abra **ManagementAPI.sln** en Visual Studio.
2. Abra Program.cs.
3. Proporcione `ClientId`. Desde la página de configuración de la aplicación de AD que dejó abierta en el paso anterior, copie el identificador de cliente de la página de configuración de la aplicación de AD en el portal y péguelo en Program.cs.
4. Proporcione `RedirectUrl`. Copie el URI de redirección de la misma página del portal y péguelo en Program.cs.
5. Proporcione `TenantID.`

   * Vuelva a Active Directory | SearchTutorial (servicio).
   * Haga clic en **Aplicaciones** en la barra superior.
   * Haga clic en **Ver extremos** en la parte inferior de la página.
   * Copie el extremo de autorización OAUTH 2.0 en la parte inferior de la lista.
   * Pegue el extremo en TenantID y recorte el valor de todos los parámetros URI excepto el identificador del inquilino.

     Dado "https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0", elimine todo excepto "55e324c7-1656-4afe-8dc3-43efcd4ffa50".

6. Proporcione `SubscriptionID`.

   * Vaya a la página principal del portal.
   * Haga clic en **Configuración** en la parte inferior del panel de navegación izquierdo.
   * En la pestaña Suscripciones, copie el identificador de suscripción y péguelo en Program.cs.
7. Guarde y, a continuación, compile la solución.

## <a name="explore-the-application"></a>Exploración de la aplicación
Agregue un punto de interrupción en la primera llamada al método para que pueda explorar el programa. Presione **F5** para ejecutar la aplicación y luego **F11** para recorrer el código.

La aplicación de ejemplo crea un servicio gratuito de Azure Search para una suscripción de Azure existente. Si ya existe un servicio gratuito para la suscripción, se producirá un error en la aplicación de ejemplo. Se permite solo un servicio gratuito de búsqueda por suscripción.

1. Abra Program.cs desde el Explorador de soluciones y vaya a la función Main(string[] void).
2. Observe que **ExecuteArmRequest** se utiliza para ejecutar solicitudes en el punto de conexión de Azure Resource Manager, `https://management.azure.com/subscriptions`para `subscriptionID` especificado. Este método se utiliza en todo el programa para realizar operaciones mediante la API del Administrador de recursos de Azure o la API de administración de búsqueda.
3. Las solicitudes al Administrador de recursos de Azure se deben autenticar y autorizar. Esto se consigue mediante el método **GetAuthorizationHeader**, al que se llama mediante el método **ExecuteArmRequest**, que se ha pedido prestado de [Autenticación de solicitudes de Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx). Observe que **GetAuthorizationHeader** llama a `https://management.core.windows.net` para obtener un token de acceso.
4. Se le pedirá que inicie sesión con un nombre de usuario y una contraseña que sea válida para la suscripción.
5. A continuación, se registra un nuevo servicio de Búsqueda de Azure con el proveedor del Administrador de recursos de Azure. De nuevo, este es el método **ExecuteArmRequest`providers/Microsoft.Search/register`, que se usa esta vez para crear el servicio Search en Azure para su suscripción a través de**.
6. El resto del programa utiliza la [API de REST de administración de Búsqueda de Azure](http://msdn.microsoft.com/library/dn832684.aspx). Observe que la `api-version` de esta API es diferente de la versión de api del Administrador de recursos de Azure. Por ejemplo, `/listAdminKeys?api-version=2014-07-31-Preview` muestra la `api-version` de la API de REST de Administración de Búsqueda de Azure.

    La siguiente serie de operaciones recupera la definición de servicio que acaba de crear, las claves de api de administración, vuelve a generar y recupera las claves, cambia la réplica y la partición y finalmente elimina el servicio.

    Al cambiar el número de réplica o de partición de servicio, se espera que esta acción produzca un error si está utilizando la edición gratuita. Solo la edición standard puede hacer uso de particiones o réplicas adicionales.

    La eliminación del servicio es la última operación.

## <a name="next-steps"></a>Pasos siguientes
Una vez terminado este tutorial, puede obtener más información acerca de la administración o la autenticación de servicios con el servicio de Active Directory:

* Más información acerca de cómo integrar una aplicación cliente con Active Directory. Consulte [Integración de aplicaciones en Azure Active Directory](http://msdn.microsoft.com/library/azure/dn151122.aspx).
* Obtenga información acerca de otras operaciones de administración de servicios en Azure. Consulte [Administración de los servicios](http://msdn.microsoft.com/library/azure/dn578292.aspx).

<!--Anchors-->
[Download the sample application]: #Download
[Configure the application]: #config
[Explore the application]: #explore
[Next Steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Manage your search solution in Microsoft Azure]: search-manage.md
[Azure Search development workflow]: search-workflow.md
[Create your first azure search solution]: search-create-first-solution.md
[Create a geospatial search app using Azure Search]: search-create-geospatial.md

