---
title: Delegación y roles en la administración de derechos - Azure AD
description: Aprenda a delegar la gobernanza del acceso de los administradores de TI en los administradores de departamentos y los jefes de proyecto para que ellos mismos puedan encargarse del acceso.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 12/23/2020
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 204d83b96e3cbe26759d678126d8826d0b2e492e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100577851"
---
# <a name="delegation-and-roles-in-azure-ad-entitlement-management"></a>Delegación y roles en la administración de derechos de Azure AD

De forma predeterminada, los administradores globales y los administradores de usuarios pueden crear y gestionar todos los aspectos de Administración de derechos de Azure AD. No obstante, es posible que los usuarios de estos roles no conozcan todas las situaciones en las que se requieren paquetes de acceso. Normalmente son los usuarios de los respectivos departamentos, equipos o proyectos los que saben con quién colaboran, los recursos que usan y durante cuánto tiempo lo hacen. En lugar de conceder permisos sin restricciones a los usuarios que no sean administradores, puede conceder a los usuarios los permisos mínimos que necesitan para realizar su trabajo, lo que evita la creación de derechos de acceso conflictivos o inadecuados.

En este vídeo se ofrece información general sobre el modo en que el administrador de TI delega la gobernanza del acceso en usuarios que no son administradores.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3Lq00]

## <a name="delegate-example"></a>Ejemplo de delegación

Para comprender cómo se puede delegar la gobernanza del acceso desde la administración de derechos, resulta útil plantearse un ejemplo. Supongamos que en su organización cuenta con el administrador y los responsables siguientes.

![Delegación del administrador de TI en los responsables](./media/entitlement-management-delegate/delegate-admin-dept-managers.png)

Al ser administrador de TI, Hana tiene contactos en todos los departamentos (Mamta en el de marketing, Mark en el financiero y Joe en el legal) que son los responsables de los recursos de su departamento y del contenido crítico para la empresa.

Por medio de la administración de derechos se puede delegar la gobernanza del acceso a estos usuarios no administradores, ya que son los que saben qué usuarios necesitan acceso, durante cuánto tiempo y a qué recursos. Esta delegación garantiza que las personas adecuadas administran el acceso de sus departamentos.

Esta es una de las formas en las que Hana podría delegar la gobernanza del acceso en los departamentos de marketing, finanzas y legal.

1. Hana crea un nuevo grupo de seguridad de Azure AD y agrega a Mamta, Mark y Joe como miembros del grupo.

1. Hana agrega ese grupo al rol de creadores de catálogos.

    Ahora Mamta, Mark y Joe pueden crear catálogos para sus departamentos, agregar los recursos que sus departamentos necesitan y realizar más delegaciones en el catálogo. No pueden ver los catálogos de los demás.

1. Mamta crea un catálogo **Marketing**, que es un contenedor de recursos.

1. Mamta agrega a este catálogo los recursos que su departamento de marketing posee.

1. Mamta puede agregar otras personas de su departamento como propietarios en este catálogo, lo que ayuda a compartir las responsabilidades de administración del catálogo.

1. Mamta puede delegar aún más la creación y la administración de los paquetes de acceso del catálogo de marketing en los jefes de proyecto de su departamento. Para ello, les asigna el rol de administrador de paquetes de acceso. Este tipo de administrador puede crear y administrar paquetes de acceso. 

En el siguiente diagrama se muestran catálogos con recursos para los departamentos de marketing, finanzas y legal. Con estos catálogos, los jefes de proyecto pueden crear paquetes de acceso para sus equipos o proyectos.

![Ejemplo de delegación de administración de derechos](./media/entitlement-management-delegate/elm-delegate.png)

Después de la delegación, el departamento de marketing podría tener roles similares a los de la tabla siguiente.

| Usuario | Rol de trabajo | Rol de Azure AD | Rol de administración de derechos |
| --- | --- | --- | --- |
| Hana | Administrador de TI | administrador global o administrador de usuarios. |  |
| Mamta | Administrador de marketing | Usuario | Creador del catálogo y propietario del catálogo |
| Bob | Responsable de marketing | Usuario | Propietario del catálogo |
| Jessica | Jefe de proyecto de marketing | Usuario | Administrador de paquetes de acceso |

## <a name="entitlement-management-roles"></a>Roles de administración de derechos

La administración de derechos incluye los siguientes roles específicos de administración de derechos.

| Rol de administración de derechos | Descripción |
| --- | --- |
| Creador de catálogos | Crear y administrar catálogos. Normalmente, los administradores de TI que no son administradores globales ni propietarios de recursos en una colección de recursos. La persona que crea un catálogo se convierte automáticamente en el primer propietario del mismo y puede agregar otros propietarios. Un creador de catálogos no puede administrar o ver los catálogos que no son de su propiedad y no puede agregar recursos que no son de su propiedad a un catálogo. Si el creador del catálogo tiene que administrar otro catálogo o agregar recursos que no son de su propiedad, puede solicitar ser copropietario de ese catálogo o recurso. |
| Propietario del catálogo | Editar y administrar catálogos existentes. Normalmente, un administrador de TI, los propietarios de recursos o un usuario que el propietario del catálogo haya elegido. |
| Administrador de paquetes de acceso | Editar y administrar todos los paquetes de acceso existentes en un catálogo. |
| Administrador de asignaciones de paquetes de acceso | Editar y administrar todas las asignaciones de paquetes de acceso existentes. |

Además, el aprobador elegido y un solicitante de un paquete de acceso tienen derechos, aunque no son roles.

| Right | Descripción |
| --- | --- |
| Aprobador | Usuario autorizado por una directiva para aprobar o rechazar solicitudes de acceso a paquetes, aunque no puede cambiar las definiciones de paquetes de acceso. |
| Solicitante | Usuario autorizado por una directiva de un paquete de acceso para solicitar ese paquete de acceso. |

