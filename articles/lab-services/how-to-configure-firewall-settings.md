---
title: Configuración del firewall para Azure Lab Services
description: Obtenga información sobre cómo determinar la dirección IP pública y el intervalo de números de puerto de las máquinas virtuales de un laboratorio para que se pueda agregar información a las reglas de firewall.
author: emaher
ms.author: enewman
ms.date: 06/26/2020
ms.topic: article
ms.openlocfilehash: 067a2c9672b87974557f650cef07b0394e7d5a63
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "85445855"
---
# <a name="firewall-settings-for-azure-lab-services"></a>Configuración del firewall para Azure Lab Services

Cada organización o escuela configurará su propia red de la forma que mejor se adapte a sus necesidades.  En ocasiones, eso incluye el establecimiento de reglas de firewall que bloquean conexiones de Protocolo de escritorio remoto (rdp) o Secure Shell (ssh) a máquinas fuera de su propia red.  Dado que Azure Lab Services se ejecuta en la nube pública, es posible que se necesite una configuración adicional para permitir a los estudiantes obtener acceso a su máquina virtual al conectarse desde la red del campus.

Cada laboratorio usa una sola dirección IP pública y varios puertos.  Todas las máquinas virtuales, tanto la plantilla de máquina virtual como las máquinas virtuales de estudiantes, usarán esta dirección IP pública.  La dirección IP pública no cambiará durante la vida del laboratorio.  Sin embargo, cada máquina virtual tendrá un número de puerto diferente.  Los números de puerto pueden oscilar entre 49152 y 65535.  La combinación de la dirección IP pública y el número de puerto se usa para conectar el instructor y los estudiantes a la máquina virtual correcta.  En este artículo se explicará cómo buscar la dirección IP pública específica usada por un laboratorio.  Esa información se puede usar para actualizar reglas de firewall de entrada y salida para que los estudiantes puedan obtener acceso a sus máquinas virtuales.

>[!IMPORTANT]
>Cada laboratorio tendrá una dirección IP pública diferente.

## <a name="find-public-ip-for-a-lab"></a>Búsqueda de la dirección IP pública de un laboratorio

Las direcciones IP públicas de cada laboratorio se muestran en la página **Todos los laboratorios** de la cuenta de laboratorio de Lab Services.  Para obtener instrucciones sobre cómo buscar la página **Todos los laboratorios**, consulte [Visualización de laboratorios en una cuenta de laboratorio](manage-labs.md#view-labs-in-a-lab-account).  

> [!div class="mx-imgBorder"]
> ![Página Todos los laboratorios](./media/how-to-configure-firewall-settings/all-labs-properties.png)

>[!NOTE]
>No verá la dirección IP pública si aún no se ha publicado la máquina de plantilla de su laboratorio.

## <a name="conclusion"></a>Conclusión

Ahora conocemos la dirección IP pública del laboratorio.  Las reglas de entrada y salida se pueden crear para el firewall de la organización de la dirección IP pública y el intervalo de puertos 49152-65535.  Una vez actualizadas las reglas, los estudiantes pueden obtener acceso a sus máquinas virtuales sin que el firewall de red bloquee el acceso.

## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- [Permiso para que el creador del laboratorio seleccione su ubicación](allow-lab-creator-pick-lab-location.md)
- [Conexión de la red del laboratorio con una red virtual del mismo nivel en Azure Lab Services](how-to-connect-peer-virtual-network.md)
- [Asociación o desasociación de una galería de imágenes compartidas en Azure Lab Services](how-to-attach-detach-shared-image-gallery.md)
- [Cómo agregar un usuario como propietario de un laboratorio de clase en Azure Lab Services](how-to-add-user-lab-owner.md)
- [Configuración del firewall para Azure Lab Services](how-to-configure-firewall-settings.md)
- [Configuración de cuentas de laboratorio en Azure Lab Services](how-to-configure-lab-accounts.md)
