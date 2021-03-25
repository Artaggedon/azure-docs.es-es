---
title: 'Apache Spark y Apache Kafka con Cosmos DB: Azure HDInsight'
description: Obtenga información acerca de cómo usar el flujo estructurado de Apache Spark para leer datos de Apache Kafka y, después, almacenarlos en Azure Cosmos DB. En este ejemplo, se transmiten datos con Jupyter Notebook de Spark en HDInsight.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive, devx-track-azurecli
ms.date: 11/18/2019
ms.openlocfilehash: d78b629e90903c58b98de86f425f0c1225d90997
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104867056"
---
# <a name="use-apache-spark-structured-streaming-with-apache-kafka-and-azure-cosmos-db"></a>Uso del flujo estructurado de Apache Spark con Apache Kafka y Azure Cosmos DB

Aprenda a usar el [flujo estructurado](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) de [Apache Spark](https://spark.apache.org/) para leer datos de [Apache Kafka](https://kafka.apache.org/) en Azure HDInsight y, después, almacenarlos en Azure Cosmos DB.

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) es una base de datos de varios modelos distribuida globalmente. Este ejemplo utiliza un modelo de base de datos de la API de SQL. Para obtener más información, consulte el documento [Bienvenido a Azure Cosmos DB](../cosmos-db/introduction.md).

