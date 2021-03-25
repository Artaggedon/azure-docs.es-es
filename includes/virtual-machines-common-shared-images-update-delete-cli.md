---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 4392e7f146f13e581f722b94f13038ad8abff0ba
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102245098"
---
## <a name="update-resources"></a>Actualización de recursos

Existen algunas limitaciones en lo que se puede actualizar. Se pueden actualizar los siguientes elementos: 

Galería de imágenes compartidas:
- Descripción

Definición de la imagen:
- vCPU recomendadas:
- Memoria recomendada
- Descripción
- Fecha final del ciclo de vida

Versión de la imagen:
- Recuento de réplicas regionales
- Regiones de destino
- Exclusión de la versión más reciente
- Fecha final del ciclo de vida

Si planea agregar regiones de réplica, no elimine la imagen administrada de origen. La imagen administrada de origen es necesaria para replicar la versión de la imagen a regiones adicionales. 

Actualice la descripción de una galería con [az sig update](/cli/azure/sig#az-sig-update). 

```azurecli-interactive
az sig update \
   --gallery-name myGallery \
   --resource-group myGalleryRG \
   --set description="My updated description."
```


Actualice la descripción de una definición de imagen con [az sig image-definition update](/cli/azure/sig/image-definition#az-sig-image-definition-update).

```azurecli-interactive
az sig image-definition update \
   --gallery-name myGallery\
   --resource-group myGalleryRG \
   --gallery-image-definition myImageDefinition \
   --set description="My updated description."
```

Actualice una versión de imagen para agregar una región para replicar con [az sig image-version update](/cli/azure/sig/image-definition#az-sig-image-definition-update). Este cambio tardará un poco, a medida que la imagen se replica en la nueva región.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --add publishingProfile.targetRegions  name=eastus
```

En este ejemplo se muestra cómo usar [az sig image-version update](/cli/azure/sig/image-definition#az-sig-image-definition-update) para impedir que esta versión de imagen se use como la imagen *más reciente*.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --set publishingProfile.excludeFromLatest=true
```

En este ejemplo se muestra cómo usar [az sig image-version update](/cli/azure/sig/image-definition#az-sig-image-definition-update) para que esta versión de imagen se considere como imagen *más reciente*.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --set publishingProfile.excludeFromLatest=false
```

## <a name="delete-resources"></a>Eliminar recursos

Debe eliminar los recursos en orden inverso, eliminando primero la versión de imagen. Después de eliminar todas las versiones de imagen, puede eliminar la definición de imagen. Después de eliminar todas las definiciones de imagen, puede eliminar la galería. 

Elimine una versión de imagen con [az sig image-version delete](/cli/azure/sig/image-version#az-sig-image-version-delete).

```azurecli-interactive
az sig image-version delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 
```

Elimine una definición de imagen con [az sig image-definition delete](/cli/azure/sig/image-definition#az-sig-image-definition-delete).

```azurecli-interactive
az sig image-definition delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition
```


Elimine una galería de imágenes mediante [az sig delete](/cli/azure/sig#az-sig-delete).

```azurecli-interactive
az sig delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery
```