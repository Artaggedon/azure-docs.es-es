---
title: 'Azure VPN Gateway: Sobre los perfiles de cliente de P2S VPN'
description: Use este artículo para encontrar la información que necesita para un perfil de cliente VPN.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/08/2021
ms.author: cherylmc
ms.openlocfilehash: 9bb66363d525648df08f32451842402ad1d0d93b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99979085"
---
# <a name="working-with-p2s-vpn-client-profile-files"></a>Uso de archivos de perfil de cliente VPN de punto a sitio

Los archivos de perfil contienen información necesaria para configurar una conexión VPN. Este artículo le ayudará a obtener y comprender la información necesaria para un perfil de cliente de VPN.

## <a name="generate-and-download-profile"></a>Generación y descarga de perfiles

Para generar archivos de configuración de cliente, puede usar PowerShell o Azure Portal. Cualquiera de los métodos devuelve el mismo archivo ZIP.

### <a name="portal"></a>Portal

1. En Azure Portal, navegue a la puerta de enlace de red virtual correspondiente a la red virtual a la que desea conectarse.
1. En la página de la puerta de enlace de red virtual, seleccione **Configuración de punto a sitio**.
1. En la parte superior de la página Configuración de punto a sitio, haga clic en **Descargar cliente VPN**. La generación del paquete de configuración del cliente tarda unos minutos.
1. El explorador indica que hay disponible un archivo ZIP de configuración del cliente. Tiene el mismo nombre que su puerta de enlace. Descomprima el archivo para ver las carpetas.

### <a name="powershell"></a>PowerShell

Para generar mediante PowerShell, puede usar el ejemplo siguiente:

1. Al generar archivos de configuración de cliente VPN, el valor de "-AuthenticationMethod" es "EapTIs". Genere los archivos de configuración de cliente VPN con el comando siguiente:

   ```azurepowershell-interactive
   $profile=New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

   $profile.VPNProfileSASUrl
   ```

1. Copie la dirección URL en el explorador para descargar el archivo ZIP y, luego, descomprima el archivo para ver las carpetas.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* La **carpeta OpenVPN** contiene el perfil de *ovpn* que debe modificarse para incluir la clave y el certificado. Para más información, consulte [Configurar clientes de OpenVPN para Azure VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows). Si se selecciona la autenticación de Azure AD en la puerta de enlace de la VPN, esta carpeta no estará presente en el archivo zip. En su lugar, vaya a la carpeta AzureVPN y busque azurevpnconfig. xml.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la ubicación de punto a sitio, consulte [Sobre ubicación de punto a sitio](point-to-site-about.md).