El flujo estructurado de Spark es un motor de procesamiento de flujo basado en Spark SQL. Permite expresar los cálculos de streaming de la misma forma que el cálculo por lotes de los datos estáticos. Para obtener más información sobre el flujo estructurado, consulte el artículo [Structured Streaming Programming Guide](https://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) (Guía de programación de flujo estructurado) en Apache.org.

> [!IMPORTANT]  
> En este ejemplo, se utiliza Spark 2.2 en HDInsight 3.6.
>
> Los pasos que se describen en este documento crean un grupo de recursos de Azure que contiene un clúster Spark de HDInsight y un clúster Kafka de HDInsight. Estos dos clústeres se encuentran en una instancia de Azure Virtual Network, lo que permite al clúster Spark comunicarse directamente con el clúster Kafka.
>
> Cuando haya terminado los pasos indicados en este documento, no olvide eliminar los clústeres para evitar gastos innecesarios.

## <a name="create-the-clusters"></a>Creación de los clústeres

Apache Kafka en HDInsight no proporciona acceso a los agentes de Kafka a través de Internet. Cualquier comunicación con Kafka debe realizarse en la misma red virtual de Azure que utilizan los nodos del clúster Kafka. En este ejemplo, los clústeres Kafka y Spark se encuentran en una red virtual de Azure. En el diagrama siguiente, se muestra cómo fluye la comunicación entre los clústeres:

:::image type="content" source="./media/apache-kafka-spark-structured-streaming-cosmosdb/apache-spark-kafka-vnet.png" alt-text="Diagrama de clústeres Spark y Kafka en una red virtual de Azure" border="false":::

> [!NOTE]  
> El servicio Kafka se limita a la comunicación dentro de la red virtual. Se puede acceder a otros servicios del clúster, como SSH y Ambari, a través de Internet. Para más información sobre los puertos públicos disponibles en HDInsight, consulte [Puertos e identificadores URI usados en HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Aunque puede crear manualmente la red virtual de Azure y los clústeres Kafka y Spark, resulta más sencillo utilizar una plantilla de Azure Resource Manager. Siga los pasos que se indican a continuación para implementar una red virtual de Azure y los clústeres Kafka y Spark en la suscripción de Azure.

1. Utilice el siguiente botón para iniciar sesión en Azure y abrir la plantilla en Azure Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="./media/apache-kafka-spark-structured-streaming-cosmosdb/resource-manager-deploy.png" alt="Deploy to Azure"/>
    </a>

    La plantilla de Azure Resource Manager se encuentra en el repositorio de GitHub para este proyecto ([https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)).

    Esta plantilla crea los siguientes recursos:

   * Un clúster de Kafka en HDInsight 3.6.

   * Un clúster de Spark en HDInsight 3.6

   * Una instancia de Azure Virtual Network, que contiene los clústeres de HDInsight. La red virtual creada por la plantilla utiliza el espacio de direcciones 10.0.0.0/16.

   * Una base de datos de la API de SQL de Azure Cosmos DB.

    > [!IMPORTANT]  
    > El cuaderno de flujo estructurado que se utiliza en este ejemplo requiere Spark en HDInsight 3.6. Si usa una versión anterior de Spark en HDInsight, recibirá errores al usar dicho cuaderno.

1. Utilice los datos siguientes para rellenar las entradas de la sección **Implementación personalizada**:

    |Propiedad |Value |
    |---|---|
    |Subscription|Seleccione su suscripción a Azure.|
    |Resource group|cree un grupo o seleccione uno existente. Este grupo contiene el clúster de HDInsight.|
    |Nombre de cuenta de Cosmos DB|este valor se utiliza como nombre para la cuenta de Cosmos DB. El nombre solo puede contener letras minúsculas, números y el carácter de guion (-). Debe tener una longitud de entre 3 y 31 caracteres.|
    |Nombre del clúster base|este valor se utiliza como nombre base en los clústeres Spark y Kafka. Por ejemplo, si especifica **myhdi**, creará un clúster de Spark denominado __spark-myhdi__ y un clúster de Kafka denominado **kafka-myhdi**.|
    |Versión del clúster|versión del clúster de HDInsight. Este ejemplo se ha probado con HDInsight 3.6 y podría no funcionar con otros tipos de clúster.|
    |Cluster Login User Name (Nombre de usuario de inicio de sesión del clúster)|nombre de usuario del administrador de los clústeres Spark y Kafka.|
    |Cluster Login Password (Contraseña de inicio de sesión del clúster)|contraseña de usuario del administrador de los clústeres Spark y Kafka.|
    |Nombre de usuario de SSH|usuario de SSH que se va a crear para los clústeres Spark y Kafka.|
    |Contraseña de SSH|contraseña del usuario de SSH para los clústeres Spark y Kafka.|

    :::image type="content" source="./media/apache-kafka-spark-structured-streaming-cosmosdb/hdi-custom-parameters.png" alt-text="Valores de implementación personalizada de HDInsight":::

1. Consulte los **Términos y condiciones** y seleccione **Acepto los términos y condiciones indicados anteriormente**.

1. Por último, seleccione **Adquirir**. Puede tardar hasta 45 minutos en crear los clústeres, la red virtual y la cuenta de Cosmos DB.

## <a name="create-the-cosmos-db-database-and-collection"></a>Creación de la base de datos y la colección de Cosmos DB

El proyecto utilizado en este documento almacena los datos en Cosmos DB. Antes de ejecutar el código, primero debe crear una _base de datos_ y una _colección_ en la instancia de Cosmos DB. También se debe recuperar el punto de conexión del documento y la _clave_ usada para autenticar solicitudes en Cosmos DB.

Una manera de hacerlo es mediante la [CLI de Azure](/cli/azure/). El script siguiente creará una base de datos denominada `kafkadata` y una colección denominada `kafkacollection`. A continuación, devuelve la clave principal.

```azurecli
#!/bin/bash

# Replace 'myresourcegroup' with the name of your resource group
resourceGroupName='myresourcegroup'
# Replace 'mycosmosaccount' with the name of your Cosmos DB account name
name='mycosmosaccount'

# WARNING: If you change the databaseName or collectionName
#          then you must update the values in the Jupyter Notebook
databaseName='kafkadata'
collectionName='kafkacollection'

# Create the database
az cosmosdb sql database create --account-name $name --name $databaseName --resource-group $resourceGroupName

# Create the collection
az cosmosdb sql container create --account-name $name --database-name $databaseName --name $collectionName --partition-key-path "/my/path" --resource-group $resourceGroupName

# Get the endpoint
az cosmosdb show --name $name --resource-group $resourceGroupName --query documentEndpoint

# Get the primary key
az cosmosdb keys list --name $name --resource-group $resourceGroupName --type keys
```

La información del punto de conexión del documento y de la clave principal es similar al siguiente texto:

```text
# endpoint
"https://mycosmosaccount.documents.azure.com:443/"
# key
"YqPXw3RP7TsJoBF5imkYR0QNA02IrreNAlkrUMkL8EW94YHs41bktBhIgWq4pqj6HCGYijQKMRkCTsSaKUO2pw=="
```

> [!IMPORTANT]  
> Guarde los valores de punto de conexión y de clave, ya que son necesarios en las instancias de Jupyter Notebook.

## <a name="get-the-notebooks"></a>Obtención de los cuadernos

El código para el ejemplo descrito en este documento está disponible en [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb).

## <a name="upload-the-notebooks"></a>Carga de los cuadernos

Siga estos pasos para cargar los cuadernos del proyecto en el clúster de Spark en HDInsight:

1. En el explorador web, conéctese a Jupyter Notebook en el clúster de Spark. En la siguiente URL, reemplace `CLUSTERNAME` por el nombre del clúster de __Spark__:

    ```http
    https://CLUSTERNAME.azurehdinsight.net/jupyter
    ```

    Cuando se lo soliciten, escriba el nombre de inicio de sesión del clúster (administrador) y la contraseña que utilizó cuando creó el clúster.

2. En la parte superior derecha de la página, utilice el botón __Cargar__ para cargar el archivo __Stream-taxi-data-to-kafka.ipynb__ en el clúster. Seleccione __Open__ (Abrir) para iniciar la carga.

3. Busque la entrada __Stream-taxi-data-to-kafka.ipynb__ en la lista de cuadernos y seleccione el botón __Cargar__ que está junto a ella.

4. Repita los pasos del 1 al 3 para cargar el cuaderno __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__.

## <a name="load-taxi-data-into-kafka"></a>Cargar los datos del taxi en Kafka

Cuando se hayan cargado los archivos, seleccione la entrada __Stream-taxi-data-to-kafka.ipynb__ para abrir el cuaderno. Siga los pasos descritos en el cuaderno para cargar datos en Kafka.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>Procesamiento de datos de taxi mediante el flujo estructurado de Spark

En la página principal de [Jupyter Notebook](https://jupyter.org/), seleccione la entrada __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__. Siga los pasos descritos en el cuaderno para transmitir datos en secuencia de Kafka y en Azure Cosmos DB mediante el flujo estructurado de Spark.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar el flujo estructurado de Apache Spark, consulte los siguientes documentos para aprender más sobre cómo trabajar con Apache Spark, Apache Kafka y Azure Cosmos DB:

* [Uso de Apache Spark Streaming (DStream) con Apache Kafka](hdinsight-apache-spark-with-kafka.md).
* [Introducción a Jupyter Notebook y Apache Spark en HDInsight](spark/apache-spark-jupyter-spark-sql.md)
* [Bienvenido a Azure Cosmos DB](../cosmos-db/introduction.md)
