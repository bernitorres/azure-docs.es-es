---
title: "Ayuda y soporte técnico para la aplicación Microsoft Authenticator | Microsoft Docs"
description: "Proporciona una lista de las preguntas y respuestas más frecuentes relacionadas con la aplicación Microsoft Authentication y Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar, librown
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 7c91ae265dd86e8d7f206c1ada021c40fd8c4479
ms.openlocfilehash: cb3b7cfefe22176e17064ad6cbb9df4da8c57c0f
ms.lasthandoff: 03/01/2017


---
# <a name="microsoft-authenticator-app-faq"></a>Preguntas más frecuentes de la aplicación Microsoft Authenticator

En este artículo se proporcionan respuestas a preguntas habituales que recibimos sobre la aplicación Microsoft Authenticator. Si no ve una respuesta a su pregunta, vaya al [foro de la aplicación Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). También tenemos otras preguntas frecuentes de una característica específica de la aplicación: [la de iniciar sesión con un teléfono](microsoft-authenticator-app-phone-signin-faq.md).

La aplicación Microsoft Authenticator reemplaza la aplicación Azure Authenticator y es la aplicación recomendada cuando se utiliza Azure Multi-Factor Authentication. La aplicación Microsoft Authenticator está disponible para [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) e [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>¿Para qué sirven los códigos de la aplicación? ¿Por qué el número es regresivo?

Cuando se abre la aplicación Microsoft Authenticator, podrá ver las cuentas que ha agregado y un número de seis u ocho dígitos en cada una de ellas. Es posible que vea un temporizador de treinta segundos que cuenta de manera regresiva.

Estos códigos se usan cuando inicia sesión en la cuenta. Después de escribir el nombre de usuario y la contraseña, puede que se le pida que especifique un código de comprobación. Abra la aplicación Microsoft Authenticator y copie el código que se mostrando. Introduzca ese código en la página de inicio de sesión para finalizar.

El motivo por el que los códigos cambian cada 30 segundos es para que nunca se utilice el mismo código dos veces. No es lo mismo que una contraseña, que se supone que debe recordar. La idea es que solo un usuario con acceso a su teléfono conozca el código de comprobación.

Los códigos no requieren Internet ni datos, por lo que no tiene que preocuparse de tener el servicio de teléfono para iniciar sesión ni de que la aplicación utilice el plan de datos. Cuando se cierra la aplicación, esta no sigue ejecutándose en segundo plazo, por lo que no agotará la batería. Puede cerrar la aplicación e ignorarla hasta la próxima vez que inicie sesión.  

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Ya uso la aplicación Microsoft Authenticator con los códigos de comprobación. ¿Cómo cambio a las notificaciones push de un solo clic?
La aprobación de un inicio de sesión mediante notificaciones push solo está disponible para las cuentas personales, profesionales y educativas de Microsoft. No está disponible para cuentas de terceros, como Google o Facebook. Si tiene una cuenta profesional o educativa de Microsoft, su organización puede deshabilitar esta opción.

Si usa una cuenta de Microsoft como cuenta personal y quiere cambiar a las notificaciones push, deberá volver a agregar su cuenta. Vuelva a registrar el dispositivo con su cuenta y configure las notificaciones de inserción.  

Si usa Microsoft Authenticator para su cuenta profesional o educativa, su organización decide si debe permitir notificaciones de un solo clic.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>¿Las notificaciones push de un solo clic funcionan con cuentas que no sean de Microsoft?
No, las notificaciones de inserción solo funcionan con cuentas de Microsoft y de Azure Active Directory. Si su trabajo o centro educativo utiliza cuentas de Azure AD, se puede deshabilitar esta característica.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>He restaurado el dispositivo de una copia de seguridad y me faltan los códigos de cuentas o no funcionan. ¿Qué ha ocurrido?
Por motivos de seguridad, no restauramos cuentas de copias de seguridad de aplicaciones.  Después de restaurar la aplicación, elimine las cuentas y agréguelas de nuevo.

### <a name="i-got-a-new-device-how-do-i-remove-the-microsoft-authenticator-app-from-my-old-device-and-move-to-the-new-one"></a>Tengo un nuevo dispositivo. ¿Cómo quito la aplicación Microsoft Authenticator del antiguo y la muevo al nuevo?
El hecho de agregar la aplicación Microsoft Authenticator a un nuevo dispositivo no la quita automáticamente de los demás dispositivos. Para administrar los dispositivos que están configurados para su cuenta, visite el mismo sitio web que usa para administrar la comprobación en dos pasos y elija quitar las aplicaciones antiguas.

Para las cuentas de Microsoft personales, este sitio web es la página de su [cuenta seguridad](https://account.microsoft.com/security). Para las cuentas de Microsoft profesionales o educativas, este sitio web puede ser [Mis aplicaciones](https://myapps.microsoft.com) o un portal personalizado que su organización haya configurado.

### <a name="how-do-i-remove-an-account-from-the-app"></a>¿Cómo se quita una cuenta de la aplicación?
* iOS: desde la pantalla principal, deslice el dedo hacia la izquierda en un icono de cuenta. Seleccione **Eliminar**.
* Windows Phone: en la pantalla principal, seleccione el botón de menú y luego **Editar cuentas**. Pulse la **X** junto al nombre de la cuenta.
* Android: en la pantalla principal, seleccione el botón de menú y luego **Editar cuentas**. Pulse la **X** junto al nombre de la cuenta.

Si tiene un dispositivo que está registrado en su organización, puede que deba realizar un paso adicional para quitar la cuenta. En estos dispositivos, la aplicación Microsoft Authenticator se registra automáticamente como administrador del dispositivo. Si quiere desinstalar totalmente la aplicación, primero debe anular el registro de la aplicación en la configuración de la aplicación.

### <a name="why-does-the-app-request-so-many-permissions"></a>¿Por qué la aplicación solicita tantos permisos?
Esta es una lista completa de permisos que es posible que se le pidan y de cómo se usan en la aplicación. Los permisos específicos que se verá dependerán del tipo de teléfono que tenga.

* **Cámara**: usamos la cámara para escanear códigos QR cuando agrega una cuenta profesional, educativa o que no es de Microsoft.
* **Contactos y teléfono**: cuando inicia sesión con su cuenta personal de Microsoft, intentamos simplificar el proceso buscando cuentas existentes que utiliza en su teléfono.
* **SMS**: cuando inicia sesión con su cuenta personal de Microsoft por primera vez, tenemos que asegurarnos de que su número de teléfono coincida con el que tenemos en el registro. Enviamos un mensaje de texto al teléfono donde descargó la aplicación. El mensaje contiene un código de comprobación de 6 a 8 dígitos. En lugar de pedirle que busque este código y que lo escriba en la aplicación, lo buscamos en el mensaje de texto para usted.
* **Colocar sobre otras aplicaciones**: cuando recibe una notificación para comprobar su identidad, mostramos esa notificación sobre cualquier otra aplicación que podría estar ejecutándose.
* **Recibir datos de Internet**: este permiso es necesario para enviar notificaciones.
* **Impedir que el teléfono entre en modo de suspensión**: si registra el dispositivo con su organización, puede cambiar esta directiva en su teléfono.
* **Controlar la vibración**: tiene la opción de elegir si quiere que el dispositivo vibre cada vez que se recibe una notificación para comprobar su identidad.
* **Usar hardware de huella digital**: algunas cuentas profesionales o educativas requieran un PIN adicional cada vez que comprueba su identidad. PARA facilitar el proceso, se permite el uso de la huella digital en lugar de escribir el PIN.
* **Ver conexiones de red**: cuando se agrega una cuenta de Microsoft, la aplicación requiere conexión de red o a Internet.
* **Leer el contenido del almacenamiento**: este permiso solo se usa al notificar un problema técnico a través de la configuración de la aplicación. Parte de la información de su almacenamiento se recopila para diagnosticar el problema.
* **Acceso completo a la red**: este permiso es necesario para enviar notificaciones para comprobar tu identidad.
* **Ejecutar al inicio**: si reinicia el teléfono, este permiso garantiza que continuará recibiendo notificaciones para comprobar su identidad.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>¿Por qué la aplicación Microsoft Authenticator permite aprobar una solicitud sin desbloquear el servicio?

Esto es así por diseño. La verificación en dos pasos realiza dos comprobaciones: una comprobación de algo que sabe y una comprobación de algo que tiene. Lo que sabe es la contraseña. Lo que tiene es el teléfono (configurado con la aplicación Microsoft Authenticator y registrado como una prueba de MFA).  Por tanto, al tener el teléfono y aprobar la solicitud, se cumplen los criterios del segundo factor de la autenticación. 

## <a name="next-steps"></a>Pasos siguientes

### <a name="contact-us"></a>Ponerse en contacto con nosotros
Si no encontró aquí la respuesta a su pregunta, nos gustaría ayudarle. Acceda al [foro de la aplicación Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) para publicar la pregunta y obtener ayuda de la comunidad, para dejar un comentario en la página.


### <a name="related-topics"></a>Temas relacionados
* [Acerca de la comprobación en dos pasos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) de cuentas de Microsoft
* [Problemas con la verificación en dos pasos](multi-factor-authentication-end-user-troubleshoot.md) de la cuenta profesional o educativa
* [Uso de Microsoft Authenticator para iniciar sesión desde el teléfono](microsoft-authenticator-app-phone-signin-faq.md)


