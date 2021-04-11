---
title: Solución de problemas de SQL Insights (versión preliminar)
description: Solución de problemas de SQL Insights en Azure Monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/04/2021
ms.openlocfilehash: 85a3505dd347b96036c28c85c089afa04e3e3bd5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608850"
---
# <a name="troubleshooting-sql-insights-preview"></a>Solución de problemas de SQL Insights (versión preliminar)
Para solucionar los problemas de recopilación de datos en SQL insights, compruebe el estado de la máquina de supervisión en la pestaña **Administrar perfil**. Contendrá uno de los siguientes estados:

- Recopilando 
- No se está recopilando 
- Recopilando con errores 
 
Haga clic en **Estado** para explorar en profundidad y ver registros y más detalles que tal vez puedan servirle para resolver el problema. 

:::image type="content" source="media/sql-insights-enable/monitoring-machine-status.png" alt-text="Supervisión del estado de la máquina":::

## <a name="not-collecting-state"></a>Estado "No se está recopilando" 
La máquina de supervisión muestra un estado *No se está recopilando* si no hay ningún dato en *InsightsMetrics* para SQL en los últimos 10 minutos. 

SQL Insights usa la siguiente consulta para recuperar esta información:

```
InsightsMetrics 
    | extend Tags = todynamic(Tags) 
    | extend SqlInstance = tostring(Tags.sql_instance) 
    | where TimeGenerated > ago(10m) and isnotempty(SqlInstance) and Namespace == 'sqlserver_server_properties' and Name == 'uptime' 
```

Compruebe si hay registros de Telegraf que ayuden a identificar la causa principal del problema. Si hay entradas de registro, puede hacer clic en *No se está recopilando* y consultar los registros y la información para solucionar problemas comunes. 


Si no hay registros, debe comprobar los registros de la máquina virtual de supervisión para buscar los siguientes servicios instalados por dos extensiones de máquina virtual:

- Microsoft.Azure.Monitor.AzureMonitorLinuxAgent 
  - Servicio: mdsd 
- Microsoft.Azure.Monitor.Workloads.Workload.WLILinuxExtension 
  - Servicio: wli 
  - Servicio: ms-telegraf 
  - Servicio: td-agent-bit-wli 
  - Registro de extensión para comprobar los errores de instalación: /var/log/azure/Microsoft.Azure.Monitor.Workloads.Workload.WLILinuxExtension/wlilogs.log 



### <a name="wli-service-logs"></a>Registros del servicio wli 

Registros del servicio: `/var/log/wli.log`

Para ver los registros recientes: `tail -n 100 -f /var/log/wli.log`
 

Si ve el siguiente registro de errores, indica un problema con el servicio **mdsd**.

```
2021-01-27T06:09:28Z [Error] Failed to get config data. Error message: dial unix /var/run/mdsd/default_fluent.socket: connect: no such file or directory 
```


### <a name="telegraf-service-logs"></a>Registros del servicio telegraf 

Registros del servicio: `/var/log/ms-telegraf/telegraf.log`

Para ver los registros recientes: `tail -n 100 -f /var/log/ms-telegraf/telegraf.log` para ver los registros de errores y de advertencias recientes: `tail -n 1000 /var/log/ms-telegraf/telegraf.log | grep "E\!\|W!"`

 La configuración usada por telegraf se genera mediante el servicio wli y se coloca en: `/etc/ms-telegraf/telegraf.d/wli`
 
Si se genera una configuración incorrecta, es posible que no se inicie el servicio ms-telegraf; compruebe si el servicio ms-telegraf se está ejecutando con el comando: `service ms-telegraf status`

Para ver los mensajes de error del servicio telegraf, ejecútelo manualmente con el siguiente comando: 

```
/usr/bin/ms-telegraf --config /etc/ms-telegraf/telegraf.conf --config-directory /etc/ms-telegraf/telegraf.d/wli --test 
```

### <a name="mdsd-service-logs"></a>Registros del servicio mdsd 

