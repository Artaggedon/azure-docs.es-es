---
title: Supervisión de los recursos delegados a escala
description: Aprenda a usar los registros de Azure Monitor de forma eficaz y escalable en los inquilinos del cliente que está administrando.
ms.date: 02/02/2021
ms.topic: how-to
ms.openlocfilehash: 8847c2e5ee4986d35ad676440720b150794003e8
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99575913"
---
# <a name="monitor-delegated-resources-at-scale"></a>Supervisión de los recursos delegados a escala

Como proveedor de servicios, es posible que haya incorporado varios inquilinos de cliente para [Azure Lighthouse](../overview.md). Azure Lighthouse permite a los proveedores de servicios realizar operaciones a escala a través de varios inquilinos a la vez, lo que hace que las tareas de administración sean más eficaces.

En este tema se muestra cómo usar los [registros de Azure Monitor](../../azure-monitor/platform/data-platform-logs.md) de forma eficaz y escalable en los inquilinos del cliente que está administrando. Aunque en este tema hacemos referencia a proveedores de servicios y clientes, esta guía es aplicable también a [empresas que usan Azure Lighthouse para administrar varios inquilinos](../concepts/enterprise.md).

> [!NOTE]
> Asegúrese de que se han concedido a los usuarios de los inquilinos de administración los [roles necesarios para administrar las áreas de trabajo de Log Analytics](../../azure-monitor/platform/manage-access.md#manage-access-using-azure-permissions) en las suscripciones de clientes delegados.

## <a name="create-log-analytics-workspaces"></a>Creación de áreas de trabajo de Log Analytics

Para recopilar datos, deberá crear áreas de trabajo de Log Analytics. Estas áreas de trabajo de Log Analytics son entornos únicos para los datos que recopila Azure Monitor. Cada área de trabajo tiene su propio repositorio de datos y configuración, y las soluciones y orígenes de datos están configurados para almacenar sus datos en una determinada área de trabajo.

Se recomienda crear estas áreas de trabajo directamente en los inquilinos del cliente. De esta forma, los datos permanecen en sus respectivos inquilinos en lugar de exportarse al suyo. Esto también permite la supervisión centralizada de los recursos o servicios que admite Log Analytics, lo que le proporciona más flexibilidad respecto a los tipos de datos que supervisa.

> [!TIP]
> Cualquier cuenta de Automation usada para acceder a los datos de un área de trabajo de Log Analytics debe crearse en el mismo inquilino que el área de trabajo.

Puede crear un área de trabajo de Log Analytics mediante [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md), la [CLI de Azure](../../azure-monitor/learn/quick-create-workspace-cli.md) o [Azure PowerShell](../../azure-monitor/platform/powershell-workspace-configuration.md).

> [!IMPORTANT]
> Incluso si todas las áreas de trabajo se crean en el inquilino del cliente, el proveedor de recursos Microsoft.Insights también debe registrarse en una suscripción del inquilino de administración.

## <a name="deploy-policies-that-log-data"></a>Implementación de directivas que registran datos

Después de crear sus áreas de trabajo de Log Analytics, puede implementar [Azure Policy](../../governance/policy/index.yml) en las jerarquías del cliente para que los datos de diagnóstico se envíen al área de trabajo correspondiente en cada inquilino. Las directivas exactas implementadas pueden variar en función de los tipos de recursos que quiera supervisar.

Para obtener más información acerca de la creación de directivas, vea [Tutorial: Creación y administración de directivas para aplicar el cumplimiento](../../governance/policy/tutorials/create-and-manage.md). Esta [herramienta de la comunidad](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/azure-diagnostics-policy-generator) proporciona un script para ayudarle a crear directivas para supervisar los tipos de recursos específicos que elija.

Cuando haya determinado las directivas que se van a implementar, puede [implementarlas en las suscripciones delegadas a escala](policy-at-scale.md).

## <a name="analyze-the-gathered-data"></a>Análisis de los datos recopilados

Una vez implementadas las directivas, los datos se registrarán en las áreas de trabajo de Log Analytics que ha creado en cada inquilino del cliente. Para obtener información sobre todos los clientes administrados, puede usar herramientas como [Libros de Azure Monitor](../../azure-monitor/platform/workbooks-overview.md) para recopilar y analizar información de varios orígenes de datos.

## <a name="view-alerts-across-customers"></a>Visualización de alertas entre clientes

Puede ver [alertas](../../azure-monitor/platform/alerts-overview.md) para las suscripciones delegadas en los inquilinos de clientes que administra.

Desde el inquilino de administración, puede [crear, ver y administrar las alertas del registro de actividad](../../azure-monitor/platform/alerts-activity-log.md) en Azure Portal o a través de las API y las herramientas de administración.

Para actualizar las alertas automáticamente entre varios clientes, use una consulta de [Azure Resource Graph](../../governance/resource-graph/overview.md) para filtrar las alertas. Puede anclar la consulta al panel y seleccionar todos los clientes y suscripciones adecuados. Por ejemplo, en la consulta siguiente se mostrarán las alertas de gravedad 0 y 1, que se actualizan cada 60 minutos.

```kusto
alertsmanagementresources
| where type == "microsoft.alertsmanagement/alerts"
| where properties.essentials.severity =~ "Sev0" or properties.essentials.severity =~ "Sev1"
| where properties.essentials.monitorCondition == "Fired"
| where properties.essentials.startDateTime > ago(60m)
| project StartTime=properties.essentials.startDateTime,name,Description=properties.essentials.description, Severity=properties.essentials.severity, subscriptionId
| sort by tostring(StartTime)
```

## <a name="next-steps"></a>Pasos siguientes

- Explore este [libro de ejemplo creado por MVP](https://github.com/scautomation/Azure-Automation-Update-Management-Workbooks), que realiza un seguimiento de los informes de cumplimiento de revisiones mediante la [consulta de registros de Update Management](../../automation/update-management/query-logs.md) en varias áreas de trabajo de Log Analytics. 
- Más información acerca de [Azure Monitor](../../azure-monitor/index.yml).
- Más información acerca de los [registros de Azure Monitor](../../azure-monitor/platform/data-platform-logs.md).
- Más información sobre las [experiencias de administración entre inquilinos](../concepts/cross-tenant-management-experience.md).