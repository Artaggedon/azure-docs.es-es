---
title: Referencia de cmdlets de Powershell
description: Obtenga información sobre los cmdlets de PowerShell para Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: ccc9f709348d961e49bced00946658a6997837c0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92368118"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Referencia de cmdlets de PowerShell para Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes. 
>
> Scheduler ya no está disponible en Azure portal, pero la [API REST](/rest/api/scheduler) y los [cmdlets de PowerShell para Azure Scheduler](scheduler-powershell-reference.md) siguen disponibles en la actualidad para que pueda administrar los trabajos y las colecciones de trabajos.

Para generar scripts para crear y administrar trabajos de Scheduler y colecciones de trabajos, puede usar cmdlets de PowerShell. En este artículo se enumeran los principales cmdlets de PowerShell para Azure Scheduler con vínculos a los artículos de referencia. Para instalar Azure PowerShell para su suscripción de Azure, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/). Para más información sobre los [cmdlets de Azure PowerShell](/powershell/azure/), consulte [Uso de Azure PowerShell con Azure Resource Manager](../azure-resource-manager/management/manage-resources-powershell.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

| Cmdlet | Descripción |
|--------|-------------|
| [Disable-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Deshabilita una colección de trabajos. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Habilita una colección de trabajos. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Obtiene trabajos de Scheduler. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Obtiene colecciones de trabajos. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Obtiene el historial de trabajos. |
| [New-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Crea un trabajo HTTP. |
| [New-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Crea una colección de trabajos. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Crea un trabajo de cola de Service Bus. |
| [New-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Crea un trabajo de tema de Service Bus. |
| [New-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Crea un trabajo de cola de Storage. |
| [Remove-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Elimina un trabajo de Scheduler. |
| [Remove-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Quita una colección de trabajos. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Modifica un trabajo HTTP de Scheduler. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Modifica una colección de trabajos. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Modifica un trabajo de cola de Service Bus. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Modifica un trabajo de tema de Service Bus. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Modifica un trabajo de cola de Storage. |
||| 

Para obtener más detalles, puede ejecutar cualquiera de estos cmdlets: 

```text
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="next-steps"></a>Pasos siguientes

* [Conceptos, terminología y jerarquía de entidades de Azure Scheduler](scheduler-concepts-terms.md)
* [Límites, valores predeterminados y códigos de error de Azure Scheduler](scheduler-limits-defaults-errors.md)
* [Referencia de API de REST de Azure Scheduler](/rest/api/scheduler)