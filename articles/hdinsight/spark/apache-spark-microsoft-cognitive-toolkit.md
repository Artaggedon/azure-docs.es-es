---
title: 'Microsoft Cognitive Toolkit con Apache Spark: Azure HDInsight'
description: Aprenda cómo un modelo de aprendizaje profundo de Microsoft Cognitive Toolkit formado se puede aplicar a un conjunto de datos mediante la API de Spark Python en un clúster de Azure HDInsight Spark.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 01/14/2020
ms.openlocfilehash: 6be70f448cec7cf6db2b8152565ad8102d3a3294
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104868756"
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Uso del modelo de aprendizaje profundo de Microsoft Cognitive Toolkit con un clúster de Azure HDInsight Spark

En este artículo, realice los pasos siguientes.

1. Ejecute un script personalizado para instalar [Microsoft Cognitive Toolkit](/cognitive-toolkit/) en un clúster de Azure HDInsight Spark.

2. Cargue un cuaderno de [Jupyter Notebook](https://jupyter.org/) en el clúster de [Apache Spark](https://spark.apache.org/) para ver cómo aplicar un modelo de aprendizaje profundo de Microsoft Cognitive Toolkit formado a los archivos de una cuenta de Azure Blob Storage mediante la [API de Spark Python (PySpark)](https://spark.apache.org/docs/latest/api/python/index.html).

## <a name="prerequisites"></a>Prerequisites

* Un clúster de Apache Spark en HDInsight. Vea [Creación de un clúster de Apache Spark](./apache-spark-jupyter-spark-sql-use-portal.md).

* Experiencia en el uso de Jupyter Notebooks con Spark en HDInsight. Para más información, consulte [Carga de datos y ejecución de consultas en un clúster de Apache Spark en Azure HDInsight](./apache-spark-load-data-run-query.md).

## <a name="how-does-this-solution-flow"></a>¿Cómo funciona esta solución?

Esta solución se divide entre este artículo y una instancia de Jupyter Notebook que se carga como parte del artículo. En este artículo, realice los pasos siguientes:

* Ejecute una acción de script en un clúster de HDInsight Spark para instalar los paquetes de Microsoft Cognitive Toolkit y Python.
* Cargue la instancia de Jupyter Notebook que ejecuta la solución en el clúster de HDInsight Spark.

Los pasos restantes siguientes se tratan en Jupyter Notebook.

* Carga de imágenes de ejemplo en un conjunto de datos distribuido resistente (o RDD) de Spark.
  * Carga de los módulos y definición de los valores preestablecidos.
  * Descarga del conjunto de datos de forma local en el clúster de Spark.
  * Conversión del conjunto de datos en un RDD.
* Puntuación de las imágenes mediante un modelo de Cognitive Toolkit entrenado.
  * Descarga del modelo de Cognitive Toolkit entrenado en el clúster de Spark.
  * Definición de las funciones que van a usar los nodos de trabajo.
  * Puntuación de las imágenes en los nodos de trabajo.
  * Evaluación de la precisión del modelo.

## <a name="install-microsoft-cognitive-toolkit"></a>Instalación de Microsoft Cognitive Toolkit

Puede instalar Microsoft Cognitive Toolkit en un clúster de Spark mediante la acción de scripts. La acción de scripts usa scripts personalizados para instalar componentes del clúster que no están disponibles de forma predeterminada. Puede usar el script personalizado desde Azure Portal, mediante el uso del SDK de .NET para HDInsight o Azure PowerShell. También puede utilizar el script para instalar el kit de herramientas como parte de la creación del clúster, o una vez que el clúster está en funcionamiento.

En este artículo se utiliza el portal para instalar el kit de herramientas una vez creado el clúster. Para conocer otras formas de ejecutar el script personalizado, consulte [Personalizar los clústeres de HDInsight mediante la acción de script](../hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-the-azure-portal"></a>Uso de Azure Portal

Para obtener instrucciones sobre cómo usar Azure Portal con el fin de ejecutar acciones de scripts, consulte [Personalizar los clústeres de HDInsight mediante la acción de script](../hdinsight-hadoop-customize-cluster-linux.md#script-action-during-cluster-creation). Asegúrese de que proporciona las siguientes entradas para instalar Microsoft Cognitive Toolkit. Use los siguientes valores para la acción de script:

|Propiedad |Value |
|---|---|
|Tipo de script|- Personalizado|
|Nombre| Instalar MCT|
|URI de script de Bash|`https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`|
|Tipos de nodo:|Principal, trabajo|
|Parámetros|None|

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a>Carga de Jupyter Notebook en el clúster de Azure HDInsight Spark

Para usar Microsoft Cognitive Toolkit con el clúster de Azure HDInsight Spark, debe cargar Jupyter Notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** en el clúster de Azure HDInsight Spark. Este cuaderno está disponible en GitHub en [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. Descargue y descomprima [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net/jupyter`, donde `CLUSTERNAME` es el nombre del clúster.

1. En Jupyter Notebook, seleccione **Cargar** en la esquina superior derecha y, luego, vaya a la descarga y seleccione el archivo `CNTK_model_scoring_on_Spark_walkthrough.ipynb`.

    :::image type="content" source="./media/apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png " alt-text="Carga de Jupyter Notebook en el clúster de Azure HDInsight Spark" border="true":::

1. Seleccione de nuevo **Cargar**.

1. Una vez cargado el cuaderno, haga clic en el nombre de este y, a continuación, siga las instrucciones del propio cuaderno sobre cómo cargar el conjunto de datos y realizar los pasos del artículo.

## <a name="see-also"></a>Consulte también

* [Introducción a Apache Spark en HDInsight de Azure](apache-spark-overview.md)

### <a name="scenarios"></a>Escenarios

* [Apache Spark con BI: Análisis de datos interactivos con Spark en HDInsight con las herramientas de BI](apache-spark-use-bi-tools.md)
* [Apache Spark con Machine Learning: Uso de Spark en HDInsight para analizar la temperatura de un edificio mediante datos de HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark con Machine Learning: uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Análisis de registros de un sitio web mediante Apache Spark en HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Análisis de datos de telemetría de Application Insights con Apache Spark en HDInsight](apache-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Creación y ejecución de aplicaciones

* [Crear una aplicación independiente con Scala](apache-spark-create-standalone-application.md)
* [Ejecución de trabajos de forma remota en un clúster de Apache Spark mediante Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Herramientas y extensiones

* [Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para crear y enviar aplicaciones de Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Apache Spark applications remotely](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) (Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para depurar aplicaciones de Apache Spark de forma remota)
* [Uso de cuadernos de Apache Zeppelin con un clúster de Apache Spark en HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponible para Jupyter Notebook en clústeres Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Uso de paquetes externos con cuadernos de Jupyter Notebook](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Administrar recursos

* [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](apache-spark-job-debugging.md)
