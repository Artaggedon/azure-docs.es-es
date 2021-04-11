---
title: Guía de migración de unidades reservadas de multimedia (MRU)
description: En este artículo se proporcionan instrucciones basadas en escenarios de MRU que le ayudarán a migrar de Azure Media Services v2 a v3.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 03/25/2021
ms.author: inhenkel
ms.openlocfilehash: 00e1fa623fc625a568f6580998443758c3916be8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105563079"
---
# <a name="media-reserved-units-mrus-scenario-based-migration-guidance"></a>Guía de migración basada en escenarios de unidad reservada de multimedia (MRU)

![logotipo de la guía de migración](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![pasos de migración 2](./media/migration-guide/steps-4.svg)

En este artículo se proporcionan instrucciones basadas en escenarios de MRU que le ayudarán a migrar de Azure Media Services v2 a v3.

- En el caso de las nuevas cuentas de v3 creadas en Azure Portal, o con la versión 2020-05-01 de la API v3, ya no es necesario establecer unidades reservadas de multimedia (MRU). El sistema se escalará y reducirá verticalmente de manera automática en función de la carga.
- Si tiene una cuenta v3 o v2 que se creó antes de la versión 2020-05-01 de la API, puede controlar la simultaneidad y el rendimiento de los trabajos con unidades reservadas de multimedia. Para más información, consulte Escalado del procesamiento de elementos multimedia. Puede administrar las MRU con la CLI 2.0 para Media Services v3, o mediante Azure Portal.  
- Si no ve la opción para administrar MRU en Azure Portal, está ejecutando una cuenta creada con la API 2020-05-01 o posterior.
- Si está familiarizado con el establecimiento del tipo MRU en S3, el rendimiento mejorará o seguirá siendo el mismo.
- Si ya es un cliente de v2, debe crear una nueva cuenta de v2 para admitir su aplicación existente antes de que se complete la migración. 
- Quizá deba habilitar de nuevo el indizador v1 u otros procesadores multimedia que no estén totalmente en desuso. 

Para obtener más información acerca de las MRU, consulte [Unidades reservadas de multimedia](concept-media-reserved-units.md) y [Escalado de unidades reservadas de multimedia](media-reserved-units-cli-how-to.md).

## <a name="mru-concepts-tutorials-and-how-to-guides"></a>Conceptos, tutoriales y guías de procedimientos de MRU

### <a name="concepts"></a>Conceptos

[Unidades reservadas de multimedia](concept-media-reserved-units.md)

### <a name="how-to-guides"></a>Guías de procedimientos

[Escalado de unidades reservadas de multimedia](media-reserved-units-cli-how-to.md)

## <a name="samples"></a>Ejemplos

También puede [comparar el código de la versión v2 y v3 en los ejemplos de código](migrate-v-2-v-3-migration-samples.md).
