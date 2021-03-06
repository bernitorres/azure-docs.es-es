---
title: "Azure Backup: Copias de seguridad coherentes con la aplicación de las máquinas virtuales Linux de Azure | Microsoft Docs"
description: "Copias de seguridad coherentes con la aplicación de las máquinas virtuales Linux de Azure"
services: backup
documentationcenter: dev-center-name
author: anuragm
manager: shivamg
keywords: "copias de seguridad coherentes con la aplicación; copias de seguridad coherentes con la aplicación de máquinas virtuales de Azure; copia de seguridad de máquinas virtuales Linux; Azure Backup"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 3/20/2017
ms.author: anuragm;markgal
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 044b5d3834518b44209485d1c1a68f2ac0f8a455
ms.lasthandoff: 03/22/2017


---
# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Copias de seguridad coherentes con la aplicación de las máquinas virtuales Linux de Azure (versión preliminar)

En este artículo se describe el marco de trabajo de scripts anteriores y posteriores y cómo se puede usar para realizar copias de seguridad coherentes con las aplicaciones de las máquinas virtuales Linux de Azure.

> [!Note]
> Este marco de trabajo se admite solo con máquinas virtuales Linux implementadas según el modelo de Resource Manager. No se admite para máquinas virtuales clásicas ni para máquinas virtuales Windows.
>

## <a name="how-the-framework-works"></a>Funcionamiento del marco de trabajo

El marco de trabajo proporciona una opción para ejecutar scripts anteriores y posteriores personalizados mientras toma una instantánea de las máquinas virtuales. El script anterior se ejecuta justo antes de tomar la instantánea de las máquinas virtuales y el posterior inmediatamente después de la finalización de esta. Esto proporciona flexibilidad a los usuarios para controlar su aplicación y el entorno al realizar copias de seguridad de máquinas virtuales.

Un escenario importante para este marco de trabajo consiste en garantizar que se puedan realizar copias de seguridad coherentes con la aplicación de las máquinas virtuales. El script anterior puede invocar las API nativas de la aplicación para poner en modo inactivo las E/S y vaciar el contenido de la memoria en el disco, lo cual garantizará que la instantánea es coherente con la aplicación, es decir, la aplicación aparecerá cuando se inicie la máquina virtual después de la restauración. El script posterior se puede usar para reanudar las E/S mediante las API nativas de la aplicación de forma que esta reanude las operaciones normales después de la instantánea de la máquina virtual.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Pasos para configurar el script anterior y posterior

1. Inicie sesión en la máquina virtual Linux de la que desea realizar la copia de seguridad como usuario raíz.

2. Descargue VMSnapshotPluginConfig.json de [github](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) y cópielo en la carpeta /etc/azure de todas las máquinas virtuales de las que va a realizar copias de seguridad. Cree el directorio /etc/azure si aún no existe.

3. Copie el script anterior y el posterior de la aplicación en todas las máquinas virtuales de las que desea realizar copias de seguridad. Puede copiar los scripts en cualquier ubicación de la máquina virtual, solo necesita actualizar la ruta de acceso completa de los archivos de script en el archivo VMSnapshotPluginConfig.json

4. Asegúrese de proporcionar los siguientes permisos para los archivos:

   - VMSnapshotPluginConfig.json: permiso "600", es decir, solo el usuario "raíz"debe tener permisos de "lectura"y"escritura" para este archivo, ningún usuario debe tener permisos de "ejecución".
   - Archivo del script anterior: permiso "700" es decir, solo el usuario "raíz" debe tener permisos de "lectura", "escritura" y "ejecución" para este archivo.
   - Archivo del script posterior: permiso "700" es decir, solo el usuario "raíz" debe tener permisos de "lectura", "escritura" y "ejecución" para este archivo.
   
   > [!Note]
   > El marco de trabajo proporciona un gran rendimiento a los usuarios, por lo que es muy importante que esté completamente protegido y solo el usuario "raíz" debe tener acceso a los importantes archivos de script y al archivo json.
   > En caso de que los anteriores requisitos no se cumplan, no se ejecutará el script, lo cual resultará en la realización de copias de seguridad coherentes con el sistema de archivos y coherentes frente a bloqueos.
   >
   
