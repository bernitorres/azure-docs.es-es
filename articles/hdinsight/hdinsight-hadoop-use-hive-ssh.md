---
title: Uso del shell de Hive en HDInsight (Hadoop) | Microsoft Docs
description: "Obtenga información acerca de cómo usar el shell de Hive con un clúster de HDInsight basado en Linux. Aprenderá a conectarse al clúster de HDInsight mediante SSh y usar el shell de Hive para ejecutar consultas de forma interactiva."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0e919171-03e6-4f5a-ab4e-3eec3e34c347
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 237b78f723fe6dbb800f9e0be2314c7c8a5c668e
ms.lasthandoff: 03/25/2017


---
# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Uso de Hive con Hadoop en HDInsight con SSH
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

En este artículo, aprenderá a usar Secure Shell (SSH) para conectarse a un clúster de Hadoop en HDInsight de Azure y, a continuación, envíe de manera interactiva consultas de Hive usando la interfaz de línea de comandos (CLI) de Hive.

> [!IMPORTANT]
> Si bien el comando de Hive está disponible en clústeres de HDInsight, considere la posibilidad de usar Beeline. Beeline es un cliente más reciente para trabajar con Hive y se incluye con el clúster de HDInsight. Para obtener más información sobre su uso, consulte [Uso de Hive con Hadoop en HDInsight con Beeline](hdinsight-hadoop-use-hive-beeline.md).


## <a id="prereq"></a>Requisitos previos
Para completar los pasos de este artículo, necesitará lo siguiente:

* Un clúster de Hadoop basado en Linux en HDInsight.

  > [!IMPORTANT]
  > Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Para más información, consulte [El contrato de nivel de servicio para las versiones de clúster de HDInsight](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Un cliente SSH. Linux, Unix y Mac OS deben incluir un cliente SSH. Los usuarios de Windows deben descargar un cliente similar [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a id="ssh"></a>Conexión con SSH
Conéctese con el nombre de dominio completo (FQDN) de su clúster de HDInsight mediante el comando SSH. El FQDN será el nombre que ha asignado al clúster, es decir, **.azurehdinsight.net**. Por ejemplo, lo siguiente debería conectarse a un clúster denominado **myhdinsight**.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Si proporcionó una clave de certificado para la autenticación de SSH** , al crear el clúster de HDInsight, es posible que tenga que especificar la ubicación de la clave privada en el sistema cliente:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Si proporcionó una contraseña para la autenticación de SSH** , al crear el clúster de HDInsight, tendrá que proporcionar la contraseña cuando se le solicite.

Para obtener más información sobre cómo utilizar SSH con HDInsight, consulte [Uso de SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hive"></a>Uso del comando de Hive
1. Una vez conectado, inicie la CLI de Hive con el siguiente comando:
   
        hive
2. Con la CLI, introduzca las siguientes instrucciones para crear una nueva tabla denominada **log4jLogs** con los datos de ejemplo:
   
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   
    Estas instrucciones realizan las acciones siguientes:
   
   * **DROP TABLE** : elimina la tabla y el archivo de datos si la tabla ya existe.
   * **CREATE EXTERNAL TABLE** : crea una tabla "externa" nueva en Hive. Las tablas externas solo almacenan la definición de Tabla en Hive. Los datos permanecen en la ubicación original.
   * **ROW FORMAT** : indica cómo se da formato a los datos de Hive. En este caso, los campos de cada registro se separan mediante un espacio.
   * **STORED AS TEXTFILE LOCATION** : indica a Hive dónde se almacenan los datos (el directorio example/data) y que se almacenan como texto.
   * **SELECT**: selecciona un número de todas las filas donde la columna **t4** contiene el valor **[ERROR]**. Esto debe devolver un valor de **3** ya que hay tres filas que contienen este valor.
   * **INPUT__FILE__NAME LIKE '%.log'**: indica a Hive que solo deberíamos devolver datos de archivos que terminan en .log. Esto restringe la búsqueda al archivo sample.log que contiene los datos y le impide que devuelva datos de otros archivos de datos de ejemplo que no coinciden con el esquema que hemos definido.
     
     > [!NOTE]
     > Las tablas externas se deben usar cuando espera que un origen externo, como por ejemplo un proceso de carga de datos automático, u otra operación MapReduce, actualice los datos subyacentes, pero siempre desea que las consultas de Hive usen los datos más recientes.
     > 
     > La eliminación de una tabla externa **no** elimina los datos, solamente la definición de tabla.
     > 
     > 
3. Use las siguientes instrucciones para crear una nueva tabla "interna" llamada **errorLogs**.
   
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   
    Estas instrucciones realizan las acciones siguientes:
   
   * **CREATE TABLE IF NOT EXISTS** : crea una tabla, si todavía no existe. Dado que la palabra clave **EXTERNAL** no se usa, se trata de una tabla interna, que se almacena en el almacenamiento de datos de Hive y es administrada por Hive.
   * **STORED AS ORC** : almacena los datos en el formato Optimized Row Columnar (ORC). Se trata de un formato altamente optimizado y eficiente para almacenar datos de Hive.
   * **INSERT OVERWRITE ... SELECT**: selecciona filas de la tabla **log4jLogs** que contienen **[ERROR]** y, a continuación, inserta los datos en la tabla **errorLogs**.
     
     Para comprobar que solamente las filas que contienen **[ERROR]** en la columna t4 se almacenaron en la tabla **errorLogs**, use la siguiente instrucción para devolver todas las filas de **errorLogs**:
     
       SELECT * de errorLogs;
     
     Se deben devolver tres filas de datos y todas ellas deben contener **[ERROR]** en la columna t4.
     
     > [!NOTE]
     > A diferencia de las tablas externas, la eliminación de una tabla interna también eliminará los datos subyacentes.
     > 
     > 

## <a id="summary"></a>Resumen
Como puede ver, el comando Hive proporciona una manera fácil de ejecutar consultas de Hive de manera interactiva en un clúster de HDInsight, supervisar el estado del trabajo y recuperar el resultado.

## <a id="nextsteps"></a>Pasos siguientes
Para obtener información general acerca de Hive en HDInsight:

* [Uso de Hive con Hadoop en HDInsight](hdinsight-use-hive.md)

Para obtener información sobre otras formas en que puede trabajar con Hadoop en HDInsight:

* [Uso de Pig con Hadoop en HDInsight](hdinsight-use-pig.md)
* [Uso de MapReduce con Hadoop en HDInsight](hdinsight-use-mapreduce.md)

Si usa Tez con Hive, consulte los siguientes documentos para la información de depuración:

* [Use the Tez UI on Windows-based HDInsight](hdinsight-debug-tez-ui.md)
* [Use the Ambari Tez view on Linux-based HDInsight](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png


