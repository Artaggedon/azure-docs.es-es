---
title: Visualización de asignaciones de roles personalizados en el portal de Azure Active Directory | Microsoft Docs
description: Ahora puede ver y administrar los miembros de un rol de administrador de Azure Active Directory en el centro de administración de Azure Active Directory.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 11/05/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c13167706c2794253108b04eaa2dcc46e0e6c09a
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98740693"
---
# <a name="view-custom-role-assignments-using-azure-active-directory"></a>Visualización de asignaciones de roles personalizados mediante Azure Active Directory

En este artículo se describe cómo ver los roles personalizados que ha asignado en Azure Active Directory (Azure AD). En Azure Active Directory (Azure AD), los roles se pueden asignar a un ámbito de toda la organización o con un ámbito de aplicación única.

- Las asignaciones de roles en el ámbito de toda la organización se agregan y pueden verse en la lista de asignaciones de roles de aplicación únicas.
- Las asignaciones de roles en el ámbito de aplicación única no se agregan y no se pueden ver en la lista de asignaciones con un ámbito de toda la organización.

## <a name="view-role-assignments-in-the-azure-portal"></a>Visualización de asignaciones de roles en Azure Portal

En este procedimiento se describe cómo ver las asignaciones de un rol con ámbito de toda la organización.

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com) con permisos de administrador de roles con privilegios o de administrador global en la organización de Azure AD.
1. Seleccione **Azure Active Directory**, elija **Roles y administradores** y, luego, seleccione un rol para abrirlo y ver sus propiedades.
1. Seleccione **Asignaciones** para ver las asignaciones del rol.

    ![Visualización de asignaciones de roles y permisos al abrir un rol de la lista](./media/view-assignments/role-assignments.png)

## <a name="view-role-assignments-using-azure-ad-powershell"></a>Visualización de las asignaciones de roles con Azure AD PowerShell

En esta sección se describe cómo ver las asignaciones de un rol con ámbito de toda la organización. Este artículo se usa el módulo [Azure Active Directory PowerShell, versión 2](/powershell/module/azuread/#directory_roles). Para ver las asignaciones con un ámbito de aplicación única con PowerShell, puede usar los cmdlets de [Asignación de roles personalizados con PowerShell](custom-assign-powershell.md).

### <a name="prepare-powershell"></a>Preparación para PowerShell

En primer lugar, debe [descargar el módulo de Azure AD PowerShell en versión preliminar](https://www.powershellgallery.com/packages/AzureAD/).

Para instalar el módulo de Azure AD PowerShell, use los siguientes comandos:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Para comprobar que el módulo esté listo para usar, ejecute el siguiente comando:

``` PowerShell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

### <a name="view-the-assignments-of-a-role"></a>Ver las asignaciones de un rol

Ejemplo de visualización de las asignaciones de un rol.

``` PowerShell
# Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole

# Fetch a specific directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Fetch role membership for a role
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="view-role-assignments-using-microsoft-graph-api"></a>Visualización de las asignaciones de roles mediante Microsoft Graph API

En esta sección se describe cómo ver las asignaciones de un rol con ámbito de toda la organización.  Para ver las asignaciones con un ámbito de aplicación única mediante Graph API, puede usar las operaciones de [Asignación de roles personalizados con Graph API](custom-assign-graph.md).

La solicitud HTTP para obtener una asignación de roles para una definición de roles determinada.

GET

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

Response

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

## <a name="view-assignments-of-single-application-scope"></a>Visualización de las asignaciones con un ámbito de aplicación única

En esta sección se describe cómo ver las asignaciones de un rol con un ámbito de aplicación única. Esta característica actualmente está en su versión preliminar pública.

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com) con permisos de administrador de roles con privilegios o de administrador global en la organización de Azure AD.
1. Seleccione **Registros de aplicaciones** y, luego, seleccione el registro de aplicación para ver sus propiedades. Es posible que tenga que seleccionar **Todas las aplicaciones** para ver la lista completa de los registros de aplicaciones de la organización de Azure AD.

    ![Creación o edición de registros de aplicaciones desde la página Registros de aplicaciones](./media/view-assignments/app-reg-all-apps.png)

1. En el registro de la aplicación, seleccione **Roles y administradores** y después elija un rol para ver sus propiedades.

    ![Visualización de las asignaciones de roles de registro de aplicaciones desde la página Registros de aplicaciones](./media/view-assignments/app-reg-assignments.png)

1. Seleccione **Asignaciones** para ver las asignaciones del rol. Al abrir la vista de asignaciones desde el registro de la aplicación, se muestran las asignaciones que se limitan a este recurso de Azure AD.

    ![Visualización de las asignaciones de roles de registro de aplicaciones desde las propiedades de un registro de aplicaciones](./media/view-assignments/app-reg-assignments-2.png)

## <a name="next-steps"></a>Pasos siguientes

* No dude en compartir con nosotros en el [foro de roles administrativos de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Para obtener más información sobre los roles y la asignación de roles de administrador, consulte el artículo [Assign administrator roles](permissions-reference.md) (Asignación de roles de administrador).
* Para conocer los permisos predeterminados de usuario, vea una [comparación de los permisos predeterminados de usuario miembro e invitado](../fundamentals/users-default-permissions.md).