5. Configuración del archivo VMSnapshotPluginConfig.json como se describe a continuación
    - **pluginName**: deje este campo como está ya que, de lo contrario, los scripts podrían no funcionar según lo previsto.
    - **preScriptLocation**: proporcione la ruta de acceso completa del script anterior en la máquina virtual de la que se va a realizar la copia de seguridad.
    - **postScriptLocation**: proporcione la ruta de acceso completa del script posterior en la máquina virtual de la que se va a realizar la copia de seguridad.
    - **preScriptParams**: cualquier parámetro opcional que deba pasarse al script anterior. Todos los parámetros deben estar entre comillas y separados por comas en caso de que haya varios.
    - **postScriptParams**: cualquier parámetro opcional que deba pasarse al script posterior. Todos los parámetros deben estar entre comillas y separados por comas en caso de que haya varios.
    - **preScriptNoOfRetries**: número de veces que el script anterior se debe volver a intentar si se produce cualquier error antes de finalizar. 0 significa que hay solo un intento y ningún reintento en caso de error.
    - **postScriptNoOfRetries**: número de veces que el script posterior se debe volver a intentar si se produce cualquier error antes de finalizar. 0 significa que hay solo un intento y ningún reintento en caso de error.
    - **timeoutInSeconds**: tiempos de espera individuales para el script anterior y el posterior.
    - **continueBackupOnFailure**: establezca este valor en true si desea que Azure Backup revierta a una copia de seguridad coherente con el sistema de archivos o coherente frente a bloqueos en caso de que el script anterior o posterior sufran un error. Si se establece en false, se producirá un error de la copia de seguridad en caso de error del script (excepto en el caso de una máquina virtual de un solo disco, en el que se revertirá a una copia de seguridad consistente frente a bloqueos independientemente de este valor).
    - **fsFreezeEnabled**: especifica si se debe llamar a fsfreeze de Linux al realizar la instantánea de la máquina virtual para garantizar la coherencia del sistema de archivos. Es recomendable mantener este valor en true a menos que la aplicación tenga dependencias al deshabilitar fsfreeze.
    
6. El marco de trabajo del script está ya configurado. Si la copia de seguridad de la máquina virtual ya está configurada, la siguiente copia de seguridad invocará los scripts y desencadenará la copia de seguridad coherente con la aplicación. Si la copia de seguridad de la máquina virtual no está configurada, hágalo siguiendo las instrucciones descritas en [Copia de seguridad de máquinas virtuales de Azure en almacenes de Recovery Services.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Solución de problemas

Asegúrese de que agrega el registro adecuado al escribir el script anterior y el posterior y revise los registros de script para corregir cualquier problema. Si sigue teniendo problemas para ejecutar los scripts, consulte la tabla siguiente para más información.

| Error | Mensaje de error | Acción recomendada |
| ------------------------ | -------------- | ------------------ |
| Pre-ScriptExecutionFailed |El script anterior devolvió un error por lo que puede que la copia de seguridad no sea coherente con la aplicación.    | Examine los registros de error del script para corregir el problema.|  
|    Post-ScriptExecutionFailed |    El script posterior devolvió un error que podría afectar al estado de la aplicación. |    Examine los registros de error del script para corregir el problema y compruebe el estado de la aplicación. |
| Pre-ScriptNotFound |    No se encontró el script anterior en la ubicación especificada en el archivo de configuración VMSnapshotPluginConfig.json. |    Asegúrese de que el script anterior está en la ruta de acceso especificada en el archivo de configuración para garantizar la realización de copias de seguridad coherentes con la aplicación.|
| Post-ScriptNotFound |    No se encontró el script posterior en la ubicación especificada en el archivo de configuración VMSnapshotPluginConfig.json |    Asegúrese de que el script posterior está en la ruta de acceso especificada en el archivo de configuración para garantizar la realización de copias de seguridad coherentes con la aplicación.|
| IncorrectPluginhostFile |    El archivo de Pluginhost que se incluye con la extensión VmSnapshotLinux está dañado por lo que no se puede ejecutar el script anterior ni el posterior y la copia de seguridad no será coherente con la aplicación.    | Desinstale la extensión VmSnapshotLinux. Esta se volverá a instalar automáticamente con la siguiente copia de seguridad para solucionar el problema. |
| IncorrectJSONConfigFile | El archivo VMSnapshotPluginConfig.json es incorrecto por lo que no se puede ejecutar el script anterior ni el posterior y la copia de seguridad no será coherente con la aplicación | Descargue la copia de [github](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) y configúrela de nuevo |
| InsufficientPermissionforPre-Script | Para ejecutar scripts, el usuario raíz debe ser el propietario del archivo y este debe tener permisos "700", es decir, solo el propietario tiene permisos de "lectura", "escritura" y "ejecución" | Asegúrese de que el usuario "raíz" es el "propietario" del archivo de script y que solo el propietario tiene permisos de "lectura", "escritura" y "ejecución". |
| InsufficientPermissionforPost-Script | Para ejecutar scripts, el usuario raíz debe ser el propietario del archivo y este debe tener permisos "700", es decir, solo el propietario tiene permisos de "lectura", "escritura" y "ejecución" | Asegúrese de que el usuario "raíz" es el "propietario" del archivo de script y que solo el propietario tiene permisos de "lectura", "escritura" y "ejecución". |
| Pre-ScriptTimeout | La ejecución del script anterior de la copia de seguridad coherente con la aplicación ha superado el tiempo de espera. | Compruebe el script y aumente el tiempo de espera en el archivo VMSnapshotPluginConfig.json situado en /etc/azure. |
| Post-ScriptTimeout | La ejecución del script posterior de la copia de seguridad coherente con la aplicación ha superado el tiempo de espera. | Compruebe el script y aumente el tiempo de espera en el archivo VMSnapshotPluginConfig.json situado en /etc/azure. |

## <a name="next-steps"></a>Pasos siguientes
[Configuración de la copia de seguridad en un almacén de Recovery Services](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)

