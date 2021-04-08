---
title: archivo de inclusión
titleSuffix: Azure
description: archivo de inclusión
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: c3212acd5015edbb639b8904b885c718d654e5b4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96356268"
---
En esta sección se describe cómo realizar las siguientes operaciones de modificación para el emparejamiento directo.

### <a name="add-direct-peering-connections"></a>Adición de conexiones de emparejamiento directo
1. Seleccione el botón **+ Add connections** (+ Agregar conexiones) y configure una nueva conexión de emparejamiento.
    > [!div class="mx-imgBorder"]
    > ![La página AshburnPeering - Conexiones enumera las conexiones. El botón + Agregar conexiones texto está resaltado.](../media/setup-direct-modify-addconnection.png)

1. Rellene el formulario de **conexión de emparejamiento directo** y seleccione **Save** (Guardar). Para obtener ayuda con la configuración de una conexión de emparejamiento, revise los pasos descritos en la sección anterior "Creación y aprovisionamiento de un emparejamiento directo".
    > [!div class="mx-imgBorder"]
    > ![Formulario de conexión de emparejamiento directo](../media/setup-direct-modify-savenewconnection.png)

### <a name="remove-direct-peering-connections"></a>Eliminación de conexiones de emparejamiento directo

Actualmente no se admite la eliminación de una conexión en Azure Portal. Para más información, póngase en contacto con el [servicio de emparejamiento de Microsoft](mailto:peeringexperience@microsoft.com).

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Actualización o cambio a una versión anterior del ancho de banda en las conexiones activas
1. Seleccione una conexión de emparejamiento que quiera modificar y, luego, elija **...**  > **Edit connection** (Editar conexión).
    > [!div class="mx-imgBorder"]
    > ![Editar la conexión](../media/setup-direct-modify-editconnection.png)

1. Mueva el control deslizante para modificar el ancho de banda y, luego, seleccione **Save** (Guardar).
    > [!div class="mx-imgBorder"]
    > ![Modificación del ancho de banda](../media/setup-direct-modify-editconnectionsettings.png)

### <a name="add-ipv4-or-ipv6-session-information-on-active-connections"></a>Adición de información de sesión IPv4 o IPv6 en conexiones activas
1. Seleccione una conexión de emparejamiento que quiera modificar y, luego, elija **...**  > **Edit connection** (Editar conexión), como se muestra en el paso 1.
1. Escriba la información de **Session IPv4 prefix** (Prefijo IPv4 de sesión) o **Session IPv6 prefix** (Prefijo IPv6 de sesión) y seleccione **Save** (Guardar).

### <a name="remove-ipv4-or-ipv6-session-information-on-active-connections"></a>Eliminación de la información de sesión IPv4 o IPv6 en conexiones activas
En este momento no se admite la información de **Session IPv4 prefix** (Prefijo IPv4 de sesión) o **Session IPv6 prefix** (Prefijo IPv6 de sesión) en el portal. Para más información, póngase en contacto con el [servicio de emparejamiento de Microsoft](mailto:peeringexperience@microsoft.com).
