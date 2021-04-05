---
title: Trabajo con mapas de interiores en Creator de Azure Maps (versión preliminar)
description: En este artículo se presentan los conceptos que se aplican a los servicios de Creator de Azure Maps (versión preliminar).
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 4ab00317e71f832bb677c4c7587e2356a37cb7a1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96903571"
---
# <a name="creator-preview-for-indoor-maps"></a>Creator (versión preliminar) para mapas de interiores


> [!IMPORTANT]
> Los servicios de Creator de Azure Maps se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


En este artículo se presentan los conceptos y herramientas que se aplican a los servicios de Azure Maps Creator. Le recomendamos que lea este artículo antes de empezar a usar la API y el SDK de Azure Maps Creator.

Puede usar Creator para desarrollar aplicaciones con características de plano basadas en datos de planos interiores. En este artículo se describe el proceso de carga, conversión, creación y uso de los datos del plano. Todo el flujo de trabajo se ilustra en el siguiente diagrama.

![Flujo de trabajo de datos del plano de Creator](./media/creator-indoor-maps/workflow.png)

## <a name="create-azure-maps-creator-preview"></a>Creación de Creator de Azure Maps (versión preliminar) 

Para usar los servicios de Creator (versión preliminar), se deben crear en una cuenta de Azure Maps. Para obtener información sobre cómo crear Azure Maps Creator en Azure Maps, consulte [Administrar Azure Maps Creator](how-to-manage-creator.md).

## <a name="upload-a-drawing-package"></a>Carga de un paquete de dibujo

Creator (versión preliminar) recopila datos de mapas de interiores mediante la conversión de un paquete de dibujo cargado. El paquete de dibujo representa una instalación construida o remodelada. Para obtener información sobre los requisitos de los paquetes de dibujo, consulte [Requisitos de los paquetes de dibujo](drawing-requirements.md).

Use [Upload API de Azure Maps Data (versión preliminar)](/rest/api/maps/data/uploadpreview) para cargar un paquete de dibujo.  Tras una carga correcta, la API de carga de datos devolverá un identificador de datos de usuario (`udid`). El valor `udid` se usará en el paso siguiente para convertir el paquete cargado en datos del plano interior.

## <a name="convert-a-drawing-package"></a>Conversión de un paquete de dibujo

El [servicio de conversión de Azure Maps](/rest/api/maps/conversion) convierte un paquete de dibujo cargado en datos de un plano interior. El servicio de conversión también valida el paquete. Los problemas de validación se clasifican en dos tipos: errores y advertencias. Si se detecta algún error, se produce un error en el proceso de conversión. Si se detectan advertencias, la conversión se realizará correctamente. Sin embargo, se recomienda que revise y resuelva todas las advertencias. Una advertencia significa que parte de la conversión se ignoró o corrigió automáticamente. Si no se resuelven las advertencias, podrían producirse errores en los últimos procesos. Para obtener más información, consulte [Advertencias y errores del paquete de dibujo](drawing-conversion-error-codes.md).

Cuando se produce un error, el servicio de conversión proporciona un vínculo a la aplicación web independiente [Visualizador de errores de dibujo de Azure Maps](drawing-error-visualizer.md). Puede usar el Visualizador de errores de dibujo para inspeccionar los [errores y advertencias del paquete de dibujo](drawing-conversion-error-codes.md) que se produjeron durante el proceso de conversión. Después de corregir los errores, puede intentar cargar y convertir el paquete.

## <a name="create-indoor-map-data"></a>Creación de datos de los planos interiores

Creator (versión preliminar) de Azure Maps proporciona tres servicios:

* [Servicio de conjunto de datos](/rest/api/maps/dataset/createpreview).
Use el servicio de conjunto de datos para crear un conjunto de datos a partir de los datos de un paquete de dibujo convertido.
* [Servicio de conjunto de mosaicos](/rest/api/maps/tileset/createpreview).
Use el servicio de conjunto de mosaicos para crear una representación basada en vectores de un conjunto de datos. Las aplicaciones pueden utilizar un conjunto de mosaicos para presentar una vista visual basada en mosaicos del conjunto de datos.
* [Servicio de estado de características](/rest/api/maps/featurestate). Use el servicio de estado de características para admitir el estilo de plano dinámico. El estilo de plano dinámico permite a las aplicaciones reflejar eventos en tiempo real en espacios que haya proporcionado el sistema IoT.

### <a name="datasets"></a>Conjuntos de datos

Un conjunto de datos es una colección de características del plano interior. Las características de los planos interiores representan las instalaciones definidas en un paquete de dibujo convertido. Después de crear un conjunto de datos con el [servicio de conjunto de datos](/rest/api/maps/dataset/createpreview), puede crear cualquier número de [conjuntos de mosaicos](#tilesets) o [conjuntos de estados de características](#feature-statesets).

El [servicio de conjunto de datos](/rest/api/maps/dataset/createpreview) permite a los desarrolladores agregar o quitar en cualquier momento instalaciones de un conjunto de datos existente. Para obtener más información sobre cómo actualizar un conjunto de datos existente mediante la API, consulte las opciones para anexar contenido en el [servicio de conjunto de datos](/rest/api/maps/dataset/createpreview). Para obtener un ejemplo de cómo actualizar un conjunto de datos, consulte [Mantenimiento de datos](#data-maintenance).

### <a name="tilesets"></a>Conjunto de mosaicos

Un conjunto de mosaicos es una colección de datos vectoriales que representa un conjunto de mosaicos de cuadrícula uniformes. Los desarrolladores pueden usar el [servicio de conjunto de mosaicos](/rest/api/maps/tileset/createpreview) para crear conjuntos de mosaicos desde un conjunto de datos.

Para reflejar diferentes fases de contenido, puede crear varios conjuntos de mosaicos desde el mismo conjunto de datos. Por ejemplo, podría crear un conjunto de mosaicos con mobiliario y equipamiento, y otro conjunto de mosaicos sin mobiliario ni equipo.  Puede generar un conjunto de mosaicos con las actualizaciones de datos más recientes y otro sin las actualizaciones de datos más recientes.

Además de los datos vectoriales, el conjunto de mosaicos proporciona metadatos para la optimización de la representación de planos. Por ejemplo, los metadatos del conjunto de mosaicos contienen un nivel de zoom mínimo y máximo para ese conjunto de mosaicos. Asimismo, los metadatos también proporcionan un cuadro de límite que define la extensión geográfica del conjunto de mosaicos. El cuadro de límite permite a una aplicación establecer mediante programación el punto central correcto. Para obtener más información sobre los metadatos del conjunto de mosaicos, consulte [Tileset List API](/rest/api/maps/tileset/listpreview).

Una vez que se ha creado un conjunto de mosaicos, el [servicio Render V2](#render-v2-service) puede recuperarlo.

Si un conjunto de mosaicos se queda obsoleto y ya no es útil, puede eliminar ese conjunto de mosaicos. Para obtener más información sobre cómo eliminar conjuntos de mosaicos, consulte [Mantenimiento de datos](#data-maintenance).

>[!NOTE]
>Un conjunto de mosaicos es independiente del conjunto de datos del que se creó. Si crea conjuntos de mosaicos a partir de un conjunto de datos y después actualiza ese conjunto de datos, el conjunto de mosaicos no se actualizará. Para reflejar los cambios en un conjunto de datos, debe crear nuevos conjuntos de mosaicos. Del mismo modo, si elimina un conjunto de mosaicos, el conjunto de datos no se verá afectado.

### <a name="feature-statesets"></a>Conjuntos de estados de características

Los conjunto de estados de características son colecciones de propiedades dinámicas (*estados*) asignadas a las características del conjunto de datos, como salas o equipamiento. Un ejemplo de un *estado* podría ser la temperatura o la ocupación. Cada *estado* es un par clave-valor que contiene el nombre de la propiedad, el valor y la marca de tiempo de la última actualización.

El [servicio de estado de características](/rest/api/maps/featurestate/createstatesetpreview) le permite crear y administrar un conjunto de estados de características para un conjunto de datos. El conjunto de estados se define en uno o varios *estados*. Cada característica, como una sala, puede tener un *estado* adjunto.

El valor de cada *estado* en un conjunto de estados se puede actualizar o recuperar en dispositivos IoT u otras aplicaciones.  Por ejemplo, al usar [Feature State Update API](/rest/api/maps/featurestate/updatestatespreview), los dispositivos que miden la ocupación de espacio pueden publicar sistemáticamente el cambio de estado de una habitación.

Una aplicación puede usar un conjunto de estados de características para representar dinámicamente características en una instalación según su estado actual y su estilo de plano respectivo. Para obtener más información sobre el uso del conjunto de estados de características para dar estilo a las características en un plano de representación, consulte el [módulo del SDK web de interior](#indoor-maps-module).

>[!NOTE]
>Al igual que sucede con el conjunto de mosaicos, cambiar un conjunto de datos no afecta al conjunto de estados de características existente; igualmente, eliminar un conjunto de estados de características no tendrá ningún efecto en el conjunto de datos al que esté asociado.

## <a name="using-indoor-maps"></a>Uso de planos interiores

### <a name="render-v2-service"></a>Representación del servicio V2

[Get Map Tile API del servicio Render V2 (versión preliminar)](/rest/api/maps/renderv2/getmaptilepreview) de Azure Maps se ha ampliado para admitir los conjuntos de mosaicos de Creator (versión preliminar).

Get Map State Tile API del servicio Render V2 permite que las aplicaciones soliciten conjuntos de mosaicos. El conjunto de mosaicos se puede integrar en un control de plano o un SDK. Para obtener un ejemplo de un control de mapa que use el servicio Render V2, consulte el [módulo de Indoor Maps](#indoor-maps-module).

### <a name="web-feature-service-api"></a>API del servicio de características web

Los conjuntos de datos se pueden consultar mediante la [API del servicio de características web (WFS)](/rest/api/maps/wfs). WFS sigue las [características de Open Geospatial Consortium API](http://docs.opengeospatial.org/DRAFTS/17-069r1.html). Asimismo, la API de WFS le permite consultar características en el propio conjunto de datos. Por ejemplo, puede usar WFS para buscar todas las salas de reuniones de tamaño medio de una instalación y una planta determinadas.

### <a name="indoor-maps-module"></a>Módulo de Indoor Maps

El [SDK web de Azure Maps](./index.yml) incluye el módulo Indoor Maps. Este módulo ofrece funcionalidades extendidas para la biblioteca del *control de mapa* de Azure Maps. El módulo Mapas de interiores representa los mapas de interiores creados en Creator (versión preliminar). Integra widgets como el *selector de piso*, que ayuda a los usuarios a visualizar las distintas plantas.

Igualmente, el módulo Indoor Maps permite crear aplicaciones web que integren datos de planos interiores con otros [servicios de Azure Maps](./index.yml). Las configuraciones de aplicaciones más comunes pueden incluir la adición de conocimiento a planos interiores desde otros planos, como carreteras, imágenes, el tiempo y el tránsito.

El módulo Indoor Maps también admite el estilo dinámico del plano. Para ver un tutorial paso a paso sobre cómo implementar el estilo dinámico del conjunto de estados de características en una aplicación, consulte [Cómo usar el módulo Indoor Map](how-to-use-indoor-module.md).

### <a name="azure-maps-integration"></a>Integración de Azure Maps

Cuando empiece a desarrollar soluciones para planos interiores, puede descubrir diferentes maneras de integrar las capacidades de Azure Maps existentes. Por ejemplo, los escenarios de seguridad o de seguimiento de recursos se pueden implementar mediante el uso de [Geofence API de Azure Maps](/rest/api/maps/spatial/postgeofence) con los planos interiores de Creator. Geofence API se puede usar para determinar, por ejemplo, si un trabajador entra o sale de determinadas áreas interiores. Puede obtener [aquí](tutorial-iot-hub-maps.md) más información sobre cómo conectar Azure Maps con la telemetría de IoT.

### <a name="data-maintenance"></a>Mantenimiento de datos

 Las instancias de List, Update y Delete API de Creator de Azure Maps le permiten enumerar, actualizar y eliminar conjuntos de datos, conjuntos de mosaicos y conjuntos de estados de características.

>[!NOTE]
>Siempre que revise una lista de elementos y decida eliminarlos, debe tener en cuenta el impacto que esa eliminación tendrá en todas las aplicaciones o API dependientes. Por ejemplo, si tiene que eliminar un conjunto de mosaicos que esté usando una aplicación mediante [Get Map Tile API de Render V2](/rest/api/maps/renderv2/getmaptilepreview), tenga en cuenta que si elimina ese conjunto de mosaicos se producirá un error de aplicación al representar ese conjunto de mosaicos.

### <a name="example-updating-a-dataset"></a>Ejemplo: actualización de un conjunto de datos

En el ejemplo siguiente se muestra cómo actualizar un conjunto de datos, crear un nuevo conjunto de mosaicos y eliminar un conjunto de mosaicos antiguo.

1. Siga los pasos de las secciones [Carga de un paquete de dibujo](#upload-a-drawing-package) y [Conversión de un paquete de dibujo](#convert-a-drawing-package) para cargar y convertir el nuevo paquete de dibujo.

2. Use [Dataset Create API](/rest/api/maps/dataset/createpreview) para anexar los datos convertidos al conjunto de datos existente.

3. Use [Tileset Create API](/rest/api/maps/tileset/createpreview) para generar un nuevo conjunto de mosaicos fuera del conjunto de datos actualizado. Guarde el nuevo valor de tilesetId para usarlo en el paso 4.

4. Actualice el identificador del conjunto de mosaicos en la aplicación para habilitar la visualización del conjunto de datos actualizado del campus. Si el antiguo conjunto de mosaicos ya no está en uso, puede eliminarlo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Creación de un mapa de interiores de Creator (versión preliminar)](tutorial-creator-indoor-maps.md)