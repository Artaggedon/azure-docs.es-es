---
title: Modelos de ejemplo
description: Muestra los orígenes de los modelos de ejemplo.
author: florianborn71
ms.author: flborn
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: 6817601659c841ca98031f4e3e1590743bbed171
ms.sourcegitcommit: 7ec45b7325e36debadb960bae4cf33164176bc24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530542"
---
# <a name="sample-models"></a>Modelos de ejemplo

En este artículo se enumeran algunos recursos de datos de ejemplo que se pueden usar para probar el servicio Azure Remote Rendering.

## <a name="built-in-sample-model"></a>Modelo de ejemplo integrado

Proporcionamos un modelo de ejemplo integrado que se puede cargar en cualquier momento con la dirección URL **builtin://Engine**

![Modelo de ejemplo](./media/sample-model.png "Modelo de ejemplo")

Estadísticas del modelo:

| Nombre | Value |
|-----------|:-----------|
| [Tamaño de servidor necesario](../reference/vm-sizes.md) | estándar |
| Número de triángulos | 18,7 millones |
| Número de piezas móviles | 2073 |
| Número de materiales | 94 |

## <a name="third-party-data"></a>Datos de terceros

El Grupo Khronos mantiene un conjunto de modelos de ejemplo glTF para pruebas. ARR admite el formato glTF en texto ( *.gltf*) y en formato binario ( *.glb*). Se recomienda usar los modelos PBR para obtener los mejores resultados visuales:

* [Modelos de ejemplo glTF](https://github.com/KhronosGroup/glTF-Sample-Models)

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Representación de un modelo con Unity](../quickstarts/render-model.md)
* [Inicio rápido: Conversión de un modelo para su representación](../quickstarts/convert-model.md)
