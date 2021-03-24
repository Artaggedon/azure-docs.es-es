---
title: 'Personalización de un modelo de marcas en Video Indexer: Azure'
titleSuffix: Azure Media Services
description: En este artículo se proporciona información general sobre qué es un modelo de marcas en Video Indexer y cómo personalizarlo.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 12/15/2019
ms.author: juliako
ms.openlocfilehash: 81d7dda854c6afcc9397289ff23ba45b02ed9fc4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97586081"
---
# <a name="customize-a-brands-model-in-video-indexer"></a>Personalización de un modelo de marcas en Video Indexer

Video Indexer permite la detección de marcas por voz y texto visual durante la indexación y la reindexación de contenido de audio y vídeo. La característica de detección de marcas identifica menciones de productos, servicios y compañías sugeridas por la base de datos de marcas de Bing. Por ejemplo, si Microsoft se menciona en un contenido de audio o vídeo, o si se muestra en texto visual en un vídeo, Video Indexer lo detectará como una marca en el contenido. Para eliminar la ambigüedad con otros términos, se usa el contexto.

La detección de marcas es útil en una amplia variedad de escenarios empresariales, como archivo y detección de contenido, publicidad contextual, análisis de medios sociales, análisis de la competencia en el comercio minorista, y mucho más. La detección de marcas de Video Indexer le permite indexar las menciones de marcas en voz y texto visual, mediante una base de datos de marcas de Bing, así como personalizarlas por medio de la creación de un modelo de marcas personalizado para cada cuenta de Video Indexer. La característica de modelo de marcas personalizado le permite seleccionar si Video Indexer detectará o no marcas de la base de datos de marcas de Bing; excluirá determinadas marcas de la detección (mediante la creación de una lista de marcas no permitidas); e incluirá las marcas que deben ser parte del modelo que podrían no estar en la base de datos de marcas de Bing (mediante la creación de una lista de marcas permitidas). El modelo personalizado de marcas que cree solo estará disponible en la cuenta en la que creó el modelo.

## <a name="out-of-the-box-detection-example"></a>Ejemplo de detección predeterminada

En la presentación "Microsoft Build 2017 Day 2" la marca "Microsoft Windows" aparece varias veces. Una veces en la transcripción, otras como texto visual y nunca como textual. Video Indexer detecta con alta precisión que un término es realmente una marca según el contexto, y abarca más de 90 000 marcas predeterminadas que se actualizan constantemente. A las 02:25, Video Indexer detecta la marca por la voz y de nuevo a las 02:40 por el texto visual, que es parte del logotipo de Windows.

![Introducción a las marcas](./media/content-model-customization/brands-overview.png)

Hablar sobre ventanas en el contexto de la construcción no detectará la palabra "Windows" como marca, y lo mismo sucede con Box, Apple, Fox, etc., según los algoritmos avanzados de Machine Learning que saben como eliminar la ambigüedad por el contexto. La detección de marcas funciona con todos los idiomas admitidos.  

## <a name="next-steps"></a>Pasos siguientes

Para traer sus propias marcas, consulte estos temas:

[Personalización del modelo de marcas mediante las API](customize-brands-model-with-api.md)

[Customize Brands model using the website](customize-brands-model-with-website.md) (Personalización del modelo de marcas mediante el sitio web)
