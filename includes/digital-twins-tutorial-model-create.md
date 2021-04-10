---
author: baanders
description: 'Archivo de inclusión de Azure Digital Twins: tutorial de instrucciones para el modelo de la línea de comandos'
ms.topic: include
ms.date: 3/5/2021
ms.author: baanders
ms.openlocfilehash: a94b9304ecd6c6630f6ad45652e76d2879bbc1b8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103463926"
---
1. **Actualice el número de versión** para indicar que proporciona una versión más actualizada de este modelo. Para ello, cambie el *1* al final del valor `@id` por *2*. También servirá cualquier número mayor que el número de versión actual.
1. **Edite una propiedad**. Cambie el nombre de la propiedad `Humidity` por *HumidityLevel* (o algo diferente que prefiera. Si usa algo diferente a *HumidityLevel*, recuerde lo que ha usado y no lo cambie por *HumidityLevel* a lo largo de todo el tutorial).
1. **Agregue una propiedad**. Debajo de la propiedad `HumidityLevel` que termina en la línea 15, pegue el código siguiente para agregar una propiedad `RoomName` a la sala:

    :::code language="json" source="~/digital-twins-docs-samples/models/Room.json" range="16-20":::

1. **Agregue una relación**. Debajo de la propiedad `RoomName` que acaba de agregar, pegue el código siguiente para que este tipo de gemelo pueda formar relaciones *contiene* con otros gemelos:

    :::code language="json" source="~/digital-twins-docs-samples/models/Room.json" range="21-24":::

Cuando haya terminado, el modelo actualizado coincidirá con este:

:::code language="json" source="~/digital-twins-docs-samples/models/Room.json":::

Asegúrese de guardar el archivo antes de continuar.