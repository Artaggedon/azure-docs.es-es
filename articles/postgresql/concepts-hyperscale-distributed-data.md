---
title: 'Datos distribuidos en Azure Database for PostgreSQL: Hiperescala (Citus)'
description: Obtenga información sobre las tablas distribuidas, las tablas de referencia, las tablas locales y las particiones en Azure Database for PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 7757fdb4953640597a805c3d74a9e1ef08ef2c07
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "86114506"
---
# <a name="distributed-data-in-azure-database-for-postgresql--hyperscale-citus"></a>Datos distribuidos en Azure Database for PostgreSQL: Hiperescala (Citus)

En este artículo se describen los tres tipos de tabla de Hiperescala (Citus) de Azure Database for PostgreSQL.
Se muestra la forma en que se almacenan las tablas distribuidas como particiones de base de datos y la manera en la que las particiones de base de datos se colocan en nodos.

## <a name="table-types"></a>Tipos de tablas

Hay tres tipos de tablas en un grupo de servidores Hiperescala (Citus); cada una se utiliza para propósitos diferentes.

### <a name="type-1-distributed-tables"></a>Tipo 1: Tablas distribuidas

El primer tipo, y el más común, es la tabla distribuida. Aparece como una tabla normal para las instrucciones SQL, pero está particionada horizontalmente en nodos de trabajo. Esto significa que las filas de la tabla se almacenan en nodos diferentes, en tablas de fragmentos llamadas particiones de base de datos.

Hiperescala (Citus) no solo ejecuta instrucciones SQL sino también DDL en un clúster.
El cambio del esquema de una tabla distribuida se propaga para actualizar todas las particiones de la tabla en los nodos de trabajo.

#### <a name="distribution-column"></a>Columna de distribución

Hiperescala (Citus) usa el particionamiento algorítmico para asignar filas a las particiones de base de datos. La asignación se realiza de forma determinista en función del valor de una columna de la tabla denominada columna de distribución. El administrador de clústeres debe designar esta columna al distribuir una tabla.
Realizar la elección correcta es importante para el rendimiento y la funcionalidad.

### <a name="type-2-reference-tables"></a>Tipo 2: tablas de referencia

Una tabla de referencia es un tipo de tabla distribuida en la que todo su contenido se concentra en una sola partición de base de datos. La partición se replica en todos los nodos de trabajo. Las consultas en cualquier nodo pueden acceder a la información de referencia localmente, sin la sobrecarga de red que supone solicitar filas de otro nodo. Las tablas de referencia no tienen columna de distribución porque no es necesario distinguir particiones de base de datos independientes por cada fila.

Las tablas de referencia suelen ser pequeñas y se usan para almacenar los datos que son pertinentes para las consultas que se ejecutan en cualquier nodo de trabajo. Por ejemplo, valores enumerados, como los estados de pedidos o las categorías de productos.

### <a name="type-3-local-tables"></a>Tipo 3: tablas locales

Cuando se usa Hiperescala (Citus), el nodo de coordinación al que se conecta es una base de datos PostgreSQL normal. Puede crear tablas normales en el coordinador y elegir no particionarlas.

Un buen candidato para las tablas locales serían pequeñas tablas administrativas que no participan en las consultas de combinación. Por ejemplo, una tabla de usuarios para el inicio de sesión en una aplicación y la autenticación.

## <a name="shards"></a>Particiones de base de datos

En la sección anterior, se describe la manera en que las tablas distribuidas se almacenan como particiones de base de datos en nodos de trabajo. En esta sección se tratan detalles más técnicos.

La tabla de metadatos `pg_dist_shard` del coordinador contiene una fila para cada partición de base de datos de cada tabla distribuida en el sistema. La fila asocia un identificador de partición con un intervalo de enteros en un espacio de hash (shardminvalue, shardmaxvalue).

```sql
SELECT * from pg_dist_shard;
 logicalrelid  | shardid | shardstorage | shardminvalue | shardmaxvalue
---------------+---------+--------------+---------------+---------------
 github_events |  102026 | t            | 268435456     | 402653183
 github_events |  102027 | t            | 402653184     | 536870911
 github_events |  102028 | t            | 536870912     | 671088639
 github_events |  102029 | t            | 671088640     | 805306367
 (4 rows)
```

Si el nodo de coordinación quiere determinar qué partición de base de datos contiene una fila de `github_events`, aplica un algoritmo hash al valor de la columna de distribución de la fila. A continuación, comprueba qué intervalo de la partición de base de datos contiene el valor con hash. Los intervalos se definen de modo que la imagen de la función hash sea su unión independiente.

### <a name="shard-placements"></a>Colocaciones de particiones

Supongamos que la partición de base de datos 102027 está asociada a la fila en cuestión. La fila se leerá o escribirá en una tabla denominada `github_events_102027` de uno de los nodos de trabajo. ¿Qué trabajo? Esto lo determinan por completo las tablas de metadatos. La asignación de la partición de base de datos a un nodo de trabajo se conoce como colocación de la partición.

El nodo de coordinación vuelve a escribir consultas en fragmentos que hacen referencia a las tablas específicas como `github_events_102027`, y ejecuta esos fragmentos en los nodos de trabajo adecuados. Este es un ejemplo de una consulta que se ejecuta en segundo plano para buscar el nodo que contiene el identificador de partición de base de datos 102027.

```sql
SELECT
    shardid,
    node.nodename,
    node.nodeport
FROM pg_dist_placement placement
JOIN pg_dist_node node
  ON placement.groupid = node.groupid
 AND node.noderole = 'primary'::noderole
WHERE shardid = 102027;
```

```output
┌─────────┬───────────┬──────────┐
│ shardid │ nodename  │ nodeport │
├─────────┼───────────┼──────────┤
│  102027 │ localhost │     5433 │
└─────────┴───────────┴──────────┘
```

## <a name="next-steps"></a>Pasos siguientes

- Aprender a [elegir una columna de distribución](concepts-hyperscale-choose-distribution-column.md) para las tablas distribuidas
