---
title: Procesamiento de eventos en tiempo real mediante Azure Stream Analytics
description: En este artículo se describe la arquitectura de referencia para lograr el análisis y el procesamiento de eventos en tiempo real mediante Azure Stream Analytics.
author: jseb225
ms.author: jeanb
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: 858ec5fef065acba6934945a96c21fb1c26b3685
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98012094"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Arquitectura de referencia: Procesado de eventos en tiempo real con Microsoft Azure Stream Analytics
La arquitectura de referencia para el procesado de eventos en tiempo real con Azure Stream Analytics está diseñada para proporcionar un plano genérico para implementar una plataforma en tiempo real como una solución de procesado de transmisiones (PaaS) de servicio con Microsoft Azure.

## <a name="summary"></a>Resumen
Tradicionalmente, las soluciones de análisis se basaban en funciones como ETL (extracción, transformación y carga de datos) y el almacenamiento de datos, donde se almacenan los datos antes del análisis. Los requisitos en continuo cambio y los datos que llegan más rápidamente están llevando este modelo al límite. Una solución consiste en analizar los datos de las transmisiones en movimiento antes del almacenamiento pero, aunque no se trata de una capacidad nueva, este enfoque no se ha adoptado ampliamente en todas las verticales del sector. 

Microsoft Azure proporciona un catálogo muy amplio de tecnologías de análisis que pueden admitir una matriz de diferentes requisitos y escenarios de solución. El proceso de seleccionar qué servicios de Azure se van a implementar en una solución de un extremo a otro puede ser todo un desafío dada la variedad de ofertas. Este documento está diseñado para describir las capacidades y la interoperación de los distintos servicios de Azure que admiten una solución de transmisión de eventos. Asimismo, explica algunos de los escenarios en los que los clientes pueden beneficiarse de este tipo de enfoque.

## <a name="contents"></a>Contenido
* Resumen ejecutivo
* Introducción al análisis en tiempo real
* Propuesta de valor de datos en tiempo real en Azure
* Escenarios comunes para el análisis en tiempo real
* Arquitectura y componentes
  * Orígenes de datos
  * Capa de integración de datos
  * Capa de análisis en tiempo real
  * Capa de almacenamiento de datos
  * Presentación / capa de consumo
* Conclusión

**Autor:** Charles Feddersen, arquitecto de soluciones, Centro de excelencia de Data Insights, Microsoft Corporation

**Fecha de publicación:** enero de 2015

**Revisión:** 1.0

**Descarga:** [Procesado de eventos en tiempo real con Microsoft Azure Stream Analytics](https://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Obtener ayuda
Para más ayuda, pruebe la [página de preguntas y respuestas de Microsoft sobre Azure Stream Analytics](/answers/topics/azure-stream-analytics.html)

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalación de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Azure Stream Analytics](/rest/api/streamanalytics/)