En la siguiente tabla se enumeran las tareas que pueden hacer los roles de administración de derechos.

| Tarea | Administración | Creador de catálogos | Propietario del catálogo | Administrador de paquetes de acceso | Administrador de asignaciones de paquetes de acceso |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Delegación en un creador de catálogo](entitlement-management-delegate-catalog.md) | :heavy_check_mark: |  |  |  |  |
| [Adición de una organización conectada](entitlement-management-organization.md) | :heavy_check_mark: |  |  |  |  |
| [Creación de un catálogo](entitlement-management-catalog-create.md) | :heavy_check_mark: | :heavy_check_mark: |  |  |  |
| [Adición de un recurso a un catálogo](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Adición de un propietario de catálogo](entitlement-management-catalog-create.md#add-additional-catalog-owners) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Edición de un catálogo](entitlement-management-catalog-create.md#edit-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Eliminación de un catálogo](entitlement-management-catalog-create.md#delete-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Delegación en un administrador de paquetes de acceso](entitlement-management-delegate-managers.md) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Eliminación de un administrador de paquetes de acceso](entitlement-management-delegate-managers.md#remove-an-access-package-manager) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Creación de un paquete de acceso en un catálogo](entitlement-management-access-package-create.md) | :heavy_check_mark: |  | :heavy_check_mark:  | :heavy_check_mark:  |  |
| [Cambio de roles de recurso en un paquete de acceso](entitlement-management-access-package-resources.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Creación y edición de directivas](entitlement-management-access-package-request-policy.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Asignación directa de un usuario a un paquete de acceso](entitlement-management-access-package-assignments.md#directly-assign-a-user) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [Quitar directamente un usuario de un paquete de acceso](entitlement-management-access-package-assignments.md#remove-an-assignment) | :heavy_check_mark:  |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Visualización de quién tiene una asignación en un paquete de acceso](entitlement-management-access-package-assignments.md#view-who-has-an-assignment) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [Visualización de las solicitudes de un paquete de acceso](entitlement-management-access-package-requests.md#view-requests) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  |
| [Visualización de errores de entrega de una solicitud](entitlement-management-troubleshoot.md#view-a-requests-delivery-errors) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  :heavy_check_mark: |
| [Nuevo procesamiento de una solicitud](entitlement-management-troubleshoot.md#reprocess-a-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  |
| [Cancelación de una solicitud pendiente](entitlement-management-troubleshoot.md#cancel-a-pending-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Ocultación de un paquete de acceso](entitlement-management-access-package-edit.md#change-the-hidden-setting) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Eliminación de un paquete de acceso](entitlement-management-access-package-edit.md#delete-an-access-package) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Roles necesarios para agregar recursos a un catálogo

Un administrador de empresa puede agregar o quitar cualquier grupo (grupos de seguridad creados en la nube o grupos de Microsoft 365 creados en la nube), aplicación o sitio de SharePoint Online de un catálogo. Un administrador de usuarios puede agregar o quitar cualquier grupo o aplicación de un catálogo, excepto un grupo configurado como asignable a un rol de directorio. Tenga en cuenta que un administrador de usuarios puede administrar los paquetes de acceso en un catálogo que incluya grupos configurados como asignables a un rol de directorio.  Para más información sobre los grupos no asignables, consulte [Creación de un grupo al que se pueden asignar roles en Azure Active Directory](../roles/groups-create-eligible.md).

Para que un usuario que no sea administrador global ni administrador de usuarios pueda agregar grupos, aplicaciones o sitios de SharePoint Online a un catálogo, ese usuario debe tener *tanto* el rol de directorio de Azure AD como el rol de administración de derechos de propietario del catálogo que se requieren. En la tabla siguiente se muestran las combinaciones de roles necesarios para agregar recursos a un catálogo. Para quitar recursos de un catálogo, hay que tener los mismos roles.

| Rol de directorio de Azure AD | Rol de administración de derechos | Puede agregar grupos de seguridad | Puede agregar grupos de Microsoft 365 | Puede agregar aplicaciones | Puede agregar sitios de SharePoint Online |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Administrador global](../roles/permissions-reference.md) | N/D |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Administrador de usuarios](../roles/permissions-reference.md) | N/D |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Administrador de Intune](../roles/permissions-reference.md) | Propietario del catálogo | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Administrador de Exchange](../roles/permissions-reference.md) | Propietario del catálogo |  | :heavy_check_mark: |  |  |
| [Administrador del servicio Teams](../roles/permissions-reference.md) | Propietario del catálogo |  | :heavy_check_mark: |  |  |
| [Administrador de SharePoint](../roles/permissions-reference.md) | Propietario del catálogo |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [Administrador de aplicaciones](../roles/permissions-reference.md) | Propietario del catálogo |  |  | :heavy_check_mark: |  |
| [Administrador de aplicaciones en la nube](../roles/permissions-reference.md) | Propietario del catálogo |  |  | :heavy_check_mark: |  |
| Usuario | Propietario del catálogo | Solo si es propietario del grupo | Solo si es propietario del grupo | Solo si es propietario de la aplicación |  |

Para determinar el rol con menos privilegios de una tarea, también puede hacer referencia a los [roles de administrador por tarea de administrador en Azure Active Directory](../roles/delegate-by-task.md#entitlement-management).

## <a name="next-steps"></a>Pasos siguientes

- [Delegación de la gobernanza del acceso en los creadores de catálogos](entitlement-management-delegate-catalog.md)
- [Creación y administración de un catálogo de recursos](entitlement-management-catalog-create.md)
