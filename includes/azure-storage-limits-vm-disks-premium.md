---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: ef18feb10dabc6a77e6512c6a32ad44b32c6e832
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "80334973"
---
**Discos de máquina virtual no administrados prémium: por límites de cuenta**

| Recurso | Límite |
| --- | --- |
| Capacidad total de disco por cuenta |35 TB |
| Capacidad total de instantáneas por cuenta |10 TB |
| Ancho de banda máximo por cuenta (entrada + salida<sup>1</sup>) |<=50 Gbps |

<sup>1</sup>*Entrada* hace referencia a todos los datos de las solicitudes que se envían a una cuenta de almacenamiento. *Salida* hace referencia a todos los datos de las respuestas que se reciben desde una cuenta de almacenamiento.

**Discos de máquina virtual no administrados prémium: por límites de disco**

| Tipo de disco de Premium Storage | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| Tamaño del disco |128 GiB |512 GiB |1024 GiB (1 TB) |2048 GiB (2 TB)|4095 GiB (4 TB)|
| Número máximo de IOPS por disco |500 |2,300 |5\.000 |7 500 |7 500 |
| Rendimiento máximo por disco |100 MB/s | 150 MB/s |200 MB/s |250 MB/s |250 MB/s |
| Número máximo de discos por cuenta de almacenamiento |280 |70 |35 | 17 | 8 |

**Discos de máquina virtual no administrados prémium: por límites de máquina virtual**

| Recurso | Límite |
| --- | --- |
| Número máximo de IOPS por máquina virtual |80 000 IOPS con máquina virtual GS5 |
| Rendimiento máximo por máquina virtual |2000 MB/s con máquina virtual GS5 |

