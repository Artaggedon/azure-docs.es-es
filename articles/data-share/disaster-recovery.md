---
title: Recuperación ante desastres para Azure Data Share
description: Recuperación ante desastres para Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 07/30/2020
ms.openlocfilehash: 1d7c9935d7e0d6bb2d457aa4c08f9b2b3e5fa910
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92218704"
---
# <a name="disaster-recovery-for-azure-data-share"></a>Recuperación ante desastres para Azure Data Share

En este artículo se explica cómo configurar un entorno de recuperación ante desastres para Azure Data Share. Las interrupciones del centro de datos de Azure son poco frecuentes, pero pueden durar desde unos minutos hasta algunas horas. Las interrupciones del centro de datos pueden provocar interrupciones en los entornos que se basan en los datos compartidos por el proveedor de datos. Al seguir los pasos que se detallan en este artículo, los proveedores de datos pueden seguir compartiendo los datos con los consumidores de datos en caso de que se produzca una interrupción del centro de datos en la región primaria que hospeda el recurso compartido de datos. 

## <a name="achieving-business-continuity-for-azure-data-share"></a>Consecución de la continuidad empresarial para Azure Data Share

Para estar preparado para una interrupción del centro de datos, el proveedor de datos puede tener un entorno de recurso compartido de datos aprovisionado en una región secundaria. Se pueden tomar medidas para garantizar una conmutación por error fluida en caso de que se produzca una interrupción en el centro de datos. 

Los proveedores de datos pueden aprovisionar recursos secundarios de Azure Data Share en una región adicional. Estos recursos de Data Share se pueden configurar para incluir recursos compartidos y conjuntos de datos que existan en el recurso principal de Azure Data Share. Pueden invitar a los consumidores de datos a los recursos compartidos secundarios al configurar el entorno de recuperación ante desastres, o en un momento posterior (es decir, como parte de los pasos de una conmutación por error manual).

Si los consumidores de datos tienen una suscripción activa al recurso compartido en un entorno secundario aprovisionado con fines de recuperación ante desastres, estos pueden habilitar la programación de instantáneas como parte de una conmutación por error. Si los consumidores de datos no quieren suscribirse a una región secundaria para fines de recuperación ante desastres, se pueden invitar al recurso compartido de datos secundario en un momento posterior. 

Los consumidores de datos pueden tener una suscripción activa al recurso compartido que esté inactiva para propósitos de recuperación ante desastres, o bien los proveedores de datos pueden agregarlos en un momento posterior como parte de los procedimientos de conmutación por error manual. 

## <a name="related-information"></a>Información relacionada

- [Continuidad empresarial y recuperación ante desastres](../best-practices-availability-paired-regions.md)
- [Creación de alta disponibilidad a su estrategia de continuidad empresarial y recuperación ante desastres](/azure/architecture/solution-ideas/articles/build-high-availability-into-your-bcdr-strategy)

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).