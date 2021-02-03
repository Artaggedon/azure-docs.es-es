---
title: Uso de SCP con Apache Hadoop en Azure HDInsight
description: En este documento se proporciona información sobre cómo conectarse a HDInsight mediante los comandos ssh y scp.
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: 927b8c55008c3e01d8ff1dd09c46cfa3c6618026
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98946273"
---
# <a name="use-scp-with-apache-hadoop-in-azure-hdinsight"></a>Uso de SCP con Apache Hadoop en Azure HDInsight

En este artículo se proporciona información sobre la transferencia segura de archivos con el clúster de HDInsight.

## <a name="copy-files"></a>Copiar archivos

La utilidad `scp` puede utilizarse para copiar archivos a los nodos individuales del clúster y desde ellos. Por ejemplo, el siguiente comando copia el directorio `test.txt` desde el sistema local en el nodo principal primario:

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

Dado que no se especifica ninguna ruta de acceso después de `:`, el archivo se coloca en el directorio principal `sshuser`.

En el ejemplo siguiente, se copia el archivo `test.txt` desde el directorio principal `sshuser` al nodo principal primario del sistema local:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

`scp` solo puede tener acceso al sistema de archivos de los nodos individuales dentro del clúster. No se puede utilizar para acceder a datos en el almacenamiento compatible con HDFS para el clúster.

Use `scp` cuando necesite cargar un recurso para utilizarse desde una sesión de SSH. Por ejemplo, cargue un script de Python y, a continuación, ejecútelo desde una sesión de SSH.

Para obtener información sobre cómo cargar directamente los datos en el almacenamiento compatible con HDFS, consulte los siguientes documentos:

* [HDInsight mediante Azure Storage](hdinsight-hadoop-use-blob-storage.md).
* [HDInsight mediante Azure Data Lake Storage Gen1](../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen1.md).

## <a name="next-steps"></a>Pasos siguientes

* [Uso de SSH con HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)
* [Uso de nodos perimetrales en HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node)
