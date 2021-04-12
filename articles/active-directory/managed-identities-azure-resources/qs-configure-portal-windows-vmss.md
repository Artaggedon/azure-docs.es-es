---
title: 'Configuración de identidades administradas en un conjunto de escalado de máquinas virtuales: Azure AD'
description: Instrucciones detalladas para configurar identidades administradas de recursos de Azure en un conjunto de escalado de máquinas virtuales mediante Azure Portal.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73c2f4167d6ce5465a879f6b2ef7e96d91065c4d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97590928"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-vmss-using-the-azure-portal"></a>Configuración de identidades administradas de recursos de Azure en un conjunto de escalado de máquinas virtuales mediante Azure Portal

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, mediante PowerShell, aprenderá a realizar las operaciones de identidades administradas de recursos de Azure en un conjunto de escalado de máquinas virtuales:

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles de Azure:

    > [!NOTE]
    > No se requieren asignaciones de roles del directorio de Azure AD.

    - [Colaborador de máquina virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) para habilitar y quitar la identidad administrada asignada por el sistema desde un conjunto de escalado de máquinas virtuales.

## <a name="system-assigned-managed-identity"></a>Identidad administrada asignada por el sistema

En esta sección, aprenderá a habilitar y deshabilitar la identidad administrada asignada por el sistema mediante Azure Portal.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-virtual-machine-scale-set"></a>Habilitación de la identidad administrada asignada por el sistema durante la creación de un conjunto de escalado de máquinas virtuales

En la actualidad Azure Portal no permite habilitar una identidad administrada asignada por el sistema durante la creación de un conjunto de escalado de máquinas virtuales. En su lugar, consulte la siguiente guía de inicio rápido sobre la creación de conjuntos de escalado de máquinas virtuales para crear uno primero y, después, continúe a la siguiente sección para obtener detalles sobre la habilitación de la identidad administrada asignada por el sistema en un conjunto de escalado de máquinas virtuales:

- [Creación de un conjunto de escalado de máquinas virtuales en Azure Portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-managed-identity-on-an-existing-virtual-machine-scale-set"></a>Habilitación de la identidad administrada asignada por el sistema en un conjunto de escalado de máquinas virtuales existente

Para habilitar la identidad administrada asignada por el sistema en un conjunto de escalado de máquinas virtuales que se aprovisionó originalmente sin ella, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene el conjunto de escalado de máquinas virtuales.

2. Vaya al conjunto de escalado de máquinas virtuales de Azure que desee.

3. En **Asignado por el sistema**, en **Estado**, seleccione **Activado** y haga clic en **Guardar**:

   ![Captura de pantalla que muestra la página "Identidad (versión preliminar)" con la opción "Asignada por el sistema" seleccionada, el estado "Activado" y el botón "Guardar" resaltado.](./media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png) 

### <a name="remove-system-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Eliminación de una identidad asignada por el sistema desde un conjunto de escalado de máquinas virtuales

Si tiene un conjunto de escalado de máquinas virtuales que ya no necesita una identidad administrada asignada por el sistema, haga lo siguiente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene el conjunto de escalado de máquinas virtuales. Asegúrese también de que la cuenta pertenece a un rol que le concede permisos de escritura en el conjunto de escalado de máquinas virtuales.

2. Vaya al conjunto de escalado de máquinas virtuales de Azure que desee.

3. En **Asignado por el sistema**, en **Estado**, seleccione **Desactivado** y haga clic en **Guardar**:

   ![Captura de pantalla de la página de configuración](./media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-managed-identity"></a>Identidad administrada asignada por el usuario

En esta sección aprenderá a agregar una identidad administrada asignada por el usuario a un conjunto de escalado de máquinas virtuales y a quitarla mediante Azure Portal.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Asignación de una identidad administrada asignada por el usuario durante la creación de un conjunto de escalado de máquinas virtuales

En la actualidad Azure Portal no permite las identidades administradas asignadas por el usuario durante la creación de un conjunto de escalado de máquinas virtuales. En su lugar, consulte la siguiente guía de inicio rápido sobre la creación de conjuntos de escalado de máquinas virtuales para crear uno primero y, después, continúe a la siguiente sección para detalles sobre cómo el usuario puede asignarle una identidad administrada:

- [Creación de un conjunto de escalado de máquinas virtuales en Azure Portal](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-virtual-machine-scale-set"></a>Asignación de una identidad administrada asignada por el usuario a un conjunto de escalado de máquinas virtuales existente

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene el conjunto de escalado de máquinas virtuales.
2. Vaya al conjunto de escalado de máquinas virtuales que desee y haga clic en **Identidad**, **Asignado por el usuario** y en **\+Agregar**.

   ![Captura de pantalla que muestra la página "Identidad" con la opción "Asignada por el usuario" seleccionada y el botón "Agregar" resaltado.](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Haga clic en la identidad asignada por el usuario que desea agregar al conjunto de escalado de máquinas virtuales y, luego, en **Agregar**.
   
   ![Incorporación de una identidad asignada por el usuario a un VMSS](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Eliminación de una identidad administrada asignada por el usuario de un conjunto de escalado de máquinas virtuales

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual.
2. Vaya al conjunto de escalado de máquinas virtuales que desee y haga clic en **Identidad**, en **Asignado por el usuario**, en el nombre de la identidad administrada asignada por el usuario que desea eliminar y en **Quitar** (haga clic en **Sí** en el panel de confirmación).

   ![Eliminación de una identidad asignada por el usuario de un VMSS](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="next-steps"></a>Pasos siguientes

- Desde Azure Portal, asigne a una identidad administrada de un conjunto de escalado de máquinas virtuales de Azure [acceso a otro recurso de Azure](howto-assign-access-portal.md).
