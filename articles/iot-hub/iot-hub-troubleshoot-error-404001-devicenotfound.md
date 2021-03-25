---
title: Solución de problemas del error 404001 DeviceNotFound de Azure IoT Hub
description: Corrección del error 404001 DeviceNotFound
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 15aa21c2ec2c11bb251f7208fd22c92ceb859d6d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "76960305"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

En este artículo se describen las causas y las soluciones de los errores **404001 DeviceNotFound**.

## <a name="symptoms"></a>Síntomas

Durante una comunicación de la nube al dispositivo (C2D), como el mensaje C2D, la actualización gemela o el método directo, la operación produce un error **404001 DeviceNotFound**.

## <a name="cause"></a>Causa

No se pudo realizar la operación porque IoT Hub no encuentra el dispositivo. El dispositivo no está registrado o está deshabilitado.

## <a name="solution"></a>Solución

Registre el identificador de dispositivo que usó e inténtelo de nuevo.