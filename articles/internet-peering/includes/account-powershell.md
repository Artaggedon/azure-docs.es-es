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
ms.openlocfilehash: beffb2babefd86c2807e21e9337cba66f42fcfc2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "81678494"
---
Antes de comenzar la configuración, instale e importe los módulos necesarios. Necesitará privilegios de administrador para instalar los módulos en PowerShell.

1. Instale e importe el módulo Az
    ```powershell
    Install-Module Az -AllowClobber
    Import-Module Az
    ```
1. Instale e importe el módulo Az.Peering.
    ```powershell
    Install-Module -Name Az.Peering -AllowClobber
    Import-Module Az.Peering
    ```
1. Use este comando para comprobar que los módulos se importaron correctamente:
    ```powershell
    Get-Module
    ```
1. Inicie sesión en su cuenta de Azure mediante este comando:
    ```powershell
    Connect-AzAccount
    ```
1. Compruebe las suscripciones de la cuenta y seleccione la suscripción en la que desea crear un emparejamiento.
    ```powershell
    Get-AzSubscription
    Select-AzSubscription -SubscriptionId "subscription-id"
    ```
1. Si todavía no tiene un grupo de recursos, debe crear uno antes de crear un emparejamiento. Para ello, ejecute el siguiente comando:

    ```powershell
    New-AzResourceGroup -Name "PeeringResourceGroup" -Location "Central US"
    ```
> [!IMPORTANT]
> Si aún no ha asociado el ASN y la suscripción, siga los pasos que se indican en [Asociación del ASN del mismo nivel](../howto-subscription-association-powershell.md). Esta acción es necesaria para solicitar un emparejamiento.

> [!NOTE]
> La ubicación del grupo de recursos es independiente de la ubicación en la que decida configurar un emparejamiento.
&nbsp;
