---
title: "Ejemplo de script de Azure PowerShell: creación de una aplicación web e implementación de código en un entorno de ensayo | Microsoft Docs"
description: "Ejemplo de script de Azure PowerShell: creación de una aplicación web e implementación de código en un entorno de ensayo"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 27cf0680-c3a9-4a58-9f71-6dec09f6b874
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 9fbedec9cee316b22837373e7895fc6012eb39a6
ms.lasthandoff: 03/21/2017

---

# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a>Creación de una aplicación web e implementación de código en un entorno de ensayo

Este script de ejemplo crea una aplicación web en App Service con una ranura de implementación adicional que se denomina "ensayo" y, a continuación, implementa una aplicación de ejemplo en la ranura "ensayo".

Si es necesario, instale Azure PowerShell con la instrucción que se encuentra en la [Guía de Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) y, luego, ejecute `Login-AzureRmAccount` para crear una conexión con Azure.

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Creación de una aplicación web e implementación de código en un entorno de ensayo")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos, la aplicación web y todos los recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Resources/v3.5.0/new-azurermresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [New-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermappserviceplan) | Crea un plan de App Service, |
| [New-AzureRmWebApp](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermwebapp) | Crea una aplicación web. |
| [Set-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/set-azurermappserviceplan) | Modifica un plan de App Service para cambiar su plan de tarifa. |
| [New-AzureRmWebAppSlot](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/new-azurermwebappslot) | Crea un espacio de implementación para una aplicación web. |
| [Set-AzureRmResource](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/set-azurermresource) | Modifica un recurso en un grupo de recursos. |
| [Swap-AzureRmWebAppSlot](https://docs.microsoft.com/powershell/resourcemanager/azurerm.websites/v2.5.0/swap-azurermwebappslot) | Cambia el espacio de implementación de una aplicación web a producción. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/).

Puede encontrar ejemplos de Azure PowerShell para Azure App Service Web Apps en los [ejemplos de PowerShell](../app-service-powershell-samples.md).

