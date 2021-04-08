---
title: 'VPN Gateway: modificar la configuración de la dirección IP de puerta de enlace: Portal de Azure'
description: Este artículo explica paso a paso cómo cambiar los prefijos de direcciones IP de la puerta de enlace de red local con Azure Portal.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/16/2020
ms.author: cherylmc
ms.openlocfilehash: 4ff4f1238764d7bdab6e74d29254a6388ea76d78
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92143135"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Modificación de la configuración de la puerta de enlace de red local mediante Azure Portal

A veces, cambia la configuración de la puerta de enlace de red local AddressPrefix o GatewayIPAddress. Este artículo muestra cómo modificar la configuración de puerta de enlace de red local. También puede modificar estas configuraciones mediante un método diferente, si selecciona una opción diferente de la lista siguiente:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [CLI de Azure](vpn-gateway-modify-local-network-gateway-cli.md)
>

## <a name="local-network-gateway-configuration"></a><a name="configure-lng"></a>Configuración de la puerta de enlace de red local

La siguiente captura de pantalla muestra la página **Configuración** de un recurso de puerta de enlace de red local mediante el punto de conexión de dirección IP pública:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/settings.png" alt-text="Configuración de dirección IP" lightbox="./media/vpn-gateway-modify-local-network-gateway-portal/settings-expand.png":::

Esta es la página de configuración con un punto de conexión de FQDN:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/name.png" alt-text="Configuración de FQDN":::

## <a name="to-modify-the-gateway-ip-address-or-fqdn"></a><a name="ip"></a>Para modificar la dirección IP o el nombre de dominio completo de la puerta de enlace

> [!NOTE]
> No se puede cambiar una puerta de enlace de red local entre el punto de conexión de FQDN y el punto de conexión de dirección IP. Debe eliminar todas las conexiones asociadas a esta puerta de enlace de red local, crear una nueva con el nuevo punto de conexión (dirección IP o FQDN) y, a continuación, volver a crear las conexiones.
>

Si el dispositivo VPN al que desea conectarse ha cambiado su dirección IP pública, siga estos pasos para modificar la puerta de enlace de red local:

1. En el recurso Puerta de enlace de red local, en la sección **Configuración**, seleccione **Configuración**.
2. En el cuadro **Dirección IP**, modifique la dirección IP.
3. Haga clic en **Save** (Guardar) para guardar la configuración.

Si el dispositivo VPN al que desea conectarse ha cambiado su FQDN (nombre de dominio completo), siga estos pasos para modificar la puerta de enlace de red local:

1. En el recurso Puerta de enlace de red local, en la sección **Configuración**, seleccione **Configuración**.
2. En el cuadro **FQDN**, modifique el nombre de dominio.
3. Haga clic en **Save** (Guardar) para guardar la configuración.

## <a name="to-modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Procedimiento para modificar los prefijos de direcciones IP

Para agregar prefijos de dirección adicionales:

1. En el recurso Puerta de enlace de red local, en la sección **Configuración**, seleccione **Configuración**.
2. Agregue el espacio de direcciones IP en el cuadro *Agregar otro intervalo de direcciones*.
3. Haga clic en **Guardar** para guardar la configuración.

Para quitar prefijos de dirección:

1. En el recurso Puerta de enlace de red local, en la sección **Configuración**, seleccione **Configuración**.
2. Seleccione **"..."** en la línea que contiene el prefijo que desea quitar.
3. Seleccione **Quitar**.
4. Haga clic en **Guardar** para guardar la configuración.

## <a name="to-modify-bgp-settings"></a><a name="bgp"></a>Para modificar la configuración de BGP

Para agregar o actualizar la configuración de BGP:

1. En el recurso Puerta de enlace de red local, en la sección **Configuración**, seleccione **Configuración**.
2. Seleccione **"Configurar BGP"** para mostrar o actualizar las configuraciones de BGP para esta puerta de enlace de red local.
3. Agregue o actualice el número de sistema autónomo o la dirección IP del par BGP en los campos correspondientes.
4. Haga clic en **Guardar** para guardar la configuración.

Para quitar la configuración de BGP:

1. En el recurso Puerta de enlace de red local, en la sección **Configuración**, seleccione **Configuración**.
2. Anule la selección de la opción **"Configurar BGP"** para quitar el ASN de BGP existente y la dirección IP del par BGP.
3. Haga clic en **Guardar** para guardar la configuración.

## <a name="next-steps"></a>Pasos siguientes

Puede comprobar la conexión de la puerta de enlace. Consulte [Comprobación de una conexión de puerta de enlace](vpn-gateway-verify-connection-resource-manager.md).