Compruebe las [limitaciones actuales](../agents/azure-monitor-agent-overview.md#current-limitations) del agente de Azure Monitor. 


Registros del servicio:  
- `/var/log/mdsd.err`
- `/var/log/mdsd.warn`
- `/var/log/mdsd.info`

Para ver los errores recientes: `tail -n 100 -f /var/log/mdsd.err`

 Recopile la información siguiente si necesita ponerse en contacto con nuestro personal de soporte técnico: 

- Registros en `/var/log/azure/Microsoft.Azure.Monitor.AzureMonitorLinuxAgent/` 
- Registro en `/var/log/waagent.log` 
- Registros en `/var/log/mdsd*`
- Archivos en `/etc/mdsd.d/`
- File `/etc/default/mdsd`

### <a name="invalid-monitoring-virtual-machine-configuration"></a>Configuración de máquina virtual de supervisión no válida

Una causa del estado *No se está recopilando* es la existencia de una configuración de máquina virtual de supervisión no válida.  A continuación puede ver la configuración predeterminada:

```json
{
    "version": 1,
    "secrets": {
        "telegrafPassword": {
            "keyvault": "https://mykeyvault.vault.azure.net/",
            "name": "sqlPassword"
        }
    },
    "parameters": {
        "sqlAzureConnections": [
            "Server=mysqlserver.database.windows.net;Port=1433;Database=mydatabase;User Id=telegraf;Password=$telegrafPassword;"
        ],
        "sqlVmConnections": [
        ],
        "sqlManagedInstanceConnections": [
        ]
    }
}
```

Esta configuración especifica los tokens de reemplazo que se usarán en la configuración del perfil en la máquina virtual de supervisión. También le permite hacer referencia a secretos desde Azure Key Vault y evitar así tener valores de secreto en ninguna configuración, tal y como se recomienda encarecidamente.

#### <a name="secrets"></a>Secretos
Los secretos son tokens cuyos valores se recuperan en tiempo de ejecución desde Azure Key Vault. Un secreto se define mediante un par compuesto por una referencia de Key Vault y un nombre de secreto. Esto permite a Azure Monitor obtener el valor dinámico del secreto y usarlo como referencias de configuración de bajada.

Puede definir tantos secretos como sean necesarios en la configuración, incluidos los secretos guardados en almacenes Key Vault independientes.

```json
   "secrets": {
        "<secret-token-name-1>": {
            "keyvault": "<key-vault-uri>",
            "name": "<key-vault-secret-name>"
        },
        "<secret-token-name-2>": {
            "keyvault": "<key-vault-uri-2>",
            "name": "<key-vault-secret-name-2>"
        }
    }
```

Los permisos para tener acceso al Key Vault se proporcionan a un Managed Service Identity en la máquina virtual de supervisión. Azure Monitor espera que el Key Vault proporcione al menos el permiso de obtención de secretos a la máquina virtual. Puede activarlo en Azure Portal, en PowerShell, en la CLI o mediante plantillas de Resource Manager.

#### <a name="parameters"></a>Parámetros
Los parámetros son tokens a los que se puede hacer referencia en la configuración del perfil a través de plantillas JSON. Los parámetros tienen un nombre y un valor. Los valores pueden ser cualquier tipo JSON, incluidos objetos y matrices. Se hace referencia a un parámetro en la configuración del perfil mediante su nombre y con esta convención `.Parameters.<name>`.

Los parámetros pueden hacer referencia a secretos en Key Vault con la misma convención. Por ejemplo, `sqlAzureConnections` hace referencia al secreto `telegrafPassword` mediante la convención `$telegrafPassword`.

En tiempo de ejecución, todos los parámetros y secretos se resolverán y combinarán con la configuración del perfil para construir la configuración real que se va a usar en la máquina.

> [!NOTE]
> Los nombres de parámetro de `sqlAzureConnections`, `sqlVmConnections` y `sqlManagedInstanceConnections` son necesarios en la configuración aunque no tenga cadenas de conexión que vaya a proporcionar para algunos de ellos.


## <a name="collecting-with-errors-state"></a>Estado "Recopilando con errores"
La máquina de supervisión mostrará el estado *Recopilando con errores* si hay al menos un registro de *InsightsMetrics*, pero también errores en la tabla de *operaciones*.

SQL Insights usa las siguientes consultas para recuperar esta información:

```
InsightsMetrics 
    | extend Tags = todynamic(Tags) 
    | extend SqlInstance = tostring(Tags.sql_instance) 
    | where TimeGenerated > ago(240m) and isnotempty(SqlInstance) and Namespace == 'sqlserver_server_properties' and Name == 'uptime' 
```

```
Operation 
 | where OperationCategory == "WorkloadInsights" 
 | summarize Errors = countif(OperationStatus == 'Error') 
```

En casos comunes, se proporciona información de solución de problemas en nuestra vista de registros: 

:::image type="content" source="media/sql-insights-enable/troubleshooting-logs-view.png" alt-text="Vista de registros de solución de problemas.":::



## <a name="next-steps"></a>Pasos siguientes

- Obtenga detalles sobre [Cómo habilitar SQL Insights](sql-insights-enable.md).
