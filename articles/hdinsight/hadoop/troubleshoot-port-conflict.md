---
title: Conflicto de puertos al iniciar servicios en Azure HDInsight
description: Pasos para solucionar problemas y posibles soluciones para los problemas de conflicto de puertos que se producen al interactuar con clústeres de Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: f42e84d5d9c1dd49d9bf5604fe2f967eae0b6276
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98943090"
---
# <a name="scenario-port-conflict-when-starting-services-in-azure-hdinsight"></a>Escenario: Conflicto de puertos al iniciar servicios en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

El servicio no se inicia.

## <a name="cause"></a>Causa

Existe un conflicto de puerto.

## <a name="resolution"></a>Solución

### <a name="method-1"></a>Método 1

Use los comandos siguientes para obtener o eliminar todos los procesos en ejecución, que se ven afectados por el problema de los puertos.

```bash
netstat -lntp | grep <port>
ps -ef | grep <service>
kill -9 <service>
```

Luego, inicie el servicio.

### <a name="method-2"></a>Método 2

Reinicie el nodo.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](../../azure-portal/supportability/how-to-create-azure-support-request.md). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).