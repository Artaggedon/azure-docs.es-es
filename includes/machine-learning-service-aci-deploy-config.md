---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 4983c1e1e7f235fa7a5b748a0ce5b1c79176c849
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102511051"
---
Las entradas del documento `deploymentconfig.json` se asignan a los parámetros de [AciWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | Description |
| ----- | ----- | ----- |
| `computeType` | N/D | El destino de proceso. Para ACI, el valor debe ser `ACI`. |
| `containerResourceRequirements` | N/D | Contenedor de las entidades de CPU y memoria. |
| &emsp;&emsp;`cpu` | `cpu_cores` | Número de núcleos de CPU que se van a asignar. El valor predeterminado es `0.1`. |
| &emsp;&emsp;`memoryInGB` | `memory_gb` | Cantidad de memoria (en GB) que se va a asignar a este servicio web. El valor predeterminado es `0.5`. |
| `location` | `location` | Región de Azure en la que se implementará este servicio web. Si no se especifica, se usará la ubicación del área de trabajo. Aquí puede encontrar más información sobre las regiones disponibles: [regiones de ACI](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
| `authEnabled` | `auth_enabled` | Indica si se debe habilitar la autenticación para este servicio web. El valor predeterminado es False. |
| `sslEnabled` | `ssl_enabled` | Indica si se debe habilitar SSL para este servicio web. El valor predeterminado es False. |
| `appInsightsEnabled` | `enable_app_insights` | Indica si se debe habilitar AppInsights para este servicio web. El valor predeterminado es False. |
| `sslCertificate` | `ssl_cert_pem_file` | Archivo de certificado necesario si SSL está habilitado |
| `sslKey` | `ssl_key_pem_file` | Archivo de clave necesario si SSL está habilitado |
| `cname` | `ssl_cname` | CNAME si SSL está habilitado |
| `dnsNameLabel` | `dns_name_label` | Etiqueta del nombre DNS para el punto de conexión de puntuación. Si no se especifica, se generará una etiqueta de nombre DNS única para el punto de conexión de puntuación. |

El siguiente elemento JSON es un ejemplo de la configuración de implementación que se puede usar con la CLI:

```json
{
    "computeType": "aci",
    "containerResourceRequirements":
    {
        "cpu": 0.5,
        "memoryInGB": 1.0
    },
    "authEnabled": true,
    "sslEnabled": false,
    "appInsightsEnabled": false
}
```