---
title: Administración del acceso mediante SSH en cuentas de dominio en Azure HDInsight
description: Pasos para administrar el acceso mediante SSH en cuentas de Azure AD en HDInsight.
ms.service: hdinsight
ms.topic: how-to
ms.date: 02/14/2020
ms.openlocfilehash: 3fab06f5e269f311f798c096a6465c8c6ce75fc2
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98946759"
---
# <a name="manage-ssh-access-for-domain-accounts-in-azure-hdinsight"></a>Administración del acceso mediante SSH en cuentas de dominio en Azure HDInsight

En los clústeres seguros, todos los usuarios del dominio de [Azure AD DS](../../active-directory-domain-services/overview.md) pueden conectarse mediante [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) de forma predeterminada a los nodos principal y perimetral. Estos usuarios no forman parte del grupo de sudoers y no obtienen acceso a raíz. El usuario de SSH que se crea durante la creación del clúster tendrá acceso a raíz.

## <a name="manage-access"></a>Administración del acceso

Para modificar el acceso mediante SSH a usuarios o grupos específicos, actualice `/etc/ssh/sshd_config` en cada uno de los nodos.

1. Use el [comando SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) para conectarse al clúster. Modifique el comando siguiente: reemplace CLUSTERNAME por el nombre del clúster y, luego, escriba el comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Abra el archivo `ssh_confi`.

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

1. Modifique el archivo `sshd_config` según sea necesario. Si restringe los usuarios a determinados grupos, las cuentas locales no podrán conectarse mediante SSH a ese nodo. Este es solo un ejemplo de sintaxis:

    ```bash
    AllowUsers useralias1 useralias2

    AllowGroups groupname1 groupname2
    ```

    Luego, guarde los cambios: **Ctrl + X**, **Y**, **Entrar**.

1. Reinicie SSHD.

    ```bash
    sudo systemctl restart sshd
    ```

1. Repita los pasos anteriores con cada nodo.

## <a name="ssh-authentication-log"></a>Registro de autenticación de SSH

El registro de autenticación de SSH se escribe en `/var/log/auth.log`. Si observa algún error de inicio de sesión con cuentas locales o de dominio mediante SSH, tendrá que examinar el registro para depurar los errores. Con frecuencia, el problema podría estar relacionado con cuentas de usuario específicas y, por lo general, es aconsejable probar otras cuentas de usuario o conectarse mediante SSH con el usuario SSH predeterminado (cuenta local) y, luego, intentar ejecutar kinit.

## <a name="ssh-debug-log"></a>Registro de depuración de SSH

Para habilitar el registro detallado, deberá reiniciar `sshd` con la opción `-d`. Al igual que con `/usr/sbin/sshd -d`, también puede ejecutar `sshd` en un puerto personalizado (por ejemplo, 2222) para que no tenga que detener el demonio SSH principal. También puede usar la opción `-v` con el cliente SSH para obtener más registros (vista del lado cliente de los errores).

## <a name="next-steps"></a>Pasos siguientes

* [Administración de clústeres de HDInsight con Enterprise Security Package](./apache-domain-joined-manage.md)
* [Conexión a HDInsight (Apache Hadoop) con SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).
