---
title: Modelos creados previamente (LUIS)
titleSuffix: Azure Cognitive Services
description: Los modelos creados previamente proporcionan dominios, intenciones, expresiones y entidades. Puede iniciar su aplicación con un dominio creado previamente o agregar un dominio pertinente a su aplicación más adelante.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/10/2019
ms.openlocfilehash: 6642e59c2957b298d54bc587853752b9fce74686
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "95019150"
---
# <a name="prebuilt-models"></a>Modelos creados previamente

Los modelos creados previamente proporcionan dominios, intenciones, expresiones y entidades. Puede iniciar su aplicación con un modelo precompilado o agregar un dominio pertinente a la aplicación más adelante. 

## <a name="types-of-prebuilt-models"></a>Tipos de modelos creados previamente

LUIS proporciona tres tipos de modelos precompilados. Cada modelo se puede agregar a su aplicación en cualquier momento. 

|Tipo de modelo|Includes|
|--|--|
|[Dominio](luis-reference-prebuilt-domains.md)|Intenciones, expresiones, entidades|
|Intenciones|Intenciones, expresiones|
|[Entidades](luis-reference-prebuilt-entities.md)|Solo entidades| 

## <a name="prebuilt-domains"></a>Dominios creados previamente

En Language Understanding (LUIS) se proporcionan *dominios pregenerados*, que son modelos entrenados previamente de [intenciones](luis-how-to-add-intents.md) y [entidades](luis-concept-entity-types.md) que funcionan de manera conjunta para los dominios o las categorías comunes de las aplicaciones cliente. 

Los dominios creados previamente están capacitados y listos para agregarse a la aplicación LUIS. Las intenciones y entidades de un dominio creado previamente se pueden personalizar completamente una vez las haya agregado a la aplicación. 

> [!TIP]
> Las intenciones y entidades de un dominio creado previamente funcionan mejor de manera conjunta. Siempre que sea posible, se recomienda combinar las intenciones y entidades del mismo dominio.
> El dominio Utilidades creado previamente tiene intenciones que se pueden personalizar para usarlas en cualquier dominio. Por ejemplo, puede agregar `Utilities.Repeat` a la aplicación y entrenarla para que reconozca todas las acciones que es posible que el usuario quiera repetir en la aplicación. 

### <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Cambiar el comportamiento de una intención de dominio creado previamente

Es posible que descubra que un dominio creado previamente contiene una intención que es similar a otra que quiere incluir en la aplicación de LUIS pero con otro comportamiento. Por ejemplo, el dominio creado previamente **Places** (Lugares) proporciona una intención `MakeReservation` para realizar una reserva en un restaurante, pero quiere que la aplicación use esa intención para realizar reservas de hotel. En ese caso, puede modificar el comportamiento de la intención mediante la adición de expresiones de ejemplo a la intención de hacer reservas de hotel y, después, volver a entrenar la aplicación. 

En [Prebuilt domains reference](./luis-reference-prebuilt-domains.md) (Referencia de dominios creados previamente) puede encontrar una lista completa de los dominios creados previamente.

## <a name="prebuilt-intents"></a>Intenciones creadas previamente

LUIS proporciona intenciones pregeneradas y sus expresiones para cada uno de sus dominios pregenerados. Las intenciones pueden agregarse sin tener que agregar todo el dominio. Agregar una intención es el proceso de agregar una intención y sus expresiones a la aplicación. Asimismo, puede modificar tanto el nombre de la intención como la lista de expresiones.  

## <a name="prebuilt-entities"></a>Entidades precompiladas

LUIS incluye un conjunto de entidades precompiladas para reconocer tipos comunes de información, como fechas, horas, números, medidas y moneda. La compatibilidad de entidades precompiladas varía según la referencia cultural de la aplicación LUIS. Para ver una lista completa de las entidades precompiladas que LUIS admite, incluso la compatibilidad por referencia cultural, consulte la [referencia de entidades precompiladas](./luis-reference-prebuilt-entities.md).

Cuando una entidad precompilada se incluye en la aplicación, sus predicciones se incluyen en la aplicación publicada. El comportamiento de las entidades precompiladas ya está entrenado y **no** se puede modificar. 

> [!NOTE]
> **builtin.datetime** está en desuso. Se sustituye por [**builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), que permite reconocer los intervalos de fechas y horas, así como mejorar el reconocimiento de las fechas y horas ambiguas.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [agregar entidades creadas previamente](./howto-add-prebuilt-models.md) a la aplicación.