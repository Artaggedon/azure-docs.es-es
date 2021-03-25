---
title: Solución de problemas del error 409001 DeviceAlreadyExists de Azure IoT Hub
description: Corrección del error 409001 DeviceAlreadyExists
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 93ab2ecc8e820c461a7c79082ac1d50c24f0ba8b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "76960293"
---
# <a name="409001-devicealreadyexists"></a>409001 DeviceAlreadyExists

En este artículo se describen las causas y las soluciones de los errores **409001 DeviceAlreadyExists**.

## <a name="symptoms"></a>Síntomas

Al intentar registrar un dispositivo en IoT Hub, la solicitud genera un error **409001 DeviceAlreadyExists**.

## <a name="cause"></a>Causa

Ya existe un dispositivo con el mismo identificador de dispositivo en el centro de IoT. 

## <a name="solution"></a>Solución

Use un identificador de dispositivo distinto e inténtelo de nuevo.