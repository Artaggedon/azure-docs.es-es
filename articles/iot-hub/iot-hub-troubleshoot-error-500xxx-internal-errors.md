---
title: Solución de problemas de errores internos 500xxx de Azure IoT Hub
description: Corrección de errores internos 500xxx
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 1bf3c405f988edc2a75a2b54f664f7cbada7b158
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106060999"
---
# <a name="500xxx-internal-errors"></a>500xxx Errores internos

En este artículo se describen las causas y las soluciones de los **errores internos 500xxx**.

## <a name="symptoms"></a>Síntomas

La solicitud a IoT Hub produce un error que comienza con 500 y un tipo de "error de servidor". Las posibilidades son las siguientes:

* **500001 ServerError**: IoT Hub genera un error en el servidor.

* **500008 GenericTimeout**: IoT Hub no pudo completar la solicitud de conexión antes de agotar el tiempo de expiración.

* **ServiceUnavailable (sin código de error)** : IoT Hub detectó un error interno.

* **InternalServerError (sin código de error)** : IoT Hub detectó un error interno.

## <a name="cause"></a>Causa

Puede haber varias causas para una respuesta de error 500xxx. En todos los casos, lo más probable es que el problema sea transitorio. Aunque el equipo de IoT Hub trabaja duro para mantener [el SLA (Acuerdo de Nivel de Servicio)](https://azure.microsoft.com/support/legal/sla/iot-hub/), hay pequeños subconjuntos de nodos de IoT Hub que, en ocasiones, pueden experimentar errores transitorios. Cuando el dispositivo trata de conectarse a un nodo que está teniendo problemas, recibirá este error.

## <a name="solution"></a>Solución

Para mitigar los errores 500xxx, ejecute un reintento desde el dispositivo. Para [administrar automáticamente los reintentos](./iot-hub-reliability-features-in-sdks.md#connection-and-retry), asegúrese de usar la versión más reciente de los [SDK de Azure IoT](./iot-hub-devguide-sdks.md). Consulte [Control de errores transitorios](/azure/architecture/best-practices/transient-faults) para ver procedimientos recomendados sobre reintentos y cómo controlar errores transitorios.  Si el problema persiste, compruebe [Resource Health](./iot-hub-azure-service-health-integration.md#check-health-of-an-iot-hub-with-azure-resource-health) y [Estado de Azure](https://status.azure.com/) para ver si IoT Hub tiene un problema conocido. También puede usar la [característica de conmutación por error manual](./tutorial-manual-failover.md). Si no hay ningún problema conocido y el problema continúa, [póngase en contacto con el equipo de soporte técnico](https://azure.microsoft.com/support/options/) para que lo investigue.
