---
title: Uso de Pig en Hadoop con .NET en HDInsight | Microsoft Docs
description: Aprenda a usar el SDK de .NET de Hadoop para enviar trabajos de Pig a Hadoop en HDInsight.
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/03/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 7c28fda22a08ea40b15cf69351e1b0aff6bd0a95
ms.openlocfilehash: db01dd7ff7590c6cb43b5f8bc39674f585160856
ms.lasthandoff: 03/07/2017


---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Ejecución de trabajos de Pig con el SDK de .NET para Hadoop en HDInsight
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Este documento proporciona un ejemplo de uso del SDK de .NET para Hadoop para enviar trabajos de Pig a un Hadoop en clúster de HDInsight.

El SDK .NET de HDInsight proporciona bibliotecas de cliente .NET que facilitan el trabajo con los clústeres de HDInsight en .NET. Pig le permite crear operaciones de MapReduce al modelar una serie de transformaciones de datos. En este documento, aprenderá a utilizar una aplicación básica de C# para enviar un trabajo de Pig para un clúster de HDInsight.

## <a name="prerequisites"></a>Requisitos previos

Necesitará lo siguiente para completar los pasos de este artículo.

* Un clúster de HDInsight de Azure (Hadoop en HDInsight) (Windows o Linux)

  > [!IMPORTANT]
  > Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Para más información, consulte [El contrato de nivel de servicio para las versiones de clúster de HDInsight](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Visual Studio 2012, 2013, 2015 o 2017.

## <a name="create-the-application"></a>Creación de la aplicación

El SDK .NET de HDInsight ofrece bibliotecas de cliente .NET que facilitan el trabajo con los clústeres de HDInsight de .NET. 

1. En Visual Studio, en el menú **Archivo**, seleccione **Nuevo** y, luego, **Proyecto**.

2. Para el nuevo proyecto, escriba o seleccione los valores siguientes:
   
   | Propiedad | Valor |
   | ------ | ------ |
   | Categoría | Plantillas/Visual C#/Windows |
   | Plantilla | Aplicación de consola |
   | Nombre | SubmitPigJob |

3. Haga clic en **Aceptar** para crear el proyecto.

4. En el menú **Herramientas**, seleccione **Administrador de paquetes de biblioteca** o **Administrador de paquetes de Nuget** y, a continuación, seleccione **Consola del administrador de paquetes**.

5. Use el siguiente comando para instalar los paquetes de SDK de .NET:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. En el Explorador de soluciones, haga doble clic en **Program.cs** para abrirlo. Reemplace el código existente por lo siguiente.
   
    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitHDInsightJobDotNet
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;

            private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
            private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
            private const string ExistingClusterUsername = "<Cluster Username>";
            private const string ExistingClusterPassword = "<Cluster User Password>";

            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```
    
7. Presione **F5** para iniciar la aplicación.

8. Presione **ENTRAR** para salir de la aplicación.

## <a name="summary"></a>Resumen

Como puede observar, el SDK de .NET para Hadoop le permite crear aplicaciones .NET que envíen trabajos de Pig a un clúster de HDInsight y supervisar el estado del trabajo.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre Pig en HDInsight, vea [Usar Pig con Hadoop en HDInsight](hdinsight-use-pig.md).

Para más información sobre el uso de Hadoop con HDInsight, vea los documentos siguientes:

* [Uso de Hive con Hadoop en HDInsight](hdinsight-use-hive.md)
* [Uso de MapReduce con Hadoop en HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/

