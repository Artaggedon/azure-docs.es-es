---
title: Obtención de la clave de acceso para un recurso de Event Grid
description: En este artículo se describe cómo obtener una clave de acceso para un tema o dominio de Event Grid.
ms.topic: how-to
ms.date: 07/07/2020
ms.openlocfilehash: a642affbac79766684dc75a37dae0373450d20e8
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632536"
---
# <a name="get-access-keys-for-event-grid-resources-topics-or-domains"></a>Obtención de claves de acceso para recursos de Event Grid (temas o dominios)
Las claves de acceso se utilizan para autenticar los eventos de publicación de una aplicación en recursos de Azure Event Grid (temas y dominios). Se recomienda regenerar las claves periódicamente y almacenarlas de forma segura. Como se le proporcionan dos claves de acceso, puede mantener las conexiones con una mientras se regenera la otra.

En este artículo se describe cómo obtener las claves de acceso de un recurso de Event Grid (tema o dominio) mediante Azure Portal, PowerShell o la CLI. 

## <a name="azure-portal"></a>Azure portal
En Azure Portal, cambie a la pestaña **Claves de acceso** de la página **Tema de Event Grid** o **Dominio de Event Grid** para su tema o dominio.  

:::image type="content" source="./media/get-access-keys/azure-portal.png" alt-text="Página de claves de acceso":::

## <a name="azure-powershell"></a>Azure PowerShell
Use el comando [Get-AzEventGridTopicKey](/powershell/module/az.eventgrid/get-azeventgridtopickey) para obtener claves de acceso para los temas. 

```azurepowershell-interactive
Get-AzEventGridTopicKey -ResourceGroup <RESOURCE GROUP NAME> -Name <TOPIC NAME>
```

Use el comando [Get-AzEventGridDomainKey](/powershell/module/az.eventgrid/get-azeventgriddomainkey) para obtener claves de acceso para los dominios. 

```azurepowershell-interactive
Get-AzEventGridDomainKey -ResourceGroup <RESOURCE GROUP NAME> -Name <DOMAIN NAME>
```

## <a name="azure-cli"></a>Azure CLI
Use la [lista de claves del tema az eventgrid](/cli/azure/eventgrid/topic/key#az-eventgrid-topic-key-list) para obtener las claves de acceso para los temas. 

```azurecli-interactive
az eventgrid topic key list --resource-group <RESOURCE GROUP NAME> --name <TOPIC NAME>
```

Use la [lista de claves del dominio az eventgrid](/cli/azure/eventgrid/domain/key#az-eventgrid-domain-key-list) para obtener las claves de acceso para los dominios. 

```azurecli-interactive
az eventgrid domain key list --resource-group <RESOURCE GROUP NAME> --name <DOMAIN NAME>
```

## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo siguiente: [Autenticación de clientes de publicación](security-authenticate-publishing-clients.md). 
