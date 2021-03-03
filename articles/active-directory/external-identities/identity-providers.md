---
title: Proveedores de identidades para External Identities - Azure AD
description: Aprenda a usar Azure AD como proveedor de identidades predeterminado para compartir con usuarios externos.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42ed42a1fc9a2750cc928b5fd03eb4b32f770276
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101644092"
---
# <a name="identity-providers-for-external-identities"></a>Proveedores de identidades para External Identities

Un *proveedor de identidades* crea, mantiene y administra la información de identidad al tiempo que proporciona servicios de autenticación a las aplicaciones. Al compartir aplicaciones y recursos con usuarios externos, Azure AD es el proveedor de identidades predeterminado para el uso compartido. Esto significa que, al invitar a usuarios externos que ya tienen una cuenta de Azure AD o de Microsoft, estos pueden iniciar sesión automáticamente sin tener que realizar ninguna configuración adicional.

Sin embargo, puede permitir que los usuarios inicien sesión con varios proveedores de identidades.

- **Google**: La federación de Google permite a usuarios externos canjear invitaciones que les haya enviado si inician sesión en sus aplicaciones con sus propias cuentas de Gmail. La federación de Google también se puede usar en los flujos de usuario de registro de autoservicio.
   > [!IMPORTANT]
   > **A partir del 4 de enero de 2021**, Google [retira la compatibilidad con el inicio de sesión en WebView](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html). Si usa Google Federation o el registro de autoservicio con Gmail, debería [comprobar la compatibilidad de las aplicaciones nativas de línea de negocio](google-federation.md#deprecation-of-webview-sign-in-support).

- **Facebook**: al compilar una aplicación, puede configurar el registro de autoservicio y habilitar la federación de Facebook para que los usuarios puedan suscribirse a la aplicación con sus propias cuentas de Facebook. Facebook solo se puede usar para flujos de usuario de registro de autoservicio y no está disponible como una opción de inicio de sesión cuando los usuarios canjean las invitaciones que les envíe.

- **Federación directa**: también puede configurar la federación directa con cualquier proveedor de identidades externo que admita los protocolos SAML o WS-Fed. La federación directa permite a los usuarios externos canjear invitaciones que les haya enviado si inician sesión en sus aplicaciones con sus cuentas empresariales o de redes sociales existentes. 
   > [!NOTE]
   > Los proveedores de identidades de federación directa no se pueden usar en los flujos de usuario de inicio de registro de autoservicio.


## <a name="how-it-works"></a>Funcionamiento

La característica de registro de autoservicio Identidades externas de Azure AD permite a los usuarios registrarse con su cuenta de Azure AD, Google o Facebook. Para configurar proveedores de identidades sociales en el inquilino de Azure AD, creará una aplicación en cada proveedor de identidades y configurará las credenciales. Obtendrá un id. de cliente o de aplicación y un secreto de cliente o de aplicación, que puede agregar a su inquilino de Azure AD.

Una vez que haya agregado un proveedor de identidades a su inquilino de Azure AD:

- Cuando invite a un usuario externo a las aplicaciones o recursos de la organización, el usuario externo puede iniciar sesión con la cuenta que tenga con ese proveedor de identidades.
- Al habilitar el [registro de autoservicio](self-service-sign-up-overview.md) para las aplicaciones, los usuarios externos pueden registrarse en las aplicaciones con las cuentas de los proveedores de identidades que haya agregado.

> [!NOTE]
> Azure AD está habilitado de forma predeterminada para el registro de autoservicio, por lo que los usuarios siempre tienen la opción de registrarse con una cuenta de Azure AD.

Al canjear la invitación o al registrarse en la aplicación, el usuario externo tiene la opción de iniciar sesión y autenticarse con el proveedor de identidades de redes sociales:

![Captura de pantalla que muestra la pantalla de inicio de sesión con las opciones de Google y Facebook.](media/identity-providers/sign-in-with-social-identity.png)

Para conseguir una experiencia de inicio de sesión óptima, fedérese con los proveedores de identidades siempre que sea posible para que pueda proporcionar a sus invitados una experiencia de inicio de sesión sin problemas cuando accedan a las aplicaciones.  

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo agregar proveedores de identidades para iniciar sesión en las aplicaciones, consulte los siguientes artículos:

- [Agregar Google](google-federation.md) a la lista de proveedores de identidades de redes sociales
- [Agregar Facebook](facebook-federation.md) a la lista de proveedores de identidades de redes sociales
- [Configuración de federación directa](direct-federation.md) con cualquier organización cuyo proveedor de identidades (IdP) admita el protocolo SAML 2.0 o WS-Fed. Tenga en cuenta que la federación directa no es una opción para los flujos de usuario de registro de autoservicio.
