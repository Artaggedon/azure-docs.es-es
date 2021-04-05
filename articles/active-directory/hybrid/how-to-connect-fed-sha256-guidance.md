---
title: Cambio del algoritmo hash de firma para relación de confianza para usuario autenticado de Microsoft 365 en Azure
description: En esta página se proporcionan instrucciones para cambiar el algoritmo SHA correspondiente a la confianza de federación con Microsoft 365.
keywords: SHA1,SHA256,M365,federación,aadconnect,adfs,ad fs,cambiar sha,confianza de federación,relación de confianza para usuario autenticado
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/26/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6bf9347d4d14e6583febd4ffaf0447e912133b80
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "89660926"
---
# <a name="change-signature-hash-algorithm-for-microsoft-365-relying-party-trust"></a>Cambio del algoritmo hash de firma para la relación de confianza para usuario autenticado de Microsoft 365
## <a name="overview"></a>Información general
Active Directory Federation Services (ADFS) firma sus tokens en Microsoft Azure Active Directory para asegurarse de que no pueden alterarse. Esta firma se puede basar en SHA1 o SHA256. Azure Active Directory ahora es compatible con tokens firmados con un algoritmo SHA256 y recomendamos establecer el algoritmo con firma de tokens en SHA256 para proporcionar el máximo nivel de seguridad. En este artículo se describen los pasos necesarios para establecer el algoritmo de firma de tokens para el nivel SHA256 más seguro.

>[!NOTE]
>Microsoft recomienda el uso de SHA256 como el algoritmo de firma de tokens ya que es más seguro que SHA1, pero SHA1 sigue siendo una opción admitida.

## <a name="change-the-token-signing-algorithm"></a>Cambio del algoritmo de firma de tokens
Después de haber establecido el algoritmo de firma con uno de los dos procesos siguientes, AD FS firma los tokens para la relación de confianza para usuario autenticado de Microsoft 365 con SHA-256. No es necesario realizar ningún cambio de configuración adicional y este cambio no tendrá efecto en su capacidad de acceder a Microsoft 365 o a otras aplicaciones Azure AD.

### <a name="ad-fs-management-console"></a>Consola de administración de AD FS
1. Abra la consola de administración de AD FS en el servidor de AD FS principal.
2. Expanda el nodo de AD FS y haga clic en el nodo de **relaciones de confianza para usuario autenticado**.
3. Haga clic con el botón derecho en la relación de confianza para usuario autenticado de Microsoft 365 o Azure y seleccione **Propiedades**.
4. Seleccione la pestaña **Opciones avanzadas** y el algoritmo hash seguro SHA256.
5. Haga clic en **OK**.

![Algoritmo de firma SHA256 - MMC](./media/how-to-connect-fed-sha256-guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>Cmdlets de PowerShell de AD FS
1. En cualquier servidor de AD FS, abra PowerShell con privilegios de administrador.
2. Establezca el algoritmo hash seguro mediante el cmdlet **Set-AdfsRelyingPartyTrust** .
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'https://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Consulte también
* [Reparación de la confianza de Microsoft 365 con Azure AD Connect](how-to-connect-fed-management.md#repairthetrust)

