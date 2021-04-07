---
title: Uso de bucles de T-SQL
description: Sugerencias para el desarrollo de soluciones mediante bucles T-SQL y reemplazo de los cursores para los grupos de SQL dedicados en Azure Synapse Analytics.
services: synapse-analytics
author: MSTehrani
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: emtehran
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: a6c4eb98d77ece6e6ae130fd57cc263ee7e5ca64
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98683229"
---
# <a name="using-t-sql-loops-for-dedicated-sql-pools-in-azure-synapse-analytics"></a>Uso de bucles T-SQL para grupos de SQL dedicados en Azure Synapse Analytics

En este artículo se incluyen sugerencias para el desarrollo de soluciones del grupo de SQL dedicado mediante bucles T-SQL y reemplazo de cursores.

## <a name="purpose-of-while-loops"></a>Propósito de los bucles WHILE

Los grupos de SQL dedicados en Azure Synapse admiten el bucle [WHILE](/sql/t-sql/language-elements/while-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) para ejecutar bloques de instrucciones de forma repetida. El bucle WHILE continúa siempre y cuando las condiciones especificadas se cumplan o hasta que el código termine específicamente el bucle con la palabra clave BREAK.

Los bucles son útiles para reemplazar los cursores definidos en código SQL. Afortunadamente, casi todos los cursores que están escritos en código SQL son de la variedad avance rápido y solo lectura. Por lo tanto, los bucles WHILE son una buena alternativa para reemplazar los cursores.

## <a name="replacing-cursors-in-dedicated-sql-pool"></a>Reemplazo de cursores en el grupo de SQL dedicado

Sin embargo, antes de profundizar en el tema, debe hacerse la siguiente pregunta: "¿Se podría reescribir este cursor para usar operaciones basadas en conjuntos?"

En muchos casos, la respuesta es afirmativa y este suele ser el mejor enfoque. Una operación basada en conjunto a menudo se realiza más rápido que un enfoque iterativo, fila a fila.

Los cursores de avance rápido y solo lectura se pueden reemplazar fácilmente por una construcción de bucle. El ejemplo siguiente es simple. Este código de ejemplo actualiza las estadísticas de todas las tablas de la base de datos. Mediante la iteración a través de las tablas del bucle, cada comando se ejecuta en secuencia.

En primer lugar, cree una tabla temporal que contenga un número de fila único usado para identificar las instrucciones individuales:

```sql
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

En segundo lugar, inicialice las variables necesarias para realizar el bucle:

```sql
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Ahora, recorra las instrucciones ejecutándolas de una en una:

```sql
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Finalmente, elimine la tabla temporal creada en el primer paso

```sql
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).
