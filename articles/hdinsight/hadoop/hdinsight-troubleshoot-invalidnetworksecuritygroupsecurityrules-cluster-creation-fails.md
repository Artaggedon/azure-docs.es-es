---
title: 'Error de InvalidNetworkSecurityGroupSecurityRules: Azure HDInsight'
description: Error de creación del clúster con el código de error InvalidNetworkSecurityGroupSecurityRules
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/31/2019
ms.openlocfilehash: 7e0b984b3ec4a203f8a1118c0e6a166c5a9e1125
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944376"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Escenario: InvalidNetworkSecurityGroupSecurityRules: error de creación del clúster en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Recibirá el código de error `InvalidNetworkSecurityGroupSecurityRules` con una descripción similar a esta: "Las reglas de seguridad en el grupo de seguridad de red configurado con subred no permiten la conectividad entrante o saliente requerida".

## <a name="cause"></a>Causa

Es probable que haya un problema con las reglas del [grupo de seguridad de red](../../virtual-network/virtual-network-vnet-plan-design-arm.md) entrantes configuradas para el clúster.

## <a name="resolution"></a>Solución

Vaya a Azure Portal e identifique el grupo de seguridad de red que está asociado a la subred donde se va a implementar el clúster. En la sección **Reglas de seguridad de entrada**, asegúrese de que las reglas permiten el acceso entrante al puerto 443 para las direcciones IP que se mencionan [aquí](../control-network-traffic.md).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]