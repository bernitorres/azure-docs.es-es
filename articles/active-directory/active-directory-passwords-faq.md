---
title: "Preguntas más frecuentes sobre la administración de contraseñas de Azure AD | Microsoft Docs"
description: "Preguntas más frecuentes acerca de la administración de contraseñas en Azure AD, incluido el restablecimiento de contraseña, el registro, los informes y la escritura diferida en Active Directory local."
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: 3a157d27-a410-4371-bcbf-8312941ae9d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: joflore
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 636e76e6732287ac78b6c025cc936602a38f49af
ms.lasthandoff: 03/28/2017


---
# <a name="password-management-frequently-asked-questions"></a>Preguntas más frecuentes sobre la administración de contraseñas
> [!IMPORTANT]
> **¿Está aquí porque tiene problemas para iniciar sesión?** Si es así, [aquí aprenderá a cambiar y restablecer la contraseña](active-directory-passwords-update-your-own-password.md#reset-your-password).
>
>

Las siguientes son algunas de las preguntas más frecuentes sobre todos los aspectos relacionados con la administración de contraseñas.

Si encuentra una pregunta cuya respuesta no conoce, o bien si busca ayuda para solucionar un problema determinado que enfrenta, puede seguir leyendo para ver si ya hablamos al respecto.  No se preocupe si todavía no lo hemos hecho. Si tiene alguna pregunta que no se aborde aquí, no dude en preguntar en los [foros de Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) y nos comunicaremos con usted tan pronto como sea posible.

Estas preguntas más frecuentes se dividen en las siguientes secciones:

* [**Preguntas sobre el registro de restablecimiento de contraseña**](#password-reset-registration)
* [**Preguntas sobre el restablecimiento de contraseña**](#password-reset)
* [**Preguntas sobre el cambio de contraseña**](#password-change)
* [**Preguntas sobre los informes de la administración de contraseñas**](#password-management-reports)
* [**Preguntas sobre la escritura diferida de contraseñas**](#password-writeback)

## <a name="password-reset-registration"></a>Registro de restablecimiento de contraseña
* **P: ¿Mis usuarios pueden registrar sus propios datos de restablecimiento de contraseña?**

  > **R.:** Sí, siempre que el restablecimiento de contraseña esté habilitado y que los usuarios cuenten con licencia, pueden ir al portal de Registro de restablecimiento de contraseña en http://aka.ms/ssprsetup para registrar su información de autenticación y poder usarla con el restablecimiento de contraseña. Para registrarse, los usuarios también pueden ir al panel de acceso en http://myapps.microsoft.com, hacer clic en la pestaña de perfil y en la opción Registrarme para restablecer la contraseña. Consulte Cómo configurar a los usuarios para el restablecimiento de contraseña para obtener más información sobre cómo configurar a los usuarios para el restablecimiento de contraseña.
  >
  >
* **P: ¿Puedo definir los datos de restablecimiento de contraseña en nombre de mis usuarios?**

  > **R.:** Sí, puede hacerlo con DirSync o PowerShell o a través del [Portal de administración de Azure](https://manage.windowsazure.com) o el portal de administración de Office. Obtenga más información acerca de esta característica en la entrada de blog Improved Privacy for Azure AD MFA and Password Reset Phone Numbers (Seguridad mejorada para Azure AD MFA y números de teléfono para el restablecimiento de contraseña) y en Vea cómo utiliza los datos el proceso de restablecimiento de contraseña.
  >
  >
* **P: ¿Puedo sincronizar localmente los datos de las preguntas de seguridad?**

  > **R.:** No, actualmente no es posible, pero lo estamos considerando.
  >
  >
* **P: ¿Mis usuarios pueden registrar datos de manera tal que otros usuarios no puedan verlos?**

  > **R.:** Sí, cuando los usuarios registran datos a través del portal de Registro de restablecimiento de contraseña, se guardan en campos de autenticación privados que solo son visibles para los administradores globales y para el propio usuario. Obtenga más información acerca de esta característica en la entrada de blog Improved Privacy for Azure AD MFA and Password Reset Phone Numbers (Seguridad mejorada para Azure AD MFA y números de teléfono para el restablecimiento de contraseña) y en Vea cómo utiliza los datos el proceso de restablecimiento de contraseña.
  >
  >
* **P: ¿Mis usuarios necesitan registrarse antes de poder usar el restablecimiento de contraseña?**

  > **R.:** No. Si define información de autenticación suficiente en nombre de sus usuarios, no será necesario que se registren. El restablecimiento de contraseña funcionará bien siempre que tenga datos con formato correcto almacenados en los campos adecuados del directorio. Consulte Vea cómo utiliza los datos el proceso de restablecimiento de contraseña para obtener más información.
  >
  >
* **P: ¿Puedo sincronizar o definir los campos Teléfono de autenticación, Correo electrónico de autenticación o Teléfono de autenticación alternativo en nombre de mis usuarios?**

  > **R.:** No en este momento, pero estamos pensando en habilitar esta funcionalidad.
  >
  >
* **P: ¿Cómo sabe el portal de registro cuáles son las opciones que tiene que mostrar a mis usuarios?**

  > **R.:** El portal de Registro de restablecimiento de contraseña solo muestra las opciones que se habilitaron para los usuarios en la sección Políticas para restablecer la contraseña del usuario de la pestaña Configurar del directorio. Por ejemplo, esto significa que, si no habilita las preguntas de seguridad, los usuarios no podrán registrarse para obtener esa opción.
  >
  >
* **P: ¿Cuándo se considera que un usuario está registrado?**

  > **R.:** Un usuario se considera como registrado cuando tiene definidos, al menos, N fragmentos de información de autenticación, donde N es el número de métodos de autenticación requeridos que ha definido en el [Portal de administración de Azure](https://manage.windowsazure.com). Para obtener más información, consulte Personalización de la directiva de restablecimiento de contraseña.
  >
  >

## <a name="password-reset"></a>Restablecimiento de contraseña
* **P: ¿Cuánto tiempo tengo que esperar para recibir un correo electrónico, un SMS o una llamada telefónica para el restablecimiento de contraseña?**

  > **R.:** Los correos electrónicos, los mensajes SMS y las llamadas telefónicas no debieran demorar más de 1 minuto; lo normal sería entre 5 y 20 segundos. Si no recibe la notificación en este período de tiempo, revise su carpeta de correo no deseado, que el número o el correo electrónico de contacto sea el que espera y que los datos de autenticación existentes en el directorio tengan el formato correcto. Para obtener más información acerca del formato de los números de teléfono y las direcciones de correo electrónico para usarlos con el restablecimiento de contraseña, consulte Vea cómo utiliza los datos el proceso de restablecimiento de contraseña.
  >
  >
* **P: ¿Qué idiomas admite el restablecimiento de contraseña?**

  > **R.:** La interfaz de usuario del restablecimiento de contraseña, los mensajes SMS y las llamadas de voz están localizados en los mismos 40 idiomas que admite Office 365. Esos idiomas son: alemán, árabe, búlgaro, checo, chino simplificado, chino tradicional, coreano, croata, danés, eslovaco, esloveno, español, estoniano, finés, francés, griego, hebreo, hindi, húngaro, indonesio, inglés, italiano, japonés, kazajo, letón, lituano, malayo (Malasia), neerlandés, noruego (Bokmål), polaco, portugués (Brasil), portugués (Portugal), rumano, ruso, serbio (latino), sueco, tailandés, turco, ucraniano y vietnamita.
  >
  >
* **P: ¿En qué partes de la experiencia de restablecimiento de contraseña aparece mi marca si defino la personalización de marca de mi organización en la pestaña de configuración del directorio?**

  > **R.:** El portal de restablecimiento de contraseña mostrará el logotipo de su organización y también le permitirá configurar el vínculo Póngase en contacto con el administrador para dirigirlo a una dirección URL o a un correo electrónico personalizado. Todo correo electrónico enviado por el proceso de restablecimiento de contraseña incluirá el logotipo, los colores (en este caso, rojo) y el nombre de su organización en el cuerpo del correo electrónico y estará personalizado a partir del nombre. A continuación, puede ver un ejemplo con todos los elementos personalizados con la marca. Para obtener más información, consulte Personalización de la apariencia del restablecimiento de contraseña.
  >
  >

  ![][001]
* **P: ¿Cómo puedo informar a mis usuarios sobre dónde tienen que ir para restablecer sus contraseñas?**

  > **R.:** Puede enviar a los usuarios directamente a https://passwordreset.microsoftonline.com, o bien puede indicarles que hagan clic en el vínculo ¿No puede acceder a su cuenta? en cualquier página de inicio de sesión del id. educativo o profesional. Puede publicar estos vínculos (o crear redireccionamientos de direcciones URL) en cualquier lugar al que sus usuarios puedan tener acceso fácilmente.
  >
  >
* **P: ¿Puedo usar esta página desde un dispositivo móvil?**

  > **R.:** Sí, esta página funciona en dispositivos móviles.
  >
  >
* **P: ¿Se admite el desbloqueo de cuentas locales de Active Directory cuando los usuarios restablecen sus contraseñas?**

  > **R.:** Sí. Cuando un usuario restablece su contraseña y se ha implementado la escritura diferida de contraseñas con todas las versiones de Azure AD Connect o con las versiones de Sincronización de Azure AD 1.0.0485.0222 o posteriores, la cuenta de ese usuario se desbloqueará automáticamente cuando el usuario restablezca su contraseña.
  >
  >
* **P: ¿Cómo puedo integrar directamente el restablecimiento de contraseña en la experiencia de inicio de sesión de escritorio de mi usuario?**

  > **R.:** No es posible hacerlo actualmente. Sin embargo, si es absolutamente necesario para usted contar con esta funcionalidad y es cliente de Azure AD Premium, puede instalar Microsoft Identity Manager sin costo adicional e implementar la solución de restablecimiento de contraseña local que contiene para satisfacer este requisito.
  >
  >
* **P: ¿Puedo establecer distintas preguntas de seguridad para diferentes configuraciones regionales?**

  > **R.:** No, actualmente no es posible, pero lo estamos considerando.
  >
  >
* **P: ¿Cuántas preguntas podemos configurar en la opción de autenticación Preguntas de seguridad?**

  > **R.:** Puede configurar hasta 20 preguntas de seguridad personalizadas en el [Portal de administración de Azure](https://manage.windowsazure.com).
  >
  >
* **P: ¿Qué longitud pueden tener las preguntas de seguridad?**

  > **R.:** Las preguntas de seguridad pueden tener entre 3 y 200 caracteres.
  >
  >
* **P: ¿Qué longitud pueden tener las respuestas a las preguntas de seguridad?**

  > **R.:** Las respuestas pueden tener entre 3 y 40 caracteres.
  >
  >
* **P: ¿Se rechazarán las respuestas duplicadas a las preguntas de seguridad?**

  > **R.:** Sí, rechazaremos las respuestas duplicadas a las preguntas de seguridad.
  >
  >
* **P: ¿Puede un usuario registrar más de una de las mismas preguntas de seguridad?**

  > **R.:** No. Una vez que un usuario registra una pregunta específica, no puede registrar esa pregunta por segunda vez.
  >
  >
* **P: ¿Es posible establecer un límite mínimo de preguntas de seguridad para registro y restablecimiento?**

  > **R.:** Sí, es posible establecer un límite para el registro y otro para el restablecimiento. Es posible que se requieran entre 3 y 5 preguntas para el registro y entre 3 y 5 para el restablecimiento.
  >
  >
* **P: Si un usuario ha registrado más del número máximo de preguntas necesarias para el restablecimiento, ¿cómo se seleccionan las preguntas de seguridad durante el proceso de restablecimiento?**

  > **R.:** De manera aleatoria se seleccionan N preguntas de seguridad del número total de preguntas para las cuales se ha registrado un usuario, donde N es el número mínimo de preguntas necesarias para el restablecimiento de contraseña. Por ejemplo, si un usuario tiene registradas 5 preguntas de seguridad, pero solo se requieren 3 para el restablecimiento, 3 de esas 5 se seleccionarán de manera aleatoria y se presentarán al usuario en el momento del restablecimiento. Si el usuario se equivoca al responder las preguntas, el proceso de selección vuelve a ejecutarse para evitar la repetición (hammering) de las preguntas.
  >
  >
* **P: ¿Es posible evitar que los usuarios intenten restablecer la contraseña muchas veces en un período de tiempo breve?**

  > **R.:** Sí, existen varias características de seguridad integradas en el restablecimiento de contraseña. Los usuarios solo pueden intentar 5 restablecimientos de contraseña en un período de una hora antes de que se les bloquee durante 24 horas. Los usuarios solo pueden intentar validar un número de teléfono 5 veces dentro de una hora antes de que se les bloquee durante 24 horas. Los usuarios solo pueden intentar un método de autenticación 5 veces dentro de una hora antes de que se les bloquee durante 24 horas.
  >
  >
* **P: ¿Durante cuánto tiempo es válido el código de acceso de un solo uso que recibe por correo electrónico o mensaje SMS?**

  > **R.:** La duración de la sesión de restablecimiento de contraseña es de 105 minutos. Esto significa que, desde el principio de la operación de restablecimiento de contraseña, el usuario tiene 105 minutos para restablecer la contraseña. El código de acceso de un solo uso que recibe por correo electrónico o mensaje SMS no es válido después de este período de tiempo.
  >
  >

## <a name="password-change"></a>Cambio de contraseña
* **P: ¿Dónde deberían ir los usuarios para cambiar las contraseñas?**

  > **R:** Los usuarios pueden cambiar sus contraseñas en cualquier lugar en que vean su icono o imagen de perfil (como en la esquina superior derecha de las experiencias de [Office 365](https://portal.office.com) o el [Panel de acceso](https://myapps.microsoft.com)). Los usuarios pueden cambiar las contraseñas en la [página de perfil del Panel de acceso](https://account.activedirectory.windowsazure.com/r#/profile). Los usuarios también deberán cambiar automáticamente sus contraseñas en la pantalla de inicio de sesión de Azure AD si estas expiraron. Por último, los usuarios pueden navegar directamente al [portal de cambio de contraseñas de Azure AD](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) si desean cambiar las contraseñas.
  >
  >
* **P: ¿Los usuarios pueden recibir una notificación en el Portal de Office cuando expira la contraseña local?**

  > **R:** esto es posible actualmente si usa ADFS y sigue estas instrucciones: [Sending Password Policy Claims with ADFS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396) (Envío de notificaciones de directiva de contraseña con ADFS). Esto no es posible hoy si usa la sincronización de hash de contraseña. Esto se debe a que no sincronizamos las directivas de contraseña en el entorno local, por lo que no es posible publicar notificaciones de expiración en las experiencias en la nube. En cualquier caso, también es posible [notificar a los usuarios cuyas contraseñas están a punto de expirar a través de PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >

## <a name="password-management-reports"></a>Informes de administración de contraseñas
* **P: ¿Cuánto tiempo demoran en aparecer los datos en los informes de la administración de contraseñas?**

  > **R.:** Los datos deben aparecer en los informes de la administración de contraseñas en un plazo de entre 5 y 10 minutos. En algunas instancias, es posible que demoren hasta una hora en aparecer.
  >
  >
* **P: ¿Cómo puedo filtrar los informes de administración de contraseñas?**

  > **R.:** Para filtrar los informes de administración de contraseñas, haga clic en la lupa pequeña que aparece en el extremo derecho de las etiquetas de columna, hacia la parte superior del informe (observe la captura de pantalla). Si desea realizar un filtrado más completo, puede descargar el informe a Excel y crear una tabla dinámica.
  >
  >

  ![][002]
* **P: ¿Cuál es el número máximo de eventos que se almacenan en los informes de administración de contraseñas?**

  > **R:** Hasta 75 000 eventos de restablecimiento de contraseña o de registro de restablecimiento de contraseña se almacenan en los informes de administración de contraseñas, los que abarcan hasta los últimos 30 días.  Estamos trabajando para ampliar este número e incluir más eventos.
  >
  >
* **P: ¿Hasta cuándo se remontan los informes de administración de contraseñas?**

  > **R.:** Los informes de administración de contraseñas muestran las operaciones generadas dentro de los últimos 30 días. Actualmente investigamos cómo extender este período. Por ahora, si desea archivar estos datos, puede descargar periódicamente los informes y guardarlos en una ubicación independiente.
  >
  >
* **P: ¿Existe un número máximo de filas que pueden aparecer en los informes de administración de contraseñas?**

  > **R.:** Sí, pueden aparecer 75 000 filas como máximo en cualquiera de los informes de administración de contraseñas, tanto si se muestran en la interfaz de usuario o se descargan. Actualmente investigamos cómo aumentar este límite.
  >
  >
* **P: ¿Existe una API para acceder a los datos de informes de registro o de restablecimiento de contraseña?**

  > **R.:** Sí, consulte la documentación siguiente para saber cómo puede tener acceso al flujo de datos de informes de restablecimiento de contraseña.  [Obtenga información sobre cómo tener acceso a los eventos de informes de restablecimiento de contraseña mediante programación](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Escritura diferida de contraseñas
* **P: ¿Cómo funciona la escritura diferida de contraseñas en segundo plano?**

  > **R.:** Consulte [Funcionamiento de la escritura diferida de contraseñas](active-directory-passwords-learn-more.md#how-password-writeback-works) para obtener una explicación detallada de lo que ocurre cuando se habilita la escritura diferida de contraseñas, además de cómo fluyen los datos por el sistema para volver al entorno local. Consulte [Modelo de seguridad de la escritura diferida de contraseñas](active-directory-passwords-learn-more.md#password-writeback-security-model) en Funcionamiento de la escritura diferida de contraseñas para más información sobre cómo podemos asegurarnos de que la escritura diferida de contraseñas sea un servicio con un alto nivel de seguridad.
  >
  >
* **P: ¿Cuánto tarda en funcionar la escritura diferida de contraseñas?  ¿Existe un retraso en la sincronización, como ocurre con la sincronización de hash de contraseña?**

  > **R.:** La escritura diferida de contraseñas es inmediata. Se trata de una canalización sincrónica que funciona radicalmente distinto a la sincronización de hash de contraseña. La escritura diferida de contraseñas permite que los usuarios obtengan comentarios en tiempo real sobre el éxito de su operación de cambio o restablecimiento de contraseña. El tiempo promedio para la escritura diferida correcta de una contraseña es de menos de 500 ms.
  >
  >
* **P: ¿Para qué tipos de cuentas funciona la escritura diferida de contraseñas?**

  > **R.:** La escritura diferida de contraseñas funciona para usuarios federados y usuarios con sincronización de hash de contraseña.
  >
  >
* **P: ¿La escritura diferida de contraseñas aplica las directivas de contraseñas de mi dominio?**

  > **R.:** Sí, la escritura diferida de contraseñas aplica la vigencia, el historial, la complejidad y los filtros de contraseñas, además de cualquier otra restricción que pueda aplicar sobre las contraseñas en su dominio local.
  >
  >
* **P: ¿Es segura la escritura diferida de contraseñas?  ¿Cómo puedo estar seguro de no ser víctima del ataque de un hacker?**

  > **R.:** Sí, la escritura diferida de contraseñas es sumamente segura. Para más información sobre los cuatro niveles de seguridad que implementa el servicio de escritura diferida de contraseñas, consulte [Modelo de seguridad de la escritura diferida de contraseñas](active-directory-passwords-learn-more.md#password-writeback-security-model) en Funcionamiento de la escritura diferida de contraseñas.
  >
  >

## <a name="next-steps"></a>Pasos siguientes
A continuación se muestran vínculos a todas las páginas de documentación de restablecimiento de contraseña de Azure AD:

* **¿Está aquí porque tiene problemas para iniciar sesión?** Si es así, [aquí aprenderá a cambiar y restablecer la contraseña](active-directory-passwords-update-your-own-password.md#reset-your-password).
* [**Funcionamiento**](active-directory-passwords-how-it-works.md): obtenga información acerca de los seis componentes diferentes del servicio y lo que hace cada uno.
* [**Introducción**](active-directory-passwords-getting-started.md): obtenga información sobre cómo permitir a los usuarios restablecer y cambiar sus contraseñas en la nube o locales.
* [**Personalizar**](active-directory-passwords-customize.md): obtenga información acerca de cómo personalizar la apariencia y el comportamiento del servicio para ajustarse a las necesidades de su organización
* [**Procedimientos recomendados**](active-directory-passwords-best-practices.md): obtenga información acerca de cómo implementar rápidamente y administrar eficazmente las contraseñas de la organización
* [**Obtener información**](active-directory-passwords-get-insights.md): obtenga información acerca de nuestras funcionalidades integradas de creación de informes.
* [**Solución de problemas**](active-directory-passwords-troubleshoot.md): obtenga información sobre cómo solucionar rápidamente los problemas del servicio.
* [**Más información**](active-directory-passwords-learn-more.md): profundice en los detalles técnicos del funcionamiento del servicio.

[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"

