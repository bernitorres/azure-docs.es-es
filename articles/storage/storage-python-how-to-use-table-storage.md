---
title: Uso de Table Storage en Python | Microsoft Docs
description: "Almacene datos estructurados en la nube con el Almacenamiento de tablas de Azure, un almacén de datos NoSQL."
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 931503f56b32ce9d1b11283dff7224d7e2f015ae
ms.openlocfilehash: 98b02e8faa21e6d0e04d2f2c70bee6b8b018c010


---
# <a name="how-to-use-table-storage-from-python"></a>Uso del almacenamiento de tablas de Python
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Información general
Esta guía muestra cómo realizar algunas tareas comunes a través del servicio de almacenamiento Tabla de Azure. Los ejemplos están escritos en Python y usan el [SDK de Almacenamiento de Microsoft Azure para Python]. Entre los escenarios descritos se incluyen la creación y eliminación de una tabla, la inserción y la consulta de entidades en una tabla.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Creación de una tabla
El objeto **TableService** le permite trabajar con servicios de tabla. El siguiente código crea un objeto **TableService** . Agregue el código cerca de la parte superior de todo archivo Python en el que desee obtener acceso al almacenamiento de Azure mediante programación:

```python
from azure.storage.table import TableService, Entity
```

El código siguiente crea un objeto **TableService** usando el nombre de la cuenta de almacenamiento y la clave de la cuenta.  Reemplace 'myaccount' y 'mykey' por la clave y el nombre de cuenta.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Adición de una entidad a una tabla
Para agregar una entidad, primero cree un diccionario o entidad que defina sus nombres y valores de propiedad de la entidad. Tenga en cuenta que para cada entidad debe especificar valores en **PartitionKey** y **RowKey**. Estos son los identificadores únicos de las entidades. Puede consultar estos valores de una forma mucho más rápida que las demás propiedades. El sistema usa **PartitionKey** para distribuir automáticamente las entidades de la tabla entre varios nodos de almacenamiento.
Las entidades con la misma **PartitionKey** se almacenan en el mismo nodo. **RowKey** es el identificador exclusivo de la entidad de la partición a la que pertenece.

Para agregar una entidad a la tabla, pase un objeto de diccionario al método **insert\_entity**.

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

También puede pasar una instancia de la clase **Entity** al método **insert\_entity**.

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '2'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

## <a name="update-an-entity"></a>Actualización de una entidad
Este código muestra cómo reemplazar la versión anterior de una entidad existente con una versión actualizada.

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')
```

Si la entidad que se está actualizando no existe, la operación de actualización generará un error. Si desea almacenar una entidad independientemente de si ya existía antes, use **insert\_or\_replace_entity**.
En el siguiente ejemplo, la primera llamada reemplazará la entidad existente. La segunda llamada insertará una nueva entidad, debido a que en la tabla no existe ninguna entidad con **PartitionKey** y **RowKey** especificados.

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')
```

## <a name="change-a-group-of-entities"></a>Cambio de un grupo de entidades
A veces resulta útil enviar varias operaciones juntas en un lote a fin de garantizar el procesamiento atómico por parte del servidor. Para ello, use la clase **TableBatch** . Cuando quiera enviar el lote, llame a **commit\_batch**. Tenga en cuenta que todas las entidades deben estar en la misma partición para que se pueda cambiar como un lote. El ejemplo siguiente agrega dos entidades juntas en un lote.

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task10)
batch.insert_entity(task11)
table_service.commit_batch('tasktable', batch)
```

Los lotes también pueden usarse con la sintaxis de administrador de contexto:

```python
task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task12)
    batch.insert_entity(task13)
```

## <a name="query-for-an-entity"></a>Consulta de una entidad
Para consultar una entidad en una tabla, use el método **get\_entity**, pasando el valor de **PartitionKey** y **RowKey**.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Consulta de un conjunto de entidades
Este ejemplo busca todas las tareas en Seattle en función de la **PartitionKey**.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Consulta de un subconjunto de propiedades de las entidades
Una consulta de tabla puede recuperar solo algunas propiedades de una entidad.
Esta técnica, denominada *proyección*, reduce el ancho de banda y puede mejorar el rendimiento de las consultas, en especial en el caso de entidades de gran tamaño. Use el parámetro **select** y pase los nombres de las propiedades que desea que lleguen al cliente.

La consulta del código siguiente devuelve solo las descripciones de las entidades de la tabla.

> [!NOTE]
> El siguiente fragmento de código funciona solo con Azure Storage. Esto no es compatible con el emulador de almacenamiento.
>
>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Eliminación de una entidad
Puede eliminar una entidad usando la clave de partición y fila.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '1')
```

## <a name="delete-a-table"></a>Eliminación de una tabla
El código siguiente elimina una tabla de la cuenta de almacenamiento.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Pasos siguientes
Ahora que está familiarizado con los aspectos básicos del Almacenamiento de tablas, siga estos vínculos para obtener más información.

* [Centro para desarrolladores de Python](/develop/python/)
* [API de REST de servicios de almacenamiento de Azure](http://msdn.microsoft.com/library/azure/dd179355)
* [Blog del equipo de almacenamiento de Azure]
* [SDK de Almacenamiento de Microsoft Azure para Python]

[Blog del equipo de almacenamiento de Azure]: http://blogs.msdn.com/b/windowsazurestorage/
[SDK de Almacenamiento de Microsoft Azure para Python]: https://github.com/Azure/azure-storage-python



<!--HONumber=Dec16_HO2-->


