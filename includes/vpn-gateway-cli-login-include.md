---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 74d7bd087df4b00c0bafb5ec33fbbdfa5c57b379
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92755504"
---
Inicie sesión en la suscripción de Azure con el comando [az login](/cli/azure/) y siga las instrucciones que aparecen en pantalla. Para más información sobre el inicio de sesión, consulte [Introducción a la CLI de Azure](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Si tiene más de una suscripción de Azure, enumere las suscripciones de la cuenta.

```azurecli
az account list --all
```

Especifique la suscripción que desea usar.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```
