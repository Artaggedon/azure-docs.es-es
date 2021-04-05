---
title: Acceso a los registros de auditoría de Azure Database for MariaDB mediante la CLI de Azure
description: En este artículo se describe cómo configurar los registros de auditoría de Azure Database for MariaDB, y acceder a ellos, mediante la CLI de Azure.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: how-to
ms.date: 6/24/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: c1d446d8ee2863077ad84c361876758336f5a3cb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98665181"
---
# <a name="configure-and-access-azure-database-for-maria-db-audit-logs-in-the-azure-cli"></a>Configuración de los registros de auditoría de Azure Database for Maria DB y acceso a ellos en el CLI de Azure

Puede configurar los [registros de auditoría de Azure Database for MariaDB](concepts-audit-logs.md) desde la CLI de Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía:

- Necesita un [servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md).

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- En este artículo se necesita la versión 2.0 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="configure-audit-logging"></a>Configuración del registro de auditoría

>[!IMPORTANT]
> Se recomienda registrar solo los tipos de evento y los usuarios necesarios con fines de auditoría para asegurarse de que el rendimiento del servidor no se ve afectado en gran medida.

Habilite y configure el registro de auditoría mediante los pasos siguientes: 

1. Para activar los registros de auditoría, establezca el parámetro **audit_logs_enabled** en "ON". 
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_enabled --resource-group myresourcegroup --server mydemoserver --value ON
    ```

1. Seleccione los [tipos de eventos](concepts-audit-logs.md#configure-audit-logging) que se registrarán actualizando el parámetro **audit_log_events**.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_events --resource-group myresourcegroup --server mydemoserver --value "ADMIN,CONNECTION"
    ```

1. Agregue a cualquier usuario de MariaDB que se excluya del registro actualizando el parámetro **audit_log_exclude_users**. Especifique los usuarios proporcionando su nombre de usuario de MariaDB.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_exclude_users --resource-group myresourcegroup --server mydemoserver --value "azure_superuser"
    ```

1. Agregue los usuarios de MariaDB específicos que se van a incluir para el registro mediante la actualización del parámetro **audit_log_include_users**. Especifique los usuarios proporcionando su nombre de usuario de MariaDB.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_include_users --resource-group myresourcegroup --server mydemoserver --value "sampleuser"
    ```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre los [registros de auditoría](concepts-audit-logs.md) en Azure Database for MariaDB.
- Aprenda a configurar los registros de auditoría en [Azure Portal](howto-configure-audit-logs-portal.md)
