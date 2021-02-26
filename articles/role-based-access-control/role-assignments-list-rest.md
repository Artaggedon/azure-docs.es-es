---
title: 'Enumeración de asignaciones de roles de Azure mediante la API REST: RBAC de Azure'
description: Aprenda a determinar a qué recursos tienen acceso los usuarios, grupos, entidades de servicio e identidades administradas mediante la API REST y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: how-to
ms.date: 05/06/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 1d90e83405cf9c560f644277a04012571e1a5a9d
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100556903"
---
# <a name="list-azure-role-assignments-using-the-rest-api"></a>Enumeración de asignaciones de roles de Azure mediante la API REST

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control/definition-list.md)] En este artículo se describe cómo enumerar las asignaciones de roles mediante la API REST.

> [!NOTE]
> Si su organización ha externalizado funciones de administración a un proveedor de servicios que usa la [administración de recursos delegados de Azure](../lighthouse/concepts/azure-delegated-resource-management.md), las asignaciones de roles autorizadas por ese proveedor de servicios no se mostrarán aquí.

## <a name="list-role-assignments"></a>Lista de asignaciones de roles

En RBAC de Azure, para enumerar el acceso se enumeran las asignaciones de roles. Para mostrar la lista de asignación de roles, use una de las API de REST de [Asignación de roles - Lista](/rest/api/authorization/roleassignments/list). Para mejorar los resultados, especifique un ámbito y un filtro opcional.

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de asignaciones de roles quiere obtener.

    > [!div class="mx-tableFixed"]
    > | Ámbito | Tipo |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Grupo de administración |
    > | `subscriptions/{subscriptionId1}` | Subscription |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Resource |

    En el ejemplo anterior, microsoft.web es un proveedor de recursos que hace referencia a una instancia de App Service. De forma similar, puede usar cualquier otro proveedor de recursos y especificar el ámbito. Para más información, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md) y [Operaciones del proveedor de recursos de Azure](resource-provider-operations.md) compatibles.  
     
1. Reemplace *{filter}* por la condición que quiere aplicar para filtrar la lista de asignación de roles.

    > [!div class="mx-tableFixed"]
    > | Filter | Descripción |
    > | --- | --- |
    > | `$filter=atScope()` | Lista de las asignaciones de roles únicamente para el ámbito especificado, sin incluir las asignaciones de roles en ámbitos secundarios. |
    > | `$filter=assignedTo('{objectId}')` | Lista de las asignaciones de roles para un usuario o una entidad de servicio determinados.<br/>Si el usuario es miembro de un grupo que tiene una asignación de roles, también se muestra esa asignación de roles. Este filtro es transitivo para los grupos, lo que significa que, si el usuario es miembro de un grupo y ese grupo es miembro de otro grupo que tiene una asignación de roles, también se muestra esa asignación de roles.<br/>Este filtro solo acepta un identificador de objeto para un usuario o una entidad de servicio. No se puede pasar un identificador de objeto para un grupo. |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | Muestra las asignaciones de roles para el usuario o la entidad de servicio determinados y en el ámbito especificado. |
    > | `$filter=principalId+eq+'{objectId}'` | Lista de las asignaciones de roles para un usuario, un grupo o una entidad de servicio determinados. |

La solicitud siguiente enumera todas las asignaciones de roles del usuario especificado en el ámbito de la suscripción:

```http
GET https://management.azure.com/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()+and+assignedTo('{objectId1}')
```

El siguiente texto muestra un ejemplo de la salida:

```json
{
    "value": [
        {
            "properties": {
                "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
                "principalId": "{objectId1}",
                "scope": "/subscriptions/{subscriptionId1}",
                "createdOn": "2019-01-15T21:08:45.4904312Z",
                "updatedOn": "2019-01-15T21:08:45.4904312Z",
                "createdBy": "{createdByObjectId1}",
                "updatedBy": "{updatedByObjectId1}"
            },
            "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId1}",
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "{roleAssignmentId1}"
        }
    ]
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de Azure mediante la API REST](role-assignments-rest.md)
- [Azure REST API Reference](/rest/api/azure/) (Referencia de API de REST en Azure)
