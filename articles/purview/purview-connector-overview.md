---
title: Introducción a los conectores de Purview
description: En este artículo se describen los diferentes almacenes de datos y funcionalidades admitidos en Purview
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/13/2020
ms.openlocfilehash: 88fb9c823df6ae5df345911ccce1c579009fba02
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "96780219"
---
# <a name="supported-data-stores"></a>Almacenes de datos compatibles

Purview admite los siguientes almacenes de datos. Haga clic en cada almacén de datos para obtener información detallada sobre las funcionalidades admitidas y las configuraciones correspondientes.

## <a name="purview-data-sources"></a>Orígenes de datos de Purview

|**Categoría**|  **Almacén de datos**  |**Extracción de metadatos**|**Examen completo**|**Examen incremental**|**Examen con ámbito**|**Clasificación**|**Lineage**|
|---|---|---|---|---|---|---|---|
| Azure | [Azure Blob Storage](register-scan-azure-blob-storage-source.md)| Sí| Sí| Sí| Sí| Sí| Sí|
||[Azure Cosmos DB](register-scan-azure-cosmos-database.md)|Sí| Sí| Sí| Sí| Sí| Sí|
||[Azure Data Explorer](register-scan-azure-data-explorer.md)|Sí| Sí| Sí| Sí| Sí| Sí|
||[Azure Data Lake Storage Gen1](register-scan-adls-gen1.md)|Sí| Sí| Sí| Sí| Sí| Sí|
||[Azure Data Lake Storage Gen2](register-scan-adls-gen2.md)|Sí| Sí| Sí| Sí| Sí| Sí|
||[Azure SQL Database](register-scan-azure-sql-database.md)|Sí| Sí| No| Sí| Sí| Sí|
||[Instancia administrada de Azure SQL Database](register-scan-azure-sql-database-managed-instance.md)|Sí| Sí| No| Sí| Sí| Sí|
||[Azure Synapse Analytics (anteriormente SQL DW)](register-scan-azure-synapse-analytics.md)|Sí| Sí| No| Sí| Sí| Sí|
|Base de datos|[SQL Server](register-scan-on-premises-sql-server.md)|Sí| Sí| No| Sí| Sí| Sí|
|Power BI|[Power BI](register-scan-power-bi-tenant.md)|Sí| Sí| No| No| No| Sí|

## <a name="next-steps"></a>Pasos siguientes

- [Registro y examen del origen de Azure Blob Storage](register-scan-azure-blob-storage-source.md)