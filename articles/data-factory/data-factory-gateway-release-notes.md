---
title: "Notas de la versión de Data Management Gateway | Microsoft Docs"
description: "Notas de la versión de Data Management Gateway"
services: data-factory
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: spelluru
published: true
translationtype: Human Translation
ms.sourcegitcommit: db0face48d84680eabd82245bd38bd49b204f9ae
ms.openlocfilehash: a88b791bad9f71f16700ccc7efdee8ef493478a9
ms.lasthandoff: 02/13/2017

---
# <a name="release-notes-for-data-management-gateway"></a>Notas de la versión de Data Management Gateway
Uno de los desafíos de la integración de datos moderna es mover datos sin problemas entre ubicación la local y la nube. Data Factory hace que dicha integración sea perfecta con Data Management Gateway, que es un agente que se puede instalar de forma local para permitir el movimiento de datos híbridos.

En los siguientes artículos se proporciona información detallada sobre Data Management Gateway y cómo utilizarlo: 

*  [Data Management Gateway](data-factory-data-management-gateway.md)
*  [Movimiento de datos entre una ubicación local y la nube mediante Factoría de datos de Azure](data-factory-move-data-between-onprem-and-cloud.md) 


## <a name="current-version-2762192"></a>VERSIÓN ACTUAL (2.7.6219.2)

### <a name="whats-new"></a>Novedades
- Ahora puede autenticarse en Azure Data Lake Store con la entidad de servicio. Anteriormente, solo se admitiría OAuth.
- Hemos creado un nuevo controlador para leer los datos del almacén de datos local de Oracle en la puerta de enlace.

### <a name="enhancements-"></a>Mejoras-
- Mejora del rendimiento de lectura de datos del origen de datos de Oracle.
- Corregido: Problema con la expiración del token de OAuth del origen de OData.
- Corregido: Problema al leer decimales de Oracle de más de 28 bits.


## <a name="earlier-versions"></a>Versiones anteriores

## <a name="2661922"></a>2.6.6192.2
### <a name="whats-new"></a>Novedades
- Los clientes pueden proporcionar comentarios en la experiencia de registro de la puerta de enlace.
- Nuevo formato de compresión compatible: ZIP (Deflate).

### <a name="enhancements-"></a>Mejoras-
- Mejora del rendimiento del origen de HDFS y el receptor de Oracle.
- Corrección de errores de la capacidad de procesamiento en paralelo y la actualización automática de la puerta de enlace.


## <a name="2561641"></a>2.5.6164.1
### <a name="enhancements"></a>Mejoras
- Experiencia de registro de la puerta de enlace mejorada y más sólida: ahora puede realizar el seguimiento del estado del progreso durante el proceso de registro de la puerta de enlace, con lo que la experiencia de registro tiene una mayor capacidad de respuesta.
- Mejora del proceso de restauración de la puerta de enlace: puede recuperar la puerta de enlace incluso si no tiene el archivo de copia de seguridad de la puerta de enlace con esta actualización. Esto requeriría restablecer credenciales de servicio vinculado en el Portal.
- Corrección de errores.

## <a name="2461511"></a>2.4.6151.1

### <a name="whats-new"></a>Novedades

- Ahora puede almacenar localmente las credenciales de origen de datos. Las credenciales se cifran. Las credenciales del origen de datos se pueden recuperar y restaurar usando el archivo de copia de seguridad que se puede exportar desde la puerta de enlace existente, todo de manera local. 

### <a name="enhancements-"></a>Mejoras-

- Experiencia de registro de puerta de enlace mejorada y más eficaz.
- Compatibilidad con detección automática de la configuración de QuoteChar para formato de texto en el Asistente para copia y mejorar la precisión de la detección de formato general.

## <a name="2361002"></a>2.3.6100.2

- Compatibilidad con la detección automática de firstRowAsHeader y SkipLineCount en el Asistente para copia para archivos de texto en el sistema de archivos local y HDFS.
- Mejora de la estabilidad de la conexión de red entre la puerta de enlace y Service Bus
- Unas pocas correcciones de errores


