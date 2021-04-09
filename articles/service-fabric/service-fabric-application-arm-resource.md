---
title: Implementación y actualización con Azure Resource Manager
description: Aprenda a implementar aplicaciones y servicios en un clúster de Service Fabric mediante una plantilla de Azure Resource Manager.
ms.topic: conceptual
ms.date: 12/06/2017
ms.openlocfilehash: ed6bc7d96cb3ea0934929e6543c5e637a9f42c1f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97930844"
---
# <a name="manage-applications-and-services-as-azure-resource-manager-resources"></a>Administración de aplicaciones y servicios como recursos de Azure Resource Manager

Puede implementar aplicaciones y servicios en el clúster de Service Fabric mediante Azure Resource Manager. Esto significa que en lugar de implementar y administrar aplicaciones mediante PowerShell o la CLI después de tener que esperar a que el clúster esté listo, ahora puede expresar las aplicaciones y servicios en JSON e implementarlas en la misma plantilla de Resource Manager que la del clúster. El proceso de registro de la aplicación, su aprovisionamiento e implementación, sucede todo en un paso.

Esta es la manera recomendada de implementar cualquier aplicación de administración, configuración o gobernanza del clúster que necesite en el clúster. Aquí se incluyen la [aplicación de orquestación de revisiones](service-fabric-patch-orchestration-application.md), los guardianes o cualquier aplicación que se deba ejecutar en el clúster antes de implementar otras aplicaciones o servicios. 

Cuando sea aplicable, administre las aplicaciones como recursos de Resource Manager para mejorar:
* Registro de auditoría: Resource Manager audita las operaciones y mantiene un *registro de actividad* detallado que puede ayudarle a realizar el seguimiento de los cambios realizados en estas aplicaciones y en el clúster.
* Control de acceso basado en rol de Azure (Azure RBAC): la administración del acceso a los clústeres, así como a las aplicaciones implementadas en el clúster, puede realizarse mediante la misma plantilla de Resource Manager.
* Azure Resource Manager (mediante Azure Portal) se convierte en el lugar central para administrar el clúster y las implementaciones de aplicaciones críticas.

En el siguiente fragmento se muestran los diferentes tipos de recursos que se pueden administrar mediante una plantilla:

```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

## <a name="add-a-new-application-to-your-resource-manager-template"></a>Adición de una nueva aplicación a la plantilla de Resource Manager

1. Prepare la plantilla de Resource Manager del clúster para la implementación. Para más información al respecto, consulte [Creación de un clúster de Service Fabric con Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).
2. Piense en algunas de las aplicaciones que planea implementar en el clúster. ¿Habrá alguna que siempre esté en ejecución de la que puedan tener dependencias otras aplicaciones? ¿Tiene pensado implementar aplicaciones de configuración o de gobernanza del clúster? Estos tipos de aplicaciones se administran mejor mediante una plantilla de Resource Manager, como se ha indicado anteriormente. 
3. Una vez que ha estimado qué aplicaciones quiere implementar de esta forma, se deben empaquetar, comprimir y colocar en un recurso compartido de archivos. El recurso compartido debe ser accesible a través de un punto de conexión de REST para que Azure Resource Manager lo use durante la implementación. Para más información, consulte [Creación de una cuenta de almacenamiento](service-fabric-concept-resource-model.md#create-a-storage-account).
4. En la plantilla de Resource Manager, debajo de la declaración del clúster, describe las propiedades de cada aplicación. Estas propiedades incluyen el recuento de instancias o réplicas y las cadenas de dependencia entre los recursos (otras aplicaciones o servicios). Tenga en cuenta que esta especificación no sustituye a los manifiestos de aplicación o servicio, sino que describe algo de lo que contienen como parte de la plantilla de Resource Manager del clúster. A continuación se muestra una plantilla de ejemplo que incluye la implementación de un servicio sin estado *Service1* y un servicio con estado *Service2* como parte de *Application1*:

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "clusterName": {
        "type": "string",
        "defaultValue": "Cluster",
        "metadata": {
          "description": "Name of your cluster - Between 3 and 23 characters. Letters and numbers only."
        }
      },
      "applicationTypeName": {
        "type": "string",
        "defaultValue": "ApplicationType",
        "metadata": {
          "description": "The application type name."
        }
      },
      "applicationTypeVersion": {
        "type": "string",
        "defaultValue": "1",
        "metadata": {
          "description": "The application type version."
        }
      },
      "appPackageUrl": {
        "type": "string",
        "metadata": {
          "description": "The URL to the application package sfpkg file."
        }
      },
      "applicationName": {
        "type": "string",
        "defaultValue": "Application1",
        "metadata": {
          "description": "The name of the application resource."
        }
      },
      "serviceName": {
        "type": "string",
        "defaultValue": "Application1~Service1",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName": {
        "type": "string",
        "defaultValue": "Service1Type",
        "metadata": {
          "description": "The name of the service type."
        }
      },
      "serviceName2": {
        "type": "string",
        "defaultValue": "Application1~Service2",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName2": {
        "type": "string",
        "defaultValue": "Service2Type",
        "metadata": {
          "description": "The name of the service type."
        }
      }
    },
    "variables": {
      "clusterLocation": "[resourcegroup().location]"
    },
    "resources": [
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [],
        "properties": {
          "provisioningState": "Default"
        }
      },
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "appPackageUrl": "[parameters('appPackageUrl')]"
        }
      },
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applications",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "typeName": "[parameters('applicationTypeName')]",
          "typeVersion": "[parameters('applicationTypeVersion')]",
          "parameters": {},
          "upgradePolicy": {
            "upgradeReplicaSetCheckTimeout": "01:00:00.0",
            "forceRestart": "false",
            "rollingUpgradeMonitoringPolicy": {
              "healthCheckWaitDuration": "00:02:00.0",
              "healthCheckStableDuration": "00:05:00.0",
              "healthCheckRetryTimeout": "00:10:00.0",
              "upgradeTimeout": "01:00:00.0",
              "upgradeDomainTimeout": "00:20:00.0"
            },
            "applicationHealthPolicy": {
              "considerWarningAsError": "false",
              "maxPercentUnhealthyDeployedApplications": "50",
              "defaultServiceTypeHealthPolicy": {
                "maxPercentUnhealthyServices": "50",
                "maxPercentUnhealthyPartitionsPerService": "50",
                "maxPercentUnhealthyReplicasPerPartition": "50"
              }
            }
          }
        }
      },
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateless",
          "serviceTypeName": "[parameters('serviceTypeName')]",
          "instanceCount": "-1",
          "partitionDescription": {
            "partitionScheme": "Singleton"
          },
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": []
        }
      },
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName2'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateful",
          "serviceTypeName": "[parameters('serviceTypeName2')]",
          "targetReplicaSetSize": "3",
          "minReplicaSetSize": "2",
          "replicaRestartWaitDuration": "00:01:00.0",
          "quorumLossWaitDuration": "00:02:00.0",
          "standByReplicaKeepDuration": "00:00:30.0",
          "partitionDescription": {
            "partitionScheme": "UniformInt64Range",
            "count": "5",
            "lowKey": "1",
            "highKey": "5"
          },
          "hasPersistedState": "true",
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": [],
          "defaultMoveCost": "Low"
        }
      }
    ]
   }
   ```

   > [!NOTE] 
   > Consulte la [referencia de Azure Resource Manager](/azure/templates/microsoft.servicefabric/clusters/applicationtypes) de Service Fabric para buscar el uso y los detalles de las propiedades de plantilla individuales.

