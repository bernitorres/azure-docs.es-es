---
title: "Tutorial: Integración de Azure Active Directory con Pantheon | Microsoft Docs"
description: "Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Pantheon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 52a42d6705d0d2cfb86865027f8315411d7aece0
ms.openlocfilehash: b14872daab1bddf18d68b1f0fb23fc74c12078ed


---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a>Tutorial: Integración de Azure Active Directory con Pantheon

En este tutorial, aprenderá a integrar Pantheon con Azure Active Directory (Azure AD).

La integración de Pantheon con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Pantheon.
- Puede permitir que los usuarios inicien sesión automáticamente en Pantheon (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el Portal de Azure clásico.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Pantheon, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Pantheon.


>[!NOTE] 
>Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.


Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No debe usar el entorno de producción, a menos que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.

La situación descrita en este tutorial consta de dos bloques de creación principales:

1. Adición de Pantheon desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD


## <a name="adding-pantheon-from-the-gallery"></a>Adición de Pantheon desde la galería
Para configurar la integración de Pantheon en Azure AD, es preciso agregar Pantheon desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Pantheon desde la galería, realice los pasos siguientes:**

1. En el **Portal de Azure clásico**, en el panel de navegación izquierdo, haga clic en **Active Directory**.

    ![Active Directory][1]

2. En la lista **Directory** , seleccione el directorio cuya integración desee habilitar.

3. Para abrir la vista de aplicaciones, haga clic en **Applications** , en el menú superior de la vista de directorios.

    ![Applications][2]

4. Haga clic en **Agregar** en la parte inferior de la página.

    ![Aplicaciones][3]

5. En el cuadro de diálogo **¿Qué desea hacer?**, haga clic en **Agregar una aplicación de la galería**.

    ![Aplicaciones][4]

6. En el cuadro de búsqueda, escriba **Pantheon**.

    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_01.png)

7. En el panel de resultados, seleccione **Pantheon** y haga clic en **Completar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Pantheon con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Pantheon para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Pantheon.

Para establecer esta relación de vínculo, se asigna el valor del **nombre de usuario** en Azure AD como el valor de **nombre de usuario** en Pantheon.

