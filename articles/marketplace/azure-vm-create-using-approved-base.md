---
title: Creación de una oferta de máquina virtual de Azure a partir de una base aprobada, Azure Marketplace
description: Obtenga información sobre cómo crear una oferta de máquina virtual (VM) a partir de una base aprobada.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: krsh
ms.date: 10/20/2020
ms.openlocfilehash: 74cab14cd162ba608a9c5d0168485e9055661f55
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97590803"
---
# <a name="how-to-create-a-virtual-machine-using-an-approved-base"></a>Procedimiento para crear una máquina virtual con una base aprobada

En este artículo se describe cómo usar Azure en la creación de una máquina virtual (VM) que contenga un sistema operativo preconfigurado y aprobado. Si esto no es compatible con la solución, es posible [crear y configurar una máquina virtual local](azure-vm-create-using-own-image.md) mediante un sistema operativo aprobado y, después, configurarla y prepararla para la carga como se describe en [Preparación de un VHD o un VHDX de Windows antes de cargarlo en Azure](../virtual-machines/windows/prepare-for-upload-vhd-image.md).

> [!NOTE]
> Antes de comenzar este procedimiento, examine los [requisitos técnicos](marketplace-virtual-machines.md#technical-requirements) de las ofertas de máquina virtual de Azure, incluidos los requisitos de disco duro virtual (VHD).

## <a name="select-an-approved-base-image"></a>Selección de una imagen de base aprobada

Seleccione una de las siguientes imágenes de Windows o Linux como base.

### <a name="windows"></a>Windows

- [Windows Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview)
- SQL Server [2019](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview), [2014](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2014sp3-ws2012r2?tab=Overview), [2012](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2012sp4-ws2012r2?tab=Overview)

### <a name="linux"></a>Linux

Azure ofrece una gama de distribuciones de Linux aprobadas. Para obtener una lista actual, vea [Linux en distribuciones aprobadas por Azure](../virtual-machines/linux/endorsed-distros.md).

## <a name="create-vm-on-the-azure-portal"></a>Creación de la máquina virtual en Azure Portal

1. Inicie sesión en [Azure Portal](https://ms.portal.azure.com/).
2. Seleccione **Máquinas virtuales**.
3. Seleccione **+ Agregar** para abrir la pantalla **Crear una máquina virtual**.
4. Seleccione la imagen en la lista desplegable, o bien **Examinar todas las imágenes públicas y privadas** si desea buscar o examinar todas las imágenes de máquina virtual disponibles.
5. Si desea crear una máquina virtual de **Gen 1**, vaya a la pestaña **Discos** y establezca la opción **Usar disco administrado** en **NO** para que pueda obtener el VHD de la máquina virtual en la cuenta de almacenamiento especificada.

   :::image type="content" source="media/create-vm/create-virtual-machine-sizes-2.png" alt-text="Seleccione Disco no administrado.":::
   
6. Para crear una máquina virtual de **segunda generación**, vaya a la pestaña **Opciones avanzadas** y seleccione la opción **Generación 2**.

    :::image type="content" source="media/create-vm/vm-gen-option.png" alt-text="Selección Generación 1 o Generación 2.":::

7. Seleccione el tamaño de la máquina virtual que va a implementar.

    :::image type="content" source="media/create-vm/create-virtual-machine-sizes.png" alt-text="Seleccione un tamaño de máquina virtual recomendado para la imagen seleccionada.":::

7. Proporcione los otros detalles necesarios para crear la máquina virtual.
8. Seleccione **Revisar y crear** para revisar sus selecciones. Cuando aparezca el mensaje **Validación superada**, seleccione **Crear**.

Azure comienza el aprovisionamiento de la máquina virtual especificada. Para seguir su progreso, seleccione la pestaña **Máquinas virtuales** en el menú de la izquierda. En cuanto se crea, el estado de la máquina virtual cambia a **En ejecución**.


## <a name="configure-the-vm"></a>Configuración de la máquina virtual

En esta sección se describe cómo ajustar el tamaño, actualizar y generalizar una máquina virtual de Azure. Estos pasos son necesarios para preparar la máquina virtual para su implementación en Azure Marketplace.

### <a name="connect-to-your-vm"></a>Conexión a la máquina virtual

Consulte la documentación siguiente para conectarse a la máquina virtual [Windows](../virtual-machines/windows/connect-logon.md) o [Linux](../virtual-machines/linux/ssh-from-windows.md#connect-to-your-vm).

### <a name="install-the-most-current-updates"></a>Instalar las actualizaciones más recientes

[!INCLUDE [Discussion of most current updates](includes/most-current-updates.md)]

### <a name="perform-additional-security-checks"></a>Realizar comprobaciones de seguridad adicionales

[!INCLUDE [Discussion of addition security checks](includes/additional-security-checks.md)]

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Realizar una configuración personalizada y tareas programadas

[!INCLUDE [Discussion of custom configuration and scheduled tasks](includes/custom-config.md)]

[!INCLUDE [Discussion of addition security checks](includes/size-connect-generalize.md)]

## <a name="next-steps"></a>Pasos siguientes

- Siguiente paso recomendado: [Pruebe la imagen de la máquina virtual](azure-vm-image-test.md) para asegurarse de que cumple los requisitos de publicación de Azure Marketplace. Esta información es opcional.
- Si no prueba la imagen de la máquina virtual, vaya al artículo sobre la [generación del identificador URI de SAS](azure-vm-get-sas-uri.md).
- Si tiene dificultades para crear un disco duro virtual basado en Azure, consulte las [preguntas frecuentes que pueden surgir al crear una máquina virtual en Azure Marketplace](azure-vm-create-faq.md).