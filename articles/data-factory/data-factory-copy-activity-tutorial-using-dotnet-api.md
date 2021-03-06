---
title: "Tutorial: Crear una canalización con la actividad de copia mediante la API de NET | Microsoft Docs"
description: "En este tutorial, se crea una canalización de Data Factory de Azure con una actividad de copia mediante la API de .NET."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 58fc4007-b46d-4c8e-a279-cb9e479b3e2b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/17/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 4b29fd1c188c76a7c65c4dcff02dc9efdf3ebaee
ms.openlocfilehash: 733c151012e3d896f720fbc64120432aca594bda
ms.lasthandoff: 02/03/2017


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Tutorial: crear una canalización con la actividad de copia mediante la API de NET
> [!div class="op_single_selector"]
> * [Introducción y requisitos previos](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Asistente para copia](data-factory-copy-data-wizard-tutorial.md)
> * [Portal de Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Plantilla de Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [API DE REST](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [API de .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)

Este tutorial muestra cómo crear y supervisar una instancia de Data Factory de Azure mediante la API de .NET. La canalización de la factoría de datos utiliza una actividad de copia para copiar datos desde Almacenamiento de blobs de Azure a Base de datos SQL de Azure.

La actividad de copia realiza el movimiento de datos en Data Factory de Azure. La actividad funciona con un servicio disponible de forma global que puede copiar datos entre varios almacenes de datos de forma segura, confiable y escalable. Consulte el artículo [Actividades de movimiento de datos](data-factory-data-movement-activities.md) para obtener más información sobre la actividad de copia.

> [!NOTE]
> Este artículo no abarca todas las API de .NET de Data Factory. Para más información sobre el SDK de .NET de Data Factory, consulte [Referencia de API de. NTR de Data Factory](https://msdn.microsoft.com/library/mt415893.aspx) .
> 
> La canalización de datos de este tutorial copia datos de un almacén de datos de origen a un almacén de datos de destino. No transforma los datos de entrada para generar datos de salida. Para ver un tutorial acerca de cómo transformar datos mediante Azure Data Factory, consulte [Tutorial: Compilación de la primera canalización para procesar datos mediante el clúster de Hadoop](data-factory-build-your-first-pipeline.md).


## <a name="prerequisites"></a>Requisitos previos
* Para obtener información general del tutorial y completar los pasos de los [requisitos previos](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) , consulte **Copia de datos de Almacenamiento de blobs en Base de datos SQL mediante Data Factory** .
* Visual Studio 2012, 2013 o 2015
* Descargue e instale el [SDK de .NET de Azure](http://azure.microsoft.com/downloads/)
* Azure PowerShell. Siga las instrucciones del artículo [Cómo instalar y configurar Azure PowerShell](/powershell/azureps-cmdlets-docs) para instalar Azure PowerShell en su equipo. Azure PowerShell se usa para crear una aplicación de Azure Active Directory.

### <a name="create-an-application-in-azure-active-directory"></a>Creación de una aplicación en Azure Active Directory
Cree una aplicación de Azure Active Directory, cree una entidad de servicio para dicha aplicación y asígnela al rol **Colaborador de Data Factory** .

1. Inicie **PowerShell**.
2. Ejecute el siguiente comando y escriba el nombre de usuario y la contraseña que utiliza para iniciar sesión en el Portal de Azure.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Ejecute el siguiente comando para ver todas las suscripciones para esta cuenta.

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. Ejecute el comando siguiente para seleccionar la suscripción con la que desea trabajar. Reemplace **&lt;NameOfAzureSubscription**&gt; por el nombre de su suscripción de Azure.

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > Anote **SubscriptionId** y **TenantId** de la salida de este comando.

5. Cree un grupo de recursos de Azure con el nombre **ADFTutorialResourceGroup** ejecutando el siguiente comando en PowerShell.

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    Si el grupo de recursos ya existe, especifique si desea actualizarlo (Y) o mantenerlo como está (N).

    Si usa un otro grupo de recursos, deberá usar su nombre en lugar de ADFTutorialResourceGroup en este tutorial.
6. Cree una aplicación de Azure Active Directory.

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    Si aparece el siguiente error, especifique otra dirección URL distinta y vuelva a ejecutar el comando.
    
    ```PowerShell
    Another object with the same value for property identifierUris already exists.
    ```
7. Cree la entidad de servicio de AD.

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. Agregue la entidad de servicio al rol **Colaborador de Data Factory** .

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. Obtenga el identificador de aplicación.

    ```PowerShell
    $azureAdApplication    
    ```
    Anote el identificador de aplicación (**applicationID** de la salida).

Debe tener los cuatro valores siguientes de estos pasos:

* Id. de inquilino
* Id. de suscripción
* Identificador de aplicación
* Contraseña (especificado en el primer comando)

## <a name="walkthrough"></a>Tutorial
1. Con Visual Studio 2012, 2013 o 2015, cree una aplicación de consola .NET de C#.
   1. Inicie **Visual Studio** 2012/2013/2015.
   2. Haga clic en **Archivo**, seleccione **Nuevo** y, luego, haga clic en **Proyecto**.
   3. Expanda **Plantillas** y seleccione **Visual C#**. En este tutorial, se usa C# pero puede usar cualquier lenguaje .NET.
   4. Seleccione **Aplicación de consola** en la lista de tipos de proyecto de la derecha.
   5. Escriba **DataFactoryAPITestApp** en Nombre.
   6. Seleccione **C:\ADFGetStarted** para Ubicación.
   7. Haga clic en **Aceptar** para crear el proyecto.
2. Haga clic en **Herramientas**, seleccione **Administrador de paquetes NuGet** y haga clic en **Consola del Administrador de paquetes**.
3. En la **Consola del Administrador de paquetes**, siga estos pasos:
   1. Ejecute el comando siguiente para instalar el paquete de Data Factory: `Install-Package Microsoft.Azure.Management.DataFactories`
   2. Ejecute el comando siguiente para instalar el paquete de Azure Active Directory (utilizará la API de Active Directory en el código): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
4. Agregue la siguiente sección **appSetttings** al archivo **App.config**. Esta configuración la usa el método auxiliar: **GetAuthorizationHeader**.

    Reemplace los valores de **&lt;Id. de aplicación&gt;**, **&lt;Contraseña&gt;**, **&lt;Id. de suscripción&gt;** e **&lt;Id. de inquilino&gt;** por los suyos propios.

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating the AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```

5. Agregue las siguientes instrucciones **using** al archivo de origen (Program.cs) en el proyecto.

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```

6. Agregue el código siguiente que crea una instancia de la clase **DataPipelineManagementClient** al método **Main**. Este objeto se usa para crear una factoría de datos, un servicio vinculado, conjuntos de datos de entrada y salida, y una canalización. También se usa para supervisar los segmentos de un conjunto de datos en tiempo de ejecución.

    ```csharp
    // create data factory management client
    string resourceGroupName = "ADFTutorialResourceGroup";
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > Reemplace el valor de **resourcegroupname** por el nombre de su grupo de recursos de Azure.
   >
   > Actualice el nombre de la factoría de datos (**dataFactoryName**) para que sea único. El nombre de la factoría de datos debe ser único a nivel global. Consulte el tema [Factoría de datos: reglas de nomenclatura](data-factory-naming-rules.md) para las reglas de nomenclatura para los artefactos de Factoría de datos.

7. Agregue el siguiente código que crea una **factoría de datos** en el método **Main**.

    ```csharp
    // create a data factory
    Console.WriteLine("Creating a data factory");
    client.DataFactories.CreateOrUpdate(resourceGroupName,
        new DataFactoryCreateOrUpdateParameters()
        {
            DataFactory = new DataFactory()
            {
                Name = dataFactoryName,
                Location = "westus",
                Properties = new DataFactoryProperties()
            }
        }
    );
    ```

8. Agregue el siguiente código que crea un servicio vinculado de **Azure Storage** al método **Main**.

   > [!IMPORTANT]
   > Reemplace **storageaccountname** y **accountkey** por el nombre y la clave de su cuenta de Azure Storage.

    ```csharp
    // create a linked service for input data store: Azure Storage
    Console.WriteLine("Creating Azure Storage linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureStorageLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```

9. Agregue el siguiente código que crea un **servicio vinculado de Azure SQL** al método **Main**.

   > [!IMPORTANT]
   > Reemplace **servername**, **databasename**, **username** y **password** por los nombres del servidor de Azure SQL, de la base de datos, del usuario y de la contraseña.

    ```csharp
    // create a linked service for output data store: Azure SQL Database
    Console.WriteLine("Creating Azure SQL Database linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureSqlLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                )
            }
        }
    );
    ```

10. Agregue el siguiente código que crea **conjuntos de datos de entrada y salida** en el método **Main**.

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetAzureSqlDestination";

    Console.WriteLine("Creating input dataset of type: Azure Blob");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                Structure = new List<DataElement>()
                {
                    new DataElement() { Name = "FirstName", Type = "String" },
                    new DataElement() { Name = "LastName", Type = "String" }
                },
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },

                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });

    Console.WriteLine("Creating output dataset of type: Azure SQL");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new DatasetCreateOrUpdateParameters()
        {
            Dataset = new Dataset()
            {
                Name = Dataset_Destination,
                Properties = new DatasetProperties()
                {
                    Structure = new List<DataElement>()
                    {
                        new DataElement() { Name = "FirstName", Type = "String" },
                        new DataElement() { Name = "LastName", Type = "String" }
                    },
                    LinkedServiceName = "AzureSqlLinkedService",
                    TypeProperties = new AzureSqlTableDataset()
                    {
                        TableName = "emp"
                    },
                    Availability = new Availability()
                    {
                        Frequency = SchedulePeriod.Hour,
                        Interval = 1,
                    },
                }
            }
        });
    ```

11. Agregue el siguiente código que **crea y activa una canalización** al método **Main**. Esta canalización tiene un elemento **CopyActivity** que toma **BlobSource** como origen y **BlobSink** como receptor.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "ADFTutorialPipeline";

    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new PipelineCreateOrUpdateParameters()
        {
            Pipeline = new Pipeline()
            {
                Name = PipelineName,
                Properties = new PipelineProperties()
                {
                    Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                    End = PipelineActivePeriodEndTime,

                    Activities = new List<Activity>()
                    {
                        new Activity()
                        {
                            Name = "BlobToAzureSql",
                            Inputs = new List<ActivityInput>()
                            {
                                new ActivityInput() {
                                    Name = Dataset_Source
                                }
                            },
                            Outputs = new List<ActivityOutput>()
                            {
                                new ActivityOutput()
                                {
                                    Name = Dataset_Destination
                                }
                            },
                            TypeProperties = new CopyActivity()
                            {
                                Source = new BlobSource(),
                                Sink = new BlobSink()
                                {
                                    WriteBatchSize = 10000,
                                    WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                }
                            }
                        }
                    }
                }
            }
        });
    ```

12. Agregue el código siguiente al método **Main** para obtener el estado de un segmento de datos del conjunto de datos de salida. Solo se espera un segmento en este ejemplo.

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;

    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling the slice status");        
        // wait before the next status check
        Thread.Sleep(1000 * 12);

        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });

        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```

13. Agregue el código siguiente para obtener los detalles de la ejecución de un segmento de datos al método **Main** .

    ```csharp
    Console.WriteLine("Getting run details of a data slice");

    // give it a few minutes for the output slice to be ready
    Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
    Console.ReadKey();

    var datasliceRunListResponse = client.DataSliceRuns.List(
            resourceGroupName,
            dataFactoryName,
            Dataset_Destination,
            new DataSliceRunListParameters()
            {
                DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
            }
        );

    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }

    Console.WriteLine("\nPress any key to exit.");
    Console.ReadKey();
    ```

14. Agregue el siguiente método auxiliar usado por el método **Main** a la clase **Program**.

    ```csharp
    public static async Task<string> GetAuthorizationHeader()
    {
        AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        ClientCredential credential = new ClientCredential(
            ConfigurationManager.AppSettings["ApplicationId"],
            ConfigurationManager.AppSettings["Password"]);
        AuthenticationResult result = await context.AcquireTokenAsync(
            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
            clientCredential: credential);

        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed to acquire token");
    }
    ```

15. En el Explorador de soluciones, expanda el proyecto (**DataFactoryAPITestApp**), haga clic con el botón derecho en **Referencias**, y haga clic en **Agregar referencia**. Active la casilla del ensamblado "**System.Configuration**" y haga clic en **Aceptar**.
16. Compile la aplicación de la consola. Haga clic en **Compilar** en el menú y en **Compilar solución**.
17. Confirme que hay al menos un archivo en el contenedor **adftutorial** del Almacenamiento de blobs de Azure. De lo contrario, cree el archivo **Emp.txt** en el Bloc de notas con el siguiente contenido y cárguelo en el contenedor adftutorial.

    ```
    John, Doe
    Jane, Doe
    ```
18. Para ejecutar el ejemplo haga clic en **Depurar** -> **Iniciar depuración** en el menú. Cuando vea **Getting run details of a data slice**, espere unos minutos y presione **ENTRAR**.
19. Use el Portal de Azure para comprobar que la factoría de datos **APITutorialFactory** se crea con los siguientes artefactos:
   * Servicio vinculado: **LinkedService_AzureStorage**
   * Conjunto de datos: **DatasetBlobSource** y **DatasetBlobDestination**.
   * Canalización: **PipelineBlobSample**
20. Compruebe que los dos registros de empleados se han creado en la tabla "**emp**" de la base de datos de Azure SQL Database especificada.

## <a name="next-steps"></a>Pasos siguientes
| Tema. | Descripción |
|:--- |:--- |
| [Procesos](data-factory-create-pipelines.md) |Este artículo le ayuda a conocer las canalizaciones y actividades de Data Factory de Azure. |
| [Conjuntos de datos](data-factory-create-datasets.md) |Este artículo le ayuda a comprender los conjuntos de datos de Data Factory de Azure. |
| [Programación y ejecución con Data Factory](data-factory-scheduling-and-execution.md) |En este artículo se explican los aspectos de programación y ejecución del modelo de aplicación de Factoría de datos de Azure. |
[Referencia de la API de .NET de Data Factory](/dotnet/api/) | Proporciona información acerca del SDK de .NET de Data Factory (busque Microsoft.Azure.Management.DataFactories.Models en la vista de árbol).

