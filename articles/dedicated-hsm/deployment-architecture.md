---
title: Arquitectura de implementación de HSM dedicado de Azure | Microsoft Docs
description: Consideraciones de diseño básica al usar HSM dedicado de Azure como parte de una arquitectura de aplicación
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/05/2020
ms.author: mbaldwin
ms.openlocfilehash: 6a0767b077886337331f24b15715247006f3fe2c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "94888902"
---
# <a name="azure-dedicated-hsm-deployment-architecture"></a>Arquitectura de implementación de Azure Dedicated HSM

HSM dedicado de Azure proporciona almacenamiento de claves criptográficas en Azure. Cumple estrictos requisitos de seguridad. Los clientes se beneficiarán del uso de HSM dedicado de Azure si:

* Cumplen la certificación FIPS 140-2 nivel 3
* Disponen de acceso exclusivo a HSM
* Tienen un control completo de sus dispositivos

Los dispositivos HSM se distribuyen entre los centros de datos de Microsoft y se pueden aprovisionar fácilmente como un par de dispositivos como la base de una solución de alta disponibilidad. También se puede implementar en las regiones para una solución resistente ante desastres. Las regiones con Dedicated HSM disponible actualmente se pueden comprobar desde la [página de productos por región](https://azure.microsoft.com/global-infrastructure/services/?products=azure-dedicated-hsm). 

Cada una de estas regiones tiene bastidores HSM implementados en dos centros de datos independientes o en al menos dos zonas de disponibilidad independientes. Por ejemplo, Sudeste de Asia tiene tres zonas de disponibilidad y Este de EE. UU. 2 tiene dos. Hay un total de ocho regiones en Europa, Asia y Estados Unidos que ofrecen el servicio Dedicated HSM, y esto cambia a medida que se agregan nuevos bastidores HSM en nuevas regiones. Para más información sobre las regiones de Azure, consulte el sitio oficial de [información de las regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).
Algunos factores de diseño para cualquier solución basada en HSM dedicado son la ubicación y latencia, la alta disponibilidad y la compatibilidad con otras aplicaciones distribuidas.

## <a name="device-location"></a>Ubicación del dispositivo

La ubicación óptima del dispositivo HSM es cerca de las aplicaciones que realizan las operaciones criptográficas. La latencia en la región se espera que sea de un solo dígito de milisegundos. La latencia entre regiones podría ser entre cinco y diez veces superior a esta.

## <a name="high-availability"></a>Alta disponibilidad

Para lograr una alta disponibilidad, un cliente debe utilizar dos dispositivos HSM en una región que se configuren mediante software de Thales como un par de alta disponibilidad. Este tipo de implementación garantiza la disponibilidad de las claves si en un único dispositivo se produce un error que impide que se procesen las operaciones de clave. Además, también se reduce considerablemente el riesgo cuando se realiza el mantenimiento break-fix como la sustitución de la fuente de alimentación. Es importante para el diseño tener en cuenta cualquier tipo de error de nivel regional. Los errores en el nivel regional se pueden producir cuando hay catástrofes naturales como huracanes, inundaciones o terremotos. Estos tipos de eventos se deben mitigar mediante el aprovisionamiento de dispositivos HSM en otra región. Los dispositivos implementados en otra región pueden emparejarse mediante la configuración de software de Thales. Esto significa que la implementación mínima para una solución resistente ante desastres y de alta disponibilidad es de cuatro dispositivos HSM en dos regiones. La redundancia local y la redundancia entre regiones se puede utilizar como una base de referencia para agregar más implementaciones de dispositivos HSM para admitir la latencia, la capacidad o para cumplir otros requisitos específicos de la aplicación.

## <a name="distributed-application-support"></a>Compatibilidad con las aplicaciones distribuidas

Los dispositivos HSM dedicado se implementan normalmente para respaldar a las aplicaciones que necesitan realizar operaciones de recuperación de claves y de almacenamiento de claves. Los dispositivos HSM dedicado tienen diez particiones para admitir aplicaciones independientes. La ubicación del dispositivo debe basarse en una vista holística de todas las aplicaciones que necesitan usar el servicio.

## <a name="next-steps"></a>Pasos siguientes

Una vez que se haya determinado la arquitectura de la implementación, Thales proporcionará la mayor parte de las actividades de configuración para implementar esa arquitectura. Esto incluye la configuración de los dispositivos, así como escenarios de integración de aplicaciones. Para obtener más información, use el portal de [soporte al cliente de Thales](https://supportportal.gemalto.com/csm/) y descargue las guías de configuración y administración. El sitio de asociados de Microsoft tiene diversas guías de integración.
Se recomienda que todos los conceptos clave del servicio, como la alta disponibilidad y la seguridad, por ejemplo, se entiendan bien antes de aprovisionar los dispositivos o diseñar e implementar la aplicación.
Más temas de nivel de conceptos:

* [Alta disponibilidad](high-availability.md)
* [Seguridad física](physical-security.md)
* [Redes](networking.md)
* [Compatibilidad](supportability.md)
* [Supervisión](monitoring.md)
