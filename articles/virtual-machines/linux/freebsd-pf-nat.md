---
title: Uso del filtro de paquetes de FreeBSD para crear un firewall en Azure
description: Obtenga información sobre cómo implementar un firewall NAT con el filtro de paquetes de FreeBSD de Azure.
author: KylieLiang
ms.service: virtual-machines
ms.collection: linux
ms.topic: how-to
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 673ddf30771f63f559a12dbea47a1f022ea5599a
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102565927"
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Procedimiento para usar el filtro de paquetes de FreeBSD con el fin de crear un firewall seguro en Azure
En este artículo se explica cómo implementar un firewall NAT con el filtro de paquetes de FreeBSD a través de la plantilla de Azure Resource Manager para el escenario común de servidor web.

## <a name="what-is-pf"></a>¿Qué es el filtro de paquetes (PF)?
PF es un filtro de paquetes con estado con licencia para BSD, una pieza central del software de cortafuegos. Esta herramienta se ha desarrollado rápidamente y ahora tiene varias ventajas con respecto a otros firewalls disponibles. Traducción de direcciones de red (NAT) se encuentra en PF desde el primer día. El programador de paquetes y la administración de colas activas se han integrado en PF, incorporando ALTQ, con lo que se puede configurar desde PF. Características como pfsync y CARP para conmutación por error y redundancia, authpf para la autenticación de sesiones y ftp-proxy para facilitar el uso de firewalls en el difícil protocolo FTP difícil, también se han incorporado a PF. En resumen, PF es un firewall eficaz y con numerosas características. 

## <a name="get-started"></a>Introducción
Si le interesa configurar un firewall seguro en la nube para sus servidores web, empecemos a hacerlo. También puede aplicar los scripts usados en esta plantilla de Azure Resource Manager para configurar la topología de red.
La plantilla de Azure Resource Manager configura una máquina virtual de FreeBSD que realiza tareas NAT y de redirección usando dos máquinas virtuales de FreeBSD y PF con el servidor web Nginx instalado y configurado. Además de llevar a cabo NAT para el tráfico de salida de los dos servidores web, la máquina virtual de redirección/NAT intercepta las solicitudes HTTP y las redirige a los servidores web con distribución "round-robin". La red virtual usa el espacio de direcciones IP no enrutables 10.0.0.2/24 y puede modificar los parámetros de la plantilla. La plantilla de Azure Resource Manager también define una tabla de rutas para la red virtual completa, que es una colección de rutas individuales que se usa para invalidar las rutas de Azure predeterminadas en función de la dirección IP de destino. 

![Diagrama que muestra una dirección I P pública en una instancia de NAT que redirige mediante el método round robin a dos máquinas virtuales de back-end que hospedan servidores web Nginx.](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Implementación a través de la CLI de Azure
Necesita tener instalada la versión más reciente de la [CLI de Azure](/cli/azure/install-az-cli2) y haber iniciado sesión en una cuenta de Azure con [az login](/cli/azure/reference-index). Cree un grupo de recursos con [az group create](/cli/azure/group). En el ejemplo siguiente se crea un grupo de recursos denominado "`myResourceGroup`" en la ubicación `West US`.

```azurecli
az group create --name myResourceGroup --location westus
```

Después, implemente la plantilla pf-freebsd-setup con [az deployment group create](/cli/azure/deployment/group). Descargue azuredeploy.parameters.json en la misma ruta de acceso y definir sus propios valores de recursos, como `adminPassword`, `networkPrefix` y `domainNamePrefix`. 

```azurecli
az deployment group create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Después de aproximadamente cinco minutos, obtendrá la información de `"provisioningState": "Succeeded"`. Después, puede acceder mediante SSH a la máquina virtual de front-end (NAT) o al servidor de web Nginx en un explorador usando la dirección IP pública o el FQDN de la máquina virtual de front-end (NAT). En el ejemplo siguiente se muestra el FQDN y la dirección IP pública que se asigna a la máquina virtual de front-end (NAT) en el grupo de recursos `myResourceGroup`. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Pasos siguientes
¿Desea configurar su propia NAT en Azure? El código abierto es gratis, pero ¿es eficaz? PF es una buena elección. Mediante la plantilla pf-freebsd-setup, solo tiene cinco minutos para configurar un firewall NAT con PF de FreeBSD de carga equilibrada con distribución "round-robin" de Azure para el escenario común de servidores web. 

Si desea obtener información sobre la oferta de FreeBSD en Azure, consulte [Introducción a FreeBSD en Azure](freebsd-intro-on-azure.md).

Para saber más sobre PF, consulte el [manual de FreeBSD](https://www.freebsd.org/doc/handbook/firewalls-pf.html) o el [manual del usuario PF](https://www.freebsd.org/doc/handbook/firewalls-pf.html).
