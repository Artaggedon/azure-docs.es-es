---
title: Alta disponibilidad con Apache Kafka en Azure HDInsight
description: Aprenda a garantizar una alta disponibilidad con Apache Kafka en Azure HDInsight. Aprenda a reequilibrar réplicas de partición en Kafka para que se encuentren en distintos dominios de error en la región de Azure que contenga HDInsight.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/09/2019
ms.openlocfilehash: e3f31bc1876a0c1f90ad4882ff782be25b4b6ed6
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98932759"
---
# <a name="high-availability-of-your-data-with-apache-kafka-on-hdinsight"></a>Alta disponibilidad de los datos con Apache Kafka en HDInsight

Aprenda a configurar réplicas de partición para que los temas de Apache Kafka puedan aprovechar la configuración del bastidor de hardware subyacente. Esta configuración garantiza la disponibilidad de los datos almacenados en Apache Kafka en HDInsight.

## <a name="fault-and-update-domains-with-apache-kafka"></a>Dominios de error y de actualización con Apache Kafka

Un dominio de error es una agrupación lógica del hardware subyacente en un centro de datos de Azure. Todos los dominios de error comparten la fuente de energía y el conmutador de red. Las máquinas virtuales y los discos administrados que implementan los nodos en un clúster de HDInsight se distribuyen por estos dominios de error. Esta arquitectura limita el impacto potencial de errores del hardware físico.

Cada región de Azure tiene un número concreto de dominios de error. Para obtener una lista de los dominios y el número de dominios de error que contienen, consulte la documentación de los [conjuntos de disponibilidad](../../virtual-machines/availability.md#availability-sets).

> [!IMPORTANT]  
> Kafka no es compatible con dominios de error. Cuando se crea un tema en Kafka, este puede almacenar todas las réplicas de las particiones en el mismo dominio de error. Para solucionar este problema, HDInsight proporciona la [herramienta de reequilibrio de particiones de Kafka](https://github.com/hdinsight/hdinsight-kafka-tools).

## <a name="when-to-rebalance-partition-replicas"></a>Cuándo se deben reequilibrar las réplicas de las particiones

Para garantizar la máxima disponibilidad de los datos de Kafka, es preciso reequilibrar las réplicas de las particiones del tema en las siguientes ocasiones:

* Cuando se crean un nuevo tema o una partición

* Cuando un clúster se escala verticalmente

## <a name="replication-factor"></a>Factor de replicación

> [!IMPORTANT]  
> Se recomienda utilizar una región de Azure que contenga tres dominios de error y un factor de replicación de 3.

Si debe usar una región que contenga solo dos dominios de error, use un factor de replicación de 4 para distribuir las réplicas uniformemente entre los dominios de error de dos.

Para ver un ejemplo sobre la creación de temas y la configuración del factor de replicación, consulte el documento de [introducción a Apache Kafka en HDInsight](apache-kafka-get-started.md).

## <a name="how-to-rebalance-partition-replicas"></a>Reequilibrio de réplicas de particiones

Use la [herramienta de reequilibrio de particiones de Apache Kafka](https://github.com/hdinsight/hdinsight-kafka-tools) para reequilibrar los temas seleccionados. Esta herramienta se debe ejecutar desde una sesión de SSH en el nodo principal del clúster de Kafka.

Para más información acerca de la conexión con HDInsight mediante SSH, consulte el documento [Uso de SSH con HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="next-steps"></a>Pasos siguientes

* [Escalabilidad de Apache Kafka en HDInsight](apache-kafka-scalability.md)
* [Creación de reflejos con Apache Kafka en HDInsight](apache-kafka-mirroring.md)