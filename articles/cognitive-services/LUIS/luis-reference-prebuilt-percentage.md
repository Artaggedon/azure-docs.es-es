---
title: 'Entidad precompilada Percentage: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información sobre la entidad precompilada de porcentaje en Language Understanding (LUIS).
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.openlocfilehash: bb64a32e2bdd3976fba3ce63433b13eb4891afc7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91541703"
---
# <a name="percentage-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Percentage para una aplicación de LUIS
Los números de porcentaje pueden aparecer como fracciones, `3 1/2`, o como porcentaje, `2%`. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la entidad de porcentaje para las intenciones de la aplicación. La entidad de porcentaje se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md).

## <a name="types-of-percentage"></a>Tipos de porcentaje
El porcentaje se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114).

## <a name="resolution-for-prebuilt-percentage-entity"></a>Resolución para la entidad de porcentaje precompilada

La consulta devuelve los siguientes objetos de entidad:

`set a trigger when my stock goes up 2%`

#### <a name="v3-response"></a>[Respuesta de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "percentage": [
        2
    ]
}
```
#### <a name="v3-verbose-response"></a>[Respuesta detallada de V3](#tab/V3-verbose)
El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "percentage": [
        2
    ],
    "$instance": {
        "percentage": [
            {
                "type": "builtin.percentage",
                "text": "2%",
                "startIndex": 36,
                "length": 2,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[Respuesta de V2](#tab/V2)

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.percentage**.

```json
"entities": [
    {
        "entity": "2%",
        "type": "builtin.percentage",
        "startIndex": 36,
        "endIndex": 37,
        "resolution": {
        "value": "2%"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Información sobre las entidades [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) y [temperature](luis-reference-prebuilt-temperature.md).
