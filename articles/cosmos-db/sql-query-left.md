---
title: LEFT en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema de SQL LEFT en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 647bee87283f26b0d1ce832b2b2aaac690c05b92
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93333739"
---
# <a name="left-azure-cosmos-db"></a>LEFT (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve la parte izquierda de una cadena con el número especificado de caracteres.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
LEFT(<str_expr>, <num_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   Es la expresión de cadena de la que se van a extraer caracteres.  
  
*num_expr*  
   Es una expresión numérica que especifica el número de caracteres.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se devuelve la parte izquierda de "abc" para distintos valores de longitud.  
  
```sql
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{"l1": "a", "l2": "ab"}]  
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy).

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
