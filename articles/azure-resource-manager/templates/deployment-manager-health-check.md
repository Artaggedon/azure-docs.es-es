---
title: 'Lanzamiento de la integración de mantenimiento: Azure Deployment Manager'
description: Describe cómo implementar un servicio en varias regiones con Azure Deployment Manager. Muestra las prácticas de implementación segura para comprobar la estabilidad de la implementación antes de proceder en todas las regiones.
author: mumian
ms.topic: conceptual
ms.date: 09/21/2020
ms.author: jgao
ms.openlocfilehash: ae95269dbac3ef1561e19d4b7ea5dd383c1eed73
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99536974"
---
# <a name="introduce-health-integration-rollout-to-azure-deployment-manager-public-preview"></a>Introducción de la implementación de integración de mantenimiento en Azure Deployment Manager (versión preliminar pública)

[Azure Deployment Manager](./deployment-manager-overview.md) permite realizar implementaciones por fases de recursos de Azure Resource Manager. Los recursos se implementan región por región de forma ordenada. La comprobación de estado integrada de Azure Deployment Manager puede supervisar las implementaciones y detener de forma automática las que sean problemáticas, para que pueda solucionar los problemas y reducir la escala del impacto. Esta característica puede reducir la no disponibilidad del servicio causada por regresiones en las actualizaciones.

## <a name="health-monitoring-providers"></a>Proveedores de seguimiento de mantenimiento

Para que la integración del mantenimiento sea lo más sencilla posible, Microsoft ha trabajado con varias de las compañías de seguimiento del estado de los servicios más punteras para ofrecer una solución sencilla de copiar y pegar para integrar las comprobaciones de estado en las implementaciones. Si todavía no usa un monitor de estado, estas son soluciones excelentes para comenzar:

