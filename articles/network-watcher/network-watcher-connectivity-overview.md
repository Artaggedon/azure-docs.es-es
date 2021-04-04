---
title: Introducción a la solución de problemas de conexiones en Azure Network Watcher | Microsoft Docs
description: En esta página se proporciona información general de la funcionalidad de solución de problemas de conexiones en Network Watcher.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: damendo
ms.openlocfilehash: 9c855fff9e5791b9c0cf870acfc6de53e7a700b2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97654003"
---
# <a name="introduction-to-connection-troubleshoot-in-azure-network-watcher"></a>Introducción a la solución de problemas de conexiones en Azure Network Watcher

La característica de solución de problemas de conexiones de Network Watcher proporciona la funcionalidad de comprobar una conexión TCP directa de una máquina virtual con una máquina virtual (VM), un nombre de dominio completo (FQDN), un URI o una dirección IPv4. Los escenarios de red son complejos, se implementan mediante grupos de seguridad de red, firewalls, rutas definidas por el usuario y recursos proporcionados por Azure. Las configuraciones complejas dificultan la solución de problemas de conectividad. Network Watcher ayuda a reducir la cantidad de tiempo en buscar y detectar problemas de conectividad. Los resultados devueltos pueden proporcionar información sobre si un problema de conectividad se debe a una plataforma o a un problema de configuración del usuario. La conectividad se puede comprobar con [PowerShell](network-watcher-connectivity-powershell.md), la [CLI de Azure](network-watcher-connectivity-cli.md) y la [API de REST](network-watcher-connectivity-rest.md).

> [!IMPORTANT]
> La solución de problemas de conexión requiere que la VM desde la que soluciona el problema tenga la extensión de VM `AzureNetworkWatcherExtension` instalada. Para instalar la extensión en una máquina virtual Windows, consulte [Extensión de máquina virtual del agente de Azure Network Watcher para Windows](../virtual-machines/extensions/network-watcher-windows.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json), y en una máquina virtual con Linux, consulte [Extensión de máquina virtual del agente de Azure Network Watcher para Linux](../virtual-machines/extensions/network-watcher-linux.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json). La extensión no es necesaria en el punto de conexión de destino.

## <a name="response"></a>Response

En la tabla siguiente se muestran las propiedades que se devuelven cuando la solución de problemas de conexiones terminó de ejecutarse.

|Propiedad  |Descripción  |
|---------|---------|
|ConnectionStatus     | Estado de la comprobación de conectividad. Los resultados posibles son **Reachable** y **Unreachable**.        |
|AvgLatencyInMs     | Promedio de latencia durante la comprobación de conectividad en milisegundos. (Solo se muestra si el estado de la comprobación es accesible).        |
|MinLatencyInMs     | Latencia mínima durante la comprobación de conectividad en milisegundos. (Solo se muestra si el estado de la comprobación es accesible).        |
|MaxLatencyInMs     | Latencia máxima durante la comprobación de conectividad en milisegundos. (Solo se muestra si el estado de la comprobación es accesible).        |
|ProbesSent     | Número de sondeos enviados durante la comprobación. El valor máximo es 100.        |
|ProbesFailed     | Número de sondeos con error durante la comprobación. El valor máximo es 100.        |
|Hops     | Ruta salto por salto desde el origen al destino.        |
|Hops[].Type     | Tipo de recurso. Los valores posibles son **Source**, **VirtualAppliance**, **VnetLocal** e **Internet**.        |
|Hops[].Id | Identificador único del salto.|
|Hops[].Address | Dirección IP del salto.|
|Hops[].ResourceId | Identificador de recurso del salto si este es un recurso de Azure. Si se trata de un recurso de Internet, el identificador de recurso es **Internet**. |
|Hops[].NextHopIds | Identificador único del siguiente salto.|
|Hops[].Issues | Colección de problemas encontrados durante la comprobación en ese salto. Si no hubiera ningún problema, el valor está en blanco.|
|Hops[].Issues[].Origin | Salto actual donde se produjo el problema. Los valores posibles son:<br/> **Inbound**: El problema se encuentra en el enlace entre el salto anterior y el salto actual.<br/>**Outbound**: El problema se encuentra en el enlace entre el salto actual y el salto anterior.<br/>**Local**: El problema se encuentra en el salto actual.|
|Hops[].Issues[].Severity | La gravedad del problema detectado. Los valores posibles son **Error** and **Warning**. |
|Hops[].Issues[].Type |Tipo de problema encontrado. Los valores posibles son: <br/>**Cpu**<br/>**Memoria**<br/>**GuestFirewall**<br/>**DNSResolution**<br/>**NetworkSecurityRule**<br/>**UserDefinedRoute** |
|Hops[].Issues[].Context |Detalles relacionados con el problema encontrado.|
|Hops[].Issues[].Context[].key |Clave del par clave-valor devuelta.|
|Hops[].Issues[].Context[].value |Valor del par clave-valor devuelto.|

A continuación, se muestra un ejemplo de un problema encontrado en un salto.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Tipo de error

La solución de problemas de conexiones devuelve tipos de error sobre la conexión. En la tabla siguiente se proporciona una lista de los tipos de errores actuales devueltos.

|Tipo  |Descripción  |
|---------|---------|
|CPU     | Alta utilización de CPU.       |
|Memoria     | Alta utilización de memoria.       |
|GuestFirewall     | El tráfico se bloquea debido a una configuración de firewall de máquina virtual.        |
|DNSResolution     | Error en la resolución DNS para la dirección de destino.        |
|NetworkSecurityRule    | Una regla de NSG bloquea el tráfico (se devuelve la regla).        |
|UserDefinedRoute|El tráfico se interrumpe debido a una ruta del sistema o definida por el usuario. |

### <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo solucionar problemas de conexiones mediante [Azure Portal](network-watcher-connectivity-portal.md), [PowerShell](network-watcher-connectivity-powershell.md), la [CLI de Azure](network-watcher-connectivity-cli.md) o la [API de REST](network-watcher-connectivity-rest.md).