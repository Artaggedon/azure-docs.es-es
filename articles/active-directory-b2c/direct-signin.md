---
title: Configuración de inicio de sesión directo con Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre cómo rellenar previamente el nombre de inicio de sesión o cómo redirigir directamente a un proveedor de identidades sociales.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/14/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 35e8efa269ab72477b06e86824d368d0a3dced03
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103197321"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Configuración de inicio de sesión directo con Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Al configurar el inicio de sesión en su aplicación con Azure Active Directory (AD) B2C, puede rellenar previamente el nombre de inicio de sesión o puede iniciar sesión directamente en un proveedor de identidades sociales específico, como Facebook, LinkedIn o una cuenta de Microsoft.

## <a name="prepopulate-the-sign-in-name"></a>Rellenar previamente el nombre de inicio de sesión

Durante el recorrido de inicio de sesión de un usuario, una aplicación de usuario de confianza puede tener como destino un nombre de usuario o dominio específico. Cuando el destino es un usuario, una aplicación puede especificar, en la solicitud de autorización, el parámetro de consulta `login_hint` con el nombre de inicio de sesión de usuario. Azure AD B2C rellena automáticamente el nombre de inicio de sesión, mientras que el usuario sólo necesita proporcionar la contraseña.

![Página de inicio de sesión de registro con el parámetro de consulta login_hint resaltado en la dirección URL](./media/direct-signin/login-hint.png)

El usuario puede cambiar el valor en el cuadro de texto de inicio de sesión.

::: zone pivot="b2c-custom-policy"

Para admitir el parámetro login_hint, invalide el perfil técnico `SelfAsserted-LocalAccountSignin-Email`. En la sección `<InputClaims>`, establezca el valor DefaultValue de la notificación signInName en `{OIDC:LoginHint}`. La variable `{OIDC:LoginHint}` contiene el valor del parámetro `login_hint`. Azure AD B2C lee el valor de la notificación signInName y rellena previamente el cuadro de texto de signInName.

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <InputClaims>
        <!-- Add the login hint value to the sign-in names claim type -->
        <InputClaim ClaimTypeReferenceId="signInName" DefaultValue="{OIDC:LoginHint}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

::: zone-end

## <a name="redirect-sign-in-to-a-social-provider"></a>Redirección del inicio de sesión a un proveedor social

Si se ha configurado el recorrido de inicio de sesión para que la aplicación incluya cuentas de redes sociales como Facebook, LinkedIn o Google, puede especificar el parámetro `domain_hint`. Este parámetro de consulta proporciona una sugerencia a Azure AD B2C acerca del proveedor de identidades sociales que debe usarse para iniciar sesión. Por ejemplo, si la aplicación especifica `domain_hint=facebook.com`, el inicio de sesión va directamente a la página de inicio de sesión de Facebook.

![Página de inicio de sesión de registro con el parámetro de consulta domain_hint resaltado en la dirección URL](./media/direct-signin/domain-hint.png)

::: zone pivot="b2c-user-flow"

El parámetro de la cadena de consulta de domain_hint puede establecerse en uno de los siguientes dominios:

- amazon.com
- facebook.com
- github.com
- google.com
- linkedin.com
- microsoft.com
- qq.com
- twitter.com
- wechat.com
- weibo.com 
- Para [OpenID Connect genérico](identity-provider-generic-openid-connect.md), consulte [Sugerencia de dominio](identity-provider-generic-openid-connect.md#response-mode).

::: zone-end

::: zone pivot="b2c-custom-policy"

Para admitir el parámetro domain_hint, puede configurar el nombre de dominio mediante el `<Domain>domain name</Domain>` elemento XML de cualquier `<ClaimsProvider>`.

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```

::: zone-end

