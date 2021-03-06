---
title: Esquemas definidos por el usuario en SQL Data Warehouse | Microsoft Docs
description: Sugerencias para usar los esquemas Transact-SQL en el Almacenamiento de datos SQL Azure para desarrollar soluciones.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 52af5bd5-d5d3-4f9b-8704-06829fb924e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9d59ffe4b0f0d858af18f30f848a63a28ac5f9b8


---
# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Esquemas definidos por el usuario en el Almacenamiento de datos SQL
Los almacenamientos de datos tradicionales suelen utilizar bases de datos independientes para crear los límites de la aplicación en función de la carga de trabajo, el dominio o la seguridad. Por ejemplo, un almacenamiento de datos de SQL Server tradicional podría incluir una base de datos provisional, una base de datos de almacenamiento de datos y algunas bases de datos data mart. En esta topología, cada base de datos funciona como una carga de trabajo y el límite de seguridad de la arquitectura.

Por el contrario, el Almacenamiento de datos SQL ejecuta la carga de trabajo completa del almacenamiento de datos dentro de una base de datos. No se permiten las combinaciones entre bases de datos. Por lo tanto, el Almacenamiento de datos SQL espera que todas las tablas que el almacenamiento utiliza se almacenen en una base de datos.

> [!NOTE]
> El Almacenamiento de datos SQL no admite consultas entre bases de datos de cualquier tipo. En consecuencia, deben revisarse las implementaciones de almacenamiento de datos que utilizan este patrón.
> 
> 

## <a name="recommendations"></a>Recomendaciones
Se trata de recomendaciones para consolidar los límites de cargas de trabajo, seguridad, dominio y funcionales con esquemas definidos por el usuario.

1. Use una base de datos de Almacenamiento de datos SQL para ejecutar la carga de trabajo completa del almacenamiento de datos.
2. Consolide el entorno de almacenamiento de datos existente para utilizar una base de datos de Almacenamiento de datos SQL.
3. Utilice **esquemas definidos por el usuario** para proporcionar el límite implementado anteriormente con las bases de datos.

Si los esquemas definidos por el usuario no se han utilizado anteriormente, tendrá una pizarra limpia. Utilice simplemente el nombre anterior de la base de datos como base para los esquemas definidos por el usuario en la base de datos de Almacenamiento de datos SQL.

Si ya se han utilizado los esquemas, tienen algunas opciones:

1. Quitar los nombres de esquemas heredados y empezar desde cero.
2. Conservar los nombres de esquemas heredados anteponiendo el nombre de esquema heredado al nombre de tabla.
3. Conservar los nombres de esquemas heredados mediante la implementación de vistas sobre la tabla en un esquema adicional para volver a crear la estructura del esquema anterior.

> [!NOTE]
> En la primera inspección, la opción 3 puede resultar la opción más atractiva. No obstante, el problema radica en los detalles. Las vistas son de solo lectura en el Almacenamiento de datos SQL. Cualquier modificación de datos o tablas tendría que realizarse según la tabla de base. La opción 3 también introduce una capa de vistas en el sistema. Desea incorporar algunas más a pesar de que ya utiliza vistas en la arquitectura.
> 
> 

### <a name="examples"></a>Ejemplos:
Implementar los esquemas definidos por el usuario en función de los nombres de base de datos.

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Conservar los nombres de esquemas heredados anteponiendo estos nombres al nombre de tabla. Utilizar esquemas para el límite de carga de trabajo.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Conservar los nombres de esquemas heredados con vistas.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> Cualquier cambio en la estrategia de esquema necesita una revisión del modelo de seguridad de la base de datos. En muchos casos puede simplificar el modelo de seguridad mediante la asignación de permisos a nivel de esquema. Si se requieren permisos más granulares, puede utilizar funciones de base de datos.
> 
> 

## <a name="next-steps"></a>Pasos siguientes
Para más sugerencias sobre desarrollo, consulte la [Introducción al desarrollo][Introducción al desarrollo].

<!--Image references-->

<!--Article references-->
[Introducción al desarrollo]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->



<!--HONumber=Nov16_HO3-->


