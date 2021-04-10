---
title: 'Asignación de roles de Azure mediante la API REST: RBAC de Azure'
description: Aprenda a conceder acceso a recursos de Azure para usuarios, grupos, entidades de servicio e identidades administradas mediante la API REST y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: how-to
ms.date: 02/15/2021
ms.author: rolyon
ms.openlocfilehash: d012173adb5e238282e107b832ed9c6895237e48
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100556073"
---
# <a name="assign-azure-roles-using-the-rest-api"></a>Asignación de roles de Azure mediante la API REST

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control/definition-grant.md)] En este artículo se describe cómo asignar roles con la API REST.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="assign-an-azure-role"></a>Asignación de un rol de Azure

Para asignar un rol, use la API REST [Asignaciones de roles - Crear](/rest/api/authorization/roleassignments/create) y especifique la entidad de seguridad, la definición de roles y el ámbito. Para llamar a esta API, debe tener acceso a la operación `Microsoft.Authorization/roleAssignments/write`. Entre los roles integrados, solo se concede acceso a esta operación a [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator).

1. Use la API de REST [Definiciones de roles - Lista](/rest/api/authorization/roledefinitions/list) o consulte [Roles integrados](built-in-roles.md) para obtener el identificador de la definición de roles que quiere asignar.

1. Use una herramienta de GUID para generar un identificador único que se use para el identificador de asignación de roles. El identificador tiene el formato: `00000000-0000-0000-0000-000000000000`

1. Empiece con la solicitud y el cuerpo siguientes:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito de la asignación de roles.

    > [!div class="mx-tableFixed"]
    > | Ámbito | Tipo |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Grupo de administración |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Resource |

    En el ejemplo anterior, microsoft.web es un proveedor de recursos que hace referencia a una instancia de App Service. De forma similar, puede usar cualquier otro proveedor de recursos y especificar el ámbito. Para más información, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md) y [Operaciones del proveedor de recursos de Azure](resource-provider-operations.md) compatibles.  

1. Reemplace *{roleAssignmentId}* por el identificador GUID de la asignación de roles.

1. En el cuerpo de la solicitud, reemplace *{scope}* por el ámbito de la asignación de roles.

    > [!div class="mx-tableFixed"]
    > | Ámbito | Tipo |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Grupo de administración |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Resource |

1. Reemplace *{roleDefinitionId}* por el identificador de la definición de roles.

1. Reemplace *{principalId}* por el identificador de objeto del usuario, grupo o entidad de servicio al que se asignará el rol.

La siguiente solicitud y cuerpo asignan el rol [lector de copias de seguridad](built-in-roles.md#backup-reader) a un usuario en el ámbito de la suscripción:

```http
PUT https://management.azure.com/subscriptions/{subscriptionId1}/providers/microsoft.authorization/roleassignments/{roleAssignmentId1}?api-version=2015-07-01
```

```json
{
  "properties": {
    "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
    "principalId": "{objectId1}"
  }
}
```

El siguiente texto muestra un ejemplo de la salida:

```json
{
    "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
        "principalId": "{objectId1}",
        "scope": "/subscriptions/{subscriptionId1}",
        "createdOn": "2020-05-06T23:55:23.7679147Z",
        "updatedOn": "2020-05-06T23:55:23.7679147Z",
        "createdBy": null,
        "updatedBy": "{updatedByObjectId1}"
    },
    "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId1}",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "{roleAssignmentId1}"
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Enumeración de asignaciones de roles de Azure mediante la API REST](role-assignments-list-rest.md)
- [Implementación de recursos con las plantillas de Resource Manager y la API de REST de Resource Manager](../azure-resource-manager/templates/deploy-rest.md)
- [Azure REST API Reference](/rest/api/azure/) (Referencia de API de REST en Azure)
- [Creación o actualización de roles personalizados para los recursos de Azure con la API REST](custom-roles-rest.md)
