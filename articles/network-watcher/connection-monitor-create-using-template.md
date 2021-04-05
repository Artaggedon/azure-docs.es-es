---
title: Creación de una instancia de Connection Monitor mediante una plantilla de Resource Manager
titleSuffix: Azure Network Watcher
description: Aprenda a crear una instancia de Connection Monitor mediante ARMClient.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: vinigam
ms.openlocfilehash: 46bdaf932d4224bf97b46e7713d49d815ca1bcdd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99833004"
---
# <a name="create-a-connection-monitor-using-the-arm-template"></a>Creación de una instancia de Connection Monitor mediante una plantilla de Resource Manager

> [!IMPORTANT]
> A partir del 1 de julio de 2021, no podrá agregar nuevas pruebas en un área de trabajo existente ni habilitar un área de trabajo nueva en Network Performance Monitor. Tampoco podrá agregar nuevos monitores de conexión en Connection Monitor (clásico). Puede seguir usando las pruebas y los monitores de conexión creados antes del 1 de julio de 2021. Para minimizar la interrupción del servicio en las cargas de trabajo actuales, [migre las pruebas desde Network Performance Monitor ](migrate-to-connection-monitor-from-network-performance-monitor.md) o [desde Connection Monitor (clásico)](migrate-to-connection-monitor-from-connection-monitor-classic.md) a la nueva instancia de Connection Monitor en Azure Network Watcher antes del 29 de febrero de 2024.

Aprenda a crear una instancia de Connection Monitor para supervisar la comunicación entre los recursos mediante ARMClient. Admite implementaciones híbridas y en la nube de Azure.


## <a name="before-you-begin"></a>Antes de empezar 

En los monitores de conexión que crea en Connection Monitor, puede agregar máquinas locales y máquinas virtuales de Azure como orígenes. Estos monitores de conexión también pueden supervisar la conectividad con los puntos de conexión. Los puntos de conexión pueden estar en Azure o en cualquier otra dirección URL o IP.

Connection Monitor incluye estas entidades:

* **Recurso de monitor de conexión**: un recurso de Azure específico de la región. Todas las entidades siguientes son propiedades de un recurso de monitor de conexión.
* **Punto de conexión**: un origen o destino que participa en las comprobaciones de conectividad. Algunos ejemplos de puntos de conexión son las máquinas virtuales de Azure, los agentes locales, las direcciones URL y las direcciones IP.
* **Configuración de prueba**: la configuración de una prueba específica para el protocolo. En función del protocolo elegido, puede definir el puerto, los umbrales, la frecuencia de las pruebas y otros parámetros.
* **Grupo de prueba**: el grupo que contiene puntos de conexión de origen, puntos de conexión de destino y configuraciones de prueba. Un monitor de conexión puede contener más de un grupo de prueba.
* **Prueba**: la combinación de un punto de conexión de origen, un punto de conexión de destino y una configuración de prueba. Una prueba es el nivel más granular en el que están disponibles los datos de supervisión. Los datos de supervisión incluyen el porcentaje de comprobaciones con errores y el tiempo de ida y vuelta (RTT).

    ![Diagrama que muestra un monitor de conexión y donde se define la relación entre los grupos de prueba y las pruebas](./media/connection-monitor-2-preview/cm-tg-2.png)

## <a name="steps-to-create-with-sample-arm-template"></a>Pasos para crear una plantilla de ARM de ejemplo

Use el código siguiente para crear un monitor de conexión mediante ARMClient.

