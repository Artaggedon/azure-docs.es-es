---
title: Depuración controlada por datos en Azure Stream Analytics
description: En este artículo se describe cómo solucionar problemas de un trabajo de Azure Stream Analytics mediante el uso del diagrama de trabajo y métricas en Azure Portal.
author: jseb225
ms.author: jeanb
ms.service: stream-analytics
ms.topic: how-to
ms.date: 05/01/2017
ms.openlocfilehash: 6d20454515088ccca87665d9b3b27c0d82c3cdf9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98020407"
---
# <a name="data-driven-debugging-by-using-the-job-diagram"></a>Depuración orientada a datos mediante el diagrama de trabajo

El diagrama de trabajo de la hoja **Supervisión** del portal de Azure puede ayudarle a visualizar su canalización de trabajo. Muestra las entradas, las salidas y los pasos de consulta. Puede usar el diagrama de trabajo para examinar las métricas de cada paso y así aislar más rápidamente el origen de un problema cuando tenga que solucionarlo.

## <a name="using-the-job-diagram"></a>Uso del diagrama de trabajo

En el portal de Azure, mientras está en un trabajo de Stream Analytics, en **SOPORTE TÉCNICO Y SOLUCIÓN DE PROBLEMAS**, seleccione **Diagrama de trabajo**:

![Diagrama de trabajo con métricas: ubicación](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

Seleccione cada paso de consulta para ver la sección correspondiente en un panel de edición de consultas. Se muestra un gráfico de métricas para el paso en la sección inferior de la página.

![Diagrama de trabajo con métricas: trabajo básico](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

Para ver las particiones de la entrada de Azure Event Hubs, seleccione **. . .** Aparece un menú contextual. También puede ver la fusión de entrada.

![Diagrama de trabajo con métricas: expandir partición](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

Para ver el gráfico de métricas para una sola partición, seleccione el nodo de la partición. Las métricas se muestran en la parte inferior de la página.

![Diagrama de trabajo con métricas: más métricas](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

Para ver el gráfico de métricas de una fusión, seleccione el nodo de fusión. El gráfico siguiente muestra que no se quitó ni ajustó ningún evento.

![Diagrama de trabajo con métricas: cuadrícula](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

Para ver los detalles del valor de métrica y la hora, apunte al gráfico.

![Diagrama de trabajo con métricas: mantener el mouse](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a>Solución de problemas mediante métricas

La métrica **QueryLastProcessedTime** indica cuando un paso específico recibió datos. Si examina la topología, puede volver atrás desde el procesador de salida para ver qué paso no recibe datos. Si un paso no recibe datos, vaya al paso de consulta justo delante de él. Compruebe que el paso de consulta anterior tenga una ventana de tiempo, y que se le haya pasado suficiente tiempo para generar datos. (Observe que las ventanas de tiempo están ajustadas a la hora.)
 
Si el paso de consulta anterior es un procesador de entrada, use las métricas de entrada como ayuda para responder a las siguientes preguntas ayude a responder las siguientes preguntas dirigidas. Pueden ayudarle a determinar si un trabajo recibe datos de sus orígenes de entrada. Si la consulta está particionada, examine cada partición.
 
### <a name="how-much-data-is-being-read"></a>¿Cuántos datos se están leyendo?

*   **InputEventsSourcesTotal** es el número de unidades de datos leídas. Por ejemplo, el número de blobs.
*   **InputEventsTotal** es el número de eventos leídos. Esta métrica está disponible por partición.
*   **InputEventsInBytesTotal** es el número de bytes leídos.
*   **InputEventsLastArrivalTime** se actualiza con el tiempo que está puesto en cola cada evento recibido.
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a>¿Avanza el tiempo? Si se leen eventos reales, es posible que no se emita la puntuación.

*   **InputEventsLastPunctuationTime** indica cuando se emitió una puntuación para mantener el tiempo avanzando. El flujo de datos puede bloquearse si no se emite la puntuación.
 
### <a name="are-there-any-errors-in-the-input"></a>¿Hay errores en la entrada?

*   **InputEventsEventDataNullTotal** es el número de eventos que tienen datos nulos.
*   **InputEventsSerializerErrorsTotal** es el número de eventos que no se pudieron deserializar correctamente.
*   **InputEventsDegradedTotal** es el número de eventos que tuvieron un problema distinto al de la deserialización.
 
### <a name="are-events-being-dropped-or-adjusted"></a>¿Se van a descartar o ajustar los eventos?

*   **InputEventsEarlyTotal** es el número de eventos con una marca de tiempo de la aplicación antes del límite máximo.
*   **InputEventsLateTotal** es el número de eventos con una marca de tiempo de la aplicación después del límite máximo.
*   **InputEventsDroppedBeforeApplicationStartTimeTotal** es el número de eventos descartados antes de la hora de inicio del trabajo.
 
### <a name="are-we-falling-behind-in-reading-data"></a>¿Estamos retrasados en la lectura de los datos?

*   **Input Events Backlogged (Total)** indica cuántos mensajes más es necesario leer para las entradas de Event Hubs y Azure IoT Hub. Cuando este número es mayor que 0, significa que el trabajo no puede procesar los datos a la misma velocidad con la que entran. En este caso, debe aumentar el número de unidades de streaming o asegurarse de que el trabajo se puede ejecutar en paralelo. Puede ver más información al respecto en la [página de paralelización de consultas](./stream-analytics-parallelization.md). 


## <a name="get-help"></a>Obtener ayuda
Para más ayuda, pruebe nuestra [página de preguntas y respuestas de Microsoft sobre Azure Stream Analytics](/answers/topics/azure-stream-analytics.html). 

## <a name="next-steps"></a>Pasos siguientes
* [¿Qué es Stream Analytics?](stream-analytics-introduction.md)
* [Introducción a Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalado de trabajos de Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia de lenguaje de consulta de Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Stream Analytics](/rest/api/streamanalytics/)