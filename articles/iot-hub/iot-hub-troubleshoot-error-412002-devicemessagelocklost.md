---
title: Solución del error 412002 DeviceMessageLockLost de Azure IoT Hub
description: Corrección del error 412002 DeviceMessageLockLost
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: eb3d7f15109d3b217f651a7d927601ef7fae66c2
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061033"
---
# <a name="412002-devicemessagelocklost"></a>412002 DeviceMessageLockLost

En este artículo se describen las causas y las soluciones de los errores **412002 DeviceMessageLockLost**.

## <a name="symptoms"></a>Síntomas

Al intentar enviar un mensaje de la nube al dispositivo, se produce en la solicitud un error **412002 DeviceMessageLockLost**.

## <a name="cause"></a>Causa

Cuando un dispositivo recibe un mensaje de la nube al dispositivo desde la cola (por ejemplo, con [`ReceiveAsync()`](/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync)), IoT Hub bloquea el mensaje durante un tiempo de expiración de bloqueo de un minuto. Si el dispositivo intenta completar el mensaje después de que expire el tiempo de expiración de bloqueo, IoT Hub genera esta excepción.

## <a name="solution"></a>Solución

Si IoT Hub no recibe la notificación dentro de la duración del tiempo de expiración de bloqueo de un minuto, vuelve a establecer el mensaje en el estado *En cola*. El dispositivo puede intentar recibir el mensaje de nuevo. Para evitar que se produzca el error en el futuro, implemente la lógica del lado del dispositivo para completar el mensaje en un minuto tras recibirse. Este tiempo de espera de un minuto no se puede cambiar.