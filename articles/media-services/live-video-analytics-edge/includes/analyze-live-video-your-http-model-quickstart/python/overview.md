---
ms.openlocfilehash: 8124f31213de1bbb794968c616e6efb940ac36b2
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105582251"
---
> [!div class="mx-imgBorder"]
> :::image type="content" source="../../../media/quickstarts/overview-qs5.svg" alt-text="Flujo de señales":::

En este diagrama se muestra cómo fluyen las señales en este inicio rápido. Un [módulo perimetral](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) simula una cámara IP que hospeda un servidor de protocolo RTSP. Un nodo de [origen RTSP](../../../media-graph-concept.md#rtsp-source) extrae la fuente de vídeo de este servidor y envía fotogramas de vídeo al nodo del [procesador de extensiones HTTP](../../../media-graph-concept.md#http-extension-processor). 

El nodo de extensión HTTP desempeña el rol de un proxy. Se muestrean los fotogramas de vídeo entrantes que configuró mediante el campo `samplingOptions` y también se convierten los fotogramas de vídeo en el tipo de imagen especificado. Luego, retransmite la imagen a través de REST a otro módulo perimetral que ejecuta un modelo de IA detrás de un punto de conexión HTTP. En este ejemplo, el módulo perimetral se crea mediante el modelo [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx), que puede detectar muchos tipos de objetos. El nodo del procesador de extensión HTTP recopila los resultados de la detección y publica los eventos en el nodo del [receptor de IoT Hub](../../../media-graph-concept.md#iot-hub-message-sink), que posteriormente los envía a [IoT Edge Hub](../../../../../iot-fundamentals/iot-glossary.md#iot-edge-hub).

En este inicio rápido realizará lo siguiente:

1. Creará e implementará el grafo de elementos multimedia.
1. Interpretará los resultados.
1. Limpieza de recursos.