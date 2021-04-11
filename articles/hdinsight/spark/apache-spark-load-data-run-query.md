---
title: 'Tutorial: Carga de datos y ejecución de consultas con Apache Spark en Azure HDInsight'
description: 'Tutorial: Aprenda a cargar datos y a ejecutar consultas interactivas en clústeres de Spark en Azure HDInsight.'
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,mvc
ms.date: 02/12/2020
ms.openlocfilehash: eee93d338a106414235b04380e5a67c772951188
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106062750"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Tutorial: Carga de datos y ejecución de consultas en un clúster de Apache Spark en Azure HDInsight

En este tutorial, aprenderá a crear una trama de datos a partir de un archivo CSV y a ejecutar consultas SQL de Spark interactivas en un clúster de [Apache Spark](https://spark.apache.org/) en Azure HDInsight. En Spark, una trama de datos es una colección distribuida de datos que se organizan en columnas con nombre. Una trama de datos es conceptualmente equivalente a una tabla de una base de datos relacional o a una trama de datos de R/Python.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Creación de una trama de datos a partir de un archivo csv
> * Ejecución de consultas en la trama de datos

## <a name="prerequisites"></a>Prerrequisitos

Un clúster de Apache Spark en HDInsight. Vea [Creación de un clúster de Apache Spark](./apache-spark-jupyter-spark-sql-use-portal.md).

## <a name="create-a-jupyter-notebook"></a>Creación de un cuaderno de Jupyter Notebook

Jupyter Notebook es un entorno de cuaderno interactivo que admite varios lenguajes de programación. El cuaderno le permite interactuar con los datos, combinar código con el texto de marcado y realizar visualizaciones básicas.

1. Edite la dirección URL `https://SPARKCLUSTER.azurehdinsight.net/jupyter`; para ello, reemplace `SPARKCLUSTER` por el nombre del clúster de Spark. Después, escriba la dirección URL editada en un explorador web. Cuando se le solicite, escriba las credenciales de inicio de sesión del clúster.

2. En la página web de Jupyter, seleccione **New** (Nuevo)  > **PySpark** para crear un cuaderno.

   :::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png " alt-text="Creación de un cuaderno de Jupyter Notebook para ejecutar consultas Spark SQL interactivas" border="true":::

   Se crea y se abre un nuevo cuaderno con el nombre Untitled(`Untitled.ipynb`).

    > [!NOTE]  
    > Si se usa el kernel de PySpark para crear un cuaderno, la sesión `spark` se crea automáticamente al ejecutar la primera celda de código. No es necesario crear la sesión explícitamente.

## <a name="create-a-dataframe-from-a-csv-file"></a>Creación de una trama de datos a partir de un archivo csv

Las aplicaciones pueden crear tramas de datos directamente desde archivos o carpetas del almacenamiento remoto, como Azure Storage o Azure Data Lake Storage; desde una tabla de Hive; o desde otros orígenes de datos compatibles con Spark, como Cosmos DB, Azure SQL DB, DW, etc. La captura de pantalla siguiente muestra una instantánea del archivo HVAC.csv que se usa en este tutorial. El archivo csv incluye todos los clústeres de HDInsight Spark. Los datos capturan las variaciones de temperatura de varios edificios.

:::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png " alt-text="Instantánea de los datos para la consulta Spark SQL interactiva" border="true":::

1. Pegue el código siguiente en una celda vacía del cuaderno de Jupyter Notebook y presione **MAYÚS + ENTRAR** para ejecutarlo. El código importa los tipos necesarios para este escenario:

    ```python
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Al ejecutar una consulta interactiva en Jupyter, la ventana del explorador web o la leyenda de la pestaña muestran el estado **(Busy)** [(Ocupado)] junto con el título del cuaderno. También verá un círculo sólido junto al texto **PySpark** en la esquina superior derecha. Cuando finaliza el trabajo, cambia a un círculo vacío.

    :::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png " alt-text="Estado de una consulta Spark SQL interactiva" border="true":::

1. Anote el identificador de sesión devuelto. En la imagen anterior, el identificador de sesión es 0. Si lo desea, puede recuperar los detalles de la sesión. Para ello, vaya a `https://CLUSTERNAME.azurehdinsight.net/livy/sessions/ID/statements`, donde CLUSTERNAME es el nombre del clúster de Spark y el identificador es el número de identificación de la sesión.

1. Ejecute el código siguiente para crear una trama de datos y una tabla temporal (**hvac**).

    ```python
    # Create a dataframe and table from sample data
    csvFile = spark.read.csv('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

## <a name="run-queries-on-the-dataframe"></a>Ejecución de consultas en la trama de datos

Una vez creada la tabla, puede ejecutar una consulta interactiva en los datos.

1. Ejecute el siguiente código en una celda vacía del cuaderno:

    ```sql
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Se muestra la siguiente salida tabular.

     :::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png " alt-text="Salida de tabla del resultado de la consulta Spark interactiva" border="true":::

2. También puede ver la salida en otras visualizaciones. Para ver un gráfico de áreas para la misma salida, seleccione **Área** y establezca otros valores tal como se muestra.

    :::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png " alt-text="Gráfico de área del resultado de la consulta Spark interactiva" border="true":::

3. En la barra de menús de cuaderno, vaya a **Archivo** > **Save and Checkpoint** (Guardar y punto de control).

4. Si ahora va a empezar con el [siguiente tutorial](apache-spark-use-bi-tools.md), deje el bloc de notas abierto. Si no va a hacerlo, cierre el cuaderno para liberar los recursos de clúster: en la barra de menús del cuaderno, vaya a **Archivo** >  **Close and Halt** (Cerrar y detener).

## <a name="clean-up-resources"></a>Limpieza de recursos

Con HDInsight, los datos y los cuadernos de Jupyter Notebook se almacenan en Azure Storage o Azure Data Lake Storage, por lo que puede eliminar de forma segura un clúster cuando no esté en uso. También se le cobrará por un clúster de HDInsight aunque no se esté usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se usen. Si tiene previsto pasar inmediatamente al siguiente tutorial, es aconsejable que no elimine el clúster.

Abra el clúster en Azure Portal y seleccione **Eliminar**.

:::image type="content" source="./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png " alt-text="Eliminación de un clúster de HDInsight" border="true":::

También puede seleccionar el nombre del grupo de recursos para abrir la página del grupo de recursos y, a continuación, seleccionar **Eliminar grupo de recursos**. Al eliminar el grupo de recursos, se eliminan tanto el clúster de HDInsight Spark como la cuenta de almacenamiento predeterminada.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprenderá a crear una trama de datos a partir de un archivo CSV y a ejecutar consultas SQL de Spark interactivas en un clúster de Apache Spark en Azure HDInsight. En el siguiente artículo podrá ver cómo extraer en una herramienta de análisis de BI, como Power BI, los datos que registró en Apache Spark.

> [!div class="nextstepaction"]
> [Análisis de datos mediante herramientas de inteligencia empresarial](apache-spark-use-bi-tools.md)
