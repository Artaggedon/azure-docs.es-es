---
title: hbase hbck devuelve incoherencias en Azure HDInsight
description: hbase hbck devuelve incoherencias en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/08/2019
ms.openlocfilehash: cbe4231bbecdf279c637cd334336437a020188d4
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98936990"
---
# <a name="scenario-hbase-hbck-command-returns-inconsistencies-in-azure-hdinsight"></a>Escenario: el comando `hbase hbck` devuelve incoherencias en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue-region-is-not-in-hbasemeta"></a>Problema: la región no está en `hbase:meta`

La región xxx está en HDFS, pero no se muestra en `hbase:meta` ni está implementada en ningún servidor de regiones.

### <a name="cause"></a>Causa

Varía.

### <a name="resolution"></a>Solución

1. Corrija la tabla meta mediante la ejecución de lo siguiente:

    ```
    hbase hbck -ignorePreCheckPermission –fixMeta
    ```

1. Asigne regiones a RegionServers mediante la ejecución de lo siguiente:

    ```
    hbase hbck -ignorePreCheckPermission –fixAssignment
    ```
---

## <a name="issue-region-is-offline"></a>Problema: la región está sin conexión

La región xxx no está implementada en ningún RegionServer. Esto significa que la región está en `hbase:meta`, pero sin conexión.

### <a name="cause"></a>Causa

Varía.

### <a name="resolution"></a>Solución

Ponga en línea las regiones mediante la ejecución de lo siguiente:

```
hbase hbck -ignorePreCheckPermission –fixAssignment
```

---

## <a name="issue-regions-have-the-same-startend-keys"></a>Problema: las regiones tienen las mismas claves de inicio y fin

### <a name="cause"></a>Causa

Varía.

### <a name="resolution"></a>Solución

Combine manualmente las regiones superpuestas. Vaya a la sección de tabla de la interfaz de usuario web de HBase HMaster y seleccione el vínculo de tabla que tiene el problema. Verá la clave de inicio y de fin de cada región que pertenezca a esa tabla. Después, combine manualmente las regiones superpuestas. En el shell de HBase, establezca `merge_region 'xxxxxxxx','yyyyyyy', true`. Por ejemplo:

```
RegionA, startkey:001, endkey:010,

RegionB, startkey:001, endkey:080,

RegionC, startkey:010, endkey:080.
```

En este escenario, debe combinar RegionA con RegionC y obtener RegionD con el mismo intervalo de claves que RegionB. Después, combine RegionB y RegionD. xxxxxxx e yyyyyy son la cadena de hash al final de cada nombre de región. Tenga cuidado de no combinar dos regiones discontinuas. Después de cada combinación, como la combinación de A y C, HBase iniciará una compactación en RegionD. Espere a que finalice la compactación antes de realizar otra combinación con RegionD. Encontrará el estado de la compactación en la página del servidor de regiones en la interfaz de usuario de HBase HMaster.

---

## <a name="issue-cant-load-regioninfo"></a>Problema: no se puede cargar `.regioninfo`

No se puede cargar `.regioninfo` para la región `/hbase/data/default/tablex/regiony`.

### <a name="cause"></a>Causa

Lo más probable es que se deba a la eliminación parcial de la región cuando se bloquea RegionServer o se reinicia la máquina virtual. Actualmente, Azure Storage es un sistema de archivos de blobs sin formato y algunas operaciones de archivo no son atómicas.

### <a name="resolution"></a>Solución

Limpie manualmente los archivos y carpetas restantes:

1. Ejecute `hdfs dfs -ls /hbase/data/default/tablex/regiony` para comprobar qué carpetas o archivos todavía se encuentran aquí.

1. Ejecute `hdfs dfs -rmr /hbase/data/default/tablex/regiony/filez` para eliminar todos los archivos o carpetas secundarios.

1. Ejecute `hdfs dfs -rmr /hbase/data/default/tablex/regiony` para eliminar la carpeta de la región.

---

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](../../azure-portal/supportability/how-to-create-azure-support-request.md). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).