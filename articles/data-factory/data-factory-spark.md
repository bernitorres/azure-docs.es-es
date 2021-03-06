---
title: Invocar programas Spark desde Data Factory de Azure
description: "Obtenga información sobre cómo invocar programas Spark desde Data Factory de Azure mediante la actividad MapReduce."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 3c7d109ec7fd92859cad4ded7422105e8094485c
ms.lasthandoff: 03/30/2017


---
# <a name="invoke-spark-programs-from-data-factory"></a>Invocar programas Spark desde Data Factory
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Actividad de Hive](data-factory-hive-activity.md) 
> * [Actividad de Pig](data-factory-pig-activity.md)
> * [Actividad MapReduce](data-factory-map-reduce.md)
> * [Actividad de streaming de Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Actividad de Spark](data-factory-spark.md)
> * [Actividad de ejecución de Batch de Machine Learning](data-factory-azure-ml-batch-execution-activity.md)
> * [Actividad Actualizar recurso de Machine Learning](data-factory-azure-ml-update-resource-activity.md)
> * [Actividad de procedimiento almacenado](data-factory-stored-proc-activity.md)
> * [Actividad U-SQL de Data Lake Analytics](data-factory-usql-activity.md)
> * [Actividad personalizada de .NET](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Introducción
La actividad de Spark de HDInsight en una [canalización](data-factory-create-pipelines.md) de Data Factory ejecuta consultas de Spark en [su propio](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) clúster de HDInsight. Este artículo se basa en el artículo sobre [actividades de transformación de datos](data-factory-data-transformation-activities.md) , que presenta información general de la transformación de datos y las actividades de transformación admitidas.

> [!IMPORTANT]
> Si no está familiarizado con Azure Data Factory, se recomienda que revise el tutorial [Creación de su primera canalización](data-factory-build-your-first-pipeline.md) antes de leer este artículo. Para información general sobre el servicio Azure Data Factory, consulte [Introducción al servicio Azure Data Factory](data-factory-introduction.md). 

## <a name="apache-spark-cluster-in-azure-hdinsight"></a>Clúster de Apache Spark en Azure HDInsight
Primero, cree un clúster de Apache Spark en HDInsight de Azure siguiendo las instrucciones del tutorial: [Creación de un clúster de Apache Spark en Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="create-data-factory"></a>Creación de Data Factory 
Estos son los pasos habituales para crear una canalización de Data Factory con una actividad de Spark.  

1. Creación de una factoría de datos.
2. Cree un servicio vinculado que vincule el clúster de Apache Spark en Azure HDInsight con su factoría de datos.
3. Actualmente, debe especificar un conjunto de datos de salida para una actividad incluso si no se produce ninguna salida. Para crear un conjunto de datos de Azure Blob, realice los siguientes pasos:
    1. Cree un servicio vinculado que vincule la cuenta de Azure Storage con la factoría de datos.
    2. Cree un conjunto de datos que haga referencia al servicio vinculado de Azure Storage.  
3. Cree una canalización con la actividad de Spark que hace referencia al servicio vinculado de Apache HDInsight creado en el paso 2. La actividad se configura con el conjunto de datos que creó en el paso anterior como un conjunto de datos de salida. El conjunto de datos de salida es lo que impulsa la programación (cada hora, cada día, etc.). Por lo tanto, debe especificar el conjunto de datos de salida aunque la actividad no produzca realmente una salida.

Para obtener instrucciones detalladas paso a paso para crear una factoría de datos, consulte el tutorial: [Creación de su primera canalización](data-factory-build-your-first-pipeline.md). En este tutorial se usa una actividad de Hive con un clúster de HDInsight Hadoop, pero los pasos son similares al uso de una actividad de Spark con un clúster de HDInsight Spark.   

En las secciones siguientes se proporciona información sobre las entidades de Data Factory para usar un clúster de Apache Spark y una actividad de Spark en su factoría de datos.   

## <a name="hdinsight-linked-service"></a>Servicio vinculado de HDInsight
Antes de emplear una actividad de Spark en una canalización de Data Factory, cree su servicio vinculado de HDInsight. El siguiente fragmento de código JSON muestra la definición de un servicio vinculado de HDInsight que apunta a un clúster de Azure HDInsight Spark.   

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<nameofyoursparkcluster>.azurehdinsight.net/",
              "userName": "admin",
              "password": "password",
              "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

> [!NOTE]
> Actualmente, la actividad de Spark no admite clústeres de Spark que usan Azure Data Lake Store como almacenamiento principal o un servicio vinculado de HDInsight a petición. 

Para obtener más información sobre el servicio vinculado de HDInsight y otros servicios vinculados de proceso, consulte el artículo sobre [servicios vinculados de proceso de Data Factory](data-factory-compute-linked-services.md). 

## <a name="spark-activity-json"></a>Definición JSON de actividad de Spark
Esta es la definición JSON de ejemplo de una canalización con la actividad de Spark:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

Tenga en cuenta los siguientes puntos: 
- El tipo de propiedad se establece en HDInsightSpark. 
- El valor de rootPath se establece en adfspark\\pyFiles, donde adfspark es el contenedor de blobs de Azure y pyFiles es la carpeta adecuada en ese contenedor. En este ejemplo, la instancia de Azure Blob Storage es la que está asociada con el clúster de Spark. Puede cargar el archivo en un almacenamiento de Azure diferente. Si lo hace, cree un servicio vinculado de Azure Storage para vincular esa cuenta de almacenamiento con la factoría de datos. A continuación, especifique el nombre del servicio vinculado como un valor de la propiedad sparkJobLinkedService. Consulte las [propiedades de la actividad de Spark](#spark-activity-properties) para más detalles sobre esta y otras propiedades que admite la actividad de Spark.  
- El valor de entryFilePath se establece en test.py, que es el archivo de Python. 
- Los valores de parámetros del programa de Spark se pasan mediante la propiedad de argumentos. En este ejemplo, hay dos argumentos: arg1 y arg2. 
- La propiedad getDebugInfo está establecida en Siempre, lo que significa que siempre se generan archivos de registro (acierto o error). 
- La sección sparkConfig especifica una configuración de entorno de Python: worker.memory. 
- La sección de salida tiene un conjunto de datos de salida. Debe especificar un conjunto de datos de salida, incluso si el programa de Spark no genera ninguna salida. El conjunto de datos de salida impulsa la programación de la canalización (cada hora, cada día, etc.).     

Las propiedades de tipo (en la sección typeProperties) se describen más adelante en este artículo en la sección [Propiedades de la actividad de Spark](#spark-activity-properties). 

Como se mencionó anteriormente, debe especificar un conjunto de datos de salida para la actividad ya que esto es lo que impulsa la programación de la canalización (cada día, cada hora, etc.). En este ejemplo, se usa un conjunto de datos de Blob de Azure. Para crear un conjunto de datos de Blob de Azure, debe crear primero un servicio vinculado de Azure Storage. 

Estas son las definiciones de ejemplo del servicio vinculado de Azure Storage y el conjunto de datos de Blob de Azure: 

**Servicio vinculado de Azure Storage:**
```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<storageaccountkey>"
        }
    }
}
```
 

**Conjunto de datos de Blob de Azure:** 
```json
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "sparkoutput.txt",
            "folderPath": "spark/output/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Este conjunto de datos es más un conjunto de datos ficticio. Data Factory usa la configuración de frecuencia e intervalo y ejecuta la canalización diariamente en las horas de inicio y finalización de una canalización. En la definición de canalización de ejemplo, los tiempos de inicio y finalización tienen solo de un día de diferencia, por lo que la canalización se ejecuta solo una vez. 

## <a name="spark-activity-properties"></a>Propiedades de la actividad de Spark

En la siguiente tabla se describen las propiedades JSON que se usan en la definición de JSON: 

| Propiedad | Descripción | Obligatorio |
| -------- | ----------- | -------- |
| name | Nombre de la actividad en la canalización. | Sí |
| Descripción | Texto que describe para qué se usa la actividad. | No |
| type | Esta propiedad debe establecerse en HDInsightSpark. | Sí |
| linkedServiceName | Nombre del servicio vinculado de HDInsight en el que se ejecuta el programa de Spark. | Sí |
| rootPath | Contenedor de blobs de Azure y la carpeta que contiene el archivo de Spark. El nombre del archivo distingue mayúsculas de minúsculas. | Sí |
| entryFilePath | Ruta de acceso relativa a la carpeta raíz del código o el paquete de Spark. | Sí |
| className | Clase principal de Spark o Java de la aplicación. | No | 
| argumentos | Lista de argumentos de línea de comandos del programa de Spark. | No | 
| proxyUser | Cuenta de usuario de suplantación para ejecutar el programa de Spark. | No | 
| sparkConfig | Propiedades de configuración de Spark. | No | 
| getDebugInfo | Especifica si se copian los archivos de registro de Spark en el almacenamiento de Azure que usa el clúster de HDInsight que especifica sparkJobLinkedService. Valores permitidos: Ninguno, Siempre o Error. Valor predeterminado: Ninguno. | No | 
| sparkJobLinkedService | El servicio vinculado de Azure Storage que contiene los registros, las dependencias y los archivos de trabajos de Spark.  Si no especifica un valor para esta propiedad, se usa el almacenamiento asociado con el clúster de HDInsight. | No |

## <a name="folder-structure"></a>Estructura de carpetas
La actividad de Spark no es compatible con un script en línea, al contrario que las actividades de Pig y Hive. Los trabajos de Spark también son más ampliable que los de Pig y Hive. En los trabajos de Spark, puede proporcionar varias dependencias como paquetes JAR (ubicados en la CLASSPATH de Java), archivos de Python (ubicados en la ruta PYTHONPATH) y cualquier otro archivo.

Cree la siguiente estructura de carpetas en la instancia de Azure Blob Storage a la que hace referencia el servicio vinculado de HDInsight. Luego, cargue los archivos dependientes en las subcarpetas adecuadas de la carpeta raíz que representa **entryFilePath**. Por ejemplo, cargue los archivos de Python en la subcarpeta pyFiles y los archivos JAR en la subcarpeta jars de la carpeta raíz. En el entorno de tiempo de ejecución, el servicio Data Factory espera la siguiente estructura de carpetas en Azure Blob Storage:     

| Ruta de acceso | Descripción | Obligatorio | Escriba |
| ---- | ----------- | -------- | ---- | 
| .    | Ruta de acceso raíz del trabajo de Spark en el servicio vinculado de almacenamiento.    | Sí | Carpeta |
| &lt;Definida por el usuario&gt; | Ruta de acceso que apunta al archivo de entrada del trabajo de Spark. | Sí | Archivo | 
| ./jars | Todos los archivos de esta carpeta se cargan y se colocan en la ruta CLASSPATH de Java del clúster. | No | Carpeta |
| ./pyFiles | Todos los archivos de esta carpeta se cargan y se colocan en la ruta PYTHONPATH del clúster. | No | Carpeta |
| ./files | Todos los archivos de esta carpeta se cargan y se colocan en el directorio de trabajo del ejecutor. | No | Carpeta |
| ./archives | Todos los archivos de esta carpeta están sin comprimir. | No | Carpeta |
| ./logs | Carpeta donde se almacenan los registros del clúster de Spark.| No | Carpeta |

Este es un ejemplo de un almacenamiento que contiene dos archivos de trabajos de Spark en la instancia de Azure Blob Storage a la que hace referencia el servicio vinculado de HDInsight. 

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs    
```

> [!IMPORTANT]
> Para ver un tutorial completo sobre cómo crear una canalización con una actividad de transformación, lea el artículo [Creación de una canalización para transformar datos](data-factory-build-your-first-pipeline-using-editor.md). 



## <a name="see-also"></a>Otras referencias
* [Actividad de Hive](data-factory-hive-activity.md)
* [Actividad de Pig](data-factory-pig-activity.md)
* [Actividad MapReduce](data-factory-map-reduce.md)
* [Actividad de streaming de Hadoop](data-factory-hadoop-streaming-activity.md)
* [Invocar scripts de R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


