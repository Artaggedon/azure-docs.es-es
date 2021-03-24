---
title: 'Ejemplo de script de la CLI Azure : equilibrio de carga entre varios sitios web con la CLI de Azure | Microsoft Docs'
description: 'Ejemplo de script de la CLI Azure : equilibrio de carga entre varios sitios web con la misma máquina virtual'
services: load-balancer
documentationcenter: load-balancer
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: allensu
ms.openlocfilehash: 7a3bd4cab8123814fe360fe4ab724650c785e9f1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98234122"
---
# <a name="load-balance-multiple-websites"></a>Equilibrio de carga entre varios sitios web

Este ejemplo de script crea una red virtual con dos máquinas virtuales que son miembros de un conjunto de disponibilidad. Un equilibrador de carga dirige el tráfico de dos direcciones IP independientes a las dos máquinas virtuales. Después de ejecutar el script, podría implementar software de servidor web en las máquinas virtuales y hospedar varios sitios web, cada uno con su propia dirección IP.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una red virtual, un equilibrador de carga y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az network vnet create](/cli/azure/network/vnet) | Crea una red virtual y una subred de Azure. |
| [az network public-ip create](/cli/azure/network/public-ip) | Crea una dirección IP pública con una dirección IP estática y un nombre DNS asociado. |
| [az network lb create](/cli/azure/network/lb) | Crea una instancia de Azure Load Balancer. |
| [az network lb probe create](/cli/azure/network/lb/probe) | Crea un sondeo de equilibrador de carga. Un sondeo de equilibrador de carga se usa para supervisar cada máquina virtual del conjunto de equilibrador de carga. Si alguna máquina virtual deja de estar accesible, el tráfico no se enruta a la máquina virtual. |
| [az network lb rule create](/cli/azure/network/lb/rule) | Crea una regla de equilibrador de carga. En este ejemplo, se crea una regla para el puerto 80. Según va llegando el tráfico HTTP al equilibrador de carga, se enruta al puerto 80 de una de las máquinas virtuales del conjunto de equilibrador de carga. |
| [az network lb frontend-ip create](/cli/azure/network/lb/frontend-ip) | Crea una dirección IP de front-end para el equilibrador de carga. |
| [az network lb address-pool create](/cli/azure/network/lb/address-pool) | Crea un grupo de direcciones de back-end. |
| [az network nic create](/cli/azure/network/nic) | Crea una tarjeta de red virtual y la conecta con la red virtual y la subred. |
| [az vm availability-set create](/cli/azure/network/lb/rule) | Crea un conjunto de disponibilidad. Los conjuntos de disponibilidad garantizan la disponibilidad de las aplicaciones al repartir las máquinas virtuales entre los recursos físicos, de forma que si se produce un error, el conjunto no se verá afectado en su totalidad. |
| [az network nic ip-config create](/cli/azure/network/nic/ip-config) | Crea una configuración IP. Debe tener la característica de Microsoft.Network/AllowMultipleIpConfigurationsPerNic habilitada en su suscripción. Solo se puede designar una configuración como configuración IP primaria por adaptador de red, mediante la marca --make-primary. |
| [az vm create](/cli/azure/vm/availability-set) | Crea la máquina virtual y la conecta con la tarjeta de red, la red virtual, la subred y el NSG. Este comando también especifica la imagen de máquina virtual que se usará, y las credenciales administrativas.  |
| [az group delete](/cli/azure/vm/extension) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

En la [documentación de la información general de redes de Azure](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json) puede encontrar ejemplos adicionales de script de la CLI de redes.