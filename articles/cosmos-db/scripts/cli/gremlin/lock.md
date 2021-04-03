---
title: Creación de un bloqueo de recurso para una base de datos o un grafo de Gremlin para Azure Cosmos DB
description: Creación de un bloqueo de recurso para una base de datos o un grafo de Gremlin para Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 492246b5dfb19664ea54ce8b5462c7d77f8d951b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "94562719"
---
# <a name="create-a-resource-lock-for-azure-cosmos-gremlin-api-database-and-graph-using-azure-cli"></a>Cree un bloqueo de recurso para una base de datos y un grafo de Gremlin API de Azure Cosmos DB con la CLI de Azure.
[!INCLUDE[appliesto-gremlin-api](../../../includes/appliesto-gremlin-api.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- En este artículo se necesita la versión 2.9.1 de la CLI de Azure o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

> [!IMPORTANT]
> Los bloqueos de recursos no funcionarán para los cambios que realicen los usuarios que se conecten mediante cualquier SDK de Gremlin o Azure Portal, a menos que se bloquee en primer lugar la cuenta de Cosmos DB con la propiedad `disableKeyBasedMetadataWriteAccess` habilitada. Para más información sobre cómo habilitar esta propiedad, consulte [Evitar cambios de SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/gremlin/lock.sh "Create a resource lock for an Azure Cosmos DB Gremlin API database and graph.")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az lock create](/cli/azure/lock#az-lock-create) | Crea un bloqueo. |
| [az lock list](/cli/azure/lock#az-lock-list) | Muestra información del bloqueo. |
| [az lock show](/cli/azure/lock#az-lock-show) | Muestra las propiedades de un bloqueo. |
| [az lock delete](/cli/azure/lock#az-lock-delete) | Elimina un bloqueo. |

## <a name="next-steps"></a>Pasos siguientes

- [Bloqueo de recursos para impedir cambios inesperados](../../../../azure-resource-manager/management/lock-resources.md)

- [Documentación sobre la CLI de Azure Cosmos DB](/cli/azure/cosmosdb).

- [Repositorio de GitHub para la CLI de Azure Cosmos DB](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
