---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/01/2021
ms.author: tamram
ms.openlocfilehash: 16da73fe453760e2dc84e7d683c3a16c12b8a06f
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2021
ms.locfileid: "106218621"
---
| Recurso | Destino |
|-|-|
| Tamaño máximo de contenedor de un blob | Igual que la capacidad máxima de la cuenta de almacenamiento |
| Número máximo de bloques en un blob en bloques o blob en anexos | 50 000 bloques |
| Tamaño máximo de un bloque en un blob en bloques | 4000 MiB |
| Tamaño máximo de un blob en bloques | 50 000 x 4000 MiB (190,7 TiB, aproximadamente) |
| Tamaño máximo de un bloque en un blob en anexos | 4 MiB |
| Tamaño máximo de un blob en anexos | 50 000 x 4 MiB (195 GiB, aproximadamente) |
| Tamaño máximo de un blob en páginas | 8 TiB<sup>2</sup> |
| Número máximo de directivas de acceso almacenadas por contenedor de blobs | 5 |
| Velocidad de solicitudes de destino para un solo blob | Hasta 500 solicitudes por segundo |
| Rendimiento de destino para un blob en una sola página | Hasta 60 MiB por segundo<sup>2</sup> |
| Rendimiento de destino para un blob en un solo bloque | Hasta los límites de entrada/salida de cuenta de almacenamiento<sup>1</sup> |

<sup>1</sup> El rendimiento de un único blob depende de varios factores, incluidos, sin limitación: simultaneidad, tamaño de la solicitud, nivel de rendimiento, velocidad de origen de las cargas y destino de las descargas. Para aprovechar las mejoras de rendimiento de [blobs en bloques de alto rendimiento](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/), cargue blobs o bloques más grandes. En concreto, llame a la operación [Put Blob](/rest/api/storageservices/put-blob) o [Put Block](/rest/api/storageservices/put-block) con un tamaño de blob o de bloque superior a 4 MiB para las cuentas de almacenamiento estándar. Para los blobs en bloques prémium o las cuentas de almacenamiento de Data Lake Storage Gen2, use un tamaño de bloque o de blob superior a 256 KiB.

<sup>2</sup> Los blobs en páginas aún no se admiten en las cuentas que tienen el valor **Espacio de nombres jerárquico**.

En la tabla siguiente se describen los tamaños máximos de bloque y blob que permite la versión del servicio.

| Versión del servicio | Tamaño máximo de bloque (a través de Put Block) | Tamaño máximo de blob (a través de Put Block List) | Tamaño máximo del blob a través de una operación de escritura única (a través de Put Blob) |
|-|-|-|-|
| Versión 2019-12-12 y posteriores | 4000 MiB | Aproximadamente 190,7 TiB (4000 MiB x 50 000 bloques) | 5000 MiB (versión preliminar) |
| De la versión 2016-05-31 a la versión 2019-07-07 | 100 MiB | Aproximadamente 4,75 TiB (100 MiB x 50 000 bloques) | 256 MiB |
| Versiones anteriores a 2016-05-31 | 4 MiB | Aproximadamente 195 GiB (4 MiB x 50 000 bloques) | 64 MiB |
