---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4ace70abe0112e0fe27d177c02bcb697746c92cc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "78262633"
---
### <a name="set-the-storage-account-connection"></a>Establecimiento de la conexión de la cuenta de almacenamiento

Abra el archivo local.settings.json y copie el valor de `AzureWebJobsStorage`, que es la cadena de conexión de la cuenta de almacenamiento. Establezca la variable de entorno `AZURE_STORAGE_CONNECTION_STRING` en la cadena de conexión con el siguiente comando de Bash:

```bash
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

La establecer la cadena de conexión en la variable de entorno `AZURE_STORAGE_CONNECTION_STRING`, puede acceder a la cuenta de almacenamiento sin tener que autenticarse cada vez.