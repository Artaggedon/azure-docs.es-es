---
title: Serie HB
description: Especificaciones de las máquinas virtuales de la serie HB.
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 10/09/2020
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: 964ed9b7d2631d4a545a53a0dc2d89d978406c49
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99096838"
---
# <a name="hb-series"></a>Serie HB

Las máquinas virtuales de la serie HB están optimizadas para aplicaciones que funcionan con ancho de banda de la memoria, como la dinámica de fluidos, el análisis explícito de elementos finitos y los modelos de clima. Las máquinas virtuales HB cuentan con 60 núcleos de procesador AMD EPYC 7551, 4 GB de RAM por núcleo de CPU y ningún multithreading simultáneo. Una máquina virtual HB proporciona hasta 260 GB/s de ancho de banda de memoria.

Las máquinas virtuales de la serie HB incluyen la característica MeliBox EDR InfiniBand a 100 GB/s. Estas máquinas virtuales están conectadas en un árbol FAT sin bloqueos para un rendimiento de RDMA optimizado y coherente. Estas máquinas virtuales admiten el enrutamiento adaptativo y el transporte conectado dinámico (DCT, además de los transportes estándar RC y UD). Estas características mejoran el rendimiento, la escalabilidad y la coherencia de las aplicaciones, y se recomienda totalmente su uso.

[ACU](acu.md): 199-216<br>
[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1 y 2<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>

| Size | vCPU | Procesador | Memoria (GiB) | Ancho de banda de memoria, en GB/s | Frecuencia de CPU base (GHz) | Frecuencia de todos los núcleos (GHz, pico) | Frecuencia de cada núcleo (GHz, pico) | Rendimiento de RDMA (GB/s) | Compatibilidad con MPI | Almacenamiento temporal (GiB) | Discos de datos máx. | vNIC Ethernet máx. |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYC 7551 | 240 | 263 | 2.0 | 2.55 | 2.55 | 100 | All | 700 | 4 | 8 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre la [configuración de máquinas virtuales](./workloads/hpc/configure.md), la [habilitación de InfiniBand](./workloads/hpc/enable-infiniband.md), la [configuración de MPI](./workloads/hpc/setup-mpi.md) y la optimización de las aplicaciones HPC para Azure en el archivo sobre [cargas de trabajo de HPC](./workloads/hpc/overview.md).
- En los [blogs de la comunidad de Azure Compute Tech](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute), encontrará los anuncios más recientes y algunos ejemplos y resultados de HPC.
- Si desea una visión general de la arquitectura de la ejecución de cargas de trabajo de HPC, consulte [Informática de alto rendimiento (HPC) en Azure](/azure/architecture/topics/high-performance-computing/).
- Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
