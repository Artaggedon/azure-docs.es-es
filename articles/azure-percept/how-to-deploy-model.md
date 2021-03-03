---
title: Implementación de un modelo de IA de visión en Azure Percept DK
description: Aprenda a implementar un modelo de IA de visión en Azure Percept DK desde Azure Percept Studio
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to
ms.openlocfilehash: 6e1ed39edfd3c395fbc3e4d26a4aa358d48a1d5b
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660601"
---
# <a name="deploy-a-vision-ai-model-to-your-azure-percept-dk"></a>Implementación de un modelo de IA de visión en Azure Percept DK

Siga esta guía para implementar un modelo de IA de visión en Azure Percept DK desde Azure Percept Studio.

## <a name="prerequisites"></a>Requisitos previos

- Kit de desarrollo Azure Percept DK
- [Suscripción de Azure](https://azure.microsoft.com/free/)
- [Experiencia de instalación de Azure Percept DK](./quickstart-percept-dk-set-up.md): ya ha conectado el dispositivo DevKit a una red Wi-Fi, creado una instancia de IoT Hub y conectado el dispositivo DevKit a esa instancia.

## <a name="model-deployment"></a>Implementación de modelo

1. Encienda el dispositivo DevKit.

1. Vaya a [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. En el lado izquierdo de la página de información general, haga clic en **Dispositivos**.

    :::image type="content" source="./media/how-to-deploy-model/overview-devices-inline.png" alt-text="Pantalla de información general de Azure Percept Studio." lightbox="./media/how-to-deploy-model/overview-devices.png":::

1. Seleccione el dispositivo DevKit de la lista.

    :::image type="content" source="./media/how-to-deploy-model/select-device.png" alt-text="Lista de dispositivos Percept.":::

1. En la página siguiente, haga clic en **Deploy a sample model** (Implementar un modelo de ejemplo) si desea implementar uno de los modelos de visión de ejemplo previamente entrenados. Si desea implementar una [solución de visión sin código personalizada](./tutorial-nocode-vision.md) existente, haga clic en **Deploy a Custom Vision project** (Implementar un proyecto de Custom Vision).

    :::image type="content" source="./media/how-to-deploy-model/deploy-model.png" alt-text="Lista de dispositivos Percept.":::

1. Si ha optado por implementar una solución sin código, seleccione el proyecto y la iteración del modelo que prefiera y haga clic en **Deploy** (Implementar).

1. Si ha optado por implementar un modelo de ejemplo, seleccione el modelo y haga clic en **Deploy to device** (Implementar en el dispositivo).

    :::image type="content" source="./media/how-to-deploy-model/select-sample-model.png" alt-text="Lista de dispositivos Percept.":::

1. Cuando la implementación del modelo se realice correctamente, recibirá un mensaje de estado en la esquina superior derecha de la pantalla. Para ver la inferencia del modelo en acción, haga clic en el vínculo **View stream** (Ver flujo) en el mensaje de estado para ver la secuencia de vídeo RTSP del módulo de sistema de visión de su dispositivo DevKit.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo ver los [datos de telemetría de Azure Percept DK](how-to-view-telemetry.md).