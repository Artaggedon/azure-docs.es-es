---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/29/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 51467968815e2b55871fff5fe747ad8da03000ed
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99215227"
---
|Nombre |Descripción |Directivas |Versión |
|---|---|---|---|
|[Auditar las máquinas que tengan una configuración de seguridad de contraseña no segura](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_WindowsPasswordSettingsAINE.json) |Esta iniciativa implementa los requisitos de la directiva y audita las máquinas que cuenten con una configuración de seguridad de contraseña no segura. Para más información sobre las directivas de configuración de invitados, visite [https://aka.ms/gcpol](https://aka.ms/gcpol) |9 |1.0.0 |
|[Implementar los requisitos previos para habilitar directivas de configuración de invitado en máquinas virtuales](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_Prerequisites.json) |Esta iniciativa agrega una identidad administrada asignada por el sistema e implementa la extensión de configuración de invitado adecuada para la plataforma en las máquinas virtuales que se puedan supervisar mediante directivas de configuración de invitado. Este es un requisito previo para todas las directivas de configuración de invitado y se debe agregar al ámbito de asignación de directivas antes de usar cualquier directiva de configuración de invitado. Para más información sobre la configuración de invitado, visite [https://aka.ms/gcpol](https://aka.ms/gcpol). |4 |1.0.0-preview |
|[Las máquinas Windows deben cumplir los requisitos de la línea de base de seguridad de Azure](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_AzureBaseline.json) |Esta iniciativa audita las máquinas Windows con parámetros que no cumplen la línea de base de seguridad de Azure. Para obtener detalles, consulte: [https://aka.ms/gcpol](https://aka.ms/gcpol) |29 |2.0.0-preview |
