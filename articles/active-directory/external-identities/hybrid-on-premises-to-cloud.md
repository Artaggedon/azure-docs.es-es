---
title: 'Sincronización de cuentas de asociado local en la nube como usuarios de B2B: Azure AD'
description: Proporcione a los asociados externos administrados localmente acceso a recursos locales y en la nube con las mismas credenciales mediante la colaboración B2B de Azure AD.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 11/03/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 29aeca30e1fbdd28d3f69597d902a9b714056cd5
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99575930"
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Conceder a las cuentas de asociado administradas localmente acceso a los recursos en la nube mediante la colaboración B2B de Azure AD

Antes de Azure Active Directory (Azure AD), las organizaciones con sistemas de identidad locales han administrado tradicionalmente las cuentas de asociado en sus directorios locales. En este tipo de organización, cuando comienzan a moverse las aplicaciones a Azure AD, quiere tener la seguridad de que los asociados pueden acceder a los recursos que necesitan. No importa si los recursos se encuentran en el entorno local o en la nube. Además, quiere que los usuarios asociados puedan usar las mismas credenciales de inicio de sesión tanto para los recursos locales como para los de Azure AD. 

Si crea cuentas para los asociados externos en el directorio local (por ejemplo, crea una cuenta con un nombre de inicio de sesión de "wmoran" para un usuario externo llamado Wendy Moran en el dominio partners.contoso.com), ahora puede sincronizarlas con la nube. En concreto, puede usar Azure AD Connect para sincronizar las cuentas de asociados con la nube, lo que crea una cuenta de usuario con UserType = Guest. De esta manera, los usuarios asociados pueden acceder a los recursos en la nube con las mismas credenciales que las de sus cuentas locales, sin concederles más acceso del que necesitan.

> [!NOTE]
> Consulte también [Invitación de usuarios internos a la colaboración B2B](invite-internal-users.md). Con esta característica, puede proponer a los usuarios internos invitados que usen la colaboración B2B, independientemente de si se han sincronizado sus cuentas del directorio local con la nube. Una vez que los usuarios aceptan la invitación para usar la colaboración B2B, pueden usar su propia identidad y credenciales para iniciar sesión en los recursos a los que se les da acceso. Ya no tendrá que mantener contraseñas ni administrar los ciclos de vida de la cuenta.

## <a name="identify-unique-attributes-for-usertype"></a>Identificación de atributos únicos para UserType

Antes de permitir la sincronización del atributo UserType, primero debe decidir cómo obtener el atributo UserType de Active Directory local. En otras palabras, ¿qué parámetros del entorno local son únicos para los colaboradores externos? Determine un parámetro que distinga estos colaboradores externos de los miembros de su propia organización.

Los dos enfoques comunes para ello son los siguientes:

- Designe un atributo de Active Directory local sin usar (por ejemplo, Atributodeextensión1) que se usará como el atributo de origen. 
- Como alternativa, obtenga el valor del atributo UserType de otras propiedades. Por ejemplo, querrá sincronizar todos los usuarios como Invitado si su atributo UserPrincipalName de Active Directory local finaliza con el dominio *\@partners.contoso.com*.
 
Para conocer los requisitos detallados de atributos, consulte [Habilitar la sincronización de UserType](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Configuración de Azure AD Connect para sincronizar los usuarios con la nube

Después de identificar el atributo único, puede configurar Azure AD Connect para sincronizar estos usuarios con la nube, lo que crea cuentas de usuario con UserType = Guest. Desde un punto de vista de autorización, estos usuarios no se distinguen de los usuarios B2B creados mediante el proceso de invitación a la colaboración B2B de Azure AD.

Para obtener instrucciones de implementación, consulte [Habilitar la sincronización de UserType](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Pasos siguientes

- [Colaboración B2B de Azure Active Directory para organizaciones híbridas](hybrid-organizations.md)
- [Conceder a los usuarios B2B de Azure AD acceso a las aplicaciones locales](hybrid-cloud-to-on-premises.md)
- Para información general sobre Azure AD Connect, consulte [Integración de los directorios locales con Azure Active Directory](../hybrid/whatis-hybrid-identity.md).

