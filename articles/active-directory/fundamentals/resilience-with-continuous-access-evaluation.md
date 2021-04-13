---
title: Aumento de la resistencia mediante Evaluación continua de acceso en Azure Active Directory
description: Guía para arquitectos y administradores de TI sobre el uso de CAE
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d792c1eb0a85f2a898d1dc5c63047a4553f13b0
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2021
ms.locfileid: "106106779"
---
# <a name="build-resilience-by-using-continuous-access-evaluation"></a>Aumento de la resistencia mediante Evaluación continua de acceso

[Evaluación continua de acceso](../conditional-access/concept-continuous-access-evaluation.md) (CAE) permite que las aplicaciones de Azure AD se suscriban a eventos críticos que luego se pueden evaluar y aplicar. Esto incluye la evaluación de los siguientes eventos:

* La cuenta de usuario se ha eliminado o deshabilitado.

* La contraseña de un usuario ha cambiado.

* MFA está habilitado para el usuario.

* El administrador revoca explícitamente un token.

* Se detecta un riesgo de usuario elevado.

Como resultado, las aplicaciones pueden rechazar los tokens que no hayan expirado en función de los eventos señalados por Azure AD, como se describe en el diagrama siguiente.

![diagrama conceptual de CAE](./media/resilience-with-cae/admin-resilience-continuous-access-evaluation.png)

## <a name="how-does-cae-help"></a>¿Cómo ayuda CAE?

Este mecanismo permite a Azure AD emitir tokens de larga duración, al tiempo que permite a las aplicaciones revocar el acceso y forzar la reautenticación solo cuando sea necesario. El resultado neto de este patrón es la disminución de llamadas para adquirir tokens, lo que significa que el flujo de un extremo a otro es más resistente. 

Para usar CAE, el servicio y el cliente deben ser compatibles con CAE. Los servicios de Microsoft 365, como Exchange Online, Teams y SharePoint Online, son compatibles con CAE. En el lado del cliente, las experiencias basadas en explorador que usan estos servicios de Office 365 (por ejemplo, Outlook Web App) y las versiones específicas de los clientes nativos de Office 365 son compatibles con CAE. Más servicios en la nube de Microsoft pasarán a ser compatibles con CAE.

Microsoft trabaja con el sector para crear [estándares](https://openid.net/wg/sse/) que permitirán a las aplicaciones de terceros usar esta funcionalidad. También puede desarrollar aplicaciones que sean compatibles con CAE. Vea cómo compilar la resistencia en la aplicación para obtener más información.

## <a name="how-do-i-implement-cae"></a>¿Cómo se implementa CAE?

* [Actualice el código para usar las API habilitadas para CAE](../develop/app-resilience-continuous-access-evaluation.md).

* [Habilite CAE](../conditional-access/concept-continuous-access-evaluation.md) en la configuración de seguridad de Azure AD.

* Asegúrese de que su organización usa [versiones compatibles](../conditional-access/concept-continuous-access-evaluation.md) de aplicaciones nativas de Microsoft Office.

* [Optimice los mensajes de reautenticación](../authentication/concepts-azure-multi-factor-authentication-prompts-session-lifetime.md).

 
## <a name="next-steps"></a>Pasos siguientes
Recursos de resistencia para administradores y arquitectos
 
* [Aumento de la resistencia con la administración de credenciales](resilience-in-credentials.md)

* [Aumento de la resistencia con estados de dispositivos](resilience-with-device-states.md)

* [Aumento de la resistencia en la autenticación de usuario externo](resilience-b2b-authentication.md)

* [Aumento de la resistencia en la autenticación híbrida](resilience-in-hybrid.md)

* [Aumento de la resistencia en el acceso a la aplicación con Application Proxy](resilience-on-premises-access.md)

Recursos de resistencia para desarrolladores

* [Aumento de la resistencia IAM en las aplicaciones](resilience-app-development-overview.md)

* [Aumento de la resistencia en los sistemas CIAM](resilience-b2c.md)
