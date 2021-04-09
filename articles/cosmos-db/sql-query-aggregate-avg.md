---
title: AVG en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema de SQL Average (AVG) en Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 2cfbca798a7b404e4ee12b93396b2a5b08d7d5bb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96551350"
---
# <a name="avg-azure-cosmos-db"></a>AVG (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Esta función de agregado devuelve la media de los valores de la expresión.
  
## <a name="syntax"></a>Sintaxis
  
```sql
AVG(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr*  
   Es una expresión numérica.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
El siguiente ejemplo devuelve el valor medio de `propertyA`:
  
```sql
SELECT AVG(c.propertyA)
FROM c
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy). Si algún argumento de `AVG` es una cadena, un valor booleano o un valor null, toda la función del sistema de agregado devolverá `undefined`. Si algún argumento tiene un valor `undefined`, no afectará al cálculo de `AVG`.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas en Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Funciones del sistema en Azure Cosmos DB](sql-query-system-functions.md)
- [Funciones de agregado en Azure Cosmos DB](sql-query-aggregate-functions.md)