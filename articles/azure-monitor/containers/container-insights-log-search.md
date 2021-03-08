---
title: Procedimiento para consultar registros desde Container Insights | Microsoft Docs
description: Container Insights recopila datos de registro y métricas, y en este artículo se describen los registros y se incluyen consultas de ejemplo.
ms.topic: conceptual
ms.date: 06/01/2020
ms.openlocfilehash: 79efa714548adbde67774cab741bf953a4ff1e83
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101711117"
---
# <a name="how-to-query-logs-from-container-insights"></a>Procedimiento para consultar registros desde Container Insights

Container Insights recopila métricas de rendimiento, datos de inventario e información de estado de mantenimiento de los contenedores y los hosts de contenedor. Los datos se recopilan cada tres minutos y se reenvían al área de trabajo de Log Analytics en Azure Monitor. Estos datos están disponibles para [consulta](../logs/log-query-overview.md) en Azure Monitor. Estos datos se pueden aplicar a escenarios que incluyen la planeación de la migración, el análisis de la capacidad, la detección y la solución de problemas de rendimiento a petición.

## <a name="container-records"></a>Registros de contenedor

En la tabla siguiente se proporcionan detalles de los registros recopilados por Container Insights. Para obtener una lista de las descripciones de columna, consulte la referencia de las tablas [ContainerInventory](/azure/azure-monitor/reference/tables/containerinventory) y [ContainerLog](/azure/azure-monitor/reference/tables/containerlog).

| data | Origen de datos | Tipo de datos | Fields |
|------|-------------|-----------|--------|
| Inventario de contenedor | Kubelet | `ContainerInventory` | TimeGenerated, Computer, Name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Registro de contenedor | Docker | `ContainerLog` | TimeGenerated, Computer, image ID, Name, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Inventario de nodo de contenedor | API de Kube | `ContainerNodeInventory`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Inventario de pods en un clúster de Kubernetes | API de Kube | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus,  ContainerStatusReason, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Inventario de la parte de nodos de un clúster de Kubernetes | API de Kube | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Eventos de Kubernetes | API de Kube | `KubeEvents` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, SourceSystem | 
| Servicios en el clúster de Kubernetes | API de Kube | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Métricas de rendimiento de la parte de nodos del clúster de Kubernetes | Las métricas de uso se obtienen de cAdvisor y los límites de la API de Kube. | Perf &#124; donde ObjectName == "K8SNode" | Computer, ObjectName, CounterName &#40;cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes, memoryRssBytes, cpuUsageNanoCores, memoryWorkingsetBytes, restartTimeEpoch&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas de rendimiento de la parte de contenedores del clúster de Kubernetes | Las métricas de uso se obtienen de cAdvisor y los límites de la API de Kube. | Perf &#124; donde ObjectName == "K8SContainer" | CounterName &#40; cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, cpuUsageNanoCores, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas personalizadas ||`InsightsMetrics` | Computer, Name, Namespace, Origin, SourceSystem, Tags<sup>1</sup>, TimeGenerated, Type, Va, _ResourceId | 