Para configurar y probar el inicio de sesión único de Azure AD con Pantheon, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Pantheon](#creating-a-pantheon-test-user)**: para tener un homólogo de Britta Simon en Pantheon que esté vinculado a la representación de ella en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en el portal clásico y configurará el inicio de sesión único en la aplicación Pantheon.


**Para configurar el inicio de sesión único de Azure AD con Pantheon, siga estos pasos:**

La aplicación Pantheon espera la aserción de SAML en un formato específico, lo que requiere establecer el valor del atributo NameIdentifier con la dirección de correo electrónico del usuario. De forma predeterminada, Azure AD usa el valor de UserPrincipalName como el atributo NameIdentifier. Para una correcta integración, deberá ajustar este valor para que coincida con la dirección de correo electrónico del usuario.
La integración solo funcionará después de realizar la asignación correcta.

![Configurar inicio de sesión único](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_06.png)
    
1. En el portal clásico, en la página de integración de aplicaciones de **Pantheon**, haga clic en **Configurar inicio de sesión único** para abrir el cuadro de diálogo **Configurar inicio de sesión único**.
     
    ![Configurar inicio de sesión único][6] 

2. En la página **How would you like users to sign on to Pantheon** (¿Cómo desea que los usuarios inicien sesión en Pantheon?), seleccione **Inicio de sesión único de Azure AD** y después haga clic en **Siguiente**.

    ![Configurar inicio de sesión único](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_03.png) 

3. En la página de diálogo **Configurar las opciones de la aplicación** , realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_04.png) 

    a. En el cuadro de texto **Identificador**, escriba la dirección URL con el siguiente patrón: `urn:auth0:pantheon:<orgname>-SSO`
    
    b. En el cuadro de texto **URL de respuesta**, escriba la dirección URL con el siguiente patrón: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`. 

    c. click **Siguiente**
 
4. En la página **Configurar inicio de sesión único en Pantheon**, siga estos pasos:

    ![Configurar inicio de sesión único](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_05.png)

    a. Haga clic en **Descargar certificado**y después guarde el archivo en el equipo.

    b. Haga clic en **Siguiente**.


5. Para configurar el inicio de sesión único para su aplicación, póngase en contacto con el equipo de [soporte técnico de Pantheon](https://pantheon.io/docs/getting-support) y proporcione la siguiente información:

    • El **certificado**

    • La **dirección URL de servicio de inicio de sesión único**

    >[!NOTE] 
    >También debe proporcionar la información del dominio de correo electrónico y la fecha y hora en que quiere habilitar esta conexión. Puede encontrar más detalles al respecto [aquí](https://pantheon.io/docs/sso-organizations/).

6. En el portal clásico, seleccione la confirmación de la configuración de inicio de sesión único y haga clic en **Siguiente**.
    
    ![Inicio de sesión único de Azure AD ][10]

7. En la página **Confirmación del inicio de sesión único**, haga clic en **Completar**.  
 
    ![Inicio de sesión único de Azure AD ][11]


### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
En esta sección, creará un usuario de prueba llamado Britta Simon en el portal clásico.


![Creación de un usuario de Azure AD][20]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo del **Portal de Azure clásico**, haga clic en **Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_09.png) 

2. En la lista **Directory** , seleccione el directorio cuya integración desee habilitar.

3. Para mostrar la lista de usuarios, en el menú de la parte superior, haga clic en **Usuarios**.

    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_03.png) 

4. Para abrir el cuadro de diálogo **Agregar usuario**, en la barra de herramientas de la parte inferior, haga clic en **Agregar usuario**.

    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_04.png) 

5. En el cuadro de diálogo **Proporcione información sobre este usuario**, siga estos pasos:  ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_05.png) 

    a. En Tipo de usuario, seleccione Nuevo usuario de la organización.

    b. En el cuadro de texto **Nombre de usuario**, escriba**BrittaSimon**.

    c. Haga clic en **Siguiente**.

6.  En el cuadro de diálogo **Perfil de usuario**, siga estos pasos: ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_06.png) 

    a. En el cuadro de texto **Nombre**, escriba **Britta**.  

    b. En el cuadro de texto **Apellidos**, escriba **Simon**.

    c. En el cuadro de texto **Nombre para mostrar**, escriba **Britta Simon**.

    d. En la lista **Rol**, seleccione **Usuario**.

    e. Haga clic en **Siguiente**.

7. En el cuadro de diálogo **Obtener contraseña temporal**, haga clic en **Crear**.

    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_07.png) 

8. En la página de diálogo **Obtener contraseña temporal** , realice los pasos siguientes:

    ![Creación de un usuario de prueba de Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_08.png) 

    a. Anote el valor del campo **Nueva contraseña**.

    b. Haga clic en **Completo**.   



### <a name="creating-an-pantheon-test-user"></a>Creación de un usuario de prueba de Pantheon

En esta sección, creará un usuario llamado Britta Simon en Pantheon. Siga los pasos siguientes para agregar el usuario en Pantheon. 

>[!NOTE] 
>Para que funcione el inicio de sesión único, primero es necesario crear el usuario en Pantheon.

1. Inicie sesión en Pantheon con credenciales de administrador.

2. Vaya a la página del panel **Organization** (Organización).
 
3. Haga clic en **Contactos**.

4. Haga clic en **Agregar usuario**.

5. Escriba la dirección de correo electrónico del usuario.

6. Elija el rol del usuario.

7. Haga clic en **Agregar usuario**.


### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a Pantheon.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Pantheon, realice los pasos siguientes:**

1. En el portal clásico, para abrir la vista de aplicaciones, en la vista del directorio, haga clic en **Aplicaciones** en el menú superior.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Pantheon**.

    ![Configurar inicio de sesión único](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_50.png) 

3. En el menú de la parte superior, haga clic en **Usuarios**.

    ![Asignar usuario][203]

4. En la lista Usuarios, seleccione **Britta Simon**.

5. En la barra de herramientas de la parte inferior, haga clic en **Asignar**.

    ![Asignar usuario][205]


### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Pantheon en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Pantheon.


## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO1-->


