---
title: "Supervisión del rendimiento de aplicaciones web de Azure | Microsoft Docs"
description: "Supervisión del rendimiento de aplicaciones web de Azure Carga y tiempo de respuesta de gráfico, información de dependencia y establecer alertas en el rendimiento."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: d7a8d669e75991f6707641e4ce53c9b63d04853c
ms.lasthandoff: 03/16/2017


---
# <a name="monitor-azure-web-app-performance"></a>Supervisión del rendimiento de aplicaciones web de Azure
En [Azure Portal](https://portal.azure.com), puede configurar la supervisión de rendimiento de aplicaciones para sus [aplicaciones web de Azure](../app-service-web/app-service-web-overview.md). [Azure Application Insights](app-insights-overview.md) instrumenta la aplicación para que envíe datos de telemetría sobre sus actividades al servicio Application Insights, donde se almacenan y analizan. En esta plataforma, se pueden usar los gráficos de métricas y las herramientas de búsqueda para ayudar a diagnosticar problemas, mejorar el rendimiento y evaluar el uso.

## <a name="run-time-or-build-time"></a>Tiempo de ejecución o de compilación
Puede configurar la supervisión mediante la instrumentación de la aplicación de dos maneras:

* **Tiempo de ejecución**: puede seleccionar una extensión de supervisión de rendimiento cuando la aplicación web ya esté activa. No es necesario volver a compilar o instalar la aplicación. Tendrá a disposición un conjunto estándar de paquetes que supervisan tiempos de respuesta, tasas de éxito, excepciones, dependencias, etc. 
* **Tiempo de compilación** : puede instalar un paquete en la aplicación que esté en desarrollo. Esta opción es más versátil. Además de los mismos paquetes estándares, puede escribir código para personalizar los datos de telemetría o enviar los suyos propios. Puede registrar las actividades específicas o grabar eventos según la semántica del dominio de la aplicación. 

## <a name="run-time-instrumentation-with-application-insights"></a>Instrumentación del tiempo de ejecución con Application Insights
Si ya está ejecutando una aplicación web en Azure, ya goza de cierta supervisión: tasas de solicitudes y errores. Agregue Application Insights para obtener una mayor supervisión, como tiempos de respuesta, supervisión de las llamadas a las dependencias, detección inteligente y el eficaz lenguaje de consulta de Analytics. 

1. **Seleccione Application Insights** en el panel de control de Azure para la aplicación web.
   
    ![En Supervisión, elija Application Insights.](./media/app-insights-azure-web-apps/05-extend.png)
   
   * Elija crear un nuevo recurso a menos que ya haya configurado un recurso de Application Insights para esta aplicación por otra ruta.
2. **Instrumente la aplicación web** después de haber instalado Application Insights. 
   
    ![Instrumentación de la aplicación web](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)
3. **Supervise la aplicación**.  [Explore los datos](#explore-the-data).

Posteriormente, puede compilar y volver a implementar la aplicación con Application Insights si lo desea.

*¿Cómo puedo quitar Application Insights o cambiar para enviar a otro recurso?*

* En Azure, abra la hoja de control de la aplicación web y en Herramientas de desarrollo, abra **Extensiones**. Elimine la extensión de Application Insights. En Supervisión, elija Application Insights y cree o seleccione el recurso que desee.

## <a name="build-the-app-with-application-insights"></a>Compilación de la aplicación con Application Insights
Application Insights puede proporcionar una telemetría más detallada instalando un SDK en la aplicación. En concreto, puede recopilar registros de seguimiento, [escribir telemetría personalizada](app-insights-api-custom-events-metrics.md), y obtener informes de excepción más detallados.

1. **En Visual Studio** (2013 Update 2 o posterior), configure Application Insights para el proyecto.
   
    ![Haga clic con el botón derecho en el proyecto web y elija Agregar o Configurar Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    Cuando se la pide que inicie sesión, use las credenciales de su cuenta de Azure.
   
    La operación tiene dos efectos:
   
   1. Crea un recurso de Application Insights en Azure, donde se almacenan, analizan y muestran los datos de telemetría.
   2. Agrega el paquete NuGet de Application Insights al código (si todavía no está) y lo configura para enviar los datos de telemetría al recurso de Azure.
2. **Pruebe la telemetría** ejecutando la aplicación en la máquina de desarrollo (F5).
3. **Publique la aplicación** en Azure de la forma habitual. 

*¿Cómo cambio para enviar a un recurso de Application Insights diferente?*

* En Visual Studio, haga clic con el botón derecho en el proyecto, elija **Configurar Application Insights** y seleccione el recurso que desea. Tiene la opción de crear un nuevo recurso. Vuelva a compilar e implementar.

## <a name="explore-the-data"></a>Exploración de los datos
1. En la hoja Application Insights del panel de control de la aplicación web, verá Live Metrics, que muestra los errores y solicitudes al segundo o dos segundos de producirse. Esta información es muy útil cuando se vuelve a publicar la aplicación ya que permite ver inmediatamente cualquier problema.
2. Haga clic en las distintas opciones hasta llegar al recurso de Application Insights.

    ![Hacer clic en las distintas opciones](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    También puede ir allí directamente desde la exploración de recursos de Azure.

1. Haga clic en cualquier gráfico para obtener información más detallada:
   
    ![En la hoja de información general de Application Insights, haga clic en un gráfico.](./media/app-insights-azure-web-apps/07-dependency.png)
   
    También puede [personalizar las hojas de métricas](app-insights-metrics-explorer.md).
2. Siga haciendo clic para ver los eventos individuales y sus propiedades:
   
    ![Haga clic en un tipo de evento para abrir una búsqueda filtrada en ese tipo.](./media/app-insights-azure-web-apps/08-requests.png)
   
    Observe el vínculo "..." para abrir todas las propiedades.
   
    También puede [personalizar las búsquedas](app-insights-diagnostic-search.md).

Para realizar búsquedas más eficaces sobre los datos de telemetría, use el [lenguaje de consulta Analytics](app-insights-analytics-tour.md).

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Pasos siguientes
* [Diagnósticos de Microsoft Azure](app-insights-azure-diagnostics.md) para enviar este tipo de información a Application Insights.
* [Supervise las métricas del estado del servicio](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) para asegurarse de que el servicio está disponible y responde adecuadamente.
* [Reciba notificaciones de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) cada vez que se produzcan eventos de operaciones o las métricas traspasen un umbral.
* Use [aplicaciones y páginas web de Application Insights para JavaScript](app-insights-web-track-usage.md) para obtener la telemetría del cliente de los exploradores que visitan una página web.
* [Configure pruebas web de disponibilidad](app-insights-monitor-web-app-availability.md) para recibir una alerta si el sitio deja de estar activo.


