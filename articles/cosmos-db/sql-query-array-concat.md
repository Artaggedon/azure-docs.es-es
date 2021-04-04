---
title: ARRAY_CONCAT en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema SQL Array Concat en Azure Cosmos DB devuelve una matriz que es el resultado de concatenar dos o más valores de matriz
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8b6424a161b5fc4d0c075ade6f852b33dff20a09
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93332770"
---
# <a name="array_concat-azure-cosmos-db"></a>ARRAY_CONCAT (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve una matriz que es el resultado de concatenar dos o más valores de la matriz.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ARRAY_CONCAT (<arr_expr1>, <arr_expr2> [, <arr_exprN>])  
```  
  
## <a name="arguments"></a>Argumentos
  
*arr_expr*  
   Es una expresión de matriz que se va a concatenar a los otros valores. La función `ARRAY_CONCAT` requiere al menos dos argumentos *arr_expr*.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de matriz.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo concatenar dos matrices.  
  
```sql
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  
## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de matriz (Azure Cosmos DB)](sql-query-array-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
