---
title: 'Azure VPN Gateway: Administración de sesiones de VPN de punto a sitio'
description: Este artículo le ayuda a ver y desconectar sesiones VPN de punto a sitio.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/23/2020
ms.author: cherylmc
ms.openlocfilehash: 2f2184507e17e3ecae40bb33be4202c183d32b77
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91274240"
---
# <a name="point-to-site-vpn-session-management"></a>Administración de sesiones de VPN de punto a sitio

Las puertas de enlace de red virtual de Azure proporcionan una manera fácil de ver y desconectar las sesiones actuales de VPN de punto a sitio. Este artículo le ayuda a ver y desconectar las sesiones actuales.

>[!NOTE]
>El estado de la sesión se actualiza cada cinco minutos. No lo hace inmediatamente.
>

## <a name="portal"></a>Portal

Para ver y desconectar una sesión en el portal:

1. Vaya a la puerta de enlace de VPN.
1. En la sección **Supervisión**, seleccione **Point-to-site Sessions** (Sesiones de punto a sitio).

   :::image type="content" source="./media/p2s-session-management/portal.png" alt-text="Ejemplo del portal":::
1. Puede ver todas las sesiones actuales en el panel de la ventana.
1. Seleccione **"..."** en la sesión que quiera desconectar y, luego, elija **Desconectar**.

## <a name="powershell"></a>PowerShell

Para ver y desconectar una sesión mediante PowerShell:

1. Ejecute el siguiente comando de PowerShell para mostrar las sesiones activas:

   ```azurepowershell-interactive
   Get-AzVirtualNetworkGatewayVpnClientConnectionHealth -VirtualNetworkGatewayName <name of the gateway>  -ResourceGroupName <name of the resource group>
   ```
1. Copie el valor de **VpnConnectionId** de la sesión que quiere desconectar.

   :::image type="content" source="./media/p2s-session-management/powershell.png" alt-text="Ejemplo de PowerShell":::
1. Para desconectar la sesión, ejecute el siguiente comando:

   ```azurepowershell-interactive
   Disconnect-AzVirtualNetworkGatewayVpnConnection -VirtualNetworkGatewayName <name of the gateway> -ResourceGroupName <name of the resource group> -VpnConnectionId <VpnConnectionId of the session>
   ```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las conexiones de punto a sitio, consulte [Acerca de las conexiones VPN de punto a sitio](point-to-site-about.md).