```armclient
$connectionMonitorName = "sampleConnectionMonitor"

$ARM = "https://management.azure.com"

$SUB = "subscriptions/<subscription id 1>;"

$NW = "resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher\_<region>"

$body =

"{

location: '<region>',

properties: {

endpoints: [{

name: 'endpoint_workspace_machine',

type: 'MMAWorkspaceMachine',

resourceId: '/subscriptions/<subscription id>/resourcegroups/<resource group>/providers/Microsoft.OperationalInsights/workspaces/sampleWorkspace',

//Example 1: Choose a machine

address : '<non-Azure machine FQDN>'
}

//Example 2: Select IP from chosen machines

address : '<non-Azure machine FQDN>

"scope": {
      "include": [
            {
                  "address": "<IP belonging to machine chosen above>"  
        }
       ]
      }
   }    
   
name: 'endpoint_workspace_network',

type: 'MMAWorkspaceNetwork',

resourceId: '/subscriptions/<subscription id>/resourcegroups/<resource group>/providers/Microsoft.OperationalInsights/workspaces/sampleWorkspace',

 coverage level : 'high', //Optional
 
 //Include subnets. You can also exclude IPs from subnet to exclude from monitoring
 
 scope: {
      "include": [
            {
                  "address": "<subnet 1 mask>" // Eg: 10.10.1.0/28
            },
            {
                  "address": "<subnet 2 mask>" 
            }
      ],
      "exclude": [
            { 
            "address" : "<ip-from-included-subnets-that-should-be-excluded>"
        }
      ]
     }
},

//Use a Azure VM as an endpoint
{

name: 'endpoint_virtualmachine',

resourceId: '/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<vm-name>'

},

//Use an Azure VNET or Subnet as an endpoint

 {

name: 'endpoint_vnet_subnet',

resourceId: '<resource id of VNET or subnet'
coverage level: 'high' //Optional

//Scope is optional.

  "scope": {
      "include": [
            {
                  "address": "<subnet 1 mask>" // Eg: 10.10.1.0/28 .This subnet should match with any existing subnet in vnet
            }
      ],
    "exclude": [
            {
                  "address": "<ip-from-included-subnets-that-should-be-excluded>" // If used with include, IP should be part of the subnet defined above. Without include, this could be any address within vnet range or any specific subnet range as a whole.
            }
      ]
  }
   },

//Endpoint as a URL
{

name: 'azure portal'

address: '<URL>'

   },

//Endpoint as an IP 
 {

    name: 'ip',

     address: '<IP>'

 }

  ],

  testGroups: [{

    name: 'Connectivity to Azure Portal and Public IP',

    testConfigurations: ['http', 'https', 'tcpEnabled', 'icmpEnabled'],

    sources: ['vm1', 'workspace'],

    destinations: ['azure portal', 'ip']

   },

{

    name: 'Connectivty from Azure VM 1 to Azure VM 2',

   // Choose your protocol
   
    testConfigurations: ['http', 'https', 'tcpDisabled', 'icmpDisabled'],

    sources: ['vm1'],

    destinations: ['vm2'],

    disable: true

   }

  ],

  testConfigurations: [{

    name: 'http',

    testFrequencySec: <frequency>,

    protocol: 'HTTP',

    successThreshold: {

     checksFailedPercent: <threshold for checks failed %>,

     roundTripTimeMs: <threshold for RTT>

    }

   }, {

    name: 'https',

    testFrequencySec: <frequency>,

    protocol: 'HTTP',

    httpConfiguration: {
    
     port: '<port of choice>'
  
    preferHTTPS: true // If port chosen is not 80 or 443
    
    method: 'GET', //Choose GET or POST
    
    path: '/', //Specify path for request
         
    requestHeaders: [
            {
              "name": "Content-Type",
              "value": "appication/json"
            }
          ],
          
    validStatusCodeRanges: [ "102", "200-202", "3xx" ], //Samples
          
    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, 
   {

    name: 'tcpEnabled',

    testFrequencySec: <frequency>,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'icmpEnabled',

    testFrequencySec: <frequency>,

    protocol: 'ICMP',

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'icmpDisabled',

    testFrequencySec: <frequency>,

    protocol: 'ICMP',

    icmpConfiguration: {

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'tcpDisabled',

    testFrequencySec: <frequency>,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80,

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }

  ]

 }

} "
```

Este es el comando de implementación:
```
armclient PUT $ARM/$SUB/$NW/connectionMonitors/$connectionMonitorName/?api-version=2019-07-01 $body -verbose
```

## <a name="description-of-properties"></a>Descripción de las propiedades

* connectionMonitorName: nombre del recurso de Connection Monitor.

* SUB: identificador de la suscripción donde quiere crear el monitor de conexión.

* NW: identificador de recurso de Network Watcher en el que se creará el monitor de conexión. 

* location: región en la que se creará el monitor de conexión.

* Puntos de conexión
    * name: nombre único de cada punto de conexión.
    * resourceId: en el caso de los puntos de conexión de Azure, el id. de recurso hace referencia al id. de recurso de Azure Resource Manager para las máquinas virtuales. En el caso de los puntos de conexión que no son de Azure, el id. de recurso hace referencia al id. de recurso de Azure Resource Manager para el área de trabajo de Log Analytics vinculada a agentes que no son de Azure.
    * address: solo se aplica cuando no se especifica ningún identificador de recurso o si el identificador de recurso es el área de trabajo de Log Analytics. Si se usa con el identificador de recurso de Log Analytics, se refiere al FQDN del agente que se puede usar para la supervisión. Si se usa sin el identificador de recurso, puede ser la dirección URL o IP de cualquier punto de conexión público.
    * filter: en el caso de los puntos de conexión que no son de Azure, use este valor para seleccionar los agentes del área de trabajo de Log Analytics que se usarán para supervisar los recursos del monitor de conexión. Si no se establecen filtros, todos los agentes que pertenezcan al área de trabajo Log Analytics se pueden usar en la supervisión.
        * type: establezca el tipo como "dirección del agente".
        * address: establezca la dirección como el FQDN del agente local.

