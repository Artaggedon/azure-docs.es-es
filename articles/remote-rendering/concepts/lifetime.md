---
title: Duración de objetos y recursos
description: En este artículo se explica la administración de la duración en los distintos tipos.
author: jakrams
ms.author: jakras
ms.date: 02/06/2020
ms.topic: conceptual
ms.openlocfilehash: 0945b35f7aff8e93a1a3ba23b89db288db3d8efa
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99593898"
---
# <a name="object-and-resource-lifetime"></a>Duración de objetos y recursos

Azure Remote Rendering distingue entre dos tipos: **objetos** y **recursos**.

## <a name="object-lifetime"></a>Duración de los objetos

Los *objetos* se consideran cosas que el usuario puede crear, modificar y destruir a su discreción. Los objetos se pueden duplicar libremente y cada instancia puede mutar con el tiempo. Por consiguiente, las [entidades](entities.md) y los [componentes](components.md) son objetos.

La duración de los objetos queda totalmente bajo control del usuario. Sin embargo, no está relacionada con la duración de la representación del lado cliente. Las clases como `Entity` y `Component` tienen una función `Destroy` a la que se debe llamar para desasignar el objeto en el host de representación remoto. Además, `Entity.Destroy()` destruirá la entidad, sus elementos secundarios y todos los componentes de esa jerarquía.

## <a name="resource-lifetime"></a>Duración de los recursos

Los *recursos* son cosas cuya duración administra completamente el host de representación remoto. Los recursos son referencias que se cuentan internamente. Se desasignan cuando nadie hace referencia a ellas.

La mayoría de los recursos solo se pueden crear indirectamente, normalmente mediante su carga desde un archivo. Cuando se carga el mismo archivo varias veces, Azure Remote Rendering devolverá la misma referencia y no volverá a cargar los datos.

Muchos recursos son inmutables, por ejemplo, las [mallas](meshes.md) y las [texturas](textures.md). Sin embargo, algunos recursos son mutables, por ejemplo, los [materiales](materials.md). Puesto que los recursos suelen compartirse, la modificación de un recurso puede afectar a varios objetos. Por ejemplo, cambiar el color de un material cambiará el color de todos los objetos que usan mallas, que a su vez hacen referencia a ese material.

### <a name="built-in-resources"></a>Recursos integrados

Azure Remote Rendering contiene algunos recursos integrados, que se pueden cargar anteponiendo su identificador respectivo con `builtin://` durante la llamada a `RenderingSession.Connection.LoadXYZAsync()`. Los recursos integrados disponibles se enumeran en la documentación de cada una de las características respectivas. Por ejemplo, en el capítulo sobre [Sky](../overview/features/sky.md) se enumeran las texturas cielo integradas.

## <a name="general-lifetime"></a>Duración general

La duración de todos los objetos y recursos se enlaza a la conexión. En un estado de desconexión, todo se descarta. Al volver a conectarse a la misma sesión, el gráfico de escena estará vacío y se purgarán todos los recursos.

En la práctica, la carga del mismo recurso en una sesión, después de una desconexión, suele ser más rápida que la primera vez. Esto se debe a que la mayoría de los recursos se deben descargar desde Azure Storage la primera vez, lo que no es necesario la segunda vez, lo que ahorra una cantidad considerable de tiempo.

## <a name="next-steps"></a>Pasos siguientes

* [Entidades](entities.md)
* [Componentes](components.md)
* [Modelos](models.md)
