---
title: "Tutorial: Integración de Azure Active Directory con Freshdesk | Microsoft Docs"
description: "Aprenda a usar Freshdesk con Azure Active Directory para habilitar el inicio de sesión único, el aprovisionamiento automático, etc."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 31177994-7910-4a72-bd76-5fa6260242fb
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 4778d46702a08ea6bae7e71e06e5b30d4fb697c3
ms.openlocfilehash: 927fa651dac67031bb26234023f23294497aea06
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Tutorial: integración de Azure Active Directory con Freshdesk
El objetivo de este tutorial es mostrar la integración de Azure y Freshdesk.  

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

* Una suscripción de Azure válida
* Un inquilino de Freshdesk

Después de completar este tutorial, los usuarios de Azure AD que haya asignado a Freshdesk podrán realizar un inicio de sesión único en la aplicación en el sitio de la compañía de Freshdesk (inicio de sesión iniciado por el proveedor de servicios) o desde [Introducción al Panel de acceso](active-directory-saas-access-panel-introduction.md).

La situación descrita en este tutorial consta de los siguientes bloques de creación:

* Habilitación de la integración de aplicaciones para Freshdesk
* Configuración del inicio de sesión único (SSO)
* Configuración del aprovisionamiento de usuario
* Asignación de usuarios

