---
title: Reacción ante eventos de Azure SignalR Service
description: Use Azure Event Grid para suscribirse a los eventos de Azure SignalR Service. Estos eventos pueden desencadenar otros servicios de nivel inferior.
services: azure-signalr,event-grid
author: chenyl
ms.author: chenyl
ms.reviewer: zhshang
ms.date: 11/13/2019
ms.topic: conceptual
ms.service: signalr
ms.openlocfilehash: 77c8887ac19c6ce4c7d83734bdd2b44d9213914d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92151108"
---
# <a name="reacting-to-azure-signalr-service-events"></a>Reacción ante eventos de Azure SignalR Service

Los eventos de Azure SignalR Service permiten a las aplicaciones reaccionar a las conexiones de cliente que se conectan o desconectan mediante modernas arquitecturas sin servidor. Esto se consigue sin necesidad de código complejo ni de servicios de sondeo costosos e ineficientes.  En su lugar, se insertan eventos mediante [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) a los suscriptores como [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) o incluso su propio cliente de escucha http. Con Azure SignalR, solo paga por lo que consume.

Los eventos de Azure SignalR Service se envían de forma confiable al servicio Event Grid, que proporciona servicios de entrega confiables para sus aplicaciones mediante directivas de reintento enriquecidas y la entrega de mensajes fallidos. Para más información, consulte [Entrega y reintento de entrega de mensajes de Event Grid](../event-grid/delivery-and-retry.md).

![Modelo de Event Grid](/azure/event-grid/media/overview/functional-model.png)

## <a name="serverless-state"></a>Estado sin servidor
Los eventos de Azure SignalR Service solo están activos si las conexiones de cliente están en estado sin servidor. Si un cliente no se enruta a un servidor concentrador, entra en un estado sin servidor. El modo clásico solo funciona si el centro al que se conectan las conexiones de cliente no tiene un servidor concentrador. Es preferible usar el modo sin servidor como procedimiento recomendado. Para más información acerca del modo de servicio, consulte [cómo elegir el modo de servicio](https://github.com/Azure/azure-signalr/blob/dev/docs/faq.md#what-is-the-meaning-of-service-mode-defaultserverlessclassic-how-can-i-choose).

## <a name="available-azure-signalr-service-events"></a>Eventos disponibles de Azure SignalR Service
Event Grid usa las [suscripciones a eventos](../event-grid/concepts.md#event-subscriptions) para enrutar los mensajes de eventos a los suscriptores. Las suscripciones a eventos de Azure SignalR Service admiten dos tipos de eventos:  

|Nombre del evento|Descripción|
|----------|-----------|
|`Microsoft.SignalRService.ClientConnectionConnected`|Se genera cuando una conexión de cliente está conectada.|
|`Microsoft.SignalRService.ClientConnectionDisconnected`|Se genera cuando una conexión de cliente está desconectada.|

## <a name="event-schema"></a>Esquema del evento
Los eventos de Azure SignalR Service contienen toda la información necesaria para responder a cualquier cambio que se produzca en los datos. Puede identificar un evento de Azure SignalR Service por la propiedad eventType que empieza por "Microsoft.SignalRService". Puede encontrar información adicional sobre el uso de las propiedades de evento de Event Grid en [Esquema de eventos de Event Grid](../event-grid/event-schema.md).  

Este es un ejemplo de un evento conectado de conexión de cliente:
```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Para más información, consulte el [esquema de eventos de SignalR Service](../event-grid/event-schema-azure-signalr.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre Event Grid y pruebe los eventos de Azure SignalR Service:

> [!div class="nextstepaction"]
> [Pruebe una integración de Event Grid de ejemplo con Azure SignalR Service](./signalr-howto-event-grid-integration.md)
> [Acerca de Event Grid](../event-grid/overview.md)