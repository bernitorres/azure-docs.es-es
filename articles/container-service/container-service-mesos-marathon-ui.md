---
title: "Administración de un clúster DC/OS de Azure con la interfaz de usuario de Marathon | Microsoft Docs"
description: "Implemente contenedores en un clúster del servicio Contenedor de Azure mediante la interfaz de usuario web de Marathon."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, contenedores, microservicios, Mesos, Azure
ms.assetid: d148ed1e-b582-4d51-944f-1ac7ae3c4fd6
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: f26b191d9d98768d766e4c974138c9d191340027
ms.lasthandoff: 03/15/2017


---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Administrar un clúster DC/OS de Azure Container Service a través de la interfaz de usuario web de Marathon
DC/OS proporciona un entorno para implementar y escalar cargas de trabajo agrupadas, al tiempo que reduce el hardware subyacente. Por encima de DC/OS hay un marco que administra la programación y ejecución de cargas de trabajo de proceso.

Aunque hay marcos de trabajo disponibles para muchas cargas de trabajo conocidas, en este documento se detalla cómo crear y escalar implementaciones de contenedores con Marathon. 


## <a name="prerequisites"></a>Requisitos previos
Antes de trabajar con estos ejemplos, necesita un clúster de DC/OS configurado en el servicio Contenedor de Azure. También debe tener conectividad remota con este clúster. Para más información sobre estos aspectos, consulte los siguientes artículos:

* [Implementación de un clúster del servicio Contenedor de Azure](container-service-deployment.md)
* [Conexión a un clúster del servicio Contenedor de Azure](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Exploración de la interfaz de usuario de DC/OS
Acceda a http://localhost/ con un túnel de Secure Shell (SSH) [establecido](container-service-connect.md). Con ello, se cargará la interfaz de usuario web de DC/OS y aparecerá información acerca del clúster, como los recursos usados, los agentes activos y los servicios en ejecución.

![Interfaz de usuario de DC/OS](./media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Exploración de la interfaz de usuario de Marathon
Para ver la interfaz de usuario de Marathon, vaya a http://localhost/marathon. En esta pantalla puede iniciar un nuevo contenedor u otra aplicación en el clúster de DC/OS del servicio Contenedor de Azure. También puede ver información acerca de cómo ejecutar contenedores y aplicaciones.  

![Interfaz de usuario de Marathon](./media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Implementación de un contenedor con formato Docker
Para implementar un nuevo contenedor mediante Marathon, haga clic en el botón **Create Application** (Crear aplicación) y escriba la siguiente información en las pestañas del formulario:

| Campo | Valor |
| --- | --- |
| ID |nginx |
| Memoria | 32 |
| Imagen |nginx |
| Red |Bridged |
| Puerto de host |80 |
| Protocol |TCP |

![Nueva interfaz de usuario de la aplicación: General](./media/dcos/dcos4.png)

![Nueva interfaz de usuario de la aplicación: Contenedor de Docker](./media/dcos/dcos5.png)

![Nueva interfaz de usuario de la aplicación: Detección de servicios y puertos](./media/dcos/dcos6.png)

Si desea asignar estáticamente el puerto del contenedor a un puerto en el agente, debe utilizar el modo JSON. Para ello, cambie el Asistente para nuevas aplicaciones al **modo JSON** mediante el botón de alternancia. A continuación, escriba el siguiente ajuste en la sección `portMappings` de la definición de la aplicación. En este ejemplo se enlaza el puerto 80 del contenedor al puerto 80 del agente DC/OS. Puede volver a cambiar el modo JSON del Asistente después de realizar este cambio.

```none
"hostPort": 80,
```

![Nueva interfaz de usuario de la aplicación: Ejemplo con el puerto 80](./media/dcos/dcos13.png)

Si desea habilitar las comprobaciones de estado, establezca una ruta de acceso en la pestaña **Health Checks** (Comprobaciones de estado).

![Interfaz de usuario para nueva aplicación: comprobaciones de estado](./media/dcos/dcos_healthcheck.png)

El clúster de DC/OS se implementa con un conjunto de agentes públicos y privados. Para que el clúster pueda acceder a aplicaciones de Internet, debe implementar las aplicaciones en un agente público. Para ello, seleccione la pestaña **Opcional** del Asistente para nuevas aplicaciones y especifique **slave_public** en **Accepted Resource Roles** (Roles de recursos aceptados).

Haga clic en **Create Application** (Crear aplicación).

![Nueva interfaz de usuario de la aplicación: Configuración del agente público](./media/dcos/dcos14.png)

De nuevo en la página principal de Marathon, puede ver el estado de implementación para el contenedor. Inicialmente verá un estado **Deploying** (Implementando). Después de que la implementación se haya realizado correctamente, el estado cambia a **Running** (Ejecutando).

![Página principal de la interfaz de usuario de Marathon: Estado de la implementación del contenedor](./media/dcos/dcos7.png)

Al volver a la interfaz de usuario web de DC/OS (http://localhost/), puede ver que hay una tarea en ejecución (en este caso, un contenedor con formato Docker) en el clúster de DC/OS.

![Interfaz de usuario web de DC/OS: Tarea que se ejecuta en el clúster](./media/dcos/dcos8.png)

Para ver el nodo del clúster en el que se está ejecutando la tarea haga clic en la pestaña **Nodes** (Nodos).

![Interfaz de usuario web de DC/OS: nodo de clúster de la tarea](./media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Escalado de los contenedores
La interfaz de usuario de Marathon se puede utilizar para escalar el recuento de instancias de un contenedor. Para ello, acceda a la página de **Marathon**, seleccione el contenedor que desea escalar y haga clic en **Scale Application** (Escalar aplicación). En el cuadro de diálogo **Scale Application** (Escalar aplicación), escriba el número de instancias del contenedor que desee y seleccione **Scale Application** (Escalar aplicación).

![Interfaz de usuario de Marathon: Cuadro de diálogo Scale Application (Escalar aplicación)](./media/dcos/dcos10.png)

Una vez que finaliza la operación de escalado, verá varias instancias de la misma tarea distribuidas entre los agentes de DC/OS.

![Panel de interfaz de usuario web de DC/OS: Tarea distribuida entre varios agentes](./media/dcos/dcos11.png)

![Interfaz de usuario web de DC/OS: Nodos](./media/dcos/dcos12.png)

## <a name="next-steps"></a>Pasos siguientes
* [Trabajo con la API de DC/OS y Marathon](container-service-mesos-marathon-rest.md)

* Profundice en Azure Container Service con Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 

