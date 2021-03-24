---
title: Materiales de color
description: Describe el tipo de material de color.
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: fb26a669229ac140aba262032f8ce84ef9f37727
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99593390"
---
# <a name="color-materials"></a>Materiales de color

*Los materiales de color* son uno de los [tipos de material](../../concepts/materials.md) admitidos en Azure Remote Rendering. Se usan para las [mallas](../../concepts/meshes.md) que no deben recibir ningún tipo de iluminación, sino que deben tener una luminosidad completa en todo momento. Este podría ser el caso de los materiales "brillantes", como los paneles de automóviles, las bombillas o los datos que ya incorporan iluminación estática, como los modelos obtenidos mediante [fotogrametría](https://en.wikipedia.org/wiki/Photogrammetry).

Los materiales de color tienen una representación más eficiente que los [materiales de PBR](pbr-materials.md) debido a su modelo de sombreado más sencillo. También admiten diferentes modos de transparencia.

## <a name="common-material-properties"></a>Propiedades de material comunes

Estas propiedades son comunes para todos los materiales:

* **albedoColor:** este color se multiplica con otros colores, como *albedoMap* o los *colores del :::no-loc text="vertex":::* . Si el elemento *transparency* está habilitado en un material, el canal alfa se usa para ajustar la opacidad. En este contexto, `1` significa totalmente opaco y `0`, completamente transparente. El valor predeterminado es el blanco.

  > [!NOTE]
  > Dado que los materiales de color no reflejan el entorno, un material de color completamente transparente se vuelve invisible. Esto es diferente para los [materiales de PBR](pbr-materials.md).

* **albedoMap:** una [textura en 2D](../../concepts/textures.md) para valores albedo por píxel.

* **alphaClipEnabled** y **alphaClipThreshold:** si *alphaClipEnabled* es true, no se dibujarán todos los píxeles en que el valor alfa de albedo sea inferior a *alphaClipThreshold*. El recorte alfa se puede usar incluso sin habilitar la transparencia y se representa mucho más rápidamente. Sin embargo, los materiales recortados alfa se siguen representando más lentamente que los materiales completamente opacos. De forma predeterminada, el recorte alfa está deshabilitado.

* **textureCoordinateScale** y **textureCoordinateOffset:** la escala se multiplica en las coordenadas de textura UV y se le agrega el desplazamiento. Se puede usar para ajustar y desplazar las texturas. La escala predeterminada es (1, 1) y el desplazamiento es (0, 0).

* **useVertexColor:** si la malla contiene colores de :::no-loc text="vertex"::: y esta opción está habilitada, el color de los :::no-loc text="vertex"::: de las mallas se multiplica en *albedoColor* y *albedoMap*. De forma predeterminada, *useVertexColor* está deshabilitado.

* **isDoubleSided:** si el valor de doble cara está establecido en true, los triángulos con este material se representan aunque la cámara apunte a sus caras posteriores. Esta opción está deshabilitada de manera predeterminada. Consulte también [Representación :::no-loc text="Single-sided":::](single-sided-rendering.md).

* **TransparencyWritesDepth:** Si la marca TransparencyWritesDepth se establece en el material y el material es transparente, los objetos que usen este material también contribuirán al búfer de profundidad final. Vea la propiedad de material de color *transparencyMode* en la sección siguiente. La habilitación de esta característica se recomienda si el caso de uso necesita una [reproyección en fase más tardía](late-stage-reprojection.md) más plausible de escenas completamente transparentes. En el caso de escenas transparentes u opacas mixtas, este valor puede presentar artefactos de reproyección o un comportamiento de reproyección improbable. Por esta razón, la configuración predeterminada y recomendada para el caso de uso general es deshabilitar esta marca. Los valores de profundidad escritos se toman de la capa de profundidad por píxel del objeto más cercano a la cámara.

* **FresnelEffect:** Esta marca de material habilita el efecto [Fresnel](../../overview/features/fresnel-effect.md) aditivo en el material correspondiente. La apariencia del efecto se rige por los otros parámetros de Fresnel que se explican a continuación. 

* **FresnelEffectColor:** Color de Fresnel utilizado para este material. Solo es importante si se ha establecido el bit de efecto Fresnel en este material (consulte más arriba). Esta propiedad controla el color base del brillo de Fresnel (consulte una explicación completa en [Efecto Fresnel](../../overview/features/fresnel-effect.md)). Actualmente, solo son importantes los valores de canal RGB; se omitirá el valor alfa.

* **FresnelEffectExponent:** El exponente de Fresnel utilizado para este material. Solo es importante si se ha establecido el bit de efecto Fresnel en este material (consulte más arriba). Esta propiedad controla la distribución del brillo de Fresnel. El valor mínimo 0,01 provoca que el brillo se aplique por todo el objeto. El valor máximo 10,0 limita el brillo solo a los bordes visibles más oblicuos.

## <a name="color-material-properties"></a>Propiedades de material de color

Las propiedades siguientes son específicas de los materiales de color:

* **vertexMix:** este valor entre `0` y `1` especifica en qué medida el color del :::no-loc text="vertex"::: de una [malla](../../concepts/meshes.md) contribuye al color final. En el valor predeterminado de 1, el color del :::no-loc text="vertex"::: se multiplica por completo en el color albedo. Con un valor de 0, los colores de los :::no-loc text="vertex"::: se omiten por completo.

* **transparencyMode:** al contrario de los [materiales de PBR](pbr-materials.md), los materiales de color distinguen entre diferentes modos de transparencia:

  1. **Opaque:** el modo predeterminado deshabilita la transparencia. Sin embargo, el recorte alfa todavía es posible y, si es suficiente, es la opción preferible.
  
  1. **AlphaBlended:** este modo es similar al modo de transparencia para los materiales de PBR. Debe usarse para los materiales transparentes, como el cristal.

  1. **Additive:** este modo es el modo de transparencia más sencillo y eficaz. La contribución del material se agrega a la imagen representada. Este modo se puede usar para simular objetos brillantes (pero transparentes, igualmente), como los marcadores que se usan para resaltar objetos importantes.

## <a name="api-documentation"></a>Documentación de la API

* [Clase ColorMaterial de C#](/dotnet/api/microsoft.azure.remoterendering.colormaterial)
* [RenderingConnection.CreateMaterial() de C#](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.creatematerial)
* [Clase ColorMaterial de C++](/cpp/api/remote-rendering/colormaterial)
* [RenderingConnection::CreateMaterial() de C++](/cpp/api/remote-rendering/renderingconnection#creatematerial)

## <a name="next-steps"></a>Pasos siguientes

* [Materiales de PBR](pbr-materials.md)
* [Texturas](../../concepts/textures.md)
* [Mallas](../../concepts/meshes.md)