---
title: "Creación de una identidad profesional o educativa en AAD para Linux | Microsoft Docs"
description: "Aprenda a crear una identidad profesional o educativa en Azure Active Directory para su uso con máquinas virtuales Linux."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: b0f86d77-c669-4aa1-a095-c2aa4d9105fe
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 1d35114800ba024d0c201840722df2c6a74d1d61
ms.openlocfilehash: 0eaa83fc19316708685eed0ca994112bc09153e5


---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-linux-vms"></a>Creación de una identidad profesional o educativa en Azure Active Directory para usarla con máquinas virtuales Linux
Si creó una cuenta de Azure personal o tiene una suscripción a MSDN personal y creó la cuenta de Azure para beneficiarse de los créditos de Azure de MSDN, usó una identidad de *cuenta de Microsoft* para crearla. Muchas características excelentes de Azure (las [plantillas de grupo de recursos](../azure-resource-manager/resource-group-overview.md) son un ejemplo) requieren una cuenta profesional o educativa (una identidad administrada por Azure Active Directory) para que funcione. Puede seguir las instrucciones que se indican a continuación para crear una cuenta profesional o educativa porque, afortunadamente, una de las ventajas de su cuenta de Azure personal es que se incluye con un dominio de Azure Active Directory predeterminado que se puede usar para crear una nueva cuenta profesional o educativa que puede usar con las características de Azure que lo requieran.

Sin embargo, los cambios recientes hacen posible administrar la suscripción con cualquier tipo de cuenta de Azure mediante el método de inicio de sesión interactivo `azure login` que se describe [aquí](../xplat-cli-connect.md). Puede usar ese mecanismo o seguir las instrucciones que se indican a continuación. También puede [crear una identidad profesional o educativa en Azure Active Directory para usarla con máquinas virtuales Windows](virtual-machines-windows-create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]




<!--HONumber=Jan17_HO4-->


