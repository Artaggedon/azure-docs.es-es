---
title: Preparación de los servidores de Windows Server 2003 para la migración con Azure Migrate
description: Obtenga información sobre cómo preparar de los servidores de Windows Server 2003 para la migración con Azure Migrate.
author: rahulg1190
ms.author: rahugup
ms.manager: bsiva
ms.topic: how-to
ms.date: 05/27/2020
ms.openlocfilehash: f8d3dea970d07d951467a44661e12000ba413f72
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96753761"
---
# <a name="prepare-windows-server-2003-machines-for-migration"></a>Preparación de las máquinas de Windows Server 2003 para la migración

En este artículo se describe cómo preparar las máquinas que ejecutan Windows Server 2003 para la migración a Azure. 


> [!NOTE]
> El [soporte extendido de Windows Server 2003](/troubleshoot/azure/virtual-machines/run-win-server-2003#microsoft-windows-server-2003-end-of-support) finalizó el 14 de julio de 2015.  El equipo de soporte técnico de Azure sigue ayudando a solucionar problemas relacionados con la ejecución de Windows Server 2003 en Azure. Sin embargo, este soporte se limita a los problemas que no requieren revisiones o ni soluciones en el nivel de sistema operativo. La migración de las aplicaciones a instancias de Azure que ejecutan una versión más reciente de Windows Server es el enfoque recomendado para asegurarse de que se aprovecha de forma eficaz la flexibilidad y confiabilidad de la nube de Azure. Sin embargo, si todavía opta por migrar Windows Server 2003 a Azure, puede usar la herramienta Azure Migrate: Server Migration si Windows Server se encuentra en una máquina virtual que se ejecuta en VMware o Hyper-V.


- Puede usar la migración sin agente para migrar [máquinas virtuales de Hyper-V](tutorial-migrate-hyper-v.md) y [máquinas virtuales de VMware](tutorial-migrate-vmware.md) a Azure.
- Para conectarse a las máquinas virtuales de Azure después de la migración, servicios de integración de Hyper-V debe estar instalado en la máquina virtual de Azure. Las máquinas con Windows Server 2003 no lo tienen instalado de forma predeterminada.
- No hay ningún vínculo de descarga directa para instalar los servicios de integración de Hyper-V, por lo que debe hacer lo siguiente:
    - En el caso de las máquinas virtuales de Hyper-V que no los tienen instalado, extraiga los archivos de instalación de los servicios de integración en una máquina que ejecute Windows Server 2012 R2/Windows Server 2012 con el rol de Hyper-V y, a continuación, copie el instalador en la máquina con Windows Server 2003. Los archivos de instalación no están disponibles en las máquinas que ejecutan Windows Server 2016.
    - En el caso de las máquinas virtuales de VMware, se crea una tarea de inicio que instala los servicios de integración cuando la máquina virtual de Azure se inicia después de la migración.


## <a name="install-on-hyper-v-vms"></a>Instalación en máquinas virtuales de Hyper-V

Antes de la migración, compruebe si los servicios de integración de Hyper-V están instalados y, a continuación, instálelos si es necesario.

1. Siga [estas instrucciones](/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#turn-an-integration-service-on-or-off-using-hyper-v-manager) para comprobar si están instalados.
2. Si no están instalados, inicie sesión en una máquina que ejecute Windows Server 2012 R2/Windows Server 2012 con el rol de Hyper-V.
3. Navegue hasta el archivo de instalación en **C:\Windows\System32\vmguest.iso** y móntelo.
2. Copie la carpeta de instalación en la máquina con Windows Server 2003 e instale los servicios de integración.
4. Después de la instalación, puede dejar la configuración predeterminada en los servicios de integración. 

## <a name="install-on-vmware-vms"></a>Instalación en máquinas virtuales de VMware

1. Inicie sesión en una máquina que ejecute Windows Server 2012 R2/Windows Server 2012 con el rol de Hyper-V.
2. Navegue hasta el archivo de instalación en **C:\Windows\System32\vmguest.iso** y móntelo.
3. Copie la carpeta de instalación en la máquina virtual de VMware.
4. En la línea de comandos de la máquina virtual, ejecute ```gpedit.msc```.
5. Abra **Configuración del equipo** > **Configuración de Windows** > **Scripts (inicio o apagado)** .
6. En **Inicio** > **Agregar** > **Nombre del script**, escriba la dirección de setup.exe.
7. Después de la migración a Azure, el script se ejecuta la primera vez que se inicia la máquina virtual de Azure.
8. Reinicie manualmente la máquina virtual de Azure. Aparece un elemento emergente en los diagnósticos de arranque para indicar que es necesario un reinicio.
9. Después de ejecutar el script e instalar los servicios de integración de Hyper-V en la máquina virtual de Azure, puede quitar el script del inicio.
10. Después de la instalación, puede dejar la configuración predeterminada en los servicios de integración. 

## <a name="next-steps"></a>Pasos siguientes

- Revise los requisitos de migración para [VMWare](migrate-support-matrix-vmware-migration.md) y las máquinas virtuales de [Hyper-V](migrate-support-matrix-hyper-v-migration.md).
- Migre máquinas virtuales de [VMware](server-migrate-overview.md) y [Hyper-V](tutorial-migrate-hyper-v.md).