<sup>1</sup> La propiedad *Tags* representa [varias dimensiones](../essentials/data-platform-metrics.md#multi-dimensional-metrics) para la métrica correspondiente. Para más información sobre las métricas recopiladas y almacenadas en la tabla `InsightsMetrics` y una descripción de las propiedades de registro, consulte [Información general de InsightsMetrics](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md).

## <a name="search-logs-to-analyze-data"></a>Búsqueda de registros para analizar datos

Los registros de Azure Monitor pueden ayudarle a buscar tendencias, diagnosticar cuellos de botellas, realizar previsiones o correlacionar datos, que pueden servirle para determinar si la configuración actual del clúster funciona óptimamente. Las búsquedas de registros predefinidas se proporcionan para que comience a usarlas inmediatamente o para que las personalice con el fin de devolver la información de la forma deseada.

Puede realizar un análisis interactivo de los datos en el área de trabajo mediante la selección de la opción **Ver registros de eventos de Kubernetes** o **Ver registros del contenedor** en el panel de vista previa de la lista desplegable **Ver en Analytics**. La página **Búsqueda de registros** aparece a la derecha de la página de Azure Portal en que se encontraba.

![Análisis de los datos en Log Analytics](./media/container-insights-analyze/container-health-log-search-example.png)

Los registros de contenedor generador que se reenvían al área de trabajo son STDOUT y STDERR. Dado que Azure Monitor supervisa Kubernetes administrado por Azure (AKS), el sistema de Kubernetes no se recopila hoy debido al gran volumen de datos generados. 

### <a name="example-log-search-queries"></a>Ejemplos de consultas de búsqueda de registros

A menudo resulta útil crear consultas que comiencen con un ejemplo o dos y luego modificarlas para ajustarlas a sus requisitos. Para ayudarle a crear consultas más avanzadas, puede experimentar con las siguientes consultas de ejemplo:

| Consultar | Descripción | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Se muestra toda la información del ciclo de vida de un contenedor| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Inventario de imágenes | 
| **Seleccione la opción de visualización del gráfico de líneas**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "cpuUsageNanoCores" &#124; summarize AvgCPUUsageNanoCores = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | CPU de contenedor | 
| **Seleccione la opción de visualización del gráfico de líneas**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "memoryRssBytes" &#124; summarize AvgUsedRssMemoryBytes = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Memoria de contenedor |
| InsightsMetrics<br> &#124; where Name == "requests_count"<br> &#124; summarize Val=any(Val) by TimeGenerated=bin(TimeGenerated, 1m)<br> &#124; sort by TimeGenerated asc<br> &#124; project RequestsPerMinute = Val - prev(Val), TimeGenerated <br> &#124; render barchart  | Solicitudes por minuto con métricas personalizadas |

## <a name="query-prometheus-metrics-data"></a>Consulta de los datos de las métricas de Prometheus

El ejemplo siguiente es una consulta de métricas de Prometheus que muestra lecturas de disco por segundo por disco y por nodo.

```
InsightsMetrics
| where Namespace == 'container.azm.ms/diskio'
| where TimeGenerated > ago(1h)
| where Name == 'reads'
| extend Tags = todynamic(Tags)
| extend HostName = tostring(Tags.hostName), Device = Tags.name
| extend NodeDisk = strcat(Device, "/", HostName)
| order by NodeDisk asc, TimeGenerated asc
| serialize
| extend PrevVal = iif(prev(NodeDisk) != NodeDisk, 0.0, prev(Val)), PrevTimeGenerated = iif(prev(NodeDisk) != NodeDisk, datetime(null), prev(TimeGenerated))
| where isnotnull(PrevTimeGenerated) and PrevTimeGenerated != TimeGenerated
| extend Rate = iif(PrevVal > Val, Val / (datetime_diff('Second', TimeGenerated, PrevTimeGenerated) * 1), iif(PrevVal == Val, 0.0, (Val - PrevVal) / (datetime_diff('Second', TimeGenerated, PrevTimeGenerated) * 1)))
| where isnotnull(Rate)
| project TimeGenerated, NodeDisk, Rate
| render timechart

```

Para ver las métricas de Prometheus que ha extraído Azure Monitor filtradas por espacio de nombres, especifique "prometheus". A continuación, se muestra una consulta de ejemplo para ver las métricas de Prometheus desde el espacio de nombres `default` de Kubernetes.

```
InsightsMetrics 
| where Namespace == "prometheus"
| extend tags=parse_json(Tags)
| summarize count() by Name
```

Los datos de Prometheus también se pueden consultar directamente en función del nombre.

```
InsightsMetrics 
| where Namespace == "prometheus"
| where Name contains "some_prometheus_metric"
```

### <a name="query-config-or-scraping-errors"></a>Consulta de errores de configuración o extracción

Para investigar los errores de configuración o extracción, la consulta de ejemplo siguiente devuelve los eventos informativos de la tabla `KubeMonAgentEvents`.

```
KubeMonAgentEvents | where Level != "Info" 
```

La salida muestra unos resultados similares al ejemplo siguiente:

![Registro de los resultados de la consulta de eventos informativos del agente](./media/container-insights-log-search/log-query-example-kubeagent-events.png)

## <a name="next-steps"></a>Pasos siguientes

Container Insights no incluye un conjunto de alertas predefinido. Revise [Creación de alertas de rendimiento con Container Insights](./container-insights-log-alerts.md) para obtener información sobre cómo crear alertas recomendadas en caso de uso elevado de CPU y memoria a fin de permitir las operaciones de desarrollo o los procesos y procedimientos operativos.