![Escenario](./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Escenario")

## <a name="enable-the-application-integration-for-freshdesk"></a>Habilitación de la integración de aplicaciones para Freshdesk
El objetivo de esta sección es describir cómo se habilita la integración de aplicaciones para Freshdesk.

**Siga estos pasos para habilitar la integración de aplicaciones para Freshdesk:**

1. En el panel de navegación izquierdo del Portal de Azure clásico, haga clic en **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Active Directory")
2. En la lista **Directory** , seleccione el directorio cuya integración desee habilitar.
3. Para abrir la vista de aplicaciones, haga clic en **Applications** , en el menú superior de la vista de directorios.
   
   ![Aplicaciones](./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Aplicaciones")
4. Haga clic en **Agregar** en la parte inferior de la página.
   
   ![Agregar aplicaciones](./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Agregar aplicaciones")
5. En el cuadro de diálogo **¿Qué desea hacer?**, haga clic en **Agregar una aplicación de la galería**.
   
   ![Agregar una aplicación de la galería](./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Agregar una aplicación de la galería")
6. En el **cuadro de búsqueda**, escriba **Freshdesk**.
   
   ![Galería de aplicaciones](./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Galería de aplicaciones")
7. En el panel de resultados, seleccione **Freshdesk** y luego haga clic en **Completar** para agregar la aplicación.
   
   ![Freshdesk](./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
   
## <a name="configure-single-sign-on"></a>Configurar inicio de sesión único

El objetivo de esta sección es describir cómo permitir a los usuarios autenticarse en Freshdesk con su cuenta de Azure AD mediante federación basada en el protocolo SAML.  

La configuración de un inicio de sesión único para Freshdesk requiere la recuperación de un valor de huella digital de un certificado. Si no está familiarizado con este procedimiento, consulte [Recuperación del valor de huella digital de un certificado](http://youtu.be/YKQF266SAxI).

**Siga estos pasos para configurar el inicio de sesión único:**

1. En el Portal de Azure clásico, en la página de integración de aplicaciones de **Freshdesk**, haga clic en **Configurar inicio de sesión único** para abrir el cuadro de diálogo **Configurar inicio de sesión único**.
   
   ![Configurar inicio de sesión único](./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Configurar inicio de sesión único")
2. En la página **How would you like users to sign on to Freshdesk** (¿Cómo desea que los usuarios inicien sesión en Freshdesk?), seleccione **Inicio de sesión único de Microsoft Azure AD** y luego haga clic en **Siguiente**.
   
   ![Configurar inicio de sesión único](./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Configurar inicio de sesión único")
3. En la página **Configurar dirección URL de la aplicación**, en el cuadro de texto **URL de inicio de sesión de Freshdesk**, escriba su dirección URL con el siguiente patrón "*https://\<tenant-name\>.Freshdesk.com*" y luego haga clic en **Siguiente**.
   
   ![Configurar dirección URL de la aplicación](./media/active-directory-saas-freshdesk-tutorial/IC776766.png "Configurar dirección URL de la aplicación")
4. En la página **Configurar inicio de sesión único en Freshdesk**, para descargar el certificado, haga clic en **Descargar certificado** y luego guarde el archivo de certificado localmente como **c:\\Freshdesk.cer**.
   
   ![Configurar inicio de sesión único](./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Configurar inicio de sesión único")
5. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Freshdesk como administrador.
6. En el menú de la parte superior, haga clic en **Administrador**.
   
   ![Administración](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Administración")
7. En la pestaña **Configuración general**, haga clic en **Seguridad**.
   
   ![Seguridad](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Seguridad")
8. En la sección **Seguridad** , realice estos pasos:
   
   ![Inicio de sesión único](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "inicio de sesión único")
   
   1. En **Inicio de sesión único (SSO)**, seleccione **Activado**.
   2. Seleccione **Inicio de sesión único de SAML**.
   3. En el Portal de Azure clásico, en la página del cuadro de diálogo **Configurar inicio de sesión único en Freshdesk**, copie el valor de **Dirección URL de inicio de sesión remoto** y péguelo en el cuadro de texto **Dirección URL de inicio de sesión de SAML**.
   4. En el Portal de Azure clásico, en la página del cuadro de diálogo **Configurar inicio de sesión único en Freshdesk**, copie el valor de **Dirección URL de cierre de sesión remoto** y péguelo en el cuadro de texto **Dirección URL de cierre de sesión**.
   5. Copie el valor de **Huella digital** del certificado exportado y luego péguelo en el cuadro de texto **Security Certificate Fingerprint** (Huella digital de certificado de seguridad).  
 
      >[!TIP]
      >Para obtener más información, consulte [Recuperación del valor de huella digital de un certificado](http://youtu.be/YKQF266SAxI). 
      > 
   6. Haga clic en **Guardar**.
9. En el Portal de Azure clásico, seleccione la confirmación de configuración de inicio de sesión único y haga clic en **Completar** para cerrar el cuadro de diálogo **Configurar inicio de sesión único**.
   
   ![Configurar inicio de sesión único](./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Configurar inicio de sesión único")
   
## <a name="configure-user-provisioning"></a>Configurar aprovisionamiento de usuarios

Para permitir que los usuarios de Azure AD inicien sesión en Freshdesk, deben aprovisionarse en Freshdesk. En el caso de Freshdesk, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **Freshdesk** .
2. En el menú de la parte superior, haga clic en **Administrador**.
   
   ![Administración](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Administración")
3. En la pestaña **Configuración general**, haga clic en **Agentes**.
   
   ![Agentes](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agentes")
4. Haga clic en **Nuevo agente**.
   
   ![Nuevo agente](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Nuevo agente")
5. En el cuadro de diálogo Agent Information (Información de agente), realice los pasos siguientes:
   
   ![Información sobre agentes](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Información sobre agentes")
   
   1. En el cuadro de texto **Nombre completo** , escriba el nombre de la cuenta de Azure AD que quiera aprovisionar.
   2. En el cuadro de texto **Email** (Correo electrónico), escriba la dirección de correo electrónico de la cuenta de Azure AD que quiera aprovisionar.
   3. En el cuadro de texto **Título** , escriba el título de la cuenta de Azure AD que quiera aprovisionar.
   4. Seleccione **Agents role** (Rol de agentes) y luego haga clic en **Asignar**.
   5. Haga clic en **Guardar**.     
   
      >[!NOTE]
      >El titular de la cuenta de Azure AD recibirá un mensaje de correo electrónico que incluye un vínculo para confirmar la cuenta antes de que se active. 
      > 

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Freshdesk que proporcione Freshdesk para aprovisionar cuentas de usuario de AAD. 
> 

## <a name="assign-users"></a>Asignar usuarios
Para probar la configuración, debe conceder acceso a los usuarios de Azure AD a los que quiere permitir el uso de su aplicación.

**Para asignar usuarios a Freshdesk, lleve a cabo los siguientes pasos:**

1. En el Portal de Azure clásico, cree una cuenta de prueba.
2. En la página de integración de aplicaciones de **Freshdesk**, haga clic en **Asignar usuarios**.
   
   ![Asignar usuarios](./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Asignar usuarios")
3. Seleccione su usuario de prueba, haga clic en **Asignar** y en **Sí** para confirmar la asignación.
   
   ![Sí](./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Sí")

Si desea probar la configuración de inicio de sesión único, abra el Panel de acceso. Para obtener más información sobre el Panel de acceso, vea [Introducción al Panel de acceso](active-directory-saas-access-panel-introduction.md).


