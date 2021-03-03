---
title: 'Introducción a las máquinas virtuales de la serie HBv2: Azure Virtual Machines | Microsoft Docs'
description: Obtenga información sobre el tamaño de las máquinas virtuales de la serie HBv2 en Azure.
services: virtual-machines
author: vermagit
tags: azure-resource-manager
ms.service: virtual-machines
ms.subservice: hpc
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/28/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: 6648f77c5eacf40f848bc9b24aa6e257d8adf626
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101674638"
---
# <a name="hbv2-series-virtual-machine-overview"></a>Introducción a las máquinas virtuales de la serie HBv2 

 
Maximizar el rendimiento de la aplicación de proceso de alto rendimiento (HPC) en AMD EPYC requiere un enfoque bien meditado con respecto a la ubicación de los procesos y de la memoria. A continuación se describen la arquitectura de AMD EPYC y nuestra implementación de la misma en Azure para aplicaciones HPC. Vamos a utilizar el término **pNUMA** para referirnos a un dominio físico de NUMA y **vNUMA** para referirnos a un dominio virtualizado de NUMA. 

Físicamente, los servidores de la [serie HBv2](../../hbv2-series.md) tienen dos CPU EPYC 7742 con 64 núcleos cada una, lo que hace un total de 128 núcleos físicos. Estos 128 núcleos se dividen en 32 dominios de pNUMA (16 por socket), cada uno de los cuales tiene cuatro núcleos y AMD lo denomina **Complejo de núcleo** (Core Complex, o **CCX**). Cada CCX tiene su propia memoria caché L3, que es el modo en que un sistema operativo verá un límite pNUMA/vNUMA. Cuatro CCX adyacentes comparten acceso a dos canales de DRAM física. 

Para dejar margen suficiente para que el hipervisor de Azure trabaje sin interferir en la máquina virtual, reservamos los dominios 0 y 16 físicos de pNUMA (es decir, el primer CCX de todos los sockets de la CPU). Los treinta dominios restantes de pNUMA se asignan a la máquina virtual en la que se convierten en vNUMA. Por lo tanto, la máquina virtual verá:

`(30 vNUMA domains) * (4 cores/vNUMA) = 120` núcleos por máquina virtual 

La propia máquina virtual no sabe de que los dominios 0 y 16 de pNUMA están reservados. Enumera el vNUMA que ve como 0-29, con 15 vNUMA por socket de forma simétrica, 0-14 de vNUMA en vSocket 0 y 15-29 de vNUMA en vSocket 1. En la siguiente sección, encontrará las instrucciones necesarias para ejecutar mejor aplicaciones de MPI en este diseño NUMA asimétrico. 

El anclaje de procesos funcionará en las máquinas virtuales de la serie HBv2 porque exponemos el silicio subyacente tal cual está a la máquina virtual invitada. Se recomienda encarecidamente anclar los procesos para disfrutar de una coherencia y un rendimiento óptimos. 


## <a name="hardware-specifications"></a>Especificaciones del hardware 

| Especificaciones del hardware          | Máquina virtual de la serie HBv2                   | 
|----------------------------------|----------------------------------|
| Núcleos                            | 120 (SMT deshabilitado)               | 
| CPU                              | AMD EPYC 7742                    | 
| Frecuencia de CPU (no AVX)          | Aprox. 3,1 GHz (solo + todos los núcleos)    | 
| Memoria                           | 4 GB/núcleo (480 GB en total)         | 
| Disco local                       | NVMe de 960 GB (bloque), SSD de 480 GB (archivo de paginación) | 
| Infiniband                       | EDR Mellanox ConnectX-6 a 200 Gb/s | 
| Red                          | Ethernet a 50 Gb/s (40 Gb/s útiles) SmartNIC de segunda generación de Azure | 


## <a name="software-specifications"></a>Especificaciones de software 

| Especificaciones de software     | Máquina virtual de la serie HBv2                                            | 
|-----------------------------|-----------------------------------------------------------|
| Tamaño de trabajo de MPI máximo            | 36000 núcleos (300 máquinas virtuales en un solo conjunto de escalado de máquinas virtuales con singlePlacementGroup=true) |
| Compatibilidad con MPI                 | HPC-X, Intel MPI, OpenMPI, MVAPICH2, MPICH, Platform MPI  |
| Otros marcos       | Unified Communication X, libfabric, PGAS                  |
| Soporte técnico para Azure Storage       | Discos Estándar y Premium (un máximo de ocho discos)              |
| Soporte técnico de sistemas operativos para SRIOV RDMA   | CentOS/RHEL 7.6+, SLES 12 SP4+, WinServer 2016+           |
| Compatibilidad con Orchestrator        | CycleCloud, Batch                                         | 


## <a name="next-steps"></a>Pasos siguientes

- Aprenda sobre la [arquitectura de EPYC AMD](https://bit.ly/2Epv3kC) y las [arquitecturas de varios chips](https://bit.ly/2GpQIMb). Para más información, consulte la [guía de optimización de HPC para procesadores de AMD EPYC](https://bit.ly/2T3AWZ9).
- En los [blogs de la comunidad de Azure Compute Tech](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute), encontrará los anuncios más recientes y algunos ejemplos de HPC.
- Si desea una visión general de la arquitectura de la ejecución de cargas de trabajo de HPC, consulte [Informática de alto rendimiento (HPC) en Azure](/azure/architecture/topics/high-performance-computing/).