## <a name="2260721"></a>2.2.6072.1

*  Admite la configuración de proxy HTTP para la puerta de enlace con el Administrador de configuración de Data Management Gateway. Si está configurado, se accede a Blob de Azure, Tabla de Azure, Azure Data Lake y DocumentDB a través del proxy HTTP.
*  Admite el control de encabezados para TextFormat al copiar datos en Blob de Azure, Azure Data Lake Store, el sistema de archivos local y el HDFS local como origen y destino.
*  Admite la copia de datos desde blobs en anexos y blobs en páginas junto con los blobs en bloques que ya eran compatibles.
*  Introduce un nuevo estado de puerta de enlace **Online (Limited)**(En línea [limitado]), lo que indica que se puede utilizar sin problemas la funcionalidad principal de la puerta de enlace, pero la operación interactiva del Asistente para copia.
*  Mejora la estabilidad del registro de puerta de enlace mediante la clave de registro.

## <a name="216040"></a>2.1.6040.

*  El controlador DB2 se incluye ahora en el paquete de instalación de la puerta de enlace. No es necesario instalarlo por separado. 
*  El controlador DB2 ahora admite z/OS y DB2 para i (AS /&400;) junto con las plataformas ya admitidas (Linux, Unix y Windows). 
*  Admite el uso de DocumentDB como origen o destino para almacenes de datos locales
*  Admite la copia de datos desde/a blobs de almacenamiento en frío y en caliente junto con la cuenta de almacenamiento general ya admitida. 
*  Le permite conectarse a la instancia local de SQL Server a través de la puerta de enlace con privilegios de inicio de sesión remoto.  

## <a name="2060131"></a>2.0.6013.1

*  Puede seleccionar el idioma o la referencia cultural que utilizará una puerta de enlace durante la instalación manual.

*  Cuando la puerta de enlace no funciona del modo previsto, puede optar por enviar los registros de la puerta de enlace de los últimos&7; días a Microsoft para que pueda solucionar el problema. Si la puerta de enlace no está conectada al servicio en la nube, puede guardar y archivar los registros de la puerta de enlace.  

