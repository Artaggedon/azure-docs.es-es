---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: styli365
ms.service: virtual-machines
ms.topic: include
ms.date: 11/05/2020
ms.author: sttsinar
ms.custom: include file
ms.openlocfilehash: e22c2b7cb561e30e84ea5ede5481fbdc35be8cdf
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100515046"
---
Azure Compute ofrece tamaños de máquinas virtuales que están aislados para un tipo concreto de hardware y dedicados a un solo cliente. Los tamaños aislados viven y funcionan en una generación de hardware específica y quedarán en desuso cuando se retire la generación de hardware.

Los tamaños de máquina virtual aislados son más adecuados para cargas de trabajo que requieren un alto grado de aislamiento de otros clientes por motivos como, por ejemplo, el cumplimiento normativo y de requisitos legales.  Usar un tamaño aislado garantiza que la máquina virtual será la única que se ejecute en esa instancia de servidor específica. 


Además, dado que las VM de tamaño aislado son más grandes, los clientes puede elegir subdividir aún más los recursos de estas VM mediante la [compatibilidad de Azure con máquinas virtuales anidadas](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).

Las ofertas de máquinas virtuales aisladas actuales incluyen:
* Standard_E80ids_v4
* Standard_E80is_v4
* Standard_F72s_v2
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5


> [!NOTE]
> Los tamaños de VM aislados tienen una duración limitada de hardware. Vea los detalles a continuación

## <a name="deprecation-of-isolated-vm-sizes"></a>Desuso de los tamaños de VM aislados

Los tamaños de VM aislados tienen una duración limitada de hardware. Azure emitirá recordatorios de 12 meses antes de la fecha de desuso oficial de los tamaños y le proporcionará una oferta aislada actualizada para que la tenga en cuenta.

| Size | Fecha de retirada del aislamiento | 
| --- | --- |
| Standard_DS15_v2<sup>1</sup> | 15 de mayo de 2020 |
| Standard_D15_v2<sup>1</sup>  | 15 de mayo de 2020 |

<sup>1</sup>  Para obtener más información sobre los programas de retirada de aislamiento Standard_DS15_v2 y Standard_D15_v2, consulte las preguntas frecuentes.


## <a name="faq"></a>Preguntas más frecuentes
### <a name="q-is-the-size-going-to-get-retired-or-only-its-isolation-feature"></a>P: ¿Se retirará el tamaño o solo la característica de "aislamiento"?
**R.** : Si el tamaño de la máquina virtual no tiene el subíndice "i", solo se retirará la característica de "aislamiento". Si no es necesario el aislamiento, no es necesario realizar ninguna acción y la VM seguirá funcionando según lo previsto. Entre los ejemplos se incluyen Standard_DS15_v2, Standard_D15_v2, Standard_M128ms, etc. Si el tamaño de la máquina virtual incluye el subíndice "i", el tamaño se retirará.

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>P: ¿Hay un tiempo de inactividad cuando la máquina virtual se encuentra en un hardware no aislado?
**R.** : Si no hay necesidad de aislamiento, no es necesaria ninguna acción y no se producirá ningún tiempo de inactividad.

### <a name="q-is-there-any-cost-delta-for-moving-to-a-non-isolated-virtual-machine"></a>P: ¿Hay alguna diferencia de costo por cambiar a una máquina virtual no aislada?
**R.** : No

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>P: ¿Cuándo se van a retirar los otros tamaños aislados?
**R.** : Se enviarán recordatorios 12 meses antes de que el tamaño aislado esté oficialmente en desuso.

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>P: Soy un cliente de Azure Service Fabric que se basa en los niveles de durabilidad Silver o Gold. ¿Me afectará este cambio?
**R.** : No. Las garantías proporcionadas por los [niveles de durabilidad](../articles/service-fabric/service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster) de Service Fabric seguirán funcionando incluso después de este cambio. Si necesita aislamiento de hardware físico por otras razones, es posible que tenga que realizar una de las acciones descritas anteriormente. 
 
### <a name="q-what-are-the-milestones-for-d15_v2-or-ds15_v2-isolation-retirement"></a>P: ¿Cuáles son los hitos para la retirada del aislamiento D15_v2 o DS15_v2? 
**R.** : 
 
| Date | Acción |
|---|---| 
| 18 de noviembre de 2019 | Disponibilidad de D/DS15i_v2 (pago por uso, instancia reservada de un año) | 
| 14 de mayo de 2020 | Último día para comprar la instancia reservada de un año de D/DS15i_v2 | 
| 15 de mayo de 2020 | Se ha quitado la garantía de aislamiento de D/DS15_v2 | 
| 15 de mayo de 2021 | Retirada de D/DS15i_v2 (todos los clientes, excepto los que compraron una instancia reservada de tres años de D/DS15_v2 antes del 18 de noviembre de 2019)| 
| 17 de noviembre de 2022 | Retirada de D/DS15i_v2 cuando se realizan instancias reservadas de tres años (para los clientes que compraron instancias reservadas de tres años de D/DS15_v2 antes del 18 de noviembre de 2019) |

## <a name="next-steps"></a>Pasos siguientes

Los clientes también puede elegir subdividir aún más los recursos de estas máquinas virtuales aisladas mediante la [compatibilidad de Azure para máquinas virtuales anidadas](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).
