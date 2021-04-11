---
title: Solución de problemas del error 403006 DeviceMaximumActiveFileUploadLimitExceeded de Azure IoT Hub
description: Corrección del error 403006 DeviceMaximumActiveFileUploadLimitExceeded
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 3a70c11fd4f2a0549370933c300f431a03ffcf1d
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106061305"
---
# <a name="403006-devicemaximumactivefileuploadlimitexceeded"></a>403006 DeviceMaximumActiveFileUploadLimitExceeded

En este artículo se describen las causas y las soluciones de los errores **403006 DeviceMaximumActiveFileUploadLimitExceeded**.

## <a name="symptoms"></a>Síntomas

Se produce un error en la solicitud de carga del archivo con el código de error **403006** y un mensaje que indica que el número de solicitudes de carga de archivos activas no puede ser superior a 10.

## <a name="cause"></a>Causa

Cada cliente de dispositivo se limita a [diez cargas de archivos simultáneas](./iot-hub-devguide-quotas-throttling.md#other-limits). 

Puede superar fácilmente el límite si el dispositivo no notifica a IoT Hub cuando se completan las cargas de archivos. Este problema suele deberse a una red en el dispositivo no confiable.

## <a name="solution"></a>Solución

Asegúrese de que el dispositivo pueda [notificar que se ha completado la carga de archivos de IoT Hub](./iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload) rápidamente. A continuación, intente [reducir el TTL del token de SAS para la configuración de la carga de archivos](iot-hub-configure-file-upload.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las cargas de archivos, consulte [Carga de archivos con IoT Hub](./iot-hub-devguide-file-upload.md) y [Configuración de las cargas de archivos de IoT Hub mediante Azure Portal](./iot-hub-configure-file-upload.md).