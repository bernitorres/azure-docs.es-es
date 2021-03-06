---
title: "Información general sobre las alertas en Microsoft Azure y Azure Monitor | Microsoft Docs"
description: "Las alertas permiten supervisar los registros, los eventos o las métricas de recursos de Azure y recibir una notificación cuando se cumple una condición especificada."
author: rboucher
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: robb
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: c9065c3346c924ea21060c9e7e5b297a3cb26941
ms.lasthandoff: 03/09/2017


---
# <a name="what-are-alerts-in-microsoft-azure"></a>¿Qué son las alertas en Microsoft Azure?
En este artículo se explica qué son las alertas, sus beneficios y cómo empezar a utilizarlas. Se aplica específicamente a Azure Monitor, pero brinda indicios sobre otros servicios.  

Las alertas son un método de supervisión de los registros, los eventos o las métricas de recursos de Azure, con la intención de recibir una notificación cuando una condición especificada se cumple.  

## <a name="alerts-in-different-azure-services"></a>Alertas en los distintos servicios de Azure
Las alertas están disponibles en los diferentes servicios, incluidos:

* **Application Insights**: admite las alertas de métricas y pruebas web. Vea [Definición de alertas en Application Insights](../application-insights/app-insights-alerts.md) y [Supervisión de la disponibilidad y la capacidad de respuesta de cualquier sitio web](../application-insights/app-insights-monitor-web-app-availability.md).
* **Log Analytics (Operations Management Suite)**: habilita el enrutamiento de los registros de actividad y diagnóstico para Log Analytics. Operations Management Suite permite métrica, registro y otros tipos de alerta. Para más información, vea [Alertas de Log Analytics](../log-analytics/log-analytics-alerts.md).  
* **Azure Monitor**: admite alertas basadas en valores de métricas y en eventos de registro de actividades. Puede usar la [API de REST de Azure Monitor](https://msdn.microsoft.com/library/dn931943.aspx) para administrar las alertas.  Para más información, vea la información sobre cómo [utilizar Azure Portal, PowerShell o la interfaz de la línea de comandos para crear alertas](insights-alerts-portal.md).

## <a name="visual-summary"></a>Resumen visual
En el diagrama siguiente se resumen las alertas y lo que puede hacer con ellas específicamente en "Azure Monitor". Otras acciones pueden estar disponibles para los servicios enumerados anteriormente. Por ejemplo, las alertas en los registros de diagnóstico solo están disponibles actualmente en Log Analytics.

![Alertas explicadas](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="what-can-trigger-alerts-in-azure-monitor"></a>Qué puede desencadenar alertas en Azure Monitor

Puede recibir alertas basadas en:

* **Valores de métrica**: la alerta se desencadena cuando el valor de una métrica específica cruza un umbral asignado en cualquier dirección. Es decir, se desencadena tanto la primera vez que se cumple la condición como después, cuando dicha condición ya deja de cumplirse. Para ver una lista creciente de las métricas disponibles compatibles con Azure Monitor, consulte la [lista de métricas que se admiten en Azure Monitor](monitoring-supported-metrics.md).
* **Eventos de registro de actividad**: esta alerta puede desencadenarse en cada evento o solo cuando se produce una serie de eventos específica.

## <a name="what-can-alerts-do"></a>¿Qué pueden hacer las alertas?
Puede configurar una alerta para hacer las siguientes acciones:

* Enviar notificaciones de correo electrónico al administrador de servicios o a los coadministradores, o a las direcciones de correo electrónico adicionales que especifique.
* Llamar a un webhook, que permite iniciar acciones de automatización adicionales. Algunos ejemplos incluyen llamar a:
    - Runbook de Automatización de Azure
    - Función de Azure
    - Aplicación lógica de Azure
    - un servicio de terceros


## <a name="next-steps"></a>Pasos siguientes
Obtener información sobre las reglas de alertas y su configuración mediante:

* [Portal de Azure](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Interfaz de la línea de comandos (CLI)](insights-alerts-command-line-interface.md)
* [API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx)