* Grupos de prueba
    * name: asigne un nombre al grupo de prueba.
    * testConfigurations: configuraciones de prueba basadas en los puntos de conexión de origen que se conectan a los puntos de conexión de destino.
    * sources: elija entre los puntos de conexión creados anteriormente. Los puntos de conexión de origen basados en Azure deben tener instalada la extensión de Azure Network Watcher y los que no se basan en Azure deben tener instalado el agente de Azure Log Analytics. Para instalar un agente para el origen, consulte [Instalación de los agentes de supervisión](./connection-monitor-overview.md#install-monitoring-agents).
    * destinations: elija entre los puntos de conexión creados anteriormente. Se puede supervisar la conectividad a las máquinas virtuales de Azure o a cualquier punto de conexión (una IP pública, una dirección URL o un FQDN) si se especifican como destinos. En un solo grupo de prueba, puede agregar máquinas virtuales de Azure, direcciones URL de Office 365, direcciones URL de Dynamics 365 y puntos de conexión personalizados.
    * disable: use este campo para deshabilitar la supervisión de todos los orígenes y destinos que especifica el grupo de prueba.

* Configuraciones de prueba
    * name: asigne un nombre a la configuración de prueba.
    * testFrequencySec: especifique la frecuencia con la que los orígenes harán ping a los destinos en el protocolo y el puerto especificados. Puede elegir 30 segundos, 1 minuto, 5 minutos, 15 minutos o 30 minutos. Los orígenes probarán la conectividad a los destinos según el valor que elija. Por ejemplo, si selecciona 30 segundos, los orígenes comprobarán la conectividad con el destino al menos una vez en un período de 30 segundos.
    * protocol: puede elegir TCP, ICMP, HTTP o HTTPS. Dependiendo del protocolo, puede realizar algunas configuraciones específicas del protocolo.
    
        * preferHTTPS: especifique si se usará HTTPS sobre HTTP cuando el puerto usado no sea el 80 ni el 443.
        * port: especifique el puerto de destino de su elección.
        * disableTraceRoute: se aplica a las configuraciones de prueba cuyo protocolo es TCP o ICMP. Impide que los orígenes detecten la topología y el valor de RTT de salto a salto.
        * method: se aplica a las configuraciones de prueba cuyo protocolo es HTTP. Seleccione el método de solicitud HTTP, ya sea GET o POST.
        * path: especifique los parámetros de ruta de acceso para anexar a la dirección URL.
        * validStatusCodes: elija los códigos de estado aplicables. Si el código de respuesta no coincide con esta lista, obtendrá un mensaje de diagnóstico.
        * requestHeaders: especifique cadenas de encabezado de solicitud personalizadas que se pasarán al destino.
        
    * successThreshold: puede establecer los umbrales de estos parámetros de red:
        * checksFailedPercent: establezca el porcentaje de comprobaciones que pueden presentar errores cuando los orígenes comprueban la conectividad a los destinos mediante los criterios especificados. En el caso del protocolo TCP o ICMP, el porcentaje de comprobaciones con error puede ser igual al porcentaje de pérdida de paquetes. En el caso del protocolo HTTP, este campo representa el porcentaje de solicitudes HTTP que no recibieron respuesta.
        * roundTripTimeMs: establezca el RTT en milisegundos para indicar cuánto tiempo pueden tardar los orígenes en conectarse al destino a través de la configuración de prueba.

## <a name="scale-limits"></a>Límites de escalado

Los monitores de conexión tienen los límites de escala siguientes:

* Máximo de monitores de conexión por suscripción por región: 100
* Máximo de grupos de prueba por monitor de conexión: 20
* Máximo de orígenes y destinos por monitor de conexión: 100
* Máximo de configuraciones de prueba por monitor de conexión: 20 a través de ARMClient

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [analizar los datos de supervisión y a definir alertas](./connection-monitor-overview.md#analyze-monitoring-data-and-set-alerts).
* Aprenda a [diagnosticar los problemas de la red](./connection-monitor-overview.md#diagnose-issues-in-your-network).
