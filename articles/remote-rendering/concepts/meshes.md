---
title: Mallas
description: Definición de mallas en el ámbito de Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: 33894e0d0b6f3ea50ffeac4f0c90c60f28e09f5e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99594525"
---
# <a name="meshes"></a>Mallas

## <a name="mesh-resource"></a>Recurso de malla

Las mallas son un [recurso compartido](../concepts/lifetime.md) inmutable, que solo se puede crear a través de la [conversión de datos](../how-tos/conversion/model-conversion.md). Las mallas contienen una o varias *submallas* junto con una representación física de [consultas de raycast](../overview/features/spatial-queries.md). Cada submalla hace referencia a un [material](materials.md) con el que se debe representar de forma predeterminada. Para colocar una malla en un espacio 3D, agregue un componente [MeshComponent](#meshcomponent) a una [entidad](entities.md).

### <a name="mesh-resource-properties"></a>Propiedades del recurso de malla

Las propiedades de clase `Mesh` son:

* **Materiales:** una matriz de materiales. Cada material se usa por medio de una submalla diferente. Varias entradas de la matriz pueden hacer referencia al mismo [material](materials.md). Estos datos no pueden modificarse en tiempo de ejecución.

* **Límites:** un rectángulo de selección alineado con el eje (AABB) del espacio local de los vértices de malla.

## <a name="meshcomponent"></a>MeshComponent

La clase `MeshComponent` se usa para colocar una instancia de un recurso de malla. Cada MeshComponent hace referencia a una sola malla. Puede invalidar los materiales que se usan para representar cada submalla.

### <a name="meshcomponent-properties"></a>Propiedades MeshComponent

* **Malla:** el recurso de malla utilizado por este componente.

* **Materiales:** la matriz de materiales especificados en el propio componente de malla. La matriz siempre tendrá la misma longitud que la matriz de *Materiales* en el recurso de malla. Los materiales que no se van a invalidar a partir del valor predeterminado de la malla se establecen en *NULL* en esta matriz.

* **UsedMaterials:** la matriz de materiales usados realmente para cada submalla. Serán idénticos a los datos de la matriz de *Materiales*, para valores que no son NULL. En caso contrario, contiene el valor de la matriz de *Materiales* en la instancia de malla.

### <a name="sharing-of-meshes"></a>Uso compartido de mallas

Un recurso `Mesh` se puede compartir entre varias instancias de componentes de malla. Además, el recurso `Mesh` que se asigna a un componente de malla se puede cambiar mediante programación en cualquier momento. En el siguiente código se muestra cómo clonar una malla:

```cs
Entity CloneEntityWithModel(RenderingConnection api, Entity sourceEntity)
{
    MeshComponent meshComp = sourceEntity.FindComponentOfType<MeshComponent>();
    if (meshComp != null)
    {
        Entity newEntity = api.CreateEntity();
        MeshComponent newMeshComp = api.CreateComponent(ObjectType.MeshComponent, newEntity) as MeshComponent;
        newMeshComp.Mesh = meshComp.Mesh; // share the mesh
        return newEntity;
    }
    return null;
}
```

```cpp
ApiHandle<Entity> CloneEntityWithModel(ApiHandle<RenderingConnection> api, ApiHandle<Entity> sourceEntity)
{
    if (ApiHandle<MeshComponent> meshComp = sourceEntity->FindComponentOfType<MeshComponent>())
    {
        ApiHandle<Entity> newEntity = *api->CreateEntity();
        ApiHandle<MeshComponent> newMeshComp = api->CreateComponent(ObjectType::MeshComponent, newEntity)->as<RemoteRendering::MeshComponent>();
        newMeshComp->SetMesh(meshComp->GetMesh()); // share the mesh
        return newEntity;
    }
    return nullptr;
}
```

## <a name="api-documentation"></a>Documentación de la API

* [Clase Mesh de C#](/dotnet/api/microsoft.azure.remoterendering.mesh)
* [Clase MeshComponent de C#](/dotnet/api/microsoft.azure.remoterendering.meshcomponent)
* [Clase Mesh de C++](/cpp/api/remote-rendering/mesh)
* [Clase MeshComponent de C++](/cpp/api/remote-rendering/meshcomponent)


## <a name="next-steps"></a>Pasos siguientes

* [Materiales](materials.md)