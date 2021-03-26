---
title: EndsWith en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información sobre la función del sistema SQL ENDSWITH en Azure Cosmos DB para devolver un valor booleano que indica si la primera expresión de cadena finaliza con la segunda.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c0cc93fee8aacc711a797925cb2e2808b73cafd1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93338839"
---
# <a name="endswith-azure-cosmos-db"></a>ENDSWITH (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Devuelve un valor booleano que indica si la primera expresión de cadena finaliza con la segunda.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ENDSWITH(<str_expr1>, <str_expr2> [, <bool_expr>])
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr1*  
   Es una expresión de cadena.  
  
*str_expr2*  
   Es una expresión de cadena que se va a comparar con el final de *str_expr1*.

*bool_expr* Valor opcional para omitir mayúsculas y minúsculas. Cuando se establece en true, ENDSWITH realizará una búsqueda sin distinguir mayúsculas y minúsculas. Si no se especifica, este valor es false.
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión condicional.  
  
## <a name="examples"></a>Ejemplos
  
En el ejemplo siguiente se comprueba si la cadena "abc" termina con "b" y "bC".  
  
```sql
SELECT ENDSWITH("abc", "b", false) AS e1, ENDSWITH("abc", "bC", false) AS e2, ENDSWITH("abc", "bC", true) AS e3
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[
    {
        "e1": false,
        "e2": false,
        "e3": true
    }
]
```  

## <a name="remarks"></a>Observaciones

Esta función del sistema se beneficiará de un [índice de intervalo](index-policy.md#includeexclude-strategy).

El consumo de RU de Contains aumentará a medida que se incremente la cardinalidad de la propiedad en la función del sistema. En otras palabras, si está comprobando si un valor de propiedad contiene una cadena determinada, el cargo de la consulta RU dependerá del número de valores posibles para esa propiedad.

Por ejemplo, considere dos propiedades: ciudad y país. La cardinalidad de ciudad es 5000 y la de país es 200. Aquí se muestran dos consultas de ejemplo:

```sql
    SELECT * FROM c WHERE ENDSWITH(c.town, "York", false)
```

```sql
    SELECT * FROM c WHERE ENDSWITH(c.country, "States", false)
```

La primera consulta probablemente usará más RU que la segunda porque la cardinalidad de ciudad es mayor que la de país.

Si el tamaño de la propiedad en EndsWith es superior a 1 KB para algunos documentos, el motor de consulta tendrá que cargar esos documentos. En este caso, el motor de consulta no podrá evaluar totalmente EndsWith con un índice. El cargo de RU para EndsWith será alto si tiene un gran número de documentos con tamaños de propiedad superiores a 1 KB.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](introduction.md)
