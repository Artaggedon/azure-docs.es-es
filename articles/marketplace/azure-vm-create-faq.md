---
title: Preguntas comunes sobre máquinas virtuales en Azure Marketplace
description: Preguntas comunes que surgen al crear una máquina virtual en Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: guide
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 10/15/2020
ms.openlocfilehash: d045af3b170d585b4bf1f8c57b7ba924c6b30695
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102489793"
---
# <a name="common-questions-about-vm-in-azure-marketplace"></a>Preguntas comunes sobre máquinas virtuales en Azure Marketplace

Estas preguntas frecuentes abordan problemas comunes que pueden surgir al crear una oferta de máquina virtual en Azure Marketplace.

## <a name="how-do-i-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>¿Cómo se configura una red privada virtual (VPN) para trabajar con las máquinas virtuales?

Si usa el modelo de implementación de Azure Resource Manager, tiene tres opciones:

- [Creación de una instancia de VPN Gateway basada en rutas mediante Azure Portal](../vpn-gateway/tutorial-create-gateway-portal.md)
- [Creación de una instancia de VPN Gateway basada en rutas mediante Azure PowerShell](../vpn-gateway/create-routebased-vpn-gateway-powershell.md)
- [Creación de una instancia de VPN Gateway basada en rutas mediante la CLI](../vpn-gateway/create-routebased-vpn-gateway-cli.md)

## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>¿Cuáles son las directivas de soporte técnico de Microsoft para ejecutar software de servidor de Microsoft en máquinas virtuales basadas en Azure?

Puede encontrar información detallada en [Soporte técnico del software de servidor de Microsoft para máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="in-a-vm-how-do-i-manage-the-custom-script-extension-in-the-startup-task"></a>¿Cómo se puede administrar la extensión de script personalizada en la tarea de inicio en una máquina virtual?

Para obtener información detallada sobre cómo usar la extensión de script personalizado mediante el módulo de Azure PowerShell y plantillas de Azure Resource Manager, y también se detallan los pasos para solucionar problemas en los sistemas Windows, vea [Extensión de script personalizado para Windows](../virtual-machines/extensions/custom-script-windows.md).

## <a name="are-32-bit-applications-or-services-supported-in-azure-marketplace"></a>¿Se admiten en Azure Marketplace las aplicaciones o los servicios de 32 bits?

No. Todos los sistemas operativos compatibles y los servicios estándar para las máquinas virtuales de Azure son de 64 bits. Aunque la mayoría de los sistemas operativos de 64 bits admiten versiones de las aplicaciones de 32 bits para mantener la compatibilidad con versiones anteriores; el uso de aplicaciones de 32 bits como parte de la solución de máquina virtual no se admite y no se recomienda. Vuelva a crear la aplicación como un proyecto de 64 bits.

Para obtener más información, consulte estos artículos:

- [Running 32-bit applications](/windows/desktop/WinProg64/running-32-bit-applications) (Ejecución de aplicaciones de 32 bits)
- [Soporte para sistemas operativos de 32 bits en máquinas virtuales de Azure](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
- [Soporte técnico del software de servidor de Microsoft para máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)

## <a name="error-vhd-is-already-registered-with-image-repository-as-the-resource"></a>Error: El disco duro virtual ya está registrado con el repositorio de imágenes como un recurso.

Cada vez que trato de crear una imagen desde el VHD, se produce el error "VHD ya está registrado con el repositorio de imágenes como un recurso" en Azure PowerShell. No he creado ninguna imagen antes ni he encontrado ninguna imagen con este nombre en Azure. ¿Cómo se resuelve este problema?

Este problema suele aparecer si ha creado una máquina virtual desde un disco duro virtual que tiene un bloqueo. Asegúrese de que no hay ninguna máquina virtual asignada desde este disco duro virtual y, después, vuelva a intentar la operación. Si el problema continúa, abra una incidencia de soporte técnico. Vea [Soporte técnico para el Centro de partners](support.md).

## <a name="how-do-i-test-a-hidden-preview-image"></a>¿Cómo puedo probar una imagen de vista previa oculta?

Puede implementar imágenes de vista previa ocultas mediante plantillas de inicio rápido.
Para implementar una imagen de vista previa de Linux: 
1. Vaya a esta [plantilla de inicio rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux) y seleccione "Implementar en Azure". Esto le llevará a Azure Portal.
2. En Azure Portal, seleccione "Editar plantilla".
3. En la plantilla JSON, busque el elemento imageReference y actualice los valores de publisherid, offerid, skuid y version de la imagen. Para probar la imagen de vista previa, anexe "-PREVIEW" a offerid.
 ![image](https://user-images.githubusercontent.com/79274470/110191995-71c7d500-7de0-11eb-9f3c-6a42f55d8f03.png)
4. Haga clic en Guardar
5. Rellene el resto de los detalles. Elija Revisar y crear.



## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de certificación de máquina virtual](azure-vm-create-certification-faq.md)
