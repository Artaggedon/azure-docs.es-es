---
title: Desencadenador de Azure Event Hubs para Azure Functions
description: Obtenga información sobre el uso del desencadenador de Azure Event Hubs en Azure Functions.
author: craigshoemaker
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: c2b8302e64f7dcc657fd20ed5d918ed6816d750d
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102608927"
---
# <a name="azure-event-hubs-trigger-for-azure-functions"></a>Desencadenador de Azure Event Hubs para Azure Functions

En este artículo se explica cómo usar el desencadenador de [Azure Event Hubs](../event-hubs/event-hubs-about.md) para Azure Functions. Azure Functions admite los [enlaces de salida](functions-bindings-event-hubs-output.md) y desencadenador para Event Hubs.

Para obtener información sobre los detalles de instalación y configuración, vea la [información general](functions-bindings-event-hubs.md).

[!INCLUDE [functions-bindings-event-hubs-trigger](../../includes/functions-bindings-event-hubs-trigger.md)]

## <a name="hostjson-settings"></a>configuración de host.json

El archivo [host.json](functions-host-json.md#eventhub) contiene la configuración que controla el comportamiento del desencadenador del centro de eventos. Consulte la sección de [configuración de host.json](functions-bindings-event-hubs.md#hostjson-settings) para más información sobre las opciones de configuración disponibles.

## <a name="next-steps"></a>Pasos siguientes

- [Escribir eventos en una secuencia de eventos (enlace de salida)](./functions-bindings-event-hubs-output.md)
