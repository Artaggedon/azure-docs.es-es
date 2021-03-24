---
title: Eliminación de un grupo con Azure Active Directory | Microsoft Docs
description: Aprenda a eliminar un grupo con Azure Active Directory.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 08/29/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9aed5f282ff7b5d573a6f8511f2fc4dbfd27135d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92371824"
---
# <a name="delete-a-group-using-azure-active-directory"></a>Eliminación de un grupo con Azure Active Directory
Puede eliminar un grupo de Azure Active Directory (Azure AD) por infinidad de motivos, pero normalmente será porque:

- Ha establecido incorrectamente el **Tipo de grupo** en la opción incorrecta.

- Ha creado un grupo incorrecto o duplicado por error. 

- Ya no necesita el grupo.

## <a name="to-delete-a-group"></a>Para eliminar un grupo
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador global para el directorio.

2. Seleccione **Azure Active Directory** y después seleccione **Grupos**.

3. En la página **Grupos - Todos los grupos**, busque y seleccione el grupo que quiere eliminar. Para estos pasos, vamos a usar **MDM policy - East**.

    ![Página Grupos - Todos los grupos, con el nombre del grupo resaltado](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. En la página **Información general de MDM policy - East**, seleccione **Eliminar**.

    Se elimina el grupo del inquilino de Azure Active Directory.

    ![Página Información general de MDM policy - East, opción Eliminar resaltada](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Pasos siguientes

- Si elimina por error un grupo, puede crearlo de nuevo. Para más información, consulte [Cómo crear un grupo básico y agregar miembros](active-directory-groups-create-azure-portal.md).

- Si elimina un grupo de Microsoft 365 por error, puede restaurarlo. Para más información, consulte [Restauración de un grupo eliminado de Office 365](../enterprise-users/groups-restore-deleted.md).
