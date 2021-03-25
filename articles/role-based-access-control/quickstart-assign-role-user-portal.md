---
title: 'Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante Azure Portal: Azure RBAC'
description: En este tutorial, aprenderá a conceder acceso de usuario a los recursos de Azure mediante Azure Portal y el control de acceso basado en rol (Azure RBAC).
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/22/2019
ms.author: rolyon
ms.openlocfilehash: fcba9cad208c2ac170f91cc06a6db22e271f2a70
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100559324"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-the-azure-portal"></a>Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante Azure Portal

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en la que se administra el acceso a los recursos de Azure. En este tutorial, se concede acceso a un usuario para crear y administrar máquinas virtuales en un grupo de recursos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conceder acceso a un usuario en el ámbito del grupo de recursos
> * Quitar acceso

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

1. En la lista de navegación, haga clic en **Grupos de recursos**.

1. Haga clic en **Agregar** para abrir la hoja **Grupo de recursos**.

   ![Adición de un grupo de recursos nuevo](./media/quickstart-assign-role-user-portal/resource-group.png)

1. En **Nombre del grupo de recursos**, escriba **rbac-resource-group**.

1. Seleccione una suscripción y una ubicación.

1. Haga clic en **Crear** para crear el grupo de recursos.

1. Haga clic en **Actualizar** para actualizar la lista de grupos de recursos.

   El grupo de recursos nuevo aparece en la lista de grupos de recursos.

   ![Lista de grupos de recursos](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Conceder acceso

En RBAC de Azure, es preciso asignar un rol de Azure para conceder acceso.

1. En la lista de **Grupos de recursos**, haga clic en el nuevo grupo de recursos **rbac-resource-group**.

1. Haga clic en **Control de acceso (IAM).**

1. Haga clic en la pestaña **Asignaciones de roles** para ver la lista actual de asignaciones de roles.

   ![Hoja Control de acceso (IAM) para un grupo de recursos](./media/quickstart-assign-role-user-portal/access-control.png)

1. Haga clic en **Agregar** > **Agregar asignación de roles** para abrir el panel Agregar asignación de roles.

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú Agregar asignación de roles](./media/shared/add-role-assignment-menu.png)

    Se abre el panel Agregar asignación de roles.

   ![Panel Agregar asignación de roles](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. En la lista desplegable **Rol**, seleccione **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, se puede seleccionar a usted mismo o a otro usuario.

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol Colaborador de la máquina virtual al usuario en el ámbito del grupo de recursos rbac-resource-group.

   ![Asignación del rol Colaborador de la máquina virtual](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Quitar acceso

En Azure RBAC, para quitar el acceso hay que quitar una asignación de roles.

1. En la lista de asignaciones de roles, agregue una marca de verificación junto al usuario con el rol Colaborador de la máquina virtual.

1. Haga clic en **Quitar**.

   ![Mensaje de eliminación de asignación de roles](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. En el mensaje de eliminación de asignación de roles que aparece, haga clic en **Sí**.

## <a name="clean-up"></a>Limpieza

1. En la lista de navegación, haga clic en **Grupos de recursos**.

1. Haga clic en **rbac-resource-group** para abrir el grupo de recursos.

1. Haga clic en **Eliminar grupo de recursos** para eliminar el grupo de recursos.

   ![Eliminación de un grupo de recursos](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. En la hoja **¿Está seguro de que desea eliminar?** , escriba el nombre del grupo de recursos: **rbac-resource-group**.

1. Haga clic en **Eliminar** para eliminar el grupo de recursos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso de usuario a recursos de Azure mediante Azure PowerShell](tutorial-role-assignments-user-powershell.md)
