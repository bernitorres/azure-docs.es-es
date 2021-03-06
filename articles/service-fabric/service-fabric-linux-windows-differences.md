---
title: Diferencias entre Azure Service Fabric en Windows y en Linux | Documentos de Microsoft
description: "Diferencias entre Azure Service Fabric, versión preliminar, en Linux y Azure Service Fabric en Windows."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2017
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 5faf7dc0b544f6fe2f83565cc368e218c6df35af
ms.lasthandoff: 03/29/2017


---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>Diferencias entre Service Fabric en Linux (versión preliminar) y Windows (disponible con carácter general)

Como Service Fabric en Linux es una versión preliminar, hay algunas características que se admiten en Windows pero no en Linux. Por último, los conjuntos de características estarán a la par cuando Service Fabric en Linux esté disponible con carácter general.

* No se admiten Reliable Collections (ni servicios con estado confiables) en Linux.
* ReverseProxy no está disponible en Linux.
* El instalador independiente no está disponible en Linux.
* No se realiza la validación del esquema XML para archivos de manifiesto en Linux. 
* No se admite el redireccionamiento de la consola en Linux. 
* El servicio de análisis de errores (FAS) no está disponible en Linux.
* Azure Active Directory no es compatible con Linux.
* Algunos comandos de la CLI equivalentes de los comandos de Powershell no están disponibles.
* Solo se puede ejecutar un subconjunto de comandos de Powershell en un clúster de Linux (tal como se explica detalladamente en la sección siguiente).

>[!NOTE]
>El redireccionamiento de la consola no se admite en los clústeres de producción, incluso en Windows.

Las herramientas de desarrollo son diferentes con VisualStudio, Powershell, VSTS y ETW, cuando estos se usan en Windows, o con Yeoman, Eclipse, Jenkins y LTTng, cuando estos se utilizan en Linux.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>Cmdlets de PowerShell que no funcionan en un clúster de Service Fabric para Linux

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Cmd
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Pasos siguientes
* [Prepare your development environment on Linux](service-fabric-get-started-linux.md)
* [Prepare your development environment on OSX](service-fabric-get-started-mac.md)
* [Creación e implementación de la primera aplicación de Java para Service Fabric en Linux con Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Creación e implementación de la primera aplicación de Java para Service Fabric con el complemento de Eclipse para Service Fabric](service-fabric-get-started-eclipse.md)
* [Creación de su primera aplicación de CSharp en Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Uso de la CLI de Azure para administrar las aplicaciones de Service Fabric](service-fabric-azure-cli.md)

