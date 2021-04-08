---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4fe159660421113e0f0ac0586ae7e4a22d5bcda7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "77474174"
---
### <a name="query-the-storage-queue"></a>Consulta de la cola de almacenamiento

Puede usar el comando [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) para ver las colas de almacenamiento de la cuenta, como en el ejemplo siguiente:

```azurecli-interactive
az storage queue list --output tsv
```

La salida de este comando incluye una cola denominada `outqueue`, que es la cola que se creó cuando se ejecutó la función.

A continuación, use el comando [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) para ver los mensajes de esta cola, como se muestra en este ejemplo:

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

La cadena devuelta debe ser la misma que el mensaje que envió para probar la función.

> [!NOTE]  
> En el ejemplo anterior se descodifica la cadena devuelta desde base64. Esto se debe a que los enlaces de Queue Storage escriben y leen en Azure Storage como [cadenas de base64](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding).