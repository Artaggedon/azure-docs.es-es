---
title: archivo de inclusión
description: archivo de inclusión
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 08/13/2020
ms.author: danlep
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 173c9156f253e43111299b53287e97ab7b2c0aa5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92746936"
---
## <a name="create-azure-container-registry"></a>Creación de una instancia de Azure Container Registry

Antes de crear el registro de contenedor, necesita un *grupo de recursos* en donde implementarlo. Un grupo de recursos es una colección lógica en la que se implementan y administran todos los recursos de Azure.

Para crear un grupo de recursos, use el comando [az group create][az-group-create]. En el siguiente ejemplo, se crea un grupo de recursos denominado *myResourceGroup* en la región *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Cuando se haya creado el grupo de recursos, cree un registro de contenedor de Azure con el comando [az acr create][az-acr-create]. El nombre del registro de contenedor debe ser único dentro de Azure y contener entre 5 y 50 caracteres alfanuméricos. Reemplace `<acrName>` por un nombre único para el registro:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Esta es la salida parcial de una nueva instancia de Azure Container Registry denominada *mycontainerregistry082*:

```output
{
  "creationDate": "2020-07-16T21:54:47.297875+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry082",
  "location": "eastus",
  "loginServer": "mycontainerregistry082.azurecr.io",
  "name": "mycontainerregistry082",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

En el resto del tutorial se hace referencia a `<acrName>` como marcador de posición del nombre del registro de contenedor que eligió en este paso.

## <a name="log-in-to-container-registry"></a>Inicio de sesión en el registro de contenedor

Debe iniciar sesión en la instancia de Azure Container Registry antes de insertar imágenes en ella. Use el comando [az acr login][az-acr-login] para completar la operación. Debe especificar el nombre único que eligió para el registro de contenedor cuando lo creó.

```azurecli
az acr login --name <acrName>
```

Por ejemplo:

```azurecli
az acr login --name mycontainerregistry082
```

El comando devuelve `Login Succeeded` una vez completado:

```output
Login Succeeded
```

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-group-create]: /cli/azure/group#az-group-create
