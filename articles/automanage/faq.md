---
title: Preguntas más frecuentes sobre Azure Automanage para máquinas virtuales
description: Respuestas a las preguntas más frecuentes sobre Azure Automanage para máquinas virtuales.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: troubleshooting
ms.date: 09/04/2020
ms.author: deanwe
ms.openlocfilehash: 2b15389ae35a63c71ac4cb9e167e578e7e1e690e
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2020
ms.locfileid: "97695767"
---
# <a name="frequently-asked-questions-for-azure-automanage-for-vms"></a>Preguntas más frecuentes sobre Azure Automanage para máquinas virtuales

En este artículo se proporcionan respuestas a algunas de las preguntas más comunes sobre [Azure Automanage para máquinas virtuales](automanage-virtual-machines.md).

Si su problema con Azure no se trata en este artículo, visite los foros de Azure en [MSDN y Stack Overflow](https://azure.microsoft.com/support/forums/). Puede publicar su problema en ellos o [@AzureSupport en Twitter](https://twitter.com/AzureSupport). También puede enviar una solicitud de soporte técnico de Azure. Para enviar una solicitud de soporte técnico, en la [página de Soporte técnico de Azure](https://azure.microsoft.com/support/options/), seleccione **Obtener soporte técnico**.


## <a name="azure-automanage-for-virtual-machines"></a>Azure Automanage para máquinas virtuales

**¿Qué requisitos previos se necesitan para habilitar Azure Automanage?**

Los requisitos previos para habilitar Azure Automanage son los siguientes:
- Solo máquinas virtuales de Windows Server.
- Las máquinas virtuales deben estar en ejecución
- Las máquinas virtuales deben estar en una región admitida.
- El usuario debe tener los permisos correctos.
- Solo máquinas virtuales que no son de conjunto de escalado.
- Automanage no admite suscripciones de espacio aislado en este momento

**¿Qué permiso de Azure RBAC es necesario para habilitar Automanage?**

Si va a habilitar Automanage en una máquina virtual con una cuenta de Automanage existente, necesita el rol de colaborador en el grupo de recursos donde reside la máquina virtual.

Si usa una cuenta de Automanage nueva al habilitarlo, debe tener el rol de propietario o de administrador de acceso de usuario y colaborador para la suscripción.


**¿Qué regiones se admiten?**

Se admiten máquinas virtuales en las regiones siguientes: Oeste de Europa, Este de EE. UU., Oeste de EE. UU. 2, Centro de Canadá, Centro-oeste de EE. UU., Este de Japón.


**¿Qué funcionalidades automatiza Azure Automanage?**

Automanage inscribe, configura y supervisa los servicios mencionados [aquí](virtual-machines-best-practices.md) durante todo el ciclo de vida de la máquina virtual.

**¿Funciona Azure Automanage con máquinas virtuales habilitadas para Azure Arc?**

Actualmente Automanage no admite máquinas virtuales habilitadas para Arc.

**¿Se pueden personalizar configuraciones en Azure Automanage?**

Los clientes pueden personalizar la configuración de servicios específicos, como la retención de Azure Backup, a través de las preferencias de configuración. Consulte la documentación que aparece [aquí](virtual-machines-best-practices.md) para ver la lista completa de las configuraciones que se pueden modificar.


**¿Azure Automanage funciona con máquinas virtuales Linux y Windows?**

Actualmente, Automanage admite máquinas virtuales de Azure con Windows Server.


**¿Se puede aplicar Automanage de manera selectiva a un solo conjunto de máquinas virtuales?**

Automanage se puede habilitar simplemente haciendo clic y señalando máquinas virtuales nuevas y existentes. Automanage también se pueden deshabilitar en cualquier momento.


**¿Azure Automanage admite máquinas virtuales en un conjunto de escalado de máquinas virtuales?**

No, actualmente Azure Automanage no admite máquinas virtuales en un conjunto de escalado de máquinas virtuales.


**¿Cuánto cuesta Azure Automanage?**

Azure Automanage está disponible sin costo adicional en la versión preliminar pública. Los recursos adjuntos de Azure, como Azure Backup, supondrán un costo.


**¿Se puede aplicar Automanage a través de Azure Policy?**

Sí, tenemos una directiva integrada que aplicará automáticamente Automanage a todas las máquinas virtuales dentro del ámbito que se defina. También se especificará el perfil de configuración (DevTest o Producción) junto con su cuenta de Automanage. Obtenga más información [aquí](virtual-machines-policy-enable.md) sobre cómo habilitar Automanage a través de Azure Policy.


**¿Qué es una cuenta de Automanage?**

Una cuenta de Automanage es una identidad de servicio administrado (MSI) que proporciona el contexto de seguridad o la identidad bajo la cual se producen las operaciones automatizadas.


**Al habilitar Automanage, ¿afecta a las máquinas virtuales adicionales además de a las máquinas virtuales seleccionadas?**

Si la máquina virtual está vinculada a un área de trabajo de Log Analytics existente, se volverá a usar esa área de trabajo para aplicar estas soluciones: Change Tracking, Inventory y Update Management. Todas las máquinas virtuales conectadas a esa área de trabajo tendrán habilitadas esas soluciones.


**¿Se puede cambiar el perfil de configuración de una máquina virtual?**

En este momento, tendrá que deshabilitar Automanage para esa máquina virtual y, a continuación, volver a habilitarlo con las preferencias y el perfil de configuración que desee.


**Si una máquina virtual ya está configurada para un servicio, como Update Management, ¿se volverá a configurar con Automanage?**
No, no se volverá a configurar con Automanage. Se iniciará la supervisión del desfase de los recursos asociados a ese servicio.


**¿Por qué una máquina virtual tiene un estado de error en el portal de Automanage?**

Si ve que el estado es *Erróneo*, puede solucionar los problemas de implementación a través del grupo de recursos donde se encuentra la máquina virtual. Vaya a **Grupos de recursos**, seleccione el grupo de recursos, haga clic en **Implementaciones** y consulte el estado *Erróneo* que aparece ahí junto con los detalles del error.

**¿Cómo se puede obtener ayuda para solucionar problemas de Automanage?**

Se puede presentar una [incidencia de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). En la opción **Servicio**, busque y seleccione *Automanage* en la sección *Supervisión y administración*.


## <a name="next-steps"></a>Pasos siguientes

Intente habilitar Automanage para máquinas virtuales en Azure Portal.

> [!div class="nextstepaction"]
> [Habilitación de Automanage para máquinas virtuales en Azure Portal](quick-create-virtual-machines-portal.md)