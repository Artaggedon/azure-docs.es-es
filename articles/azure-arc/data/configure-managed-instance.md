---
title: Configuración de una instancia de SQL Managed Instance habilitada para Azure Arc
description: Configuración de una instancia de SQL Managed Instance habilitada para Azure Arc
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 37ba4f10365fca4292171c3bd2d9a3e7d00045bb
ms.sourcegitcommit: 04297f0706b200af15d6d97bc6fc47788785950f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98985876"
---
# <a name="configure-azure-arc-enabled-sql-managed-instance"></a>Configuración de una instancia de SQL Managed Instance habilitada para Azure Arc

En este artículo se explica cómo configurar la instancia de SQL Managed Instance habilitada para Azure Arc.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="configure-resources"></a>Configure resources

### <a name="configure-using-azure-data-cli-azdata"></a>Configuración mediante [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]

Puede editar la configuración de las instancias de SQL Managed Instance habilitada para Azure Arc con [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]. Ejecute el siguiente comando para ver las opciones de configuración. 

```
azdata arc sql mi edit --help
```

En el ejemplo siguiente se establecen las solicitudes y límites de CPU, núcleos y memoria.

```
azdata arc sql mi edit --cores-limit 4 --cores-request 2 --memory-limit 4Gi --memory-request 2Gi -n <NAME_OF_SQL_MI>
```

Para ver los cambios realizados en la instancia de SQL Managed Instance, puede usar los comandos siguientes para ver el archivo de configuración YAML:

```
azdata arc sql mi show -n <NAME_OF_SQL_MI>
```

## <a name="configure-server-options"></a>Configuración de las opciones del servidor

Puede configurar las opciones de configuración del servidor para la instancia de SQL Managed Instance habilitada para Azure Arc después de la creación. En este artículo se describe cómo configurar opciones como habilitar o deshabilitar el agente mssql o habilitar marcas de seguimiento específicas para escenarios de solución de problemas.

Haga lo siguiente para cambiar alguna de esas opciones:

1. Cree un archivo `mssql-custom.conf` personalizado que incluya la configuración de destino. En el ejemplo siguiente se habilita el Agente SQL y se habilita la marca de seguimiento 1204.:

   ```
   [sqlagent]
   enabled=true
   
   [traceflag]
   traceflag0 = 1204
   ```

1. Copie el archivo `mssql-custom.conf` en `/var/opt/mssql` en el contenedor `arc-sqlmi` del pod `master-0`. Reemplace `<namespaceName>` por el espacio de nombres en el que está implementada esta instancia.

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. Reinicie la instancia administrada de SQL.  Reemplace `<namespaceName>` por el espacio de nombres en el que está implementada esta instancia.

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```


**Restricciones conocidas**
- Los procedimientos anteriores requieren permisos de administrador de clústeres de Kubernetes.
- Esto está sujeto a cambios durante la versión preliminar.
