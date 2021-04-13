---
title: 'Inicio rápido: Configuración y habilitación de Defender-IoT-micro-agent para Azure RTOS'
description: Aprenda a incorporar y habilitar el microagente de Defender para IoT para el servicio Azure RTOS en Azure IoT Hub.
services: defender-for-iot
ms.topic: quickstart
ms.date: 01/24/2021
ms.openlocfilehash: 0575e9303068b203a5d8cd51c8a8cc1843de6f4d
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2021
ms.locfileid: "106384642"
---
# <a name="quickstart-defender-iot-micro-agent-for-azure-rtos-preview"></a>Inicio rápido: Defender-IoT-micro-agent para Azure RTOS (versión preliminar)

En este artículo se proporciona una explicación de los requisitos previos antes de empezar y se explica cómo habilitar Defender-IoT-micro-agent para el servicio Azure RTOS en IoT Hub. Si actualmente no tiene una instancia de IoT Hub, consulte [Creación de una instancia de IoT Hub mediante Azure Portal](../iot-hub/iot-hub-create-through-portal.md) para comenzar.

## <a name="prerequisites"></a>Requisitos previos 

### <a name="supported-devices"></a>Dispositivos compatibles

- Kit de detección ST STM32F746G
- NXP i.MX RT1060 EVK
- Microchip SAM E54 Xplained Pro EVK

Descargue, compile y ejecute uno de los archivos .zip para la placa y la herramienta específicas (IAR, semi IDE o PC) que prefiera del [recurso Defender-IoT-micro-agent para Azure RTOS de GitHub](https://github.com/azure-rtos/azure-iot-preview/releases).

### <a name="azure-resources"></a>Recursos de Azure

La siguiente fase de introducción es preparar los recursos de Azure. Necesitará un centro de IoT y, preferiblemente, un área de trabajo de Log Analytics. Para el centro de IoT, necesitará la cadena de conexión de IoT Hub para conectarse al dispositivo. 
  
### <a name="iot-hub-connection"></a>Conexión de IoT Hub

Se necesita una conexión de IoT Hub para comenzar. 

1. Abra **IoT Hub** en Azure Portal.

1. Vaya a **Dispositivos IoT**.

1. Seleccione **Crear**.

1. Copie la cadena de conexión de IoT en el [archivo de configuración](how-to-azure-rtos-security-module.md).

Las credenciales de conexión se toman de la configuración **HOST_NAME**, **DEVICE_ID** y **DEVICE_SYMMETRIC_KEY** de la aplicación de usuario.

Defender-IoT-micro-agent para Azure RTOS utiliza conexiones de middleware de Azure IoT basadas en el protocolo **MQTT**.

## <a name="next-steps"></a>Pasos siguientes

Continúe con el siguiente artículo para finalizar la configuración y la personalización de la solución.

> [!div class="nextstepaction"]
> [Configurar y personalizar Defender-IoT-micro-agent para Azure RTOS (versión preliminar)](how-to-azure-rtos-security-module.md)
