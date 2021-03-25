---
title: Consulta de registros de Start/Stop VMs during off-hours de Azure Automation
description: En este artículo se describe cómo usar Azure Monitor para consultar los datos de registro generados por Start/Stop VMs during off-hours.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 3e9e924d6626d9f0dcd2db8a5e8b8f90a0aa01ce
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100593852"
---
# <a name="query-logs-from-startstop-vms-during-off-hours"></a>Consulta de registros de Start/Stop VMs during off-hours

Azure Automation envía dos tipos de registros al área de trabajo de Log Analytics vinculada: registros de trabajo y flujos de trabajo. En este artículo se revisan los datos disponibles para [consultar](../azure-monitor/logs/log-query-overview.md) en Azure Monitor.

## <a name="job-logs"></a>Registros de trabajo

|Propiedad | Descripción|
|----------|----------|
|Autor de llamada |  La persona que inicia la operación. Los valores posibles son una dirección de correo electrónico o el sistema para los trabajos programados.|
|Category | Clasificación del tipo de datos. Para Automation, el valor será JobLogs.|
|CorrelationId | GUID que es el identificador de correlación del trabajo de Runbook.|
|JobId | GUID que es el identificador del trabajo de Runbook.|
|operationName | Especifica el tipo de operación realizada en Azure. En Automation, el valor es Job.|
|resourceId | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|ResourceGroup | Especifica el nombre del grupo de recursos del trabajo de Runbook.|
|ResourceProvider | Especifica el servicio de Azure que proporciona los recursos que puede implementar y administrar. Para Automation, el valor es Azure Automation.|
|ResourceType | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|resultType | El estado del trabajo de Runbook. Los valores posibles son:<br>Started<br>Stopped<br>Suspended<br>Con error<br>- Correcto|
|resultDescription | Describe el estado de resultado del trabajo de Runbook. Los valores posibles son:<br>- Se inicia el trabajo<br>- Error del trabajo<br>- Trabajo completado|
|RunbookName | Especifica el nombre del Runbook.|
|SourceSystem | Especifica el sistema de origen para los datos enviados. En Automation, el valor es OpsManager|
|StreamType | Especifica el tipo de evento. Los valores posibles son:<br>- Detallado<br>- Salida<br>Error<br>Warning (Advertencia)|
|SubscriptionId | Especifica el identificador de suscripción del trabajo.
|Time | Fecha y hora en que se ejecuta el trabajo de Runbook.|

## <a name="job-streams"></a>Transmisiones de trabajo

|Propiedad | Descripción|
|----------|----------|
|Autor de llamada |  La persona que inicia la operación. Los valores posibles son una dirección de correo electrónico o el sistema para los trabajos programados.|
|Category | Clasificación del tipo de datos. Para Automation, el valor es JobStreams.|
|JobId | GUID que es el identificador del trabajo de Runbook.|
|operationName | Especifica el tipo de operación realizada en Azure. En Automation, el valor es Job.|
|ResourceGroup | Especifica el nombre del grupo de recursos del trabajo de Runbook.|
|resourceId | Especifica el identificador de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|ResourceProvider | Especifica el servicio de Azure que proporciona los recursos que puede implementar y administrar. Para Automation, el valor es Azure Automation.|
|ResourceType | Especifica el tipo de recurso en Azure. Para Automation, el valor es la cuenta de Automation asociada al Runbook.|
|resultType | El resultado del trabajo de Runbook en el momento en que se generó el evento. Un valor posible es:<br>- En curso|
|resultDescription | Incluye la secuencia de salida del Runbook.|
|RunbookName | El nombre del Runbook.|
|SourceSystem | Especifica el sistema de origen para los datos enviados. En Automation, el valor es OpsManager.|
|StreamType | El tipo de flujo de trabajo. Los valores posibles son:<br>- Progreso<br>- Salida<br>Warning (Advertencia)<br>Error<br>DEBUG<br>- Detallado|
|Time | Fecha y hora en que se ejecuta el trabajo de Runbook.|

Cuando se realiza cualquier búsqueda de registros que devuelve registros de la categoría **JobLogs** o **JobStreams**, puede seleccionar las vistas **JobLogs** o **JobStreams**, que muestran un conjunto de iconos que resumen las actualizaciones devueltas en la búsqueda.

## <a name="sample-log-searches"></a>Búsquedas de registros de ejemplo

En la tabla siguiente se proporcionan búsquedas de registros de ejemplo para los registros de trabajo recopilados por Start/Stop VMs during off-hours.

|Consultar | Descripción|
|----------|----------|
|Búsqueda de trabajos del runbook ScheduledStartStop_Parent que hayan finalizado correctamente | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Búsqueda de trabajos del runbook ScheduledStartStop_Parent que no se hayan completado correctamente | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Búsqueda de trabajos del runbook SequencedStartStop_Parent que hayan finalizado correctamente | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Búsqueda de trabajos del runbook SequencedStartStop_Parent que no se hayan completado correctamente | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Failed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="next-steps"></a>Pasos siguientes

* Para configurar la característica, vea [Configuración de Stop/Start VMs during off-hours](automation-solution-vm-management-config.md).
* Para obtener información sobre las alertas de registro durante la implementación de características, vea [Creación de alertas de registro con Azure Monitor](../azure-monitor/alerts/alerts-log.md).
* Para resolver los errores de características, consulte [Solución de problemas de Start/Stop VMs during off-hours](troubleshoot/start-stop-vm.md).