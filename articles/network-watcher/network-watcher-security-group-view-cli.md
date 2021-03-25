---
title: 'Análisis de la seguridad de red con la vista de grupos de seguridad: CLI de Azure'
titleSuffix: Azure Network Watcher
description: En este artículo se describe cómo utilizar la CLI de Azure para analizar la seguridad de máquinas virtuales con la vista de grupos de seguridad.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: b3aa963c4da7802a9db714f25e7b544b3a132d4b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "94948653"
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli"></a>Analice la seguridad de una máquina virtual con la vista de grupos de seguridad mediante la CLI de Azure

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI de Azure](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

> [!NOTE]
> La API de vista de grupo de seguridad ya no se mantiene y dejará de utilizarse pronto. Use la [característica Reglas de seguridad vigentes](./network-watcher-security-group-view-overview.md) que proporciona la misma funcionalidad.


La vista de grupos de seguridad devuelve las reglas de seguridad de red configuradas y vigentes que se aplican a una máquina virtual. Esta funcionalidad resulta útil para auditar y diagnosticar los grupos de seguridad de red y las reglas que están configuradas en una máquina virtual para asegurarse de que el tráfico se está permitiendo o denegando correctamente. En este artículo, se muestra cómo recuperar las reglas de seguridad configuradas y vigentes para una máquina virtual mediante la CLI de Azure

Para seguir los pasos de este artículo, es preciso [instalar la interfaz de la línea de comandos de Azure para Mac, Linux y Windows (CLI)](/cli/azure/install-azure-cli).

## <a name="before-you-begin"></a>Antes de empezar

En este escenario, se da por hecho que ya ha seguido los pasos descritos en [Creación de una instancia de Network Watcher](network-watcher-create.md) para crear un monitor de red.

## <a name="scenario"></a>Escenario

El escenario descrito en este artículo recupera las reglas de seguridad configuradas y vigentes para una máquina virtual dada.

## <a name="get-a-vm"></a>Obtención de una máquina virtual

Se necesita una máquina virtual para ejecutar el cmdlet `vm list`. El comando siguiente muestra las máquinas virtuales de un grupo de recursos:

```azurecli
az vm list -resource-group resourceGroupName
```

Una vez que conozca la máquina virtual, puede usar el cmdlet `vm show` para obtener su identificador de recurso:

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Recuperación de la vista de grupos de seguridad

El siguiente paso es recuperar el resultado de la vista de grupos de seguridad.

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-the-results"></a>Visualización de los resultados

El ejemplo siguiente es una respuesta reducida de los resultados devueltos. Los resultados muestran todas las reglas de seguridad vigentes y aplicadas en la máquina virtual, clasificadas en los grupos **NetworkInterfaceSecurityRules**, **DefaultSecurityRules** y **EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
      "resourceGroup": "{resourceGroupName}",
      "securityRuleAssociations": {
        "defaultSecurityRules": [
          {
            "access": "Allow",
            "description": "Allow inbound traffic from all VMs in VNET",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "*",
            "direction": "Inbound",
            "etag": null,
            "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups//providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/AllowVnetInBound",
            "name": "AllowVnetInBound",
            "priority": 65000,
            "protocol": "*",
            "provisioningState": "Succeeded",
            "resourceGroup": "",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "*"
          }...
        ],
        "effectiveSecurityRules": [
          {
            "access": "Deny",
            "destinationAddressPrefix": "*",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": null,
            "expandedSourceAddressPrefix": null,
            "name": "DefaultOutboundDenyAll",
            "priority": 65500,
            "protocol": "All",
            "sourceAddressPrefix": "*",
            "sourcePortRange": "0-65535"
          },
          {
            "access": "Allow",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "expandedSourceAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "name": "DefaultRule_AllowVnetOutBound",
            "priority": 65000,
            "protocol": "All",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "0-65535"
          },...
        ],
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "resourceGroup": "{resourceGroupName}",
          "securityRules": [
            {
              "access": "Allow",
              "description": null,
              "destinationAddressPrefix": "*",
              "destinationPortRange": "3389",
              "direction": "Inbound",
              "etag": "W/\"efb606c1-2d54-475a-ab20-da3f80393577\"",
              "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "name": "default-allow-rdp",
              "priority": 1000,
              "protocol": "TCP",
              "provisioningState": "Succeeded",
              "resourceGroup": "{resourceGroupName}",
              "sourceAddressPrefix": "*",
              "sourcePortRange": "*"
            }
          ]
        },
        "subnetAssociation": null
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Visite [Automate NSG auditing with Azure Network Watcher Security group view](network-watcher-nsg-auditing-powershell.md) (Automatización de la auditoría de grupos de seguridad de red (NSG) con la vista de grupos de seguridad de Azure Network Watcher) para aprender a automatizar la validación de grupos de seguridad de red.

Para más información sobre las reglas de seguridad que se aplican a los recursos de red, consulte la [información general de la vista de grupos de seguridad](network-watcher-security-group-view-overview.md).