---
author: baanders
description: archivo de inclusión para el paso de permisos de acceso en la configuración de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: a905bb3b4effb0381facfbfaa37c8ea412b81287
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "100551949"
---
Azure Digital Twins usa [Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) para el control de acceso basado en roles (RBAC). Esto significa que antes de que un usuario pueda hacer llamadas al plano de datos a la instancia de Azure Digital Twins, se debe asignar un rol a ese usuario con los permisos adecuados.

En el caso de Azure Digital Twins, este rol es _**Propietario de datos de Azure Digital Twins**_. Puede leer más sobre los roles y la seguridad en [*Conceptos: Seguridad para las soluciones de Azure Digital Twins*](../articles/digital-twins/concepts-security.md).

En esta sección se muestra cómo crear una asignación de roles para un usuario en la instancia de Azure Digital Twins, mediante el correo electrónico del usuario en el inquilino de Azure AD de la suscripción de Azure. En función del rol de su organización, puede configurar este permiso para usted mismo o en nombre de otra persona que vaya a administrar la instancia de Azure Digital Twins.

### <a name="assign-the-role"></a>Asignación del rol

Para conceder a un usuario permisos para administrar una instancia de Azure Digital Twins, debe asignarle al rol _**Propietario de datos de Azure Digital Twins**_ dentro de la instancia.

> [!NOTE]
> Tenga en cuenta que este rol es distinto del rol *Propietario* de Azure AD, que también se puede asignar en el ámbito de la instancia de Azure Digital Twins. Se trata de dos roles de administración distintos y el rol *Propietario* de Azure AD no concede acceso a las características del plano de datos que se conceden con *Propietario de datos de Azure Digital Twins*.