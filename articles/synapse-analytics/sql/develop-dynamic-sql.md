---
title: Uso de SQL dinámico en SQL de Synapse
description: Sugerencias para usar SQL dinámico en SQL de Synapse.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 86d5028a09a805142f7632f93530f8a54965d82f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "101675029"
---
# <a name="dynamic-sql-in-synapse-sql"></a>SQL dinámico en SQL de Synapse

En este artículo, encontrará sugerencias para usar SQL dinámico y desarrollar soluciones con SQL de Synapse.

## <a name="dynamic-sql-example"></a>Ejemplo de SQL dinámico

Al desarrollar código de aplicación, puede que necesite usar SQL dinámico con el fin de proporcionar soluciones flexibles, genéricas y modulares.

> [!NOTE]
> En este momento, el grupo de SQL dedicado no admite tipos de datos de blob. El hecho de no admitir tipos de datos blob podría limitar el tamaño de las cadenas, ya que los tipos de datos blob incluyen los tipos varchar(max) y nvarchar(max). Si ha utilizado estos tipos en el código de aplicación para crear cadenas grandes, es necesario dividir el código en fragmentos y utilizar en su lugar la instrucción EXEC.

Un ejemplo sencillo:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Si la cadena es corta, puede usar [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql?view=azure-sqldw-latest&preserve-view=true) como de costumbre.

> [!NOTE]
> Las instrucciones ejecutadas como SQL dinámico seguirán sujetas a todas las reglas de validación de T-SQL.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](develop-overview.md).
