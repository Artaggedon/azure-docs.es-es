---
title: REPLICATE en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL REPLICATE en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 7c0f1673c620ceadeb5ccca2a15cc9b7ce8d7685
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93341610"
---
# <a name="replicate-azure-cosmos-db"></a>REPLICATE (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Repite un valor de cadena un número especificado de veces.
  
## <a name="syntax"></a>Sintaxis
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es una expresión de cadena.
  
*num_expr*  
   Es una expresión numérica. Si el valor *num_expr* es negativo o no finito, el resultado es indefinido.
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de cadena.
  
## <a name="remarks"></a>Observaciones

  La longitud máxima del resultado es de 10 000 caracteres, es decir, (length(*str_expr*)  *  *num_expr*) <= 10 000. Esta función del sistema no usará el índice.

## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo usar `REPLICATE` en una consulta.
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 El conjunto de resultados es el siguiente:
  
```json
[{"replicate": "aaa"}]
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
