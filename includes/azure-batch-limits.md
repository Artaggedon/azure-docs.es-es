---
title: archivo de inclusión
description: archivo de inclusión
services: batch
author: JnHs
ms.service: batch
ms.topic: include
ms.date: 12/16/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 0a94b122f1cdd598eeac553c8cc784d2a0a5369f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97614526"
---
| **Recurso** | **Límite predeterminado** | **Límite máximo** |
| --- | --- | --- |
| Cuentas de Azure Batch por región y suscripción | 1-3 |50 |
| Núcleos dedicados por cuenta de Batch | 90-900 | Ponerse en contacto con soporte técnico |
| Núcleos de baja prioridad por cuenta de Batch | 10-100 | Ponerse en contacto con soporte técnico |
| Trabajos **[activos](/rest/api/batchservice/job/get#jobstate)** y programaciones de trabajos por cuenta de Batch (los trabajos **completados** no tienen ningún límite) | 100-300 | 1000<sup>1</sup> |
| Grupos por cuenta de Batch | 20-100 | 500<sup>1</sup> |

<sup>1</sup> Si quiere solicitar un aumento de este límite, póngase en contacto con el soporte técnico de Azure.

> [!NOTE]
> Los límites predeterminados varían según el tipo de suscripción que se use para crear una cuenta de Batch. Las cuotas de núcleos mostradas son para las cuentas de Batch del modo de servicio Batch. [Vea las cuotas de su cuenta de Batch](../articles/batch/batch-quota-limit.md#view-batch-quotas).

> [!IMPORTANT]
> Para ayudarnos a administrar mejor la capacidad durante la pandemia sanitaria global, las cuotas de núcleo predeterminadas para las nuevas cuentas de Batch en algunas regiones y para algunos tipos de suscripción se han reducido con respecto al intervalo de valores anterior, en algunos casos a cero núcleos. Al crear una nueva cuenta de Batch, debe [comprobar la cuota de núcleos](../articles/batch/batch-quota-limit.md#view-batch-quotas) y [solicitar un aumento de la cuota de núcleos](../articles/batch/batch-quota-limit.md#increase-a-quota) si es necesario. Como alternativa, considere la posibilidad de volver a usar aquellas cuentas de Batch que ya tengan suficiente cuota.