5. Así que, implemente. 

## <a name="remove-service-fabric-resource-provider-application-resource"></a>Quitar el recurso de la aplicación de proveedor de recursos de Service Fabric
Lo siguiente desencadenará el paquete de la aplicación para que sea no aprovisionado desde el clúster y limpiará el espacio usado en el disco:
```powershell
Get-AzureRmResource -ResourceId /subscriptions/{sid}/resourceGroups/{rg}/providers/Microsoft.ServiceFabric/clusters/{cluster}/applicationTypes/{apptType}/versions/{version} -ApiVersion "2019-03-01" | Remove-AzureRmResource -Force -ApiVersion "2017-07-01-preview"
```
Quitar Microsoft.ServiceFabric/clusters/application de la plantilla ARM no anulará el aprovisionamiento de la aplicación

>[!NOTE]
> Una vez completada la eliminación ya no debería ver la versión del paquete en SFX ni en ARM. No puede eliminar el recurso de la versión del tipo de aplicación con el que se está ejecutando la aplicación; ARM/SFRP evitará esto. Si intenta anular el aprovisionamiento del paquete en ejecución, el tiempo de ejecución de SF lo evitará.


## <a name="manage-an-existing-application-via-resource-manager"></a>Administración de una aplicación existente mediante Resource Manager

Si el clúster ya está activo y algunas de las aplicaciones que le gustaría administrar como recursos de Resource Manager ya están implementadas en él, en lugar de quitar las aplicaciones y volver a implementarlas, puede usar una llamada PUT con las mismas API para que las aplicaciones se confirmen como recursos de Resource Manager. Para más información, consulte [¿Qué es el modelo de recurso de aplicación de Service Fabric?](./service-fabric-concept-resource-model.md)

> [!NOTE]
> Para permitir una actualización del clúster para ignorar las aplicaciones en mal estado, el cliente puede especificar "maxPercentUnhealthyApplications: 100" en la sección "upgradeDescription/healthPolicy"; encontrará descripciones detalladas de todas las opciones de configuración en la [documentación de la directiva de actualización del clúster de la API de REST de Service Fabrics](/rest/api/servicefabric/sfrp-model-clusterupgradepolicy).

## <a name="next-steps"></a>Pasos siguientes

* Use la [CLI de Service Fabric](service-fabric-cli.md) o [PowerShell](service-fabric-deploy-remove-applications.md) para implementar otras aplicaciones en el clúster. 
* [Actualización de un clúster de Azure Service Fabric](service-fabric-cluster-upgrade.md)
