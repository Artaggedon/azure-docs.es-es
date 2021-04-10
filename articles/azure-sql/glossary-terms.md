---
title: Glosario de términos
titleSuffix: Azure SQL
description: Un glosario de términos para trabajar con Azure SQL Database, Instancia administrada de Azure SQL y SQL en la máquina virtual de Azure.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=4
ms.devlang: ''
ms.topic: reference
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/09/2020
ms.openlocfilehash: 8bd1e312463cf89ae30d54bc4a32a497fc7a9a78
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98249659"
---
# <a name="azure-sql-database-glossary-of-terms"></a>Glosario de términos de Azure SQL Database
[!INCLUDE[appliesto-asf](includes/appliesto-asf.md)]

## <a name="azure-sql-database"></a>Azure SQL Database

|Context|Término|Más información|
|:---|:---|:---|
|Servicio de Azure|Azure SQL Database o SQL Database|[Azure SQL Database](database/sql-database-paas-overview.md)|
|Modelo de compra|Modelo de compra basado en DTU|[Modelo de compra basado en DTU](database/service-tiers-dtu.md)|
||Modelo de compra basado en núcleo virtual|[Modelo de compra basado en núcleo virtual](database/service-tiers-vcore.md)|
|Opción de implementación |Base de datos única|[Bases de datos únicas](database/single-database-overview.md)|
||Grupo elástico|[Grupo elástico](database/elastic-pool-overview.md)|
|Nivel de servicio|Básico, Estándar, Premium, De uso general, Hiperescala, Crítico para la empresa|Para los niveles de servicio en el modelo núcleo virtual, consulte [Nivel de servicio de SQL Database](database/service-tiers-vcore.md#service-tiers). Para los niveles de servicio en el modelo de DTU, consulte la información sobre el [modelo de DTU](database/service-tiers-dtu.md#compare-the-dtu-based-service-tiers).|
|Nivel de proceso|Proceso sin servidor|[Proceso sin servidor](database/service-tiers-vcore.md#compute-tiers)
||Proceso aprovisionado|[Proceso aprovisionado](database/service-tiers-vcore.md#compute-tiers)
|Generación de procesos|Gen5, serie M, serie Fsv2, serie DC|[Generaciones de hardware](database/service-tiers-vcore.md#hardware-generations)
|Entidad de servidor| Server |[Servidores SQL Server lógicos](database/logical-servers.md)|
|Tipo de recurso|Núcleo virtual|Un núcleo de CPU proporcionado para el recurso de proceso para una única base de datos, grupo elástico. |
||Tamaño de proceso y cantidad de almacenamiento|El tamaño de proceso es la cantidad máxima de CPU, memoria y otros recursos no relacionados con el almacenamiento disponibles para una base de datos única o grupo elástico.  El tamaño de almacenamiento es la cantidad máxima de almacenamiento disponible para una base de datos única o grupo elástico. Para obtener más opciones de ajuste de tamaño en el modelo de núcleo virtual, vea [Bases de datos únicas de núcleo virtual](database/resource-limits-vcore-single-databases.md) y [Grupos elásticos de núcleo virtual](database/resource-limits-vcore-elastic-pools.md).  (../managed-instance/resource-limits.md).  Para obtener más opciones de ajuste de tamaño en el modelo de DTU, vea [Bases de datos únicas de DTU](database/resource-limits-dtu-single-databases.md) y [Grupos elásticos de DTU](database/resource-limits-dtu-elastic-pools.md).

## <a name="azure-sql-managed-instance"></a>Instancia administrada de Azure SQL

|Context|Término|Más información|
|:---|:---|:---|
|Servicio de Azure|Instancia administrada de Azure SQL|[Instancia administrada de SQL](managed-instance/sql-managed-instance-paas-overview.md)|
|Modelo de compra|Modelo de compra basado en núcleo virtual|[Modelo de compra basado en núcleo virtual](database/service-tiers-vcore.md)|
|Opción de implementación |Instancia única|[Instancia única](managed-instance/sql-managed-instance-paas-overview.md)|
||Grupo de instancias (versión preliminar)|[Grupos de instancias](managed-instance/instance-pools-overview.md)|
|Nivel de servicio|De uso general/Crítico para la empresa|[Niveles de servicio de Instancia administrada de SQL](managed-instance/sql-managed-instance-paas-overview.md#service-tiers)|
|Nivel de proceso|Proceso aprovisionado|[Proceso aprovisionado](database/service-tiers-vcore.md#compute-tiers)|
|Generación de procesos|Gen5|[Generaciones de hardware](database/service-tiers-vcore.md#hardware-generations)
|Entidad de servidor|Instancia administrada o instancia| N/A ya que la instancia administrada de SQL es en si misma el servidor |
|Tipo de recurso|Núcleo virtual|Un núcleo de CPU proporcionado al recurso de proceso para Instancia administrada de SQL.|
||Tamaño de proceso y cantidad de almacenamiento|El tamaño de proceso es la cantidad máxima de CPU, memoria y otros recursos no relacionados con el almacenamiento para Instancia administrada de SQL.  El tamaño de almacenamiento es la cantidad máxima de almacenamiento disponible para una instancia administrada de SQL.  Para las opciones de ajuste de tamaño, [Instancias administradas de SQL](managed-instance/resource-limits.md). |
