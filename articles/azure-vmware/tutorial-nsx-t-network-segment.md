---
title: 'Tutorial: Incorporación de un segmento de red de NSX-T en Azure VMware Solution'
description: Aprenda a crear un segmento de red de NSX-T para usarlo en máquinas virtuales en vCenter.
ms.topic: tutorial
ms.date: 03/13/2021
ms.openlocfilehash: 9125e552f9641a2d26b9584b66a4447f9c152161
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103462137"
---
# <a name="tutorial-add-a-network-segment-in-azure-vmware-solution"></a>Tutorial: Incorporación de un segmento de red en Azure VMware Solution 

Las máquinas virtuales creadas en vCenter se colocan en los segmentos de red creados en NSX-T y son visibles en vCenter.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Desplazarse en NSX-T Manager para agregar segmentos de red
> * Agregar un nuevo segmento de red
> * Observar el nuevo segmento de red en vCenter

## <a name="prerequisites"></a>Requisitos previos

Una nube privada de Azure VMware Solution con acceso a las interfaces de vCenter y NSX-T Manager. Para obtener más información, consulte el tutorial de [configuración de redes](tutorial-configure-networking.md).

## <a name="add-a-network-segment"></a>Incorporación de un segmento de red

[!INCLUDE [add-network-segment-steps](includes/add-network-segment-steps.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un segmento de red de NSX-T que se usa para las máquinas virtuales en vCenter. 

Ahora puede: 

- [Crear y administrar DHCP para Azure VMware Solution](manage-dhcp.md)
- [Crear una biblioteca de contenido para implementar máquinas virtuales en Azure VMware Solution](deploy-vm-content-library.md) 
- [Emparejamiento de entornos locales con una nube privada](tutorial-expressroute-global-reach-private-cloud.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