| ![azure monitor, proveedor de seguimiento de estado de azure deployment manager](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-azure-monitor.svg)| ![datadog, proveedor de seguimiento de estado de azure deployment manager](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-datadog.svg) | ![site24x7, proveedor de seguimiento de estado de azure deployment manager](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-site24x7.svg) | ![wavefront, proveedor de seguimiento de estado de azure deployment manager](./media/deployment-manager-health-check/azure-deployment-manager-health-monitor-provider-wavefront.svg) |
|-----|-----|------|------|
|Azure Monitor, la plataforma de visibilidad de la pila completa de Microsoft para la supervisión y el análisis de entornos de nube nativos e híbridos. |Datadog, la plataforma líder de supervisión y análisis para los entornos de nube modernos. Vea [cómo se integra Datadog con Azure Deployment Manager](https://www.datadoghq.com/azure-deployment-manager/).|Site24x7, la solución integral de supervisión de servicios de la nube privada y pública. Vea [cómo se integra Site24x7 con Azure Deployment Manager](https://www.site24x7.com/azure/adm.html).| Wavefront, la plataforma de supervisión y análisis para entornos de aplicaciones de varias nubes. Vea [cómo se integra Wavefront con Azure Deployment Manager](https://go.wavefront.com/wavefront-adm/).|

## <a name="how-service-health-is-determined"></a>Cómo se determina el estado del servicio

[Los proveedores de seguimiento de mantenimiento](#health-monitoring-providers) ofrecen varios mecanismos para la supervisión de los servicios y alertarle de cualquier problema de mantenimiento del servicio. [Azure Monitor](../../azure-monitor/overview.md) es un ejemplo de este tipo de oferta. Azure Monitor se puede usar para crear alertas cuando se superan determinados umbrales. Por ejemplo, la utilización de la CPU y la memoria supera los niveles esperados cuando se implementa una actualización nueva en el servicio. Cuando se le notifique, pueda tomar acciones correctivas.

Estos proveedores de mantenimiento normalmente ofrecen API de REST para que el estado de los monitores del servicio se pueda examinar mediante programación. Las API REST pueden devolver una sencilla señal de estado correcto o incorrecto (determinada por el código de respuesta HTTP), o bien información detallada sobre las señales que reciben.

El nuevo paso `healthCheck` de Azure Deployment Manager permite declarar códigos HTTP que indican un servicio en buen estado. En el caso de los resultados de REST complejos, puede especificar expresiones regulares que, si coinciden, indican una respuesta correcta.

El flujo de configuración de las comprobaciones de estado de Azure Deployment Manager es el siguiente:

1. Crea los monitores de mantenimiento a través del proveedor de servicios de mantenimiento que elija.
1. Crea uno o varios pasos `healthCheck` como parte de la implementación de Azure Deployment Manager. Rellene los pasos `healthCheck` con la siguiente información:

    1. El URI de la API REST para los monitores de mantenimiento (según lo definido por el proveedor de servicios de mantenimiento).
    1. Información de autenticación. Actualmente solo se admite la autenticación de estilo de clave de API. En Azure Monitor, el tipo de autenticación debe establecerse como `RolloutIdentity`, ya que la identidad administrada asignada por el usuario que se usa para la implementación de Azure Deployment Manager se amplía para Azure Monitor.
    1. [Códigos de estado HTTP](https://www.wikipedia.org/wiki/List_of_HTTP_status_codes) o expresiones regulares que definen una respuesta correcta. Puede proporcionar expresiones regulares, que todas tengan que coincidir para que la respuesta se considere correcta, o bien expresiones de las que cualquiera tenga que coincidir para que la respuesta se considere correcta. Se admiten los dos métodos.

    El siguiente JSON es un ejemplo para integrar Azure Monitor con Azure Deployment Manager. En el ejemplo se usa `RolloutIdentity` y se establece una comprobación de estado en la que se realiza una implementación si no hay ninguna alerta. La única API de Azure Monitor compatible: [Alertas – Obtener todo](/rest/api/monitor/alertsmanagement/alerts/getall).

    ```json
    {
      "type": "Microsoft.DeploymentManager/steps",
      "apiVersion": "2018-09-01-preview",
      "name": "healthCheckStep",
      "location": "[parameters('azureResourceLocation')]",
      "properties": {
        "stepType": "healthCheck",
        "attributes": {
          "waitDuration": "PT1M",
          "maxElasticDuration": "PT1M",
          "healthyStateDuration": "PT1M",
          "type": "REST",
          "properties": {
            "healthChecks": [
              {
                "name": "appHealth",
                "request": {
                  "method": "GET",
                  "uri": "[parameters('healthCheckUrl')]",
                  "authentication": {
                    "type": "RolloutIdentity"
                  }
                },
                "response": {
                  "successStatusCodes": [
                    "200"
                  ],
                  "regex": {
                    "matches": [
                      "\"value\":\\[\\]"
                    ],
                    "matchQuantifier": "All"
                  }
                }
              }
            ]
          }
        }
      }
    }
    ```

    El siguiente JSON es un ejemplo de todos los demás proveedores de seguimiento de estado:

    ```json
    {
      "type": "Microsoft.DeploymentManager/steps",
      "apiVersion": "2018-09-01-preview",
      "name": "healthCheckStep",
      "location": "[parameters('azureResourceLocation')]",
      "properties": {
        "stepType": "healthCheck",
        "attributes": {
          "waitDuration": "PT0M",
          "maxElasticDuration": "PT0M",
          "healthyStateDuration": "PT1M",
          "type": "REST",
          "properties": {
            "healthChecks": [
              {
                "name": "appHealth",
                "request": {
                  "method": "GET",
                  "uri": "[parameters('healthCheckUrl')]",
                  "authentication": {
                    "type": "ApiKey",
                    "name": "code",
                    "in": "Query",
                    "value": "[parameters('healthCheckAuthAPIKey')]"
                  }
                },
                "response": {
                  "successStatusCodes": [
                    "200"
                  ],
                  "regex": {
                    "matches": [
                      "Status: healthy",
                      "Status: warning"
                    ],
                    "matchQuantifier": "Any"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

1. Invoque los pasos `healthCheck` en el momento adecuado de la implementación de Azure Deployment Manager. En el ejemplo siguiente, se invoca un paso `healthCheck` en `postDeploymentSteps` de `stepGroup2`.

    ```json
    "stepGroups": [
        {
            "name": "stepGroup1",
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup2",
            "dependsOnStepGroups": ["stepGroup1"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit1.name)]",
            "postDeploymentSteps": [
                {
                    "stepId": "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
                }
            ]
        },
        {
            "name": "stepGroup3",
            "dependsOnStepGroups": ["stepGroup2"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup4",
            "dependsOnStepGroups": ["stepGroup3"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit1.name)]",
            "postDeploymentSteps": []
        }
    ]
    ```

Para realizar un ejemplo, vea [Tutorial: Uso de la comprobación de estado en Azure Deployment Manager](./deployment-manager-tutorial-health-check.md).

## <a name="phases-of-a-health-check"></a>Fases de una comprobación de estado

En este momento, Azure Deployment Manager sabe cómo consultar el estado del servicio y en qué fases de la implementación hacerlo. Pero Azure Deployment Manager también permite la configuración en profundidad de los intervalos de estas comprobaciones. Se ejecuta un paso `healthCheck` en tres fases secuenciales, todas con duraciones que se pueden configurar:

1. Esperar

    1. Una vez que se ha completado una operación de implementación, se pueden reiniciar las máquinas virtuales, volver a configurar en función de los datos nuevos o incluso iniciarse por primera vez. Los servicios también necesitan tiempo para empezar a emitir señales de mantenimiento que el proveedor de supervisión de mantenimiento combine en algo útil. Durante este complicado proceso, es posible que no tenga sentido comprobar el estado de servicio dado que la actualización todavía no ha alcanzado un estado estable. De hecho, el servicio puede oscilar entre los estados correcto e incorrecto mientras los recursos se consolidan.
    1. Durante la fase de espera, el estado del servicio no se supervisa. Esto se usa para proporcionar a los recursos implementados tiempo de preparación antes de que comience el proceso de comprobación de mantenimiento.

1. Elastic

    1. Como es imposible saber en todos los casos cuánto tardarán los recursos en ser estables, la fase Elastic (Elástica) permite un período de tiempo flexible entre el momento en que los recursos son potencialmente inestables y el momento en que deben mantener un estado estable correcto.
    1. Cuando se inicia la fase Elastic, Azure Deployment Manager comienza a sondear de forma periódica el punto de conexión REST proporcionado para comprobar el estado del servicio. El intervalo de sondeo es configurable.
    1. Si el monitor de mantenimiento devuelve señales que indican que el servicio está en mal estado, esas señales se omiten y continúan la fase Elastic y el sondeo.
    1. Cuando el monitor de mantenimiento devuelve señales que indican que el servicio está en buen estado, finaliza la fase Elastic y comienza la fase HealthyState.
    1. Por tanto, la duración especificada para la fase Elastic es la cantidad máxima de tiempo que se puede dedicar a sondear el mantenimiento del servicio antes de que una respuesta correcta se considere obligatoria.

1. HealthyState

    1. Durante la fase HealthyState, el estado del servicio se sondea de forma continua con el mismo intervalo que la fase Elastic.
    1. Se espera que el servicio mantenga señales correctas del proveedor de supervisión de mantenimiento a lo largo de toda la duración especificada.
    1. Si en cualquier momento se detecta una respuesta incorrecta, Azure Deployment Manager detendrá la implementación y devolverá la respuesta REST en la que se incluyen las señales de servicio en mal estado.
    1. Una vez que ha finalizado la duración de HealthyState, se completa el paso `healthCheck` y la implementación continúa con el paso siguiente.

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha obtenido información sobre cómo integrar el seguimiento de estado en Azure Deployment Manager. Pase al siguiente artículo, donde aprenderá a realizar implementaciones con Deployment Manager.

> [!div class="nextstepaction"]
> [Tutorial: Uso de la comprobación de estado en Azure Deployment Manager](./deployment-manager-tutorial-health-check.md)
