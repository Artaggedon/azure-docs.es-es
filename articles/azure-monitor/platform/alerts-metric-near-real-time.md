---
title: Recursos compatibles para las alertas de métricas de Azure Monitor
description: Referencia sobre métricas y registros de soporte técnico para las alertas de métricas en Azure Monitor
author: harelbr
ms.author: harelbr
services: monitoring
ms.topic: conceptual
ms.date: 02/10/2021
ms.subservice: alerts
ms.openlocfilehash: ee281e60a0eb6f6192e63a9733146714e4aaf2eb
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100104389"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Recursos compatibles para las alertas de métricas de Azure Monitor

Azure Monitor ahora admite un [nuevo tipo de alerta de métrica](./alerts-overview.md) que tiene ventajas considerables sobre las anteriores [alertas de métrica clásicas](./alerts-classic.overview.md). Las métricas están disponibles para una [amplia lista de servicios de Azure](./metrics-supported.md). Las nuevas alertas admiten un subconjunto (creciente) de los tipos de recurso. En este artículo se muestra ese subconjunto.

También puede utilizar las nuevas alertas de métricas en los datos de registros populares almacenados en un área de trabajo de Log Analytics extraídos como métricas. Para obtener más información, consulte [Alertas de métricas para registros](./alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST support
Actualmente, solo puede crear alertas de métricas nuevas en Azure Portal, la [API de REST](/rest/api/monitor/metricalerts/) o [plantillas de Resource Manager](./alerts-metric-create-templates.md). La compatibilidad con la configuración de nuevas alertas mediante las versiones 2.0 de PowerShell y la CLI de Azure estará disponible próximamente.

## <a name="metrics-and-dimensions-supported"></a>Métricas y dimensiones compatibles
Las nuevas alertas de métricas admiten alertas de métricas que utilizan dimensiones. Puede usar dimensiones para filtrar las métricas al nivel adecuado. Todas las métricas compatibles, junto con las dimensiones aplicables, se pueden explorar y visualizar en el [Explorador de métricas de Azure Monitor](./metrics-charts.md).

Esta es la lista completa de los orígenes de métricas de Azure Monitor que se admiten por las nuevas alertas:

|Tipo de recurso  |Dimensiones compatibles |Alertas de varios recursos| Métricas disponibles|
|---------|---------|-----|----------|
|Microsoft.Aadiam/azureADMetrics | Sí | No | |
|Microsoft.ApiManagement/service | Sí | No | [API Management](./metrics-supported.md#microsoftapimanagementservice) |
|Microsoft.AppConfiguration/configurationStores |Sí | No | [App Configuration](./metrics-supported.md#microsoftappconfigurationconfigurationstores) |
|Microsoft.AppPlatform/Spring | Sí | No | [Azure Spring Cloud](./metrics-supported.md#microsoftappplatformspring) |
|Microsoft.Automation/automationAccounts | Sí| No | [Cuentas de Automation](./metrics-supported.md#microsoftautomationautomationaccounts) |
|Microsoft.AVS/privateClouds | No | No | [Azure VMware Solution](./metrics-supported.md#microsoftavsprivateclouds) |
|Microsoft.Batch/batchAccounts | Sí | No | [Cuentas de Batch](./metrics-supported.md#microsoftbatchbatchaccounts) |
|Microsoft.Cache/Redis | Sí | Sí | [Azure Cache for Redis](./metrics-supported.md#microsoftcacheredis) |
|Microsoft.ClassicCompute/domainNames/slots/roles | No | No | [Cloud Services clásico](./metrics-supported.md#microsoftclassiccomputedomainnamesslotsroles) |
|Microsoft.ClassicCompute/virtualMachines | No | No | [Virtual Machines clásico](./metrics-supported.md#microsoftclassiccomputevirtualmachines) |
|Microsoft.ClassicStorage/storageAccounts | Sí | No | [Cuentas de almacenamiento (clásicas)](./metrics-supported.md#microsoftclassicstoragestorageaccounts) |
|Microsoft.ClassicStorage/storageAccounts/blobServices | Sí | No | [Cuentas de almacenamiento (clásicas): blobs](./metrics-supported.md#microsoftclassicstoragestorageaccountsblobservices) |
|Microsoft.ClassicStorage/storageAccounts/fileServices | Sí | No | [Cuentas de almacenamiento (clásicas): archivos](./metrics-supported.md#microsoftclassicstoragestorageaccountsfileservices) |
|Microsoft.ClassicStorage/storageAccounts/queueServices | Sí | No | [Cuentas de almacenamiento (clásicas): colas](./metrics-supported.md#microsoftclassicstoragestorageaccountsqueueservices) |
|Microsoft.ClassicStorage/storageAccounts/tableServices | Sí | No | [Cuentas de almacenamiento (clásicas): tablas](./metrics-supported.md#microsoftclassicstoragestorageaccountstableservices) |
|Microsoft.CognitiveServices/accounts | Sí | No | [Cognitive Services](./metrics-supported.md#microsoftcognitiveservicesaccounts) |
|Microsoft.Compute/virtualMachines | Sí | Sí<sup>1</sup> | [Máquinas virtuales](./metrics-supported.md#microsoftcomputevirtualmachines) |
|Microsoft.Compute/virtualMachineScaleSets | Sí | No |[Conjuntos de escalado de máquinas virtuales](./metrics-supported.md#microsoftcomputevirtualmachinescalesets) |
|Microsoft.ContainerInstance/containerGroups | Sí| No | [Grupos de contenedores](./metrics-supported.md#microsoftcontainerinstancecontainergroups) |
|Microsoft.ContainerRegistry/registries | No | No | [Registros de contenedor](./metrics-supported.md#microsoftcontainerregistryregistries) |
|Microsoft.ContainerService/managedClusters | Sí | No | [Clústeres administrados](./metrics-supported.md#microsoftcontainerservicemanagedclusters) |
|Microsoft.DataBoxEdge/dataBoxEdgeDevices | Sí | Sí | [Data Box](./metrics-supported.md#microsoftdataboxedgedataboxedgedevices) |
|Microsoft.DataFactory/datafactories| Sí| No | [Factorías de datos V1](./metrics-supported.md#microsoftdatafactorydatafactories) |
|Microsoft.DataFactory/factories |Sí | No | [Factorías de datos V2](./metrics-supported.md#microsoftdatafactoryfactories) |
|Microsoft.DataShare/accounts | Sí | No | [Instancias de Data Share](./metrics-supported.md#microsoftdatashareaccounts) |
|Microsoft.DBforMariaDB/servers | No | No | [DB for MariaDB](./metrics-supported.md#microsoftdbformariadbservers) |
|Microsoft.DBforMySQL/servers | No | No |[DB para MySQL](./metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers | No | No | [DB para PostgreSQL](./metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.DBforPostgreSQL/serversv2 | No | No | [DB for PostgreSQL V2](./metrics-supported.md#microsoftdbforpostgresqlserversv2)|
|Microsoft.DBforPostgreSQL/flexibleServers | Sí | No | [Database for PostgreSQL (servidores flexibles)](./metrics-supported.md#microsoftdbforpostgresqlflexibleservers)|
|Microsoft.Devices/IotHubs | Sí | No |[IoT Hub](./metrics-supported.md#microsoftdevicesiothubs) |
|Microsoft.Devices/provisioningServices| Sí | No | [Instancias de Device Provisioning Service](./metrics-supported.md#microsoftdevicesprovisioningservices) |
|Microsoft.DigitalTwins/digitalTwinsInstances | Sí | No | [Digital Twins](./metrics-supported.md#microsoftdigitaltwinsdigitaltwinsinstances) |
|Microsoft.DocumentDB/databaseAccounts | Sí | No | [Cosmos DB](./metrics-supported.md#microsoftdocumentdbdatabaseaccounts) |
|Microsoft.EventGrid/domains | Sí | No | [Dominios de Event Grid](./metrics-supported.md#microsofteventgriddomains) |
|Microsoft.EventGrid/systemTopics | Sí | No | [Temas del sistema de Event Grid](./metrics-supported.md#microsofteventgridsystemtopics) |
|Microsoft.EventGrid/topics |Sí | No | [Temas de Event Grid](./metrics-supported.md#microsofteventgridtopics) |
|Microsoft.EventHub/clusters |Sí| No | [Clústeres de Event Hubs](./metrics-supported.md#microsofteventhubclusters) |
|Microsoft.EventHub/namespaces |Sí| No | [Event Hubs](./metrics-supported.md#microsofteventhubnamespaces) |
|Microsoft.HDInsight/clusters | Sí | No | [Clústeres de HDInsight](./metrics-supported.md#microsofthdinsightclusters) |
|Microsoft.Insights/Components | Sí | No | [Application Insights](./metrics-supported.md#microsoftinsightscomponents) |
|Microsoft.KeyVault/vaults | Sí |Sí |[Almacenes](./metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Kusto/Clusters | Sí |No |[Clústeres de Data Explorer](./metrics-supported.md#microsoftkustoclusters)|
|Microsoft.Logic/integrationServiceEnvironments | Sí | No |[Entornos del servicio de integración](./metrics-supported.md#microsoftlogicintegrationserviceenvironments) |
|Microsoft.Logic/workflows | No | No |[Logic Apps](./metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.MachineLearningServices/workspaces | Sí | No | [Machine Learning](./metrics-supported.md#microsoftmachinelearningservicesworkspaces) |
|Microsoft.Maps/accounts | Sí | No | [Cuentas de Maps](./metrics-supported.md#microsoftmapsaccounts) |
|Microsoft.Media/mediaservices | No | No | [Media Services](./metrics-supported.md#microsoftmediamediaservices) |
|Microsoft.Media/mediaservices/streamingEndpoints | Sí | No | [Puntos de conexión de streaming de Media Services](./metrics-supported.md#microsoftmediamediaservicesstreamingendpoints) |
|Microsoft.NetApp/netAppAccounts/capacityPools | Sí | Sí | [Grupos de capacidad de Azure NetApp](./metrics-supported.md#microsoftnetappnetappaccountscapacitypools) |
|Microsoft.NetApp/netAppAccounts/capacityPools/volumes | Sí | Sí | [Volúmenes de Azure NetApp](./metrics-supported.md#microsoftnetappnetappaccountscapacitypoolsvolumes) |
|Microsoft.Network/applicationGateways | Sí | No | [Puertas de enlace de aplicaciones](./metrics-supported.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/azurefirewalls | Sí | No | [Firewalls](./metrics-supported.md#microsoftnetworkazurefirewalls) |
|Microsoft.Network/dnsZones | No | No | [Zonas DNS](./metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/expressRouteCircuits | Sí | No |[Circuitos ExpressRoute](./metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/expressRoutePorts | Sí | No |[ExpressRoute Direct](./metrics-supported.md#microsoftnetworkexpressrouteports) |
|Microsoft.Network/loadBalancers (solo para SKU estándar)| Sí| No | [Equilibradores de carga](./metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/natGateways| No | Sin | [Puertas de enlace NAT](./metrics-supported.md#microsoftnetworknatgateways) |
|Microsoft.Network/privateEndpoints| Sin | No | [Puntos de conexión privados](./metrics-supported.md#microsoftnetworkprivateendpoints) |
|Microsoft.Network/privateLinkServices| No | No | [Servicios Private Link](./metrics-supported.md#microsoftnetworkprivatelinkservices) |
|Microsoft.Network/publicipaddresses | Sin | No | [Direcciones IP públicas](./metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Sí | No | [Perfiles de Traffic Manager](./metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.OperationalInsights/workspaces| Sí | No | [Áreas de trabajo de Log Analytics](./metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft.Peering/peerings | Sí | No | [Emparejamientos](./metrics-supported.md#microsoftpeeringpeerings) |
|Microsoft.Peering/peeringServices | Sí | No | [Instancias de Peering Service](./metrics-supported.md#microsoftpeeringpeeringservices) |
|Microsoft.PowerBIDedicated/capacities | No | No | [Capacidades](./metrics-supported.md#microsoftpowerbidedicatedcapacities) |
|Microsoft.Relay/namespaces | Sí | No | [Retransmisiones](./metrics-supported.md#microsoftrelaynamespaces) |
|Microsoft.Search/searchServices | No | No | [Servicios de búsqueda](./metrics-supported.md#microsoftsearchsearchservices) |
|Microsoft.ServiceBus/namespaces | Sí | No | [Service Bus](./metrics-supported.md#microsoftservicebusnamespaces) |
|Microsoft.Sql/managedInstances | No | Sí | [Instancias administradas de SQL](./metrics-supported.md#microsoftsqlmanagedinstances) |
|Microsoft.Sql/servers/databases | No | Sí | [Bases de datos SQL Database](./metrics-supported.md#microsoftsqlserversdatabases) |
|Microsoft.Sql/servers/elasticPools | No | Sí | [Grupos elásticos de SQL](./metrics-supported.md#microsoftsqlserverselasticpools) |
|Microsoft.Storage/storageAccounts |Sí | No | [Cuentas de almacenamiento](./metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/blobServices | Sí| No | [Cuentas de almacenamiento: blobs](./metrics-supported.md#microsoftstoragestorageaccountsblobservices) |
|Microsoft.Storage/storageAccounts/fileServices | Sí| No | [Cuentas de almacenamiento: archivos](./metrics-supported.md#microsoftstoragestorageaccountsfileservices) |
|Microsoft.Storage/storageAccounts/queueServices | Sí| No | [Cuentas de almacenamiento: colas](./metrics-supported.md#microsoftstoragestorageaccountsqueueservices) |
|Microsoft.Storage/storageAccounts/tableServices | Sí| No | [Cuentas de almacenamiento: tablas](./metrics-supported.md#microsoftstoragestorageaccountstableservices) |
|Microsoft.StorageCache/caches | Sí | No | [Instancias de HPC Cache](./metrics-supported.md#microsoftstoragecachecaches) |
|Microsoft.StorageSync/storageSyncServices | Sí | No | [Servicios de sincronización de almacenamiento](./metrics-supported.md#microsoftstoragesyncstoragesyncservices) |
|Microsoft.StreamAnalytics/streamingjobs | Sí | No | [Stream Analytics](./metrics-supported.md#microsoftstreamanalyticsstreamingjobs) |
|Microsoft.Synapse/workspaces | Sí | No | [Synapse Analytics](./metrics-supported.md#microsoftsynapseworkspaces) |
|Microsoft.Synapse/workspaces/bigDataPools | Sí | No | [Grupos de Apache Spark en Synapse Analytics](./metrics-supported.md#microsoftsynapseworkspacesbigdatapools) |
|Microsoft.Synapse/workspaces/sqlPools | Sí | No | [Grupos de SQL de Synapse Analytics](./metrics-supported.md#microsoftsynapseworkspacessqlpools) |
|Microsoft.VMWareCloudSimple/virtualMachines | Sí | No | [Máquinas virtuales de CloudSimple](./metrics-supported.md#microsoftvmwarecloudsimplevirtualmachines) |
|Microsoft.Web/hostingEnvironments/multiRolePools | Sí | No | [Grupos de varios roles de App Service Environment](./metrics-supported.md#microsoftwebhostingenvironmentsmultirolepools)|
|Microsoft.Web/hostingEnvironments/workerPools | Sí | No | [Grupos de trabajo de App Service Environment](./metrics-supported.md#microsoftwebhostingenvironmentsworkerpools)|
|Microsoft.Web/serverfarms | Sí | No | [Planes de App Service](./metrics-supported.md#microsoftwebserverfarms)|
|Microsoft.Web/sites | Sí | No | [App Services y Functions](./metrics-supported.md#microsoftwebsites)|
|Microsoft.Web/sites/slots | Sí | No | [Ranuras de App Service](./metrics-supported.md#microsoftwebsitesslots)|

<sup>1</sup> No se admite con las métricas de red de máquinas virtuales (Entrada de red total, Salida de red total, Flujos de entrada, Flujos de salida, Velocidad máxima de creación de flujos entrantes, Velocidad máxima de creación de flujos salientes) ni con las métricas personalizadas.

## <a name="payload-schema"></a>Esquema de carga

> [!NOTE]
> También puede usar el [esquema de alerta común](./alerts-common-schema.md), que le ofrece la ventaja de tener una carga útil de alerta única y extensible en todos los servicios de alerta Azure Monitor, para las integraciones de su webhook. [Obtenga más información sobre las definiciones de esquemas de alertas comunes.](./alerts-common-schema-definitions.md)


La operación POST contiene el esquema y la carga útil de JSON siguientes para todas las nuevas métricas cuando se usa un [grupo de acciones](./action-groups.md) configurado correctamente:

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
    "version": "2.0",
    "status": "Activated",
    "context": {
      "timestamp": "2018-02-28T10:44:10.1714014Z",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
      "name": "StorageCheck",
      "description": "",
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "severity":"3",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Transactions",
            "metricNamespace":"microsoft.storage/storageAccounts",
            "dimensions": [
              {
                "name": "AccountResourceId",
                "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
              },
              {
                "name": "GeoType",
                "value": "Primary"
              }
            ],
            "operator": "GreaterThan",
            "threshold": "0",
            "timeAggregation": "PT5M",
            "metricValue": 1
          }
        ]
      },
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "resourceGroupName": "Contoso",
      "resourceName": "diag500",
      "resourceType": "Microsoft.Storage/storageAccounts",
      "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
      "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
    },
    "properties": {
      "key1": "value1",
      "key2": "value2"
    }
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre la nueva [experiencia de alertas](./alerts-overview.md).
* Más información sobre las [alertas de registro en Azure](./alerts-unified-log.md).
* Más información sobre las [alertas en Azure](./alerts-overview.md).
