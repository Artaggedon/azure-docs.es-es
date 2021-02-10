---
title: 'Colaboración con otros usuarios: QnA Maker'
description: Obtenga información sobre cómo colaborar con otros creadores y editores mediante el control de acceso basado en roles de Azure.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: f43fa06cf93d383efcb0fe746de28ab143280e50
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99988018"
---
# <a name="collaborate-with-other-authors-and-editors"></a>Colaboración con otros creadores y editores

Colabore con otros creadores y editores mediante el control de acceso basado en rol de Azure (RBAC de Azure) colocado en el recurso de QnA Maker.

## <a name="access-is-provided-on-the-qna-maker-resource"></a>El acceso se proporciona en el recurso de QnA Maker

Todos los permisos se controlan mediante los permisos colocados en el recurso de QnA Maker. Estos permisos se alinean con el acceso de lectura, escritura, publicación y completo.

Esta característica de RBAC de Azure incluye lo siguiente:
* Azure Active Directory (AAD) ofrece una compatibilidad con versiones anteriores total con la autenticación basada en claves para propietarios y colaboradores. Los clientes pueden usar la autenticación basada en claves o en RBAC de Azure en sus solicitudes.
* Agregue creadores y editores rápidamente a todas las bases de conocimiento del recurso, ya que el control se encuentra en el nivel de recursos, no en el de base de conocimiento.

## <a name="access-is-provided-by-a-defined-role"></a>El acceso lo proporciona un rol definido

[!INCLUDE [Azure RBAC permissions table](../includes/role-based-access-control.md)]

## <a name="authentication-flow"></a>Flujo de autenticación

En el diagrama siguiente se muestra el flujo, desde la perspectiva del creador, para iniciar sesión en el portal de QnA Maker y usar las API de creación.

> [!div class="mx-imgBorder"]
> ![En el diagrama siguiente se muestra el flujo, desde la perspectiva del creador, para iniciar sesión en el portal de QnA Maker y usar las API de creación.](../media/qnamaker-how-to-collaborate-knowledge-base/rbac-flow-from-portal-to-service.png)

|Pasos|Descripción|
|--|--|
|1|El portal adquiere el token para el recurso de QnA Maker.|
|2|El portal llama a la API de creación de QnA Maker (APIM) apropiada y pasa el token en lugar de las claves.|
|3|La API de QnA Maker valida el token.|
|4 |La API de QnA Maker llama al servicio QnAMaker.|

Si tiene previsto llamar a las [API de creación](../index.yml), obtenga más información sobre cómo configurar la autenticación.

## <a name="authenticate-by-qna-maker-portal"></a>Autenticación mediante el portal de QnA Maker

Si crea y colabora con el portal de QnA Maker, después de agregar el rol adecuado al recurso para un colaborador, en el portal de QnA Maker se administran todos los permisos de acceso.

## <a name="authenticate-by-qna-maker-apis-and-sdks"></a>Autenticación mediante API y SDK de QnA Maker

Si crea y colabora mediante API, ya sea a través de REST o los SDK, tendrá que [crear una entidad de servicio](../../authentication.md#assign-a-role-to-a-service-principal) para administrar la autenticación.

## <a name="next-step"></a>Paso siguiente

* Diseño de una base de conocimiento para lenguajes o aplicaciones cliente
