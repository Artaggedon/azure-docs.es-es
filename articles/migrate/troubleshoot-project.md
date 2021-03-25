---
title: Solución de problemas de proyectos de Azure Migrate
description: Ayuda a solucionar problemas relacionados con la creación y administración de proyectos de Azure Migrate.
author: ms-psharma
ms.author: panshar
ms.manager: abhemraj
ms.topic: troubleshooting
ms.date: 01/01/2020
ms.openlocfilehash: f68a57d3780f388488d48835f322ff04ab7c7187
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96753389"
---
# <a name="troubleshoot-azure-migrate-projects"></a>Solución de problemas de proyectos de Azure Migrate

Este artículo incluye ayuda para solucionar problemas relacionados con la creación y administración de proyectos de [Azure Migrate](migrate-services-overview.md).

## <a name="how-to-add-new-project"></a>¿Cómo se agrega un proyecto nuevo?

Puede tener varios proyectos de Azure Migrate en una suscripción. [Obtenga información](./create-manage-projects.md) acerca de cómo crear un proyecto por primera vez o cómo [agregar proyectos adicionales](create-manage-projects.md#create-additional-projects).

## <a name="what-azure-permissions-are-needed"></a>¿Qué permisos de Azure son necesarios?

Necesita permisos de colaborador o propietario en la suscripción para crear un proyecto de Azure Migrate.

## <a name="cant-find-a-project"></a>No se encuentra un proyecto

La búsqueda de un proyecto de Azure Migrate existente depende de si utiliza la versión actual o una versión anterior de Azure Migrate. [Seguir](create-manage-projects.md#find-a-project).


## <a name="cant-find-a-geography"></a>No se encuentra una geografía

Puede crear un proyecto de Azure Migrate en las geografías admitidas en las [nubes pública](migrate-support-matrix.md#supported-geographies-public-cloud) y [del gobierno](migrate-support-matrix.md#supported-geographies-azure-government).

## <a name="what-are-vm-limits"></a>¿Cuáles son los límites de VM?

Puede evaluar hasta 35 000 VM de VMware o hasta 35 000 VM de Hyper-V en un único proyecto. Un proyecto puede incluir máquinas virtuales de VMware y máquinas virtuales de Hyper-V, hasta los límites de evaluación.

## <a name="can-i-upgrade-old-project"></a>¿Puedo actualizar un proyecto antiguo?

No se pueden actualizar los proyectos de la versión anterior de Azure Migrate. Debe [crear un proyecto nuevo](./create-manage-projects.md) y agregarle herramientas.

## <a name="cant-create-a-project"></a>No se puede crear un proyecto

Si intenta crear un proyecto y detecta un error de implementación:

- Intente crear el proyecto de nuevo si se trata de un error transitorio. En **Implementaciones**, haga clic en **Volver a implementar** para intentarlo de nuevo.
- Compruebe que tiene permisos de colaborador o propietario en la suscripción.
- Si realiza la implementación en una geografía recién agregada, espere un poco y vuelva a intentarlo.
- Si recibe el error "Las solicitudes deben incluir encabezados de identidad de usuario", es posible que no tenga acceso al inquilino de Azure Active Directory (Azure AD) de la organización. En este caso:
    - Cuando agrega un usuario a un inquilino de Azure AD por primera vez, recibe una invitación por correo electrónico para unirse al inquilino.
    - Acepte la invitación para agregarse correctamente al inquilino.
    - Si no puede ver el correo electrónico, póngase en contacto con un usuario con acceso al inquilino y pídale que le [vuelva a enviar la invitación](../active-directory/external-identities/add-users-administrator.md#resend-invitations-to-guest-users).
    - Cuando reciba el correo electrónico de invitación, ábralo y seleccione el vínculo para aceptar la invitación. A continuación, cierre la sesión en Azure Portal e inicie sesión de nuevo. (La actualización del explorador no funcionará). A continuación, puede empezar a crear el proyecto de migración.

## <a name="how-do-i-delete-a-project"></a>¿Cómo puedo eliminar un proyecto?

[Siga estas instrucciones](create-manage-projects.md#delete-a-project) para eliminar un proyecto. Tenga en cuenta que, cuando se elimina un proyecto, se eliminan el proyecto y los metadatos sobre las máquinas detectadas en el proyecto.

## <a name="added-tools-dont-show"></a>Las herramientas agregadas no se muestran

Asegúrese de que tiene el proyecto correcto seleccionado. En el centro de Azure Migrate > **Servidores** o en **Bases de datos**, haga clic en **Cambiar** junto a **Migrate project (Change)** [Proyecto de migración (cambiar)] en la esquina superior derecha de la pantalla. Elija el nombre de la suscripción y del proyecto correctos > **Aceptar**. La página se debería actualizar con las herramientas agregadas del proyecto seleccionado.

## <a name="next-steps"></a>Pasos siguientes

Agregue herramientas de [evaluación](how-to-assess.md) o [migración](how-to-migrate.md) a los proyectos de Azure Migrate.