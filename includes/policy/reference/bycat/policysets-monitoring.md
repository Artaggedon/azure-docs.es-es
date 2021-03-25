---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/10/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 3859b73e61a20f5950531d1680a3015fcb53d3a2
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102613799"
---
|Nombre |Descripción |Directivas |Versión |
|---|---|---|---|
|[\[Versión preliminar\]: Implementación: configurar los requisitos previos para habilitar los agentes de Azure Monitor y de seguridad de Azure en máquinas virtuales](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitoring_Prerequisites.json) |Configure las máquinas para instalar automáticamente los agentes de seguridad de Azure y Azure Monitor. Security Center recopila eventos de los agentes y los usa para proporcionar alertas de seguridad y tareas de protección personalizadas (recomendaciones). Cree un grupo de recursos y un área de trabajo de Log Analytics en la misma región que la máquina para almacenar los registros de auditoría. Esta directiva solo se aplica a las máquinas virtuales en algunas regiones. |5 |1.0.0-preview |
|[Habilitar Azure Monitor para conjunto de escalado de máquinas virtuales](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Se habilita Azure Monitor para los conjuntos de escalado de máquinas virtuales en el ámbito especificado (grupo de administración, suscripción o grupo de recursos). Usa el área de trabajo de Log Analytics como parámetro. Nota: Si establece upgradePolicy del conjunto de escalado en Manual, debe aplicar la extensión a todas las máquinas virtuales del conjunto mediante una llamada a la actualización. En la CLI, se usaría az vmss update-instances. |6 |1.0.1 |
|[Habilitar Azure Monitor para VM](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Se habilita Azure Monitor para las máquinas virtuales en el ámbito especificado (grupo de administración, suscripción o grupo de recursos). Usa el área de trabajo de Log Analytics como parámetro. |10 |2.0.0 |
