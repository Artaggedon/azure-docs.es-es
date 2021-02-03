---
title: 'Error de IllegalArgumentException para Apache Spark: Azure HDInsight'
description: IllegalArgumentException para una actividad de Apache Spark en Azure HDInsight para Azure Data Factory
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/29/2019
ms.openlocfilehash: 429659d605cdaf8aad978841e486a17da321cce4
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98929399"
---
# <a name="scenario-illegalargumentexception-for-apache-spark-activity-in-azure-hdinsight"></a>Escenario: IllegalArgumentException para una actividad de Apache Spark en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar componentes de Apache Spark en clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

Al intentar ejecutar una actividad de Spark en una canalización de Azure Data Factory, recibirá la excepción siguiente:

```error
Exception in thread "main" java.lang.IllegalArgumentException:
Wrong FS: wasbs://additional@xxx.blob.core.windows.net/spark-examples_2.11-2.1.0.jar, expected: wasbs://wasbsrepro-2017-11-07t00-59-42-722z@xxx.blob.core.windows.net
```

## <a name="cause"></a>Causa

Se producirá un error en un trabajo de Spark si el archivo jar de la aplicación no se encuentra en el almacenamiento principal o predeterminado del clúster de Spark.

Este es un problema conocido con el marco de código abierto de Spark, y está registrado en este error: [Se produce un error en el trabajo de Spark si fs.defaultFS y el jar de la aplicación tienen direcciones URL diferentes](https://issues.apache.org/jira/browse/SPARK-22587).

Este problema se resolvió en Spark 2.3.0.

## <a name="resolution"></a>Solución

Asegúrese de que el jar de la aplicación está almacenado en el almacenamiento principal o predeterminado para el clúster de HDInsight. En el caso de Azure Data Factory, asegúrese de que el servicio vinculado ADF apunta al contenedor predeterminado de HDInsight en lugar de a un contenedor secundario.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]