*  Mejoras de la interfaz de usuario del Administrador de configuración de puertas de enlace:

    *  Mayor visibilidad del estado de la puerta de enlace en la pestaña Inicio.

    *  Controles reorganizados y simplificados.

    *  Puede copiar datos desde un almacenamiento mediante la [herramienta de vista previa de código abierto](data-factory-copy-data-wizard-tutorial.md). Para obtener más información general sobre esta característica consulte [Copias almacenadas provisionalmente](data-factory-copy-activity-performance.md#staged-copy) . 
*  Puede utilizar Data Management Gateway para transferir los datos directamente de la base de datos de SQL Server local a Aprendizaje automático de Azure.

*  Mejoras en el rendimiento

    * Se ha mejorado el rendimiento al visualizar los esquemas y la vista previa en SQL Server mediante la herramienta de vista previa de copia sin código.

## <a name="11259531"></a>1.12.5953.1

*  Corrección de errores

## <a name="11159181"></a>1.11.5918.1

*  El tamaño máximo del registro de eventos de puerta de enlace ha aumentado de 1 MB a 40 MB.

*  En caso de que se requiera un reinicio durante la actualización automática de la puerta de enlace, se muestra un cuadro de diálogo de advertencia. Puede elegir reiniciar en ese mismo momento o más adelante. 

*  En caso de error en la actualización automática, el instalador de puerta de enlace volverá a intentar esta operación&3; veces al máximo.

*  Mejoras en el rendimiento

    * Rendimiento mejorado para la carga de tablas de gran tamaño desde el servidor local en un escenario de copia sin código.

*  Corrección de errores

## <a name="11058921"></a>1.10.5892.1

*  Mejoras en el rendimiento

*  Corrección de errores

## <a name="1958652"></a>1.9.5865.2

*  Funcionalidad de actualización automática sin intervención del usuario
*  Icono de bandeja de nuevo con indicadores de estado de la puerta de enlace
*  Capacidad para "Actualizar ahora" desde el cliente
*  Capacidad para establecer la hora de programación de las actualizaciones
*  Script de PowerShell para activar y desactivar la actualización automática
*  Compatibilidad con formato JSON  
*  Mejoras en el rendimiento
*  Corrección de errores

## <a name="1858221"></a>1.8.5822.1

*  Mejora de la solución de problemas
*  Mejoras en el rendimiento
*  Corrección de errores

### <a name="1757951"></a>1.7.5795.1

*  Mejoras en el rendimiento
*  Corrección de errores

### <a name="1757641"></a>1.7.5764.1

*  Mejoras en el rendimiento
*  Corrección de errores

### <a name="1657351"></a>1.6.5735.1

*  Compatibilidad origen y receptor HDFS locales
*  Mejoras en el rendimiento
*  Corrección de errores

### <a name="1656961"></a>1.6.5696.1

*  Mejoras en el rendimiento
*  Corrección de errores

### <a name="1656761"></a>1.6.5676.1

*  Compatibilidad con herramientas de diagnóstico de Administrador de configuración
*  Columnas de la tabla de soporte técnico para orígenes de datos tabulares de la Factoría de datos de Azure
*  Compatibilidad con SQL DW para Factoría de datos de Azure
*  Compatibilidad con Reclusive en BlobSource y FileSource para Factoría de datos de Azure
*  Compatibilidad con CopyBehavior - MergeFiles, PreserveHierarchy y FlattenHierarchy en BlobSink y FileSink con copia binaria para Data Factory de Azure
*  Compatibilidad con los informes de progreso de la actividad de copia para Factoría de datos de Azure
*  Compatibilidad con la validación de conectividad de origen de datos para Factoría de datos de Azure
*  Corrección de errores

### <a name="1656721"></a>1.6.5672.1

*  Compatibilidad con nombre de tabla de origen de datos ODBC para Factoría de datos de Azure
*  Mejoras en el rendimiento
*  Corrección de errores

### <a name="1656581"></a>1.6.5658.1

*  Compatibilidad con receptor de archivos para Factoría de datos de Azure
*  Compatibilidad con la conservación jerarquía en copia binaria para Factoría de datos de Azure
*  Compatibilidad con idempotencia de la actividad de copia para Factoría de datos de Azure
*  Corrección de errores

### <a name="1656401"></a>1.6.5640.1

*  Compatibilidad con tres orígenes de datos más para Factoría de datos de Azure (ODBC, OData y HDFS)
*  Compatibilidad con carácter de comillas en el analizador de archivos .csv para Factoría de datos de Azure
*  Compatibilidad con compresión (BZip2)
*  Corrección de errores

### <a name="1556121"></a>1.5.5612.1

*  Compatibilidad con cinco bases de datos relacionales para Data Factory de Azure (MySQL, PostgreSQL, DB2, Teradata y Sybase)
*  Compatibilidad de compresión (Gzip y Deflate)
*  Mejoras en el rendimiento
*  Corrección de errores

### <a name="1455491"></a>1.4.5549.1

*  Incorporación de compatibilidad de origen de datos Oracle para Factoría de datos de Azure
*  Mejoras en el rendimiento
*  Corrección de errores

### <a name="1454921"></a>1.4.5492.1

*  Binario unificado que admite los servicios Factoría de datos de Microsoft Azure y Office 365 Power BI
*  Restricción de la interfaz de usuario de configuración y del proceso de registro
*  Factoría de datos de Azure: compatibilidad de entrada y salida de Azure con origen de datos SQL Server

### <a name="1253031"></a>1.2.5303.1

*  Corrección del problema del tiempo de espera para admitir más conexiones de orígenes de datos que requieren mucho tiempo. 

### <a name="1155268"></a>1.1.5526.8

*  Requiere .NET Framework 4.5.1 como requisito previo durante la instalación.

### <a name="1051442"></a>1.0.5144.2

*  No hay cambios que afecten a los escenarios de Factoría de datos de Azure.

