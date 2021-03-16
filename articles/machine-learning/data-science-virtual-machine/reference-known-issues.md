---
title: 'Referencia: Problemas conocidos y soluciones'
titleSuffix: Azure Data Science Virtual  Machine
description: Obtenga una lista de los problemas conocidos y soluciones para Azure Data Science Virtual Machine
services: machine-learning
ms.service: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: reference
ms.date: 10/10/2019
ms.openlocfilehash: bcd75347f91109967ac48427ca0b8855c11b7d9b
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101656863"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Problemas conocidos y solución de problemas de Azure Data Science Virtual Machine

Este artículo lo ayuda a encontrar y corregir errores que podría tener al usar Azure Data Science Virtual Machine.

## <a name="python-package-installation-issues"></a>Problemas de instalación del paquete de Python

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>La instalación de paquetes con PIP rompe las dependencias en Linux

Use `sudo pip install` en lugar de `pip install` al instalar paquetes.

## <a name="disk-encryption-issues"></a>Problemas de cifrado de disco

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>Error de cifrado de disco en Ubuntu DSVM

Azure Disk Encryption (ADE) no se admite actualmente en Ubuntu DSVM. Como alternativa, considere la posibilidad de configurar [el cifrado del lado servidor de Azure Managed Disks](../../virtual-machines/disk-encryption.md).

## <a name="tool-appears-disabled"></a>La herramienta aparece deshabilitada

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>Hyper-V no funciona en Windows DSVM

Que Hyper-V no funcione inicialmente en Windows es el comportamiento esperado. Para el rendimiento de arranque, hemos deshabilitado algunos servicios. Para habilitar Hyper-V:

1. Abra la barra de búsqueda en el DSVM de Windows.
1. Escriba "Servicios".
1. Establezca todos los servicios de Hyper-V en "Manual".
1. Establezca "Administración de máquinas virtuales de Hyper-V" en "Automático".

La pantalla final debería tener este aspecto:

   ![Habilitar Hyper-V](./media/workaround/hyperv-enable-dsvm.png)