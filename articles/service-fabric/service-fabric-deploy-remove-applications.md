---
title: "Implementación de la aplicación de Service Fabric | Microsoft Docs"
description: "Cómo implementar y quitar aplicaciones de Service Fabric"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 4e3840f68c93998a52fa2956c2ea9d0976e0f627
ms.lasthandoff: 03/28/2017


---
# <a name="deploy-and-remove-applications-using-powershell"></a>Implementación y eliminación de aplicaciones con PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> 
> 

<br/>

Una vez que un [tipo de aplicación se ha empaquetado][10], está listo para la implementación en un clúster de Azure Service Fabric. La implementación implica los tres pasos siguientes:

1. Carga del paquete de aplicación en el almacén de imágenes
2. Cargar el tipo de aplicación
3. Crear la instancia de aplicación

Después de implementar una aplicación y ejecutar una instancia del clúster, puede eliminar la instancia y el tipo de aplicación. Quitar completamente una aplicación de clúster implica los pasos siguientes:

1. Quitar (o eliminar) la instancia de la aplicación en ejecución
2. Anular el registro del tipo de aplicación si ya no lo necesita
3. Quitar el paquete de aplicación del almacén de imágenes

Si usa [Visual Studio para implementar y depurar aplicaciones](service-fabric-publish-app-remote-cluster.md) en el clúster de desarrollo local, todos los pasos anteriores se controlan automáticamente mediante un script de PowerShell,  que se encuentra en la carpeta *Scripts* del proyecto de la aplicación. En este artículo se ofrece información sobre lo que hace ese script para que pueda realizar las mismas operaciones fuera de Visual Studio. 
 
## <a name="connect-to-the-cluster"></a>Conexión al clúster
Antes de ejecutar los comandos de PowerShell en este artículo, empiece siempre usando [Connect-ServiceFabricCluster](/powershell/servicefabric/vlatest/connect-servicefabriccluster) para conectarse al clúster de Service Fabric. Para conectarse al clúster de desarrollo local, ejecute lo siguiente:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Para obtener ejemplos de conexión a un clúster remoto o protegido con Azure Active Directory, certificados X509 o Windows Active Directory, consulte [Conexión a un clúster seguro](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-the-application-package"></a>Cargar el paquete de la aplicación
Cargar el paquete de aplicación lo pone en una ubicación a la que pueden tener acceso los componentes internos de Service Fabric.
Si quiere comprobar el paquete de la aplicación de forma local, use el cmdlet [Test-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage).

El comando [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) cargará el paquete de aplicación en el almacén de imágenes del clúster.
El cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** , que forma parte del módulo de PowerShell correspondiente al SDK de Service Fabric, se utiliza para obtener la cadena de conexión del almacén de imágenes.  Para importar el módulo de SDK, ejecute el siguiente código:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Imagine que compila y empaqueta una aplicación denominada *MyApplication* en Visual Studio. De forma predeterminada, el nombre del tipo de aplicación que aparece en ApplicationManifest.xml es "MyApplicationType".  El paquete de aplicación, que contiene el manifiesto de aplicación necesario, así como los manifiestos de servicio y los paquetes code/config/data, se encuentra en *C:\Users\username\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*. 

En el siguiente comando se muestra el contenido del paquete de aplicación:

```powershell
PS C:\> $path = 'C:\Users\user\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

Si el paquete de aplicación es grande y/o tiene muchos archivos, puede [comprimirlo](service-fabric-package-apps.md#compress-a-package). La compresión reduce el tamaño y el número de archivos.
El inconveniente es que el registro y la anulación del registro del tipo de aplicación son más rápidos. El tiempo de carga puede ser más lento en la actualidad, especialmente si incluye el tiempo para comprimir el paquete. 

Para comprimir un paquete, use el mismo comando [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage). La compresión puede realizarse independiente de la carga, utilizando la marca `SkipCopy`, o junto con la operación de carga. Comprimir un paquete comprimido es una operación inútil.
Para descomprimir un paquete comprimido, use el mismo comando [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) con el modificador `UncompressPackage`.

El siguiente cmdlet comprime el paquete sin copiarlo en el almacén de imágenes. El paquete ahora incluye los archivos comprimidos para los paquetes `Code` y `Config`. La aplicación y los manifiestos de servicio no se comprimen porque son necesarios para muchas operaciones internas (por ejemplo, uso compartido de paquetes, nombre de tipo de aplicación y extracción de versiones para ciertas validaciones). La compresión de los manifiestos haría que estas operaciones resultaran ineficaces.

```
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

Para paquetes de aplicación de gran tamaño, la compresión lleva tiempo. Para obtener mejores resultados, use una unidad SSD rápida. Los tiempos de compresión y el tamaño del paquete comprimido también varían en función del contenido del paquete.
Por ejemplo, a continuación figuran estadísticas de compresión para algunos paquetes, que muestran el tamaño de paquete inicial y comprimido, con el tiempo de compresión.

|Tamaño inicial (MB)|Número de archivos|Tipos de compresión|Tamaño de paquete comprimido (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Una vez que un paquete está comprimido, se puede cargar en uno o varios clústeres de Service Fabric según sea necesario. El mecanismo de implementación es el mismo para los paquetes comprimidos y sin comprimir. Si el paquete se comprime, se almacena como tal en el almacén de imágenes de clúster y se descomprime en el nodo antes de ejecutar la aplicación.


En el ejemplo siguiente se carga el paquete en el almacén de imágenes en una carpeta denominada "MyApplicationV1":

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

Si no se especifica el parámetro *-ApplicationPackagePathInImageStore*, el paquete de aplicación se copia en la carpeta "Debug" del almacén de imágenes.

El tiempo necesario para cargar un paquete depende de varios factores. Algunos de estos factores son el número de archivos del paquete, el tamaño del paquete y los tamaños de archivo. La velocidad de la red entre la máquina de origen y el clúster de Service Fabric también afecta al tiempo de carga. El tiempo de espera predeterminado para [ServiceFabricApplicationPackage copia](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) es de 30 minutos.
Dependiendo de los factores descritos, puede que tenga que aumentar el tiempo de espera. Si va a comprimir el paquete en la llamada de copia, también debe tener en cuenta el tiempo de compresión.

Consulte [Descripción del valor ImageStoreConnectionString](service-fabric-image-store-connection-string.md) para más información sobre el almacén de imágenes y su cadena de conexión.

## <a name="register-the-application-package"></a>Registrar el paquete de la aplicación
El tipo y la versión de la aplicación declarados en el manifiesto de aplicación estarán disponibles para usarse cuando se registre el paquete de aplicación. El sistema leerá el paquete cargado en el paso anterior, comprobará dicho paquete, procesará su contenido y copiará el paquete procesado en una ubicación del sistema interno.  

Ejecute el cmdlet [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) para registrar el tipo de aplicación en el clúster y ponerlo a disposición para la implementación:

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

"MyApplicationV1" es la carpeta del almacén de imágenes donde se encuentra el paquete de aplicación. El tipo de aplicación con el nombre "MyApplicationType" y la versión "1.0.0" (ambos se encuentran en el manifiesto de aplicación) ya estará registrado en el clúster.

El comando [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) devuelve solo después de que el sistema haya registrado correctamente el paquete de aplicación. El tiempo que tarde en registrarse dependerá del contenido del paquete de aplicación. El parámetro **- TimeoutSec** se puede usar para proporcionar un tiempo de espera más largo (el tiempo de espera predeterminado es de 60 segundos).

Si tiene un paquete de aplicación grande o está experimentando tiempos de espera, use el parámetro **-Async**. El comando se devuelve cuando el clúster acepta el comando de registro, y el procesamiento continúa según sea necesario.
El comando [Get-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/get-servicefabricapplicationtype) enumera todas las versiones del tipo de aplicación registradas correctamente, así como el estado de registro. Puede utilizar este comando para determinar cuándo se realiza el registro.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-the-application"></a>Creación de la aplicación
Se pueden crear instancias de una aplicación a partir de cualquier versión del tipo de aplicación que se haya registrado correctamente mediante el cmdlet [New-ServiceFabricApplication](/powershell/servicefabric/vlatest/new-servicefabricapplication). El nombre de cada aplicación debe empezar con el esquema *fabric:* y ser únicos para cada instancia de la aplicación. También se crean los servicios predeterminados que se hayan definido en el manifiesto de aplicación del tipo de aplicación de destino.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Pueden crearse varias instancias de aplicación para cualquier versión concreta de un tipo de aplicación registrado. Cada instancia de la aplicación se ejecuta de forma aislada, con su propio proceso y directorio de trabajo.

Para ver el nombre de las aplicaciones y los servicios en ejecución en el clúster, ejecute los cmdlets [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) y [Get-ServiceFabricService](/powershell/servicefabric/vlatest/get-servicefabricservice):

```powershell
PS C:\> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}

PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a>Eliminación de una aplicación
Cuando ya no se necesite una instancia de aplicación, use el cmdlet [Remove-ServiceFabricApplication](/powershell/servicefabric/vlatest/remove-servicefabricapplication) para quitarla por nombre de manera permanente. [Remove-ServiceFabricApplication](/powershell/servicefabric/vlatest/remove-servicefabricapplication) también quita automáticamente todos los servicios que pertenezcan a la aplicación, con lo que se eliminan de forma permanente todos los estados del servicio. No se puede deshacer esta operación y no se puede recuperar el estado de la aplicación.

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Anulación de un registro del tipo de aplicación
Cuando ya no se necesita una versión concreta de un tipo de aplicación, debe anularse el registro del tipo de aplicación mediante el cmdlet [Unregister-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype). Anular el registro de tipos de aplicación que no se usan libera espacio de almacenamiento del almacén de imágenes. No se puede anular el registro de un tipo de aplicación mientras no haya ninguna aplicación con instancias con él o no haya actualizaciones de aplicaciones pendientes que hagan referencia a él.

Ejecute el cmdlet [Get-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/get-servicefabricapplicationtype) para ver los tipos de aplicación registrados actualmente en el clúster:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Ejecute [Unregister-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/unregister-servicefabricapplicationtype) para anular el registro de un tipo concreto de aplicación:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-the-image-store"></a>Eliminación de un paquete de aplicación del almacén de imágenes
Cuando ya no se necesita un paquete de aplicación, puede eliminarlo del almacén de imágenes para liberar recursos del sistema.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a>Solución de problemas
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Copy-ServiceFabricApplicationPackage pide una ImageStoreConnectionString
El entorno del SDK de Service Fabric ya debe tener configurados los valores predeterminados correctos. Pero si es necesario, ImageStoreConnectionString para todos los comandos debe coincidir con el valor que usa el clúster de Service Fabric. Encontrará ImageStoreConnectionString en el manifiesto de clúster, que se recupera con el comando [Get-ServiceFabricClusterManifest](/powershell/servicefabric/vlatest/get-servicefabricclustermanifest):

```powershell
PS C:\> Get-ServiceFabricClusterManifest
```

ImageStoreConnectionString se encuentra en el manifiesto de clúster:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Consulte [Descripción del valor ImageStoreConnectionString](service-fabric-image-store-connection-string.md) para más información sobre el almacén de imágenes y su cadena de conexión.

### <a name="deploy-large-application-package"></a>Implementación de un paquete de aplicación grande
Problema: [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) agota el tiempo de espera para un paquete de aplicación grande (del orden de GB).
Pruebe lo siguiente:
- Especifique un tiempo de espera mayor para el comando [Copy-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) con el parámetro `TimeoutSec`. De forma predeterminada, el tiempo de espera es de 30 minutos.
- Compruebe la conexión de red entre la máquina de origen y el clúster. Si la conexión es lenta, considere la posibilidad de usar una máquina con una conexión de red mejor.
Si la máquina cliente se encuentra en otra región diferente al clúster, considere el uso de una máquina cliente que se encuentre en una región más cercana o en la misma región que el clúster.
- Compruebe si está alcanzando la limitación externa. Por ejemplo, cuando el almacén de imágenes está configurado para usar Azure Storage, se puede limitar carga.

Problema: La carga del paquete se completó correctamente, pero [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) agota el tiempo de espera.
Pruebe lo siguiente:
- [Comprima el paquete](service-fabric-package-apps.md#compress-a-package) antes de realizar la copia en el almacén de imágenes.
La compresión reduce el tamaño y el número de archivos lo que, a su vez, reduce la cantidad de tráfico y trabajo que Service Fabric debe realizar. La operación de carga puede ser más lenta (especialmente si incluyen el tiempo de compresión), pero el registro y la anulación del registro del tipo de aplicación son más rápidos.
- Especifique un tiempo de espera mayor para [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) con el parámetro `TimeoutSec`.
- Especifique el modificador `Async` para [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype). El comando se devuelve cuando el clúster acepta el comando y el aprovisionamiento continúa de forma asincrónica.
Por esta razón, no hay ninguna necesidad de especificar un tiempo de espera mayor en este caso.

### <a name="deploy-application-package-with-many-files"></a>Implementación del paquete de aplicación con muchos archivos
Problema: [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) agota el tiempo de espera para un paquete de aplicación con muchos archivos (del orden de miles).
Pruebe lo siguiente:
- [Comprima el paquete](service-fabric-package-apps.md#compress-a-package) antes de realizar la copia en el almacén de imágenes. La compresión reduce el número de archivos.
- Especifique un tiempo de espera mayor para [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) con el parámetro `TimeoutSec`.
- Especifique el modificador `Async` para [Register-ServiceFabricApplicationType](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype). El comando se devuelve cuando el clúster acepta el comando y el aprovisionamiento continúa de forma asincrónica.
Por esta razón, no hay ninguna necesidad de especificar un tiempo de espera mayor en este caso. 

## <a name="next-steps"></a>Pasos siguientes
[Actualización de la aplicación de Service Fabric](service-fabric-application-upgrade.md)

[Introducción al estado de Service Fabric](service-fabric-health-introduction.md)

[Diagnosticar y solucionar problemas de un servicio de Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modelar una aplicación en Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

