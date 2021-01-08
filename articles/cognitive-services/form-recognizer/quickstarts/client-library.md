---
title: 'Inicio rápido: Biblioteca cliente de Form Recognizer'
titleSuffix: Azure Cognitive Services
description: Use la biblioteca cliente de Form Recognizer para crear una aplicación de procesamiento de formularios que extraiga pares clave-valor y datos de tabla de los documentos personalizados.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 09/21/2020
ms.author: pafarley
zone_pivot_groups: programming-languages-set-formre
ms.custom: devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
keywords: forms processing, automated data processing
ms.openlocfilehash: f01adc472f94d679366af50c136ddc020b79b811
ms.sourcegitcommit: 02ed9acd4390b86c8432cad29075e2204f6b1bc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97808579"
---
# <a name="quickstart-use-the-form-recognizer-client-library"></a>Inicio rápido: Uso de la biblioteca cliente de Form Recognizer

Comience a usar Form Recognizer en el idioma de su elección. Azure Form Recognizer es un servicio cognitivo que le permite crear software de procesamiento de datos automatizado mediante tecnología de aprendizaje automático. Identifique y extraiga texto, pares clave-valor y datos de tabla de los documentos de formulario; el servicio genera datos estructurados que incluyen las relaciones en el archivo original. Siga estos pasos para instalar el paquete SDK y probar el código de ejemplo para realizar tareas básicas. Actualmente, la biblioteca cliente de Form Recognizer tiene como destino la versión v2.0 del servicio Form Recognizer.

Use la biblioteca cliente de Form Recognizer para:

* [Reconocer el contenido del formulario](#recognize-form-content)
* [Reconocer recibos](#recognize-receipts)
* [Reconocer tarjetas de presentación](#recognize-business-cards)
* [Reconocer facturas](#recognize-invoices)
* [Entrenar un modelo personalizado](#train-a-custom-model)
* [Analizar formularios con un modelo personalizado](#analyze-forms-with-a-custom-model)
* [Administrar modelos personalizados](#manage-your-custom-models)

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](../includes/quickstarts/javascript-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-rest-api"

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end