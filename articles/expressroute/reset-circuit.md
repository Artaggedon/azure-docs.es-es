---
title: 'Restablecimiento de un circuito con errores: ExpressRoute: PowerShell: Azure | Microsoft Docs'
description: Este artículo le ayudará a restablecer un circuito de ExpressRoute que se encuentra en un estado con errores.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 01/07/2021
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: 84c8275b7a7257530a735e8612047ef42e2f8a7c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98011346"
---
# <a name="reset-a-failed-expressroute-circuit"></a>Restablecimiento de un circuito de ExpressRoute con errores

Cuando una operación de un circuito de ExpressRoute no se completa correctamente, el circuito puede entrar en un estado "con errores". Este artículo le ayudará a restablecer un circuito de Azure ExpressRoute con errores.

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

## <a name="reset-a-circuit"></a>Restablecimiento de un circuito

1. Instale la versión más reciente de los cmdlets de PowerShell de Azure Resource Manager. Para más información, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/install-az-ps).

2. Abra la consola de PowerShell con privilegios elevados y conéctese a su cuenta. Use el siguiente ejemplo para conectarse:

   ```azurepowershell-interactive
   Connect-AzAccount
   ```
3. Si tiene varias suscripciones de Azure, compruebe las suscripciones de la cuenta.

   ```azurepowershell-interactive
   Get-AzSubscription
   ```
4. Especifique la suscripción que desea usar.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```
5. Ejecute los comandos siguientes para restablecer un circuito que se encuentra en un estado con errores:

   ```azurepowershell-interactive
   $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

El circuito ahora debería funcionar correctamente. Abra una incidencia de soporte técnico con [Soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) si el circuito sigue en un estado con errores.

## <a name="next-steps"></a>Pasos siguientes

Si sigue teniendo problemas, abra una incidencia de soporte técnico dirigida al [Soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
