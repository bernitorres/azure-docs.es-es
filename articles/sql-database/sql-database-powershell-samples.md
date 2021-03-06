---
title: Ejemplos de Azure PowerShell para SQL Database | Microsoft Docs
description: "Ejemplos de Azure PowerShell: scripts para ayudar a crear y administrar servidores de Azure SQL Database, grupos elásticos, bases de datos y firewalls."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 03/07/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 8057747aea2725abf3b37481858c7902ff8fe4da
ms.lasthandoff: 03/30/2017

---

# <a name="azure-powershell-samples-for-azure-sql-database"></a>Ejemplos de Azure PowerShell para Azure SQL Database

En la tabla siguiente se incluyen vínculos a scripts de Azure PowerShell para Azure SQL Database.

| |  |
|---|---|
|**Creación de una base de datos única y un grupo elástico**||
| [Creación de una base de datos única y configuración de una regla de firewall](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2ftoc.json) | Crea una instancia única de Azure SQL Database y configura una regla de firewall en el nivel de servidor. |
| [Creación de grupos elásticos y traslado de bases de datos agrupadas](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2ftoc.json) | Crea grupos elásticos, traslada bases de datos agrupadas y cambia los niveles de rendimiento.|
|**Configuración de la replicación geográfica y de la conmutación por error**||
| [Configuración y conmutación por error de una base de datos única mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2ftoc.json)| Configura la replicación geográfica activa para una instancia única de Azure SQL Database y la conmuta por error a la réplica secundaria. |
| [Configuración y conmutación por error de una base de datos agrupada mediante la replicación geográfica activa](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2ftoc.json)| Configura la replicación geográfica activa para una instancia de Azure SQL Database de un grupo elástico y la conmuta por error a la réplica secundaria. |
|**Escalado de una base de datos única y un grupo elástico**||
| [Escalado de una base de datos única](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2ftoc.json) | Supervisa las métricas de rendimiento de una instancia de Azure SQL Database, la escala a un nivel de rendimiento superior y crea una regla de alerta en una de las métricas de rendimiento. |
| [Escalado de un grupo elástico](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2ftoc.json) | Supervisa las métricas de rendimiento de un grupo elástico, lo escala a un nivel de rendimiento superior y crea una regla de alerta en una de las métricas de rendimiento.  |
| **Detección de amenazas y auditoría** |
| [Configuración de detección de amenazas y auditoría](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2ftoc.json)| Configura las directivas de auditoría y detección de amenazas para una instancia de Azure SQL Database. |
| **Restauración, copia e importación de una base de datos**||
| [Restauración de una base de datos](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2ftoc.json)| Restaura una instancia de Azure SQL Database desde una copia de seguridad con redundancia geográfica y restaura una instancia de Azure SQL Database eliminada a la copia de seguridad más reciente. |
| [Copia de una base de datos en un nuevo servidor](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2ftoc.json)| Crea una copia de una instancia de Azure SQL Database existente en un nuevo servidor SQL de Azure. |
| [Importación de una base de datos desde un archivo bacpac](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2ftoc.json)| Importa una base de datos a un servidor SQL Server de Azure desde un archivo bacpac. |
|||

