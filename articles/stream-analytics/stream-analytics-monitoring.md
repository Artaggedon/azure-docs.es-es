---
title: Descripción de la supervisión de trabajos en Azure Stream Analytics
description: En este artículo se describe cómo supervisar los trabajos de Azure Stream Analytics en Azure Portal.
author: sidramadoss
ms.author: sidram
ms.service: stream-analytics
ms.topic: how-to
ms.date: 03/08/2021
ms.custom: seodec18
ms.openlocfilehash: 6d3558511721a91c3a195cb510a1a00d5d8a9a51
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102487885"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Descripción de la supervisión del trabajo de Stream Analytics y cómo supervisar consultas

## <a name="introduction-the-monitor-page"></a>Introducción: La página de supervisión
Azure Portal muestra las métricas de rendimiento principales que se pueden usar para supervisar y solucionar problemas de rendimiento del trabajo y consultas. Para ver estas métricas, vaya al trabajo de Stream Analytics para el que le interesa ver las métricas y consulte la sección **Supervisión** de la página de información general.  

![Vínculo de supervisión de trabajos de Stream Analytics](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Se mostrará la siguiente ventana:

![Panel de supervisión de trabajos de Stream Analytics](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Métricas disponibles para Stream Analytics
| Métrica                 | Definición                               |
| ---------------------- | ---------------------------------------- |
| Eventos de entrada pendientes       | Número de eventos de entrada que están pendientes. Un valor distinto de cero para esta métrica implica que el trabajo no puede mantenerse al día con el número de eventos entrantes. Si este valor se aumenta lentamente o sistemáticamente y pasa a ser distinto de cero, debe escalar el trabajo horizontalmente. Para más información, visite [Descripción y ajuste de las unidades de streaming](stream-analytics-streaming-unit-consumption.md). |
| Errores de conversión de datos | Número de eventos de salida que no se pudieron convertir al esquema de salida previsto. La directiva de errores puede cambiarse a "Drop" para quitar los eventos que se producen en este escenario. |
| Porcentaje de uso de CPU (versión preliminar)       | Porcentaje de CPU que utiliza el trabajo. Aunque este valor sea muy alto (90 % o superior), no debe aumentar el número de SU en función de esta métrica por sí sola. Si aumenta el número de eventos de entrada pendientes o el retraso de la marca de agua, puede usar esta métrica de porcentaje de uso de CPU para determinar si la CPU es el cuello de botella. Es posible que esta métrica tenga picos intermitentes. Se recomienda realizar pruebas de escalado para determinar el límite superior del trabajo después del cual se incrementan las entradas pendientes o el retraso de la marca de agua debido a un cuello de botella de la CPU. |
| Primeros eventos de entrada       | Eventos cuya marca de tiempo de aplicación es anterior a su hora de llegada por más de cinco minutos. |
| Solicitudes de función con errores | Número de llamadas a la función Azure Machine Learning con error (si corresponde). |
| Eventos de función        | Número de eventos enviados a la función Azure Machine Learning (si corresponde). |
| Solicitudes de función      | Número de llamadas a la función Azure Machine Learning (si corresponde). |
| Errores de deserialización de entrada       | Número de eventos de entrada que no se pudieron deserializar.  |
| Bytes del evento de entrada      | Cantidad de datos recibidos por el trabajo de Stream Analytics, en bytes. Puede usarse para validar que los eventos que se envían al origen de entrada. |
| Eventos de entrada           | Número de registros deserializados de los eventos de entrada. Este recuento no incluye los eventos de entrada que producen errores de deserialización. Los mismos eventos los puede ingerir Stream Analytics varias veces en escenarios como recuperaciones internas y autocombinaciones. Por lo tanto, se recomienda no esperar que las métricas de los eventos de entrada y los eventos de salida coincidan si el trabajo tiene una consulta de "paso a través" simple. |
| Orígenes de entrada recibidos       | Número de mensajes recibidos por el trabajo. Para el centro de eventos, un mensaje es un único EventData. Para un blob, un mensaje es un único blob. Tenga en cuenta que se cuentan los orígenes de entrada antes de la deserialización. Si hay errores de deserialización, los orígenes de entrada pueden ser mayores que los eventos de entrada. En caso contrario, puede ser menor o igual que los eventos de entrada dado que cada mensaje puede contener varios eventos. |
| Eventos de entrada retrasada      | Eventos que llegaron más tarde que el período de tolerancia de llegada tardía configurado. Más información sobre los [puntos a tener en cuenta sobre el orden de eventos de Azure Stream Analytics](./stream-analytics-time-handling.md). |
| Eventos que no funcionan    | Número de eventos recibidos fuera de orden que se eliminan o se les asigna una marca de tiempo ajustada, según la Directiva de ordenación de eventos. Puede verse afectado por la configuración del ajuste de Período de tolerancia de fuera de servicio. |
| Eventos de salida          | Cantidad de datos enviados por el trabajo de Stream Analytics al destino de salida, en términos de recuento de eventos. |
| Errores de tiempo de ejecución         | El número total de errores relacionados con el procesamiento de consultas (sin incluir los errores encontrados durante la ingesta de eventos o los resultados de salida) |
| SU % uso       | Porcentaje de memoria que utiliza el trabajo. Si el uso de SU es constantemente superior al 80 % y tanto el retraso de la marca de agua como el número de eventos retrasados aumenta, considere la posibilidad de aumentar las unidades de streaming. Una alta utilización indica que el trabajo está usando casi el máximo de recursos asignados. |
| Retraso de la marca de agua       | Retraso máximo de la marca de agua en todas las particiones de todas las salidas del trabajo. |

Puede usar estas métricas para [supervisar el rendimiento del trabajo de Stream Analytics](./stream-analytics-set-up-alerts.md#scenarios-to-monitor). 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Personalización de la supervisión en Azure Portal
Puede ajustar el tipo de gráfico, las métricas que se muestran y el intervalo de tiempo en la configuración de Editar gráfico. Para obtener detalles, vea [Personalización de la supervisión](../azure-monitor/data-platform.md).

  ![Gráfico del tiempo de supervisión de consultas de Stream Analytics](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Salida más reciente
Otro punto de datos interesante para supervisar el trabajo es la hora de la salida más reciente, que se muestra en la página Introducción.
La hora es el tiempo de aplicación (es decir, el tiempo usando la marca de tiempo desde los datos de eventos) de la salida más reciente de su trabajo.

## <a name="get-help"></a>Obtener ayuda
Para más ayuda, pruebe nuestra [Página de preguntas y respuestas de Microsoft sobre Azure Stream Analytics](/answers/topics/azure-stream-analytics.html)

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalación de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Azure Stream Analytics](/rest/api/streamanalytics/)
