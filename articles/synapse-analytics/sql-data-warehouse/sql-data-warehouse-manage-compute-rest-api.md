---
title: Pausa, reanudación y escalado con API REST
description: Administre la potencia de proceso de un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics a través de las API REST.
services: synapse-analytics
author: antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 03/29/2019
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 243a7fd71e7b4f762130db2383f449021129cc62
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2021
ms.locfileid: "98060306"
---
# <a name="rest-apis-for-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>API REST para el grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics

API REST para administrar la potencia de proceso de un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics.

## <a name="scale-compute"></a>Escalado de proceso

Para cambiar las unidades de almacenamiento de datos, use la API REST para [crear o actualizar la base de datos](/rest/api/sql/databases/createorupdate?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). En el ejemplo siguiente se establecen las unidades de almacenamiento de datos en DW1000 para la base de datos MySQLDW, que está hospedada en el servidor MyServer. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": "DW1000c"
    }
}
```

## <a name="pause-compute"></a>Pausa del proceso

Para pausar una base de datos, use la API REST para [pausar la base de datos](/rest/api/sql/databases/pause?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). El siguiente ejemplo pausa una base de datos denominada Database02 que está hospedada en un servidor llamado Server01. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>Reanudación del proceso

Para iniciar una base de datos, use la API REST para [reanudar la base de datos](/rest/api/sql/databases/resume?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). El siguiente ejemplo inicia una base de datos denominada Database02 que está hospedada en un servidor llamado Server01. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Comprobar el estado de la base de datos

> [!NOTE]
> Actualmente, la comprobación del estado de la base de datos podría devolver ONLINE mientras se está completando el flujo de trabajo en línea, lo que produce errores de conexión. Es posible que deba agregar un retraso de 2 a 3 minutos en el código de aplicación si utiliza esta llamada de API para desencadenar los intentos de conexión.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

## <a name="get-maintenance-schedule"></a>Obtención de la programación de mantenimiento

Compruebe la programación de mantenimiento que se ha establecido para un grupo de SQL dedicado (anteriormente SQL DW).

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

```

## <a name="set-maintenance-schedule"></a>Establecimiento de la programación de mantenimiento

Para establecer y actualizar una programación de mantenimiento en un grupo de SQL dedicado (anteriormente SQL DW).

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

{
    "properties": {
        "timeRanges": [
                {
                                "dayOfWeek": "Saturday",
                                "startTime": "00:00",
                                "duration": "08:00",
                },
                {
                                "dayOfWeek": "Wednesday",
                                "startTime": "00:00",
                                "duration": "08:00",
                }
                ]
    }
}

```

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [Administración de proceso en Azure SQL Data Warehouse](sql-data-warehouse-manage-compute-overview.md).
