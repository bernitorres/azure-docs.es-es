---
title: "Azure Portal: Creación de una base de datos SQL | Microsoft Docs"
description: "Aprenda a crear un servidor lógico de SQL Database, una regla de firewall de nivel de servidor y bases de datos en Azure Portal. También aprenderá a consultar de una instancia de Azure SQL Database desde Azure Portal."
keywords: "tutorial de base de datos SQL, creación de una base de datos SQL"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: be5839e04fae457b889db11dffe56f31afe723a5
ms.lasthandoff: 03/28/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Creación de una instancia de Azure SQL Database en Azure Portal

Este tutorial de inicio rápido le guía por el proceso de creación de una instancia de SQL Database en Azure.  Azure SQL Database es una oferta de "base de datos como servicio" que permite ejecutar y escalar bases de datos de SQL Server altamente disponibles en la nube.  En esta guía de inicio rápido se muestra cómo comenzar mediante la creación de una instancia nueva de SQL Database a través de Azure Portal.

## <a name="log-in-to-the-azure-portal"></a>Iniciar sesión en el portal de Azure

Inicie sesión en el [Portal de Azure](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Creación de una Base de datos SQL

Se crea una base de datos SQL de Azure con un conjunto definido de [recursos de proceso y almacenamiento](sql-database-service-tiers.md). La base de datos se crea dentro de un [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) y en un [servidor lógico de Azure SQL Database](sql-database-features.md). 

Siga estos pasos para crear una instancia de SQL Database que contenga los datos de ejemplo de Adventure Works LT. 

1. Haga clic en el botón **Nuevo** de la esquina superior izquierda de Azure Portal.

2. En la página **Nuevo**, seleccione **Bases de datos** y, en la página **Bases de datos**, seleccione **SQL Database**.

    ![create database-1](./media/sql-database-get-started/create-database-1.png)

3. Rellene el formulario de SQL Database con la siguiente información, como se muestra en la imagen anterior: 
   - Nombre de base de datos: use **mySampleDatabase**
   - Grupo de recursos: use **myResourceGroup**
   - Origen: seleccione **Sample (AdventureWorksLT)** [Ejemplo (AdventureWorksLT)]

4. Haga clic en **Servidor** para crear y configurar un servidor nuevo para la nueva base de datos. Rellene el **formulario de servidor nuevo**, en el que debe especificar un nombre de servidor único global, indique un nombre para el inicio de sesión de administrador del servidor y especifique la contraseña que desee. 

    ![create database-server](./media/sql-database-get-started/create-database-server.png)
5. Haga clic en **Seleccionar**.

6. Haga clic en **Plan de tarifa** para especificar el tanto el nivel de rendimiento como el nivel de servicio de la nueva base de datos. Para este ejemplo, seleccione **20 DTU** y **250** GB de almacenamiento

    ![create database-s1](./media/sql-database-get-started/create-database-s1.png)

7. Haga clic en **Apply**.  

8. Haga clic en **Create** (Crear) para realizar el aprovisionamiento de la base de datos. El aprovisionamiento tarda unos minutos. 

9. En la barra de herramientas, haga clic en **Notificaciones** para supervisar el proceso de implementación.

    ![notificación](./media/sql-database-get-started/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Crear una regla de firewall de nivel de servidor

El servicio SQL Database crea un firewall en el nivel de servidor, lo que impide que herramientas y aplicaciones externas se conecten al servidor o a las bases de datos del servidor, a menos que se cree una regla de firewall para abrir el firewall para direcciones IP concretas. Siga estos pasos para crear una [regla de firewall de nivel de servidor de SQL Database](sql-database-firewall-configure.md) para la dirección IP de su cliente y habilite la conectividad externa a través de dicho firewall solo para su dirección IP. 

1. Una vez finalizada la implementación, haga clic en **SQL Database** en el menú izquierdo y haga clic en la base de datos nueva, **mySampleDatabase**, en la página **SQL Database**. Se abre la página de información general de la base de datos, que muestra el nombre completo del servidor (por ejemplo, **mynewserver20170327.database.windows.net**) y proporciona opciones para otras configuraciones.

      ![regla de firewall del servidor](./media/sql-database-get-started/server-firewall-rule.png) 

2. Haga clic en **Establecer el firewall del servidor** en la barra de herramientas, como se muestra en la imagen anterior. Se abrirá la página **Configuración del firewall** del servidor de SQL Database. 

3. Haga clic en **Agregar IP de cliente** en la barra de herramientas y haga clic en **Guardar**. Se creará una regla de firewall de nivel de servidor para la dirección IP actual.

      ![establecer regla de firewall del servidor](./media/sql-database-get-started/server-firewall-rule-set.png) 

4. Haga clic en **Aceptar** y, después, en la **X** para cerrar la página **Configuración de firewall**.

Ahora puede conectarse a la base de datos y a su servidor mediante SQL Server Management Studio u otra herramienta que elija.

## <a name="query-the-sql-database"></a>Consulta a SQL Database

Al crear la instancia de SQL Database, se rellenó con la base de datos de ejemplo **AdventureWorksLT** (esta es una de las opciones que se ha seleccionado en la interfaz de usuario anteriormente en esta misma guía de inicio rápido). Ahora vamos a usar ahora la herramienta de consulta integrada en Azure Portal para consultar los datos. 

1. En la barra de herramientas de la página SQL Database de la base de datos, haga clic en **Herramientas**. Se abre la página **Herramientas**.

     ![menú herramientas](./media/sql-database-get-started/tools-menu.png) 

2. Haga clic en **Editor de consultas (versión preliminar)**, en la casilla de verificación **Términos de vista previa** y en **Aceptar**. Se abre la página Editor de consultas.

3. Haga clic en **Inicio de sesión** y después, cuando se le solicite, seleccione **Autenticación de servidor SQL Server** y especifique el inicio de sesión y la contraseña de administrador de servidor que creó antes.

    ![login](./media/sql-database-get-started/login.png) 

4. Haga clic en **Aceptar** para iniciar sesión.

5. Una vez autenticado, escriba la siguiente consulta en el panel del editor de consultas.

   ```
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Haga clic en **Ejecución** y revise los resultados de la consulta en el panel **Resultados**.

    ![resultados del editor de consultas](./media/sql-database-get-started/query-editor-results.png)

7. Haga clic en la **X** para cerrar la página **Editor de consultas** y vuelva a hacer clic en la **X** para cerrar la página **Herramientas**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Otras guías de inicio rápido de esta colección se basan en los valores de esta. Si tiene previsto seguir trabajando con las siguientes guías de inicio rápido o tutoriales, no elimine los recursos creados en esta guía de inicio rápido. Si no tiene previsto continuar, siga estos pasos para eliminar todos los recursos creados por esta guía de inicio rápido en Azure Portal.

1. En el menú izquierdo de Azure Portal, haga clic en **Grupos de recursos** y en **myResourceGroup**. 
2. En la página del grupo de recursos, haga clic en **Eliminar**, escriba **myResourceGroup** en el cuadro de texto y haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

- Para conectarse y consultar mediante SQL Server Management Studio, consulte el artículo de [Conexión y consultas con SSMS](sql-database-connect-query-ssms.md).
- Para conectarse con Visual Studio, consulte el artículo de [Conexión y consultas con Visual Studio](sql-database-connect-query.md).
- Para información técnica general de SQL Database, consulte el artículo [Acerca del servicio SQL Database](sql-database-technical-overview.md).

