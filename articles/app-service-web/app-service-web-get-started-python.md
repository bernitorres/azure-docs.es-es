---
title: "Creación de su primera aplicación web de Python en Azure en cinco minutos | Microsoft Docs"
description: "Aprenda lo fácil que es ejecutar aplicaciones web en App Service mediante la implementación de una aplicación Python de ejemplo."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 53d936366de30d3d149b170c4ef52574c65dcbc8
ms.lasthandoff: 03/18/2017


---
# <a name="create-your-first-python-web-app-in-azure-in-five-minutes"></a>Creación de su primera aplicación web de Python en Azure en cinco minutos
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Este tutorial de inicio rápido le ayuda a implementar su primera aplicación web de Python en [Azure App Service](../app-service/app-service-value-prop-what-is.md) en tan solo unos minutos.

Antes de empezar, asegúrese de que se ha instalado la CLI de Azure. Para obtener más información, consulte la [guía de instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Inicie sesión en Azure.
Inicie sesión en Azure, para lo que debe ejecutar `az login` y seguir las instrucciones de la pantalla.
   
```azurecli
az login
```
   
## <a name="create-a-resource-group"></a>Crear un grupo de recursos   
Cree un [grupo de recursos](../azure-resource-manager/resource-group-overview.md). Aquí es donde se colocan todos los recursos de Azure que quiere administrar juntos, como la aplicación web y su back-end de SQL Database.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

Para ver los posibles valores que se pueden usar para `---location`, utilice el comando `az appservice list-locations` de la CLI de Azure.

## <a name="create-an-app-service-plan"></a>Creación de un plan del Servicio de aplicaciones
Cree un [plan de App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) de nivel "Gratis". 

```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

## <a name="create-a-web-app"></a>Creación de una aplicación web
Cree una nueva aplicación web con un nombre único en `<app_name>`.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
```

## <a name="deploy-sample-application"></a>Implementación de una aplicación de ejemplo
Implemente una aplicación de Python de ejemplo de GitHub.

```azurecli
az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
--repo-url "https://github.com/Azure-Samples/app-service-web-python-get-started.git" --branch master --manual-integration 
```

## <a name="browse-to-web-app"></a>Navegación a la aplicación web
Para ver la aplicación en ejecución en Azure, ejecute este comando:

```azurecli
az appservice web browse --name <app_name> --resource-group myResourceGroup
```

Enhorabuena, la primera aplicación web de Python se está ejecutando en directo en Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore los [scripts de la CLI de aplicaciones web](app-service-cli-samples.md) previamente creados.

