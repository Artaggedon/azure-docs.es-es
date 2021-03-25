---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: 61576de4a57d55ea9d1ea209c52df556f0069617
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750764"
---
| Propiedad | Descripción |
|:--- |:---|
|**identity/type** | Tipo de autenticación que se ha usado para realizar la solicitud. Por ejemplo: `OAuth`, `Kerberos`, `SAS Key`, `Account Key` o `Anonymous` |
|**identity/tokenHash**|Este campo está reservado para uso interno exclusivamente. |
|**authorization/action** | Acción asignada a la solicitud. |
|**authorization/roleAssignmentId** | Identificador de la asignación de roles. Por ejemplo: `4e2521b7-13be-4363-aeda-111111111111`.|
|**authorization/roleDefinitionId** | Identificador de la definición de roles. Por ejemplo: `ba92f5b4-2d11-453d-a403-111111111111"`.|
|**principals/id** | Identificador de la entidad de seguridad. Por ejemplo: `a4711f3a-254f-4cfb-8a2d-111111111111`.|
|**principals/type** | Tipo de la entidad de seguridad. Por ejemplo: `ServicePrincipal`. |
|**requester/appID** | Identificador de la aplicación de Open Authorization (OAuth) que se usa como solicitante. <br> Por ejemplo: `d3f7d5fe-e64a-4e4e-871d-333333333333`.|
|**requester/audience** | Audiencia de OAuth de la solicitud. Por ejemplo: `https://storage.azure.com`. |
|**requester/objectId** | Identificador de objeto de OAuth de la solicitud. En el caso de la autenticación Kerberos, representa al identificador de objeto del usuario autenticado de Kerberos. Por ejemplo: `0e0bf547-55e5-465c-91b7-2873712b249c`. |
|**requester/tenantId** | Identificador de inquilino de OAuth de la identidad. Por ejemplo: `72f988bf-86f1-41af-91ab-222222222222`.|
|**requester/tokenIssuer** | Emisor de token de OAuth. Por ejemplo: `https://sts.windows.net/72f988bf-86f1-41af-91ab-222222222222/`.|
|**requester/upn** | Nombre principal de usuario (UPN) del solicitante. Por ejemplo: `someone@contoso.com`. |
|**requester/userName** | Este campo está reservado para uso interno exclusivamente.|
