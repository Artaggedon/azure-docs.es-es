---
title: Solución de problemas de Change Tracking e Inventario de Azure Automation
description: En este artículo se trata cómo solucionar problemas con la característica Change Tracking e Inventario de Azure Automation.
services: automation
ms.subservice: change-inventory-management
ms.date: 01/31/2019
ms.topic: troubleshooting
ms.openlocfilehash: 516f1a4e5e7c677b17a2941ee3c300db44d49a3b
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98896552"
---
# <a name="troubleshoot-change-tracking-and-inventory-issues"></a>Solución de problemas de Change Tracking e Inventario

En este artículo se describe cómo diagnosticar y solucionar problemas de Change Tracking e Inventario de Azure Automation. Para obtener información general de Change Tracking e Inventario, consulte [Información general de Change Tracking e Inventario](../change-tracking/overview.md).

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-and-inventory-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Escenario: no se muestran los registros de Change Tracking e Inventario para máquinas Windows

#### <a name="issue"></a>Problema

No aparecen los resultados de Change Tracking e Inventario para las máquinas Windows donde se ha habilitado la característica.

#### <a name="cause"></a>Causa

Este error puede tener las causas siguientes:

* El agente de Azure Log Analytics para Windows no se está ejecutando.
* Está bloqueada la comunicación a la cuenta de Automation.
* No se han descargado los módulos de administración de Change Tracking e Inventario.
* La VM que se quiere habilitar puede provenir de una máquina clonada que no se haya preparado mediante la preparación del sistema (sysprep) con el agente de Log Analytics para Windows instalado.

#### <a name="resolution"></a>Solución

En la máquina del agente de Log Analytics, vaya a **C:\Archivos de programa\Microsoft Monitoring Agent\Agent\Tools** y ejecute los siguientes comandos:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

Si sigue necesitando ayuda, puede recopilar la información de diagnóstico y ponerse en contacto con el equipo de soporte técnico.

> [!NOTE]
> El agente de Log Analytics habilita el seguimiento de errores de forma predeterminada. Para habilitar los mensajes de error detallados como en el ejemplo anterior, use el parámetro `VER`. Para más información sobre los seguimientos, use `INF` cuando invoque `StartTracing.cmd`.

##### <a name="log-analytics-agent-for-windows-not-running"></a>El agente de Log Analytics para Windows no se está ejecutando

Compruebe que el agente de Log Analytics para Windows (**HealthService.exe**) está en ejecución en la máquina.

##### <a name="communication-to-automation-account-blocked"></a>Bloqueada la comunicación con la cuenta de Automation

Consulte el Visor de eventos en el equipo y busque cualquier evento que contenga la palabra `changetracking`.

Para conocer las direcciones y puertos que deben estar permitidos para que Change Tracking e Inventario funcione, consulte la sección sobre [planeamiento de la red](../automation-hybrid-runbook-worker.md#network-planning).

##### <a name="management-packs-not-downloaded"></a>Módulos de administración no descargados

Compruebe que los siguientes módulos de administración de Change Tracking e Inventario están instalados en el entorno local:

* `Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*`
* `Microsoft.IntelligencePacks.InventoryChangeTracking.*`
* `Microsoft.IntelligencePacks.SingletonInventoryCollection.*`

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Máquina virtual a partir de una máquina clonada que no se ha preparado con sysprep

Si utiliza una imagen clonada, primero prepare con sysprep la imagen y, a continuación, instale el agente de Log Analytics para Windows.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-and-inventory-results-on-linux-machines"></a>Escenario: No hay resultados de Change Tracking e Inventario en las máquinas Linux

#### <a name="issue"></a>Problema

No ve los resultados de Change Tracking e Inventario de las máquinas Linux que tienen habilitada la característica. 

#### <a name="cause"></a>Causa
Estas son algunas causas posibles específicas de este problema:
* El agente de Log Analytics para Linux no está en ejecución.
* El agente de Log Analytics para Linux no está configurado correctamente.
* Hay conflictos de supervisión de la integridad de los archivos (FIM).

#### <a name="resolution"></a>Solución 

##### <a name="log-analytics-agent-for-linux-not-running"></a>El agente de Log Analytics para Linux no está en ejecución

Compruebe que el demonio del agente de Log Analytics para Linux (**omsagent**) está en ejecución en la máquina. Ejecute la siguiente consulta en el área de trabajo de Log Analytics que está vinculada a la cuenta de Automation.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Si no ve la máquina en los resultados de la consulta, no se ha registrado recientemente. Probablemente haya un problema de configuración local y debe volver a instalar el agente. Para obtener información sobre la instalación y configuración, consulte [Recopilación de datos de registro con el agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md).

Si el equipo aparece en los resultados de la consulta, compruebe la configuración de ámbito. Consulte [Soluciones de supervisión como destino en Azure Monitor](../../azure-monitor/insights/solution-targeting.md).

Para más información sobre cómo solucionar este problema, consulte [Problema: no se ve ningún dato de Linux](../../azure-monitor/platform/agent-linux-troubleshoot.md#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>El agente de Log Analytics para Linux no está configurado correctamente

Es posible que el agente de Log Analytics para Linux no esté correctamente configurado para el registro y la recopilación de la salida de la línea de comandos mediante la herramienta Recopilador de registros de OMS. Consulte [Información general de Change Tracking e Inventario](../change-tracking/overview.md).

##### <a name="fim-conflicts"></a>Conflictos de FIM

La característica FIM de Azure Security Center podría estar validando incorrectamente la integridad de los archivos de Linux. Compruebe que FIM está operativo y configurado correctamente para la supervisión de archivos de Linux. Consulte [Información general de Change Tracking e Inventario](../change-tracking/overview.md).

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece aquí o no puede resolverlo, intente obtener ayuda adicional mediante uno de los siguientes canales:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. El Soporte técnico de Azure pone en contacto a la comunidad de Azure con respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de Soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte técnico**.
