---
title: StringEquals en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre cómo la función del sistema SQL StringEquals en Azure Cosmos DB devuelve un valor booleano que indica si la primera expresión de cadena contiene la segunda
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 47d899534de535b5cd6a7c3fb2df78cdadbe11f2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93338057"
---
# <a name="stringequals-azure-cosmos-db"></a>STRINGEQUALS (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve un valor booleano que indica si la primera expresión de cadena coincide con la segunda.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
STRINGEQUALS(<str_expr1>, <str_expr2> [, <bool_expr>])  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr1*  
   Es la primera expresión de cadena que se va a comparar.  
  
*str_expr2*  
   Es la segunda expresión de cadena que se va a comparar.  

*bool_expr* Valor opcional para omitir mayúsculas y minúsculas. Cuando se establece en true, StringEquals realizará una búsqueda sin distinguir mayúsculas y minúsculas. Si no se especifica, este valor es false.
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión condicional.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se comprueba si "abc" contiene "abc" y si "abc" contiene "ABC".  
  
```sql
SELECT STRINGEQUALS("abc", "abc", false) AS c1, STRINGEQUALS("abc", "ABC", false) AS c2,  STRINGEQUALS("abc", "ABC", true) AS c3
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[
    {
        "c1": true,
        "c2": false,
        "c3": true
    }
]
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy).

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
