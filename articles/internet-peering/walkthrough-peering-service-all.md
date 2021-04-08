---
title: Tutorial para asociados de Peering Service
titleSuffix: Azure
description: Tutorial para asociados de Peering Service
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: 292c850f5352c80112e3f0bcc5c868f8128819be
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97592320"
---
# <a name="peering-service-partner-walkthrough"></a>Tutorial para asociados de Peering Service

En esta sección se explican los pasos que debe seguir un proveedor para habilitar un emparejamiento directo para Peering Service.

## <a name="create-direct-peering-connection-for-peering-service"></a>Crear una conexión de emparejamiento directo para Peering Service
Los proveedores de servicios pueden ampliar su alcance geográfico mediante la creación de un nuevo emparejamiento directo compatible con Peering Service. Para realizar esta acción:
1. Asóciese al servicio Peering Service si todavía no lo ha hecho.
1. Siga las instrucciones de [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md). Asegúrese de que cumple el requisito de alta disponibilidad.
1. A continuación, siga los pasos de [Habilitación de Peering Service en un emparejamiento directo mediante el portal](howto-peering-service-portal.md).

## <a name="use-legacy-direct-peering-connection-for-peering-service"></a>Uso de una conexión de emparejamiento directo heredada para Peering Service
Si tiene el emparejamiento directo heredado que desea usar para la compatibilidad con Peering Service:
1. Asóciese al servicio Peering Service si todavía no lo ha hecho.
1. Siga las instrucciones de [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md). Si es necesario, solicite circuitos adicionales para satisfacer el requisito de alta disponibilidad.
1. A continuación, siga los pasos de [Habilitación de Peering Service en un emparejamiento directo mediante el portal](howto-peering-service-portal.md).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información acerca de la [directiva de emparejamiento](https://peering.azurewebsites.net/peering).
* Para más información sobre los pasos para configurar el emparejamiento directo con Microsoft, consulte el [Tutorial del emparejamiento directo](walkthrough-direct-all.md).
* Para más información sobre los pasos para configurar el emparejamiento de Exchange con Microsoft, consulte el [Tutorial del emparejamiento de Exchange](walkthrough-exchange-all.md).