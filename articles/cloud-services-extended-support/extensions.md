---
title: Extensiones de Cloud Services (soporte extendido)
description: Extensiones de Cloud Services (soporte extendido)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: f9029a36dc3b778e139b4553524e8e2ca6b4bbad
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98757176"
---
# <a name="extensions-for-cloud-services-extended-support"></a>Extensiones de Cloud Services (soporte extendido)

Las extensiones son aplicaciones pequeñas que proporcionan tareas de automatización y configuración en roles posterior a la implementación. Por ejemplo, puede habilitar una conexión de Escritorio remoto en su rol durante la implementación del servicio en la nube (soporte extendido) mediante la extensión de Escritorio remoto.  

## <a name="remote-desktop-extension"></a>Extensión de Escritorio remoto

Escritorio remoto le permite tener acceso al escritorio de un rol que se ejecuta en Azure. Puede usar una conexión de Escritorio remoto para solucionar y diagnosticar problemas con su aplicación mientras se ejecuta.

Puede habilitar una conexión a Escritorio remoto en el rol durante el desarrollo mediante la inclusión de los módulos de Escritorio remoto en la definición de servicio o mediante la extensión de Escritorio remoto. 

Para más información, consulte [Configuración de Escritorio remoto desde Azure Portal](enable-rdp.md).

## <a name="windows-azure-diagnostics-extension"></a>Extensión de Microsoft Azure Diagnostics

Puede supervisar las métricas de rendimiento claves de cualquier servicio en la nube. Cada rol de servicio en la nube recopila datos mínimos: uso de CPU, uso de red y uso de disco. Si el servicio en la nube tiene la extensión Microsoft.Azure.Diagnostics aplicada a un rol, este último puede recopilar puntos de datos adicionales. 

Con la supervisión básica, los datos del contador de rendimiento de las instancias de rol se muestrean y recopilan en intervalos de tres minutos. Los datos de la supervisión básica no se almacenan en la cuenta de almacenamiento y, por tanto, no tienen ningún costo adicional asociado. 

Con la supervisión avanzada, las métricas adicionales se muestrean y recopilan en intervalos de cinco minutos, una hora y doce horas. Los datos agregados se almacenan en una cuenta de almacenamiento, en tablas, y se purgan después de diez días. La cuenta de almacenamiento utilizada se configura en función del rol; puede usar diferentes cuentas de almacenamiento para distintos roles. 

Para más información, consulte [Aplicación de la extensión de Windows Azure Diagnostics en Cloud Services (soporte extendido)](enable-wad.md).


## <a name="next-steps"></a>Pasos siguientes 
- Revise los [requisitos previos de implementación](deploy-prerequisite.md) de Cloud Services (soporte extendido).
- Vea las [preguntas más frecuentes](faq.md) sobre Cloud Services (soporte extendido).
- Implemente una instancia de Cloud Services (soporte extendido) mediante [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), una [plantilla](deploy-template.md) o [Visual Studio](deploy-visual-studio.md).
