---
ms.openlocfilehash: 3196d377c09ca0cce24093710bc4be13fb21d2e3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99531059"
---
Asegúrese de que haya completado los pasos de [preparación de la supervisión de eventos](../../../detect-motion-emit-events-quickstart.md#prepare-to-monitor-events).

![Iniciar la supervisión del punto de conexión de eventos integrado](../../../media/quickstarts/start-monitoring-iothub-events.png)

> [!NOTE]
> Es posible que se le pida que proporcione información del punto de conexión integrado del centro de IoT. Para obtener esa información, en Azure Portal, vaya a su centro de IoT y busque la opción **Puntos de conexión integrados** en el panel de navegación izquierdo. Haga clic ahí y busque el **punto de conexión compatible con el centro de eventos** en la sección **Punto de conexión compatible con el centro de eventos**. Copie y use el texto del cuadro. El punto de conexión será similar a este:  
    ```
    Endpoint=sb://iothub-ns-xxx.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX;EntityPath=<IoT Hub name>
    ```
