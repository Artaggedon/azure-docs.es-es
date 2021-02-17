---
title: Respuesta rápida de Azure DDoS
description: Obtenga información acerca de cómo interactuar con expertos de DDoS durante un ataque activo para obtener soporte técnico especializado.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: yitoh
ms.openlocfilehash: 8e860bf47420f2b58c44df695da7761bcc2aa0ce
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2021
ms.locfileid: "100521788"
---
# <a name="azure-ddos-rapid-response"></a>Respuesta rápida de Azure DDoS

Durante un acceso activo, los clientes de Azure DDoS Protection Estándar tienen acceso al equipo de Respuesta rápida de DDoS (DRR), que puede ayudar con la investigación de ataques durante un ataque y con el análisis posterior al ataque.

## <a name="prerequisites"></a>Prerrequisitos

- Para poder completar los pasos de este tutorial, primero debe crear un [plan de protección Estándar de Azure DDoS](manage-ddos-protection.md).

## <a name="when-to-engage-drr"></a>Cuándo se debe interactuar con DRR

Solo debe interaccionar con DRR si: 

- Durante un ataque de DDoS, encuentra que la degradación del rendimiento del recurso protegido es grave o el recurso no está disponible. 
- Cree que el recurso se encuentra bajo un ataque de DDoS, pero el servicio DDoS Protection no mitiga el ataque de forma eficaz.
- Planea un evento viral que aumentará significativamente el tráfico.
- Hay ataques que tienen un impacto empresarial crítico.

## <a name="engage-drr-during-an-active-attack"></a>Interactuación con DRR durante un ataque activo

1. En Azure Portal al crear una nueva solicitud de soporte técnico, elija Técnico para **Tipo de problema**.
2. Elija **Servicio** para **Protección contra DDOS**.
3. Elija un recurso en el menú desplegable de recursos. _Debe seleccionar un plan de DDoS que esté vinculado a la red virtual que se va a proteger mediante DDoS Protection Standard para interactuar con DRR._

    ![Elección de recurso](./media/ddos-rapid-response/choose-resource.png)

4. En la página siguiente **Problema**, seleccione A - Impacto crítico para **Gravedad** y "Under attack" (Bajo ataque) para **Tipo de problema**.

    ![Gravedad y tipo de problema](./media/ddos-rapid-response/severity-and-problem-type.png)

5. Complete los detalles adicionales y envíe la solicitud de soporte técnico.

DRR sigue el modelo de soporte técnico de Azure Rapid Response. Consulte [Ámbito de soporte técnico y capacidad de respuesta](https://azure.microsoft.com/en-us/support/plans/response/) para más información sobre la respuesta rápida.

Para más información, lea la [documentación sobre DDoS Protection Standard](./ddos-protection-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [realizar pruebas mediante simulaciones](test-through-simulations.md).
- Aprenda a [ver y configurar la telemetría de DDoS Protection](telemetry.md).
- Obtenga información sobre la [Visualización y configuración del registro de diagnóstico de DDoS](diagnostic-logging.md).
