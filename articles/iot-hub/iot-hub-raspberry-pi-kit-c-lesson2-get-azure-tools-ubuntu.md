---
title: "Conexión de Raspberry Pi (C) a Azure IoT: Lección 2: Herramientas de Azure (Ubuntu) | Microsoft Docs"
description: "Instale Python y la interfaz de la línea de comandos de Azure (CLI de Azure) en Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: servicio en la nube iot, cli de azure
ms.assetid: a03512f2-fabe-40c5-8505-b93eef8e5bec
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 4c31bb14f2b32d71f14a89576922b9701b226e66
ms.lasthandoff: 01/24/2017


---
# <a name="get-azure-tools-ubuntu-1604"></a>Obtención de las herramientas de Azure (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 y versiones posteriores](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Lo que hará
Instalación de la interfaz de la línea de comandos de Azure (CLI de Azure) Si tiene problemas, busque soluciones en la [página de solución de problemas](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Lo qué aprenderá
En este artículo, aprenderá lo siguiente:
* Cómo instalar la CLI de Azure.
* Cómo agregar un subgrupo de IoT de la CLI de Azure.

## <a name="what-you-need"></a>Lo que necesita
* Un equipo con Ubuntu con conexión a Internet.
* Una suscripción de Azure activa. Si no tiene ninguna, puede crear una [cuenta de evaluación gratuita](http://azure.microsoft.com/pricing/free-trial/) en tan solo unos minutos.

## <a name="install-the-azure-cli"></a>Instalación de la CLI de Azure
La CLI de Azure proporciona una experiencia de línea de comandos multiplataforma en Azure, lo que le permite trabajar directamente desde la línea de comandos para aprovisionar y administrar los recursos.

Para instalar la CLI de Azure más recientr, siga estos pasos:

1. Ejecute los siguientes comandos en una ventana de terminal. Este proceso puede tardar cinco minutos en instalar la CLI de Azure.

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. Compruebe la instalación ejecutando el comando siguiente:

   ```bash
   az iot -h
   ```

Si la instalación se realiza correctamente, verá el siguiente resultado.

![Resultado que indica una instalación correcta](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a>Resumen
Ha instalado la CLI de Azure. La siguiente tarea consiste en crear la identidad de Azure IoT Hub y de dispositivos mediante la CLI de Azure.

## <a name="next-steps"></a>Pasos siguientes
[Creación de un centro de IoT y registro de Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)


