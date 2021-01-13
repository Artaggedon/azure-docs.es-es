---
title: Permisos de RBAC de Azure necesarios para usar funcionalidades
titleSuffix: Azure Network Watcher
description: Obtenga información acerca de los permisos de control de acceso basado en roles de Azure necesarios para trabajar con las funcionalidades de Network Watcher.
services: network-watcher
documentationcenter: ''
author: damendo
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2021
ms.author: damendo
ms.openlocfilehash: 47418b9c5235255ff7dbf4a1a151e51e4c9aba58
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98019676"
---
# <a name="azure-role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Permisos del control de acceso basado en roles de Azure necesarios para usar las funcionalidades de Network Watcher

El control de acceso basado en roles (Azure RBAC) de Azure le permite asignar únicamente acciones específicas a los miembros de su organización que las necesiten para completar sus responsabilidades asignadas. Para usar las funcionalidades de Network Watcher, debe asignar a la cuenta con la que inicia sesión en Azure a los roles integrados de [propietario](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [colaborador](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor), o [colaborador de red](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor), o se le debe asignar un [rol personalizado](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) al que se asignan las acciones que se enumeran para cada funcionalidad de Network Watcher en las secciones siguientes. Para obtener más información sobre las funcionalidades de Network Watcher, consulte [¿Qué es Network Watcher?](network-watcher-monitoring-overview.md)

## <a name="network-watcher"></a>Network Watcher

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Obtener Network Watcher                                          |
| Microsoft.Network/networkWatchers/write                             | Crear o actualizar Network Watcher                             |
| Microsoft.Network/networkWatchers/delete                            | Eliminar Network Watcher                                       |

## <a name="nsg-flow-logs"></a>Registros de flujo de NSG

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Configurar un registro de flujo                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Consultar el estado de un registro de flujo                                    |

## <a name="connection-troubleshoot"></a>Solución de problemas de conexión

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectivityCheck/action          | Iniciar una prueba de solución de problemas de conexión
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Consultar resultados de una prueba de solución de problemas de conexión                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Ejecutar una prueba de solución de problemas de conexión                             |

## <a name="connection-monitor"></a>Monitor de conexión

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Iniciar un monitor de conexión                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Detener un monitor de conexión                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Consultar un monitor de conexión                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Obtener un monitor de conexión                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Creación de un monitor de conexión                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Eliminar un monitor de conexión                                    |

## <a name="packet-capture"></a>Captura de paquetes

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Consultar el estado de una captura de paquetes                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Detención de una captura de paquetes                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Obtención de una captura de paquetes                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | Crear una captura de paquetes                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Eliminación de una captura de paquetes                                        |

## <a name="ip-flow-verify"></a>Comprobación de flujo de IP

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Verificar un flujo de IP                                              |

## <a name="next-hop"></a>Próximo salto

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Obtener el próximo salto de una VM                                     |

## <a name="network-security-group-view"></a>Vista de grupo de seguridad de red

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Ver grupos de seguridad                                           |

## <a name="topology"></a>Topología

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Obtener topología                                                   |
| Microsoft.Network/networkWatchers/topology/read                     | Lo mismo que antes.                                                  |

## <a name="reachability-report"></a>Informe de disponibilidad

| Acción                                                              | Descripción                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Obtener un informe de disponibilidad de Azure                               |


## <a name="additional-actions"></a>Acciones adicionales

Las funcionalidades de Network Watcher también requieren las siguientes acciones:

| Acciones                                                           | Descripción                                                    |
| ---------                                                           | -------------                                                  |
| Microsoft.Authorization/\*/Read                                     | Se usa para capturar asignaciones de roles de Azure y definiciones de directivas.          |
| Microsoft.Resources/subscriptions/resourceGroups/Read               | Se usa para enumerar todos los grupos de recursos de una suscripción.    |
| Microsoft.Storage/storageAccounts/Read                              | Se usa para obtener las propiedades de la cuenta de almacenamiento especificada.   |
| Microsoft.Storage/storageAccounts/listServiceSas/Action, </br> Microsoft.Storage/storageAccounts/listAccountSas/Action, <br> Microsoft.Storage/storageAccounts/listKeys/Action| Se usa para capturar firmas de acceso compartido (SAS) que permiten el [acceso seguro a la cuenta de almacenamiento](../storage/common/storage-sas-overview.md) y escriben en la cuenta de almacenamiento. |
| Microsoft.Compute/virtualMachines/Read, </br> Microsoft.Compute/virtualMachines/Write| Se usa para iniciar sesión en la máquina virtual, realizar una captura de paquetes y cargarlos en la cuenta de almacenamiento.|
| Microsoft.Compute/virtualMachines/extensions/Read </br> Microsoft.Compute/virtualMachines/extensions/Write| Se usa para comprobar si la extensión Network Watcher está presente e instalar si es necesario. |
| Microsoft.Compute/virtualMachineScaleSets/Read, </br> Microsoft.Compute/virtualMachineScaleSets/Write| Se usa para acceder a conjuntos de escalado de máquinas virtuales, realizar capturas de paquetes y cargarlos en una cuenta de almacenamiento.|
| Microsoft.Compute/virtualMachineScaleSets/extensions/Read, </br> Microsoft.Compute/virtualMachineScaleSets/extensions/Write| Se usa para comprobar si la extensión Network Watcher está presente e instalar si es necesario. |
| Microsoft.Insights/alertRules/*                                     | Se usa para configurar alertas de métricas.                                     |
| Microsoft.Support/*                                                 | Se usa para crear y actualizar incidencias de soporte técnico desde Network Watcher. |