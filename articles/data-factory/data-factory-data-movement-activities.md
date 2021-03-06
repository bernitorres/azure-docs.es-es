---
title: Movimiento de datos con la actividad de copia | Microsoft Docs
description: "Aprenda sobre el movimiento de datos en las canalizaciones de Data Factory: migración de datos entre almacenes en la nube, entre un almacén de datos local y un almacén de datos en la nube. Utilice la actividad de copia."
keywords: "copiar datos, movimiento de datos, migración de datos, transferir datos"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 25e266441e902a06d980b3b51abdd4fcf668d4d2
ms.lasthandoff: 03/27/2017


---
# <a name="move-data-by-using-copy-activity"></a>Movimiento de datos con la actividad de copia
## <a name="overview"></a>Información general
En Data Factory de Azure, puede usar la actividad de copia para copiar datos de diferentes tipos desde diversos orígenes de datos locales y en la nube a Azure. Tras esto, se pueden analizar y transformar con más profundidad. La actividad de copia también puede utilizarse para publicar los resultados de transformación y análisis de inteligencia empresarial (BI) y el consumo de la aplicación.

![Rol de actividad de copia](media/data-factory-data-movement-activities/copy-activity.png)

La actividad de copia se basa en un [servicio globalmente disponible](#global)que es seguro, confiable y escalable. Este artículo proporciona detalles sobre el movimiento de datos en Data Factory y en la actividad de copia.

En primer lugar, veamos cómo se produce la migración de datos entre dos almacenes de datos en la nube, entre un almacén de datos local y un almacén de datos en la nube.

> [!NOTE]
> Para obtener más información sobre las actividades en general, consulte el artículo sobre [canalizaciones y actividades](data-factory-create-pipelines.md).
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>Copia de datos entre dos almacenes de datos en la nube
Cuando los almacenes de datos de origen y receptor residen en la nube, la actividad de copia pasa por las siguientes fases para copiar datos desde el origen hasta el receptor. El servicio que sustenta la actividad de copia realiza las siguientes acciones:

1. Lee datos del almacén de datos de origen.
2. Realiza procesos de serialización y deserialización, compresión y descompresión, asignación de columnas, y conversión de tipos. Lleva a cabo estas operaciones basadas en las configuraciones del conjunto de datos de entrada y de salida, y la actividad de copia.
3. Escribe datos en el almacén de datos de destino.

El servicio elige automáticamente la región más óptima para realizar el movimiento de datos. Normalmente, es la que está más cerca del almacén de datos receptor.

![Copia de la nube a la nube](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Copia de datos entre un almacén de datos local y un almacén de datos en la nube
Para mover con seguridad datos entre almacenes de datos locales y un almacén de datos en la nube de forma segura, instale la puerta de enlace de administración de datos en la máquina local. Data Management Gateway es un agente que permite procesar y mover datos híbridos. Puede instalarse en la misma máquina que el propio almacén de datos o en una independiente que pueda acceder al almacén de datos.

En este escenario, Data Management Gateway realiza procesos de serialización y deserialización, compresión y descompresión, asignación de columnas, y conversión de tipos. Los datos no fluyen a través del servicio Data Factory de Azure. En su lugar, Data Management Gateway escribe directamente los datos en el almacén de destino.

![Copia de entorno local a la nube](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Consulte [Movimiento de datos entre orígenes locales y la nube con Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) para ver una introducción y un tutorial. Consulte el artículo sobre [Data Management Gateway](data-factory-data-management-gateway.md) para obtener información detallada sobre esta agente.

También puede mover datos desde y hacia almacenes de datos compatibles hospedados en máquinas virtuales de IaaS de Azure mediante Data Management Gateway. En este caso, Data Management Gateway puede instalarse en la misma máquina virtual de Azure que el propio almacén de datos, o bien en una independiente que tenga acceso al almacén de datos.

## <a name="supported-data-stores-and-formats"></a>Almacenes de datos y formatos que se admiten
[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Si tiene que realizar operaciones de introducción o extracción de datos en relación con un almacén de datos que no sea compatible con la actividad de copia, puede usar la **actividad personalizada** de Data Factory con su propia lógica para copiar o mover los datos. Consulte el artículo [Uso de actividades personalizadas en una canalización de Data Factory de Azure](data-factory-use-custom-activities.md)para obtener más información sobre la creación y el uso de una actividad personalizada.

### <a name="supported-file-formats"></a>Formatos de archivos admitidos
La actividad de copia puede **copiar los archivos tal y como están** entre dos almacenes de datos basados en archivos, como Azure Blob, Azure Data Lake Store, Amazon S3, FTP, sistemas de archivos y HDFS. Para ello, puede hacer caso omiso de la [sección de formato](data-factory-create-datasets.md) de las definiciones de conjuntos de datos de entrada y de salida. Los datos se copian de forma eficaz sin procesos de serialización y deserialización.

La actividad de copia también lee y escribe en archivos de formatos especificados (**texto, Avro, ORC, Parquet y JSON**) y admite códecs de compresión (**GZip, Deflate, BZip2 y ZipDeflate**). Puede realizar las siguientes actividades de copia; por ejemplo:

* Copiar datos en formato de texto comprimido GZip (CSV) desde Blob de Azure y escribirlos en Azure SQL Database.
* Copiar archivos en formato de texto (CSV) desde el sistema de archivos local y escribirlos en Blob de Azure en formato Avro.
* Copiar datos en el SQL Server local y escribirlos en Azure Data Lake Store en formato ORC.
* Copiar archivos comprimidos del sistema de archivos local y, a continuación, descomprimirlos en Azure Data Lake Store.

## <a name="global"></a>Movimiento de datos disponible globalmente
Azure Data Factory solo está disponible en las regiones oeste de EE. UU., este de EE. UU. y Europa del Norte. Sin embargo, el servicio que ofrece la actividad de copia está disponible globalmente en las siguientes regiones y zonas geográficas. La topología disponible globalmente garantiza un movimiento de datos eficiente que, normalmente, evita saltos entre regiones. Consulte la sección [Servicios por región](https://azure.microsoft.com/regions/#services) para conocer la disponibilidad de Data Factory y el movimiento de datos en una región.

### <a name="copy-data-between-cloud-data-stores"></a>Copia de datos entre almacenes de datos en la nube
Si los almacenes de datos del origen y del receptor residen en la nube, Data Factory utiliza una implementación de servicio en la región más cercana a la ubicación del receptor en la misma ubicación geográfica para realizar el movimiento de datos. Consulte la tabla siguiente para la asignación:

| Geografía del almacén de datos de destino | Región del almacén de datos de destino | Región usada para el movimiento de datos |
|:--- |:--- |:--- |
| Estados Unidos | Este de EE. UU. | Este de EE. UU. |
| &nbsp; | Este de EE. UU. 2 | Este de EE. UU. 2 |
| &nbsp; | Central EE. UU.: | Central EE. UU.: |
| &nbsp; | Centro-Norte de EE. UU | Centro-Norte de EE. UU |
| &nbsp; | Centro-Sur de EE. UU | Centro-Sur de EE. UU |
| &nbsp; | Centro occidental de EE.UU. | Centro occidental de EE.UU. |
| &nbsp; | Oeste de EE. UU. | Oeste de EE. UU. |
| &nbsp; | Oeste de EE. UU. 2 | Oeste de EE. UU. |
| Canadá | Este de Canadá | Centro de Canadá |
| &nbsp; | Centro de Canadá | Centro de Canadá |
| Brasil | Sur de Brasil | Sur de Brasil |
| Europa | Europa del Norte | Europa del Norte |
| &nbsp; | Europa occidental | Europa occidental |
| Reino Unido | Oeste de Reino Unido | Sur del Reino Unido 2 |
| &nbsp; | Sur del Reino Unido | Sur del Reino Unido |
| Asia Pacífico | Sudeste asiático | Sudeste asiático |
| &nbsp; | Asia oriental | Sudeste asiático |
| Australia | Australia Oriental | Australia Oriental |
| &nbsp; | Sudeste de Australia | Sudeste de Australia |
| Japón | Este de Japón | Este de Japón |
| &nbsp; | Oeste de Japón | Este de Japón |
| India | India Central | India Central |
| &nbsp; | Oeste de la India | India Central |
| &nbsp; | Sur de la India | India Central |

También puede indicar explícitamente la región del servicio de Data Factory que se usará para realizar la copia especificando la propiedad `executionLocation` en la actividad de copia `typeProperties`. Los valores admitidos para esta propiedad se muestran en la columna **Región usada para el movimiento de datos** anterior. Tenga en cuenta que los datos se transmitirán por Internet por esa región durante la copia. Por ejemplo, para realizar copias entre almacenes de Azure de Corea, puede especificar `"executionLocation": "Japan East"` para redirigir los datos a través de la región de Japón (consulte el [ejemplo de JSON](#by-using-json-scripts) como referencia).

> [!NOTE]
> Si la región del almacén de datos de destino no está en la lista anterior o no se detecta, se producirá un error de forma predeterminada en la actividad de copia, en lugar de pasar a una región alternativa, a menos que se haya especificado `executionLocation`. En el futuro se irá ampliando la lista de regiones admitidas.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Copia de datos entre un almacén de datos local y un almacén de datos en la nube
Cuando se copian datos entre almacenes locales (o IaaS o máquinas virtuales de Azure) y almacenes en la nube, [Data Management Gateway](data-factory-data-management-gateway.md) realiza movimientos de datos en una máquina local o virtual. Los datos no fluyen a través del servicio en la nube, a menos que utilice la funcionalidad de [copia almacenada provisionalmente](data-factory-copy-activity-performance.md#staged-copy) . En este caso, lo hacen a través del almacenamiento de blobs de Azure provisional antes de que se escriban en el almacén de datos receptor.

## <a name="create-a-pipeline-with-copy-activity"></a>Creación de una canalización con actividad de copia
Puede crear una canalización con actividad de copia de dos maneras:

### <a name="by-using-the-copy-wizard"></a>Mediante el Asistente para copia
El Asistente para copia de Data Factory lo ayuda a crear una canalización con la actividad de copia. Esta canalización permite copiar datos de orígenes compatibles en destinos *sin escribir definiciones de JSON* para los servicios vinculados, los conjuntos de datos ni las canalizaciones. Consulte [Asistente para copia de Data Factory](data-factory-copy-wizard.md) para obtener más información sobre el asistente.  

### <a name="by-using-json-scripts"></a>Mediante scripts de JSON
Puede utilizar Editor de Data Factory del Portal de Azure, Visual Studio o Azure PowerShell para crear una definición de JSON para una canalización (mediante la actividad de copia). Después, puede implementarla para crear la canalización en Data Factory. Consulte [Tutorial: Uso de la actividad de copia en una canalización de Data Factory de Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) para obtener instrucciones paso a paso.    

Las propiedades JSON (como nombre, descripción, tablas de entrada y salida, y directivas) están disponibles para todos los tipos de actividades. Las propiedades que están disponibles en la sección `typeProperties` de la actividad varían con cada tipo de actividad.

En el caso de la actividad de copia, la sección `typeProperties` varía en función de los tipos de origen y receptor. Haga clic en un origen o receptor de la sección [Almacenes de datos que se admiten](#supported-data-stores-and-formats) para obtener más información sobre las propiedades de tipo compatibles con dicho almacén de datos.

Este es un ejemplo de definición de JSON:

```json
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from Azure blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputBlobTable"
          }
        ],
        "outputs": [
          {
            "name": "OutputSQLTable"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          },
          "executionLocation": "Japan East"          
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```
La programación definida en el conjunto de datos de salida determina cuándo se ejecuta la actividad (por ejemplo, **diariamente**: frecuencia **día** e intervalo **1**). Esta actividad copia los datos de un conjunto de datos de entrada (**origen**) en un conjunto de datos de salida (**receptor**).

Puede especificar más de un conjunto de datos de entrada para la actividad de copia. Se utilizan para comprobar las dependencias antes de ejecutarse la actividad. Sin embargo, solo los datos del primer conjunto de datos se copian en el de destino. Para obtener más información, vea [Programación y ejecución](data-factory-scheduling-and-execution.md).  

## <a name="performance-and-tuning"></a>Rendimiento y optimización
Vea el artículo [Guía de optimización y rendimiento de la actividad de copia](data-factory-copy-activity-performance.md), en el que se describen los factores claves que afectan al rendimiento del movimiento de datos (actividad de copia) en Data Factory de Azure. También muestra el rendimiento observado durante las pruebas internas y trata diversas maneras de optimizar el rendimiento de la actividad de copia.

## <a name="scheduling-and-sequential-copy"></a>Programación y copia secuencial
Consulte [Programación y ejecución](data-factory-scheduling-and-execution.md) para obtener información detallada sobre cómo funciona la programación y la ejecución en Data Factory. Es posible ejecutar varias operaciones de copia sucesivas de manera secuencial y ordenada. Consulte la sección [Copia secuencial](data-factory-scheduling-and-execution.md#run-activities-in-a-sequence).

## <a name="type-conversions"></a>Conversiones de tipos
Los diferentes almacenes de datos tienen sistemas con distintos tipos nativos. La actividad de copia realiza conversiones automáticas de los tipos del origen a los tipos del receptor con el siguiente enfoque de dos pasos:

1. Conversión de tipos de origen nativos al tipo .NET
2. Conversión de tipo .NET al tipo del receptor nativo

La asignación de un determinado sistema de tipo nativo a .NET para el almacén de datos se encuentra en el respectivo artículo de almacenes de datos. (haga clic en el vínculo concreto de la tabla de [almacenes de datos compatibles](#supported-data-stores) ). Puede usar estas asignaciones para determinar los tipos adecuados al crear las tablas, de forma que se realicen las conversiones adecuadas durante la actividad de copia.

## <a name="next-steps"></a>Pasos siguientes
* Para obtener más información acerca de la actividad de copia, consulte [Copia de datos de Blob Storage a Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Consulte [Movimiento de datos entre orígenes locales y la nube con Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md)para obtener más información sobre cómo mover datos de un almacén de datos local a uno en la nube.

