---
title: Consulta a los registros de Azure Monitor para supervisar clústeres de Azure HDInsight
description: Aprenda a ejecutar consultas en los registros de Azure Monitor para supervisar los trabajos que se ejecutan en un clúster de HDInsight.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/02/2019
ms.openlocfilehash: 3cf97039983ecec44a7c3a32e178fdcf9f9c45ff
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872190"
---
# <a name="query-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Consulta a los registros de Azure Monitor para supervisar clústeres de HDInsight

Conozca algunos escenarios básicos de uso de los registros de Azure Monitor para supervisar clústeres de Azure HDInsight:

* [Análisis de métricas de clúster de HDInsight](#analyze-hdinsight-cluster-metrics)
* [Creación de alertas de eventos](#create-alerts-for-tracking-events)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Requisitos previos

Debe haber configurado un clúster de HDInsight para usar registros de Azure Monitor y agregado las soluciones de supervisión de registros de Azure Monitor específicas del clúster de HDInsight al área de trabajo. Puede encontrar instrucciones para ello en [Uso de los registros de Azure Monitor con clústeres de HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>Análisis de métricas de clúster de HDInsight

Aprenda a buscar métricas específicas de su clúster de HDInsight.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
1. En **General**, seleccione **Registros**.
1. Escriba la siguiente consulta en el cuadro de texto para buscar todas las métricas disponibles para todos los clústeres de HDInsight que están configurados para usar los registros de Azure Monitor y, luego, seleccione **Ejecutar**. Revise los resultados.

    ```kusto
    search *
    ```

    :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png" alt-text="Análisis de Apache Ambari: buscar todas las métricas":::

1. En el menú izquierdo, seleccione la pestaña **Filtrar**.

1. En **Tipo**, seleccione **Latido**. Después, seleccione **Aplicar y ejecutar**.

    :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png" alt-text="métricas específicas de la búsqueda de Log Analytics":::

1. Observe que la consulta del cuadro de texto cambia a:

    ```kusto
    search *
    | where Type == "Heartbeat"
    ```

1. Puede profundizar mediante las opciones disponibles en el menú izquierdo. Por ejemplo:

   - Para ver los registros de un nodo específico:

     :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/log-analytics-specific-node.png" alt-text="Búsqueda de errores específicos output1":::

   - Para ver los registros en ciertos momentos:

     :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/log-analytics-specific-time.png" alt-text="Búsqueda de errores específicos output2":::

1. Seleccione **Aplicar y ejecutar**, y revise los resultados. Tenga en cuenta también que la consulta se actualizó a:

    ```kusto
    search *
    | where Type == "Heartbeat"
    | where (Computer == "zk2-myhado") and (TimeGenerated == "2019-12-02T23:15:02.69Z" or TimeGenerated == "2019-12-02T23:15:08.07Z" or TimeGenerated == "2019-12-02T21:09:34.787Z")
    ```

### <a name="additional-sample-queries"></a>Consultas de ejemplo adicionales

Una consulta de ejemplo basada en el promedio de los recursos usados en un intervalo de 10 minutos, clasificados por nombre del clúster:

```kusto
search in (metrics_resourcemanager_queue_root_default_CL) * 
| summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)
```

En lugar de restringir en función de los recursos medios usados, puede usar la siguiente consulta para restringir los resultados según el número máximo de recursos que se usaron (así como los percentiles 90 y 95) en una ventana de 10 minutos:

```kusto
search in (metrics_resourcemanager_queue_root_default_CL) * 
| summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)
```

## <a name="create-alerts-for-tracking-events"></a>Creación de alertas para el seguimiento de eventos

El primer paso para crear una alerta es llegar a una consulta en función de la cual se desencadena la alerta. Puede usar cualquier consulta que desee para crear una alerta.

1. Abra el área de trabajo de Log Analytics que está asociada a su clúster de HDInsight desde Azure Portal.
1. En **General**, seleccione **Registros**.
1. Ejecute la siguiente consulta en la que quiere crear una alerta y luego seleccione **Ejecutar**.

    ```kusto
    metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0
    ```

    La consulta proporciona una lista de aplicaciones con errores que se ejecutan en clústeres de HDInsight.

1. Seleccione **Nueva regla de alertas** en la parte superior de la página.

    :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png" alt-text="Nueva alerta de reglas":::

1. En la ventana **Crear regla**, escriba la consulta y otros detalles para crear una alerta; a continuación, haga clic en **Crear regla de alertas**.

    :::image type="content" source="./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png" alt-text="Definición de la condición de la alerta.":::

### <a name="edit-or-delete-an-existing-alert"></a>Edición o eliminación de una alerta existente

1. Abra el área de trabajo de Log Analytics desde Azure Portal.

1. En el menú de la izquierda, en **Supervisión**, seleccione **Alertas**.

1. En la parte superior, seleccione **Administrar reglas de alerta**.

1. Seleccione la alerta que desea editar o eliminar.

1. Tiene las siguientes opciones: **Guardar**, **Descartar**, **Deshabilitar** y **Eliminar**.

    :::image type="content" source="media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png" alt-text="Alerta para editar o eliminar registros de Azure Monitor para HDInsight":::

Para más información, vea [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../azure-monitor/alerts/alerts-metric.md).

## <a name="see-also"></a>Vea también

* [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/logs/get-started-queries.md)
* [Creación de vistas personalizadas mediante el Diseñador de vistas en Azure Monitor](../azure-monitor/visualize/view-designer.md)
