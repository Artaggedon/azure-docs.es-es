---
title: Sincronización de los usuarios de Azure Active Directory en el clúster de HDInsight
description: Sincronice los usuarios autenticados de Azure Active Directory en un clúster de HDInsight.
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 11/21/2019
ms.openlocfilehash: 2a753a33e9ddf16cc277ab10c1f91049a6382066
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104871952"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Sincronización de los usuarios de Azure Active Directory en un clúster de HDInsight

[Los clústeres de HDInsight con Enterprise Security Package (ESP)](./domain-joined/hdinsight-security-overview.md) pueden usar una autenticación sólida con los usuarios de Azure Active Directory (Azure AD) y las directivas de *control de acceso basado en roles* (RBAC de Azure). A medida que se agregan usuarios y grupos a Azure AD, se pueden sincronizar los usuarios que necesitan tener acceso al clúster.

## <a name="prerequisites"></a>Prerequisites

Si aún no lo ha hecho, [cree un clúster de HDInsight con Enterprise Security Package](./domain-joined/apache-domain-joined-configure-using-azure-adds.md).

## <a name="add-new-azure-ad-users"></a>Incorporación de nuevos usuarios de Azure AD

Para ver los hosts, abra la interfaz de usuario web de Ambari. Cada nodo se actualizará con la nueva configuración de actualización desatendida.

1. En [Azure Portal](https://portal.azure.com), vaya al directorio de Azure AD asociado al clúster de ESP.

2. Seleccione **Todos los usuarios** en el menú de la izquierda y, a continuación, seleccione **Nuevo usuario**.

    :::image type="content" source="./media/hdinsight-sync-aad-users-to-cluster/users-and-groups-new.png" alt-text="Todos los usuarios y grupos de Azure Portal":::

3. Rellene el formulario del nuevo usuario. Seleccione los grupos que creó para asignar permisos basados en el clúster. En este ejemplo, cree un grupo denominado "HiveUsers", al que puede asignar los nuevos usuarios. Las [instrucciones de ejemplo](./domain-joined/apache-domain-joined-configure-using-azure-adds.md) para crear un clúster de ESP incluyen la adición de dos grupos: `HiveUsers` y `AAD DC Administrators`.

    :::image type="content" source="./media/hdinsight-sync-aad-users-to-cluster/hdinsight-new-user-form.png" alt-text="Selección de grupos en el panel de usuario de Azure Portal":::

4. Seleccione **Crear**.

## <a name="use-the-apache-ambari-rest-api-to-synchronize-users"></a>Uso de la API REST de Apache Ambari para sincronizar los usuarios

Los grupos de usuarios especificados durante el proceso de creación del clúster se sincronizan en ese momento. La sincronización de usuario se produce automáticamente una vez cada hora. Para sincronizar los usuarios inmediatamente o para sincronizar un grupo distinto de los especificados durante la creación del clúster, use la API de REST de Ambari.

El método siguiente utiliza POST con la API de REST de Ambari. Para obtener más información, consulte [Administración de clústeres de HDInsight con la API REST de Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

1. Use el [comando SSH](hdinsight-hadoop-linux-use-ssh-unix.md) para conectarse al clúster. Modifique el comando siguiente: reemplace `CLUSTERNAME` por el nombre del clúster y, luego, escriba el comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Tras la autenticación, escriba el siguiente comando:

    ```bash
    curl -u admin:PASSWORD -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://CLUSTERNAME.azurehdinsight.net/api/v1/ldap_sync_events"
    ```

    La respuesta debería tener este aspecto:

    ```json
    {
      "resources" : [
        {
          "href" : "http://<ACTIVE-HEADNODE-NAME>.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

1. Para ver el estado de sincronización, ejecute un nuevo comando `curl`:

    ```bash
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/ldap_sync_events/1
    ```

    La respuesta debería tener este aspecto:

    ```json
    {
      "href" : "http://<ACTIVE-HEADNODE-NAME>.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
      "Event" : {
        "id" : 1,
        "specs" : [
          {
            "sync_type" : "existing",
            "principal_type" : "groups"
          }
        ],
        "status" : "COMPLETE",
        "status_detail" : "Completed LDAP sync.",
        "summary" : {
          "groups" : {
            "created" : 0,
            "removed" : 0,
            "updated" : 0
          },
          "memberships" : {
            "created" : 1,
            "removed" : 0
          },
          "users" : {
            "created" : 1,
            "removed" : 0,
            "skipped" : 0,
            "updated" : 0
          }
        },
        "sync_time" : {
          "end" : 1497994072182,
          "start" : 1497994071100
        }
      }
    }
    ```

1. Este resultado muestra que el estado es **COMPLETO**, una vez que se ha creado un nuevo usuario y se le ha asignado una pertenencia. En este ejemplo, el usuario se asigna al grupo LDAP sincronizado "HiveUsers", puesto que se agregó a ese mismo grupo en Azure AD.

    > [!NOTE]  
    > El método anterior solo sincroniza los grupos de Azure AD especificados en la propiedad **Access user group** (Grupo de usuarios con acceso) de la configuración del dominio durante la creación del clúster. Para más información, consulte [Create an HDInsight cluster](./domain-joined/apache-domain-joined-configure-using-azure-adds.md) (Creación de un clúster de HDInsight).

## <a name="verify-the-newly-added-azure-ad-user"></a>Verificación del usuario de Azure AD recién agregado

Abra la [interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md) para verificar que el nuevo usuario de Azure AD se agregó. Acceda a la interfaz de usuario web de Ambari en **`https://CLUSTERNAME.azurehdinsight.net`** . Escriba el nombre de usuario y la contraseña del administrador del clúster.

1. En el panel de Ambari, seleccione **Manage Ambari** (Administrar Ambari) en el menú **admin** (Administrador).

    :::image type="content" source="./media/hdinsight-sync-aad-users-to-cluster/manage-apache-ambari.png" alt-text="Administración de Ambari en el panel de Apache Ambari":::

2. Seleccione **Usuarios** en el grupo de menús **Usuario + Administración del grupo**, en el lado izquierdo de la página.

    :::image type="content" source="./media/hdinsight-sync-aad-users-to-cluster/hdinsight-users-menu-item.png" alt-text="Menú de usuarios y grupos de HDInsight":::

3. El nuevo usuario debe aparecer dentro de la tabla de usuarios. El tipo está establecido en `LDAP` en lugar de en `Local`.

    :::image type="content" source="./media/hdinsight-sync-aad-users-to-cluster/hdinsight-users-page.png" alt-text="Información general de la página de usuarios de AAD de HDInsight":::

## <a name="log-in-to-ambari-as-the-new-user"></a>Inicio de sesión en Ambari como el nuevo usuario

Cuando el nuevo usuario (o cualquier otro usuario del dominio) inicia sesión en Ambari, se usan sus credenciales del dominio y el nombre de usuario completo de Azure AD.  Ambari muestra un alias de usuario, que es el nombre para mostrar del usuario en Azure AD.
El nuevo usuario de ejemplo tiene el nombre de usuario `hiveuser3@contoso.com`. En Ambari, este nuevo usuario aparece como `hiveuser3`, pero el usuario inicia sesión en Ambari como `hiveuser3@contoso.com`.

## <a name="see-also"></a>Consulte también

* [Configuración de directivas de Apache Hive en HDInsight con ESP](./domain-joined/apache-domain-joined-run-hive.md)
* [Administración de clústeres de HDInsight con Enterprise Security Package](./domain-joined/apache-domain-joined-manage.md)
* [Autorización de usuarios en Apache Ambari](hdinsight-authorize-users-to-ambari.md)