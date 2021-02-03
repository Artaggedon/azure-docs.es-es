---
title: Migración a los contenedores OCR de Read v3.x
titleSuffix: Azure Cognitive Services
description: Aprenda a migrar a los contenedores OCR de Read v3.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 01/29/2021
ms.author: aahi
ms.openlocfilehash: baa91e21979022064aaf13aca6079f8d092d673e
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2021
ms.locfileid: "99221160"
---
# <a name="migrate-to-the-read-v3x-ocr-containers"></a>Migración a los contenedores OCR de Read v3.x

Si usa la versión 2 del contenedor OCR de Read de Computer Vision, utilice este artículo para aprender a actualizar la aplicación a fin de usar la versión 3.x del contenedor. 


## <a name="configuration-changes"></a>Cambios en la configuración

* `ReadEngineConfig:ResultExpirationPeriod` ya no se admite. El contenedor de Read tiene un trabajo de Cron compilado que quita los resultados y los metadatos asociados a una solicitud después de 48 horas.
* `Cache:Redis:Configuration` ya no se admite. La caché no se utiliza en los contenedores v3.x, por lo que no es necesario establecerla.

## <a name="api-changes"></a>Cambios de API

El contenedor de Read v3.2 emplea la versión 3 de la API Computer Vision y tiene los siguientes puntos de conexión:

* `/vision/v3.2-preview.1/read/analyzeResults/{operationId}`
* `/vision/v3.2-preview.1/read/analyze`
* `/vision/v3.2-preview.1/read/syncAnalyze`

Consulte la [guía de migración de la API REST Computer Vision v3](./upgrade-api-versions.md) para más información sobre cómo actualizar las aplicaciones para que usen la versión 3 de la API Read basada en la nube. Esta información también se aplica al contenedor. Tenga en cuenta que las operaciones de sincronización solo se admiten en contenedores.

## <a name="memory-requirements"></a>Requisitos de memoria

Los requisitos y las recomendaciones se basan en pruebas comparativas con una única solicitud por segundo, con una imagen de 8 MB de una carta comercial digitalizada que contiene 29 líneas y un total de 803 caracteres. En la tabla siguiente se describe la asignación mínima y recomendada de recursos para cada contenedor de lectura.

|Contenedor  |Mínima | Recomendado  |
|---------|---------|------|
|Read 3.2 versión preliminar | 8 núcleos, 16 GB de memoria         | 8 núcleos, 24 GB de memoria |

Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.

El núcleo y la memoria corresponden a los valores `--cpus` y `--memory`, que se usan como parte del comando de ejecución de Docker.

## <a name="storage-implementations"></a>Implementaciones de almacenamiento

>[!NOTE]
> MongoDB ya no se admite en las versiones 3.x del contenedor. En su lugar, los contenedores admiten Azure Storage y sistemas de archivos sin conexión.

| Implementación |    Argumentos en tiempo de ejecución necesarios |
|---------|---------|
|Nivel de archivo (predeterminado)    | No se requieren argumentos en tiempo de ejecución. Se usará el directorio `/share`. |
|Blob de Azure    | `Storage:ObjectStore:AzureBlob:ConnectionString={AzureStorageConnectionString}` |

## <a name="queue-implementations"></a>Implementaciones de colas

En la versión 3.x del contenedor, no se admite actualmente RabbitMQ. Las implementaciones de respaldo admitidas son:

| Implementación | Argumentos en tiempo de ejecución | Uso previsto |
|---------|---------|-------|
| En memoria (predeterminado) | No se requieren argumentos en tiempo de ejecución. | Desarrollo y pruebas |
| Colas de Azure | `Queue:Azure:ConnectionString={AzureStorageConnectionString}` | Producción |
| RabbitMQ    | No disponible | Producción |

Para agregar redundancia, el contenedor de Read v3.x emplea un temporizador de visibilidad para que las solicitudes se puedan procesar correctamente en caso de bloqueo, cuando se ejecuten en una instalación de varios contenedores. 

Establezca el temporizador con `Queue:Azure:QueueVisibilityTimeoutInMilliseconds`, que establece el tiempo que un mensaje es invisible cuando otro trabajo lo procesa. Para evitar que las páginas se procesen de forma redundante, se recomienda establecer el período de tiempo de espera en 120 segundos. El valor predeterminado es 30 segundos.

| Valor predeterminado | Valor recomendado |
|---------|---------|
| 30000 |    120000 |


## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](computer-vision-resource-container-config.md) (Configuración de contenedores) para ver las opciones de configuración.
* Revise [Introducción a Computer Vision](overview.md) para obtener más información sobre el reconocimiento de texto escrito a mano e impreso.
* Consulte [Computer Vision API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) para obtener más información acerca de los métodos que admite el contenedor.
* Consulte [Preguntas más frecuentes (P+F)](FAQ.md) para resolver problemas relacionados con la funcionalidad de Computer Vision.
* Uso de [Contenedores de Cognitive Services](../cognitive-services-container-support.md)