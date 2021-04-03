---
title: POWER en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL POWER en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 39db52223feea63a4e3afbd4775f884acbbca548
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93341695"
---
# <a name="power-azure-cosmos-db"></a>POWER (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Devuelve el valor de la expresión especificada a la potencia especificada.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
POWER (<numeric_expr1>, <numeric_expr2>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*numeric_expr1*  
   Es una expresión numérica.  
  
*numeric_expr2*  
   Es la potencia a la que se eleva *numeric_expr1*.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión numérica.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo elevar un número a la potencia 3 (el cubo del número).  
  
```sql
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{pow1: 8, pow2: 15.625}]  
```  

## <a name="next-steps"></a>Pasos siguientes

- [Funciones matemáticas (Azure Cosmos DB)](sql-query-mathematical-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
