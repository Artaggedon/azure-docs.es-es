---
title: Recopilación de registros de IIS con el agente de Log Analytics en Azure Monitor
description: Internet Information Services (IIS) almacena la actividad de usuario en archivos de registro que Azure Monitor puede recopilar.  En este artículo se describe cómo configurar la recopilación de registros de IIS y detalles de los registros que crean en Azure Monitor.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/26/2021
ms.openlocfilehash: 87cb19daa23c9fcca601771a9fe168b98be02627
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104586274"
---
# <a name="collect-iis-logs-with-log-analytics-agent-in-azure-monitor"></a>Recopilación de registros de IIS con el agente de Log Analytics en Azure Monitor
Internet Information Services (IIS) almacena la actividad de usuario en archivos de registro que el agente de Log Analytics puede recopilar y almacenar en [Registros de Azure Monitor](../data-platform.md).

> [!IMPORTANT]
> En este artículo se trata la recopilación de registros de IIS con el [agente de Log Analytics](./log-analytics-agent.md), que es uno de los agentes usados por Azure Monitor. Otros agentes recopilan otros datos y se configuran de forma diferente. Consulte [Información general sobre los agentes de Azure Monitor](../agents/agents-overview.md) para obtener una lista de los agentes disponibles y los datos que pueden recopilar.

![Registros IIS](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Configuración de registros de IIS
Azure Monitor recopila entradas de archivos de registro creados por IIS, por lo que debe [configurar IIS para el registro](/previous-versions/orphan-topics/ws.11/hh831775(v=ws.11)).

Azure Monitor solo admite archivos de registro de IIS almacenados en el formato W3C y no admite campos personalizados ni Advanced Logging de IIS. No recopila registros en formato nativo IIS o NCSA.

Configure los registros de IIS en Azure Monitor en el [menú Configuración del agente](../agents/agent-data-sources.md#configuring-data-sources) del agente de Log Analytics.  No se requiere otra configuración que seleccionar **Collect W3C format IIS log files**(Recopilar archivos de registro de IIS en formato W3C).


## <a name="data-collection"></a>datos, recopilación
Azure Monitor recopila las entradas de registro IIS de todos los agentes cada vez que cambia la marca de tiempo del registro. El registro se lee cada **5 minutos**. Si por alguna razón IIS no actualiza la marca de tiempo antes de la hora de sustitución, cuando se crea un archivo, las entradas se recopilarán después de la creación del archivo. La frecuencia de creación de un registro se controla mediante la opción **Log File Rollover Schedule** (Programación de sustitución incremental de archivos de registro) para el sitio IIS, que es una vez al día de forma predeterminada. Si la configuración es **Hourly** (Por hora), Azure Monitor recopila el registro cada hora. Si la configuración es **Daily** (Diario), Azure Monitor recopila el registro cada 24 horas.

> [!IMPORTANT]
> Se recomienda establecer la **Programación de sustitución incremental de archivos** en **Cada hora**. Si se establece en **Diaria**, puede experimentar picos en los datos ya que solo se recopilarán una vez al día.

## <a name="iis-log-record-properties"></a>Propiedades de registro de IIS
Los registros de IIS son del tipo **W3CIISLog** y tienen las propiedades que aparecen en la tabla siguiente:

| Propiedad | Descripción |
|:--- |:--- |
| Computer |Nombre del equipo desde el que se recopiló el evento. |
| cIP |Dirección IP del cliente. |
| csMethod |Método de la solicitud, como GET o POST. |
| csReferer |Sitio cuyo vínculo el usuario siguió desde el sitio actual. |
| csUserAgent |Tipo de explorador del cliente. |
| csUserName |Nombre del usuario autenticado que obtuvo acceso al servidor. Los usuarios anónimos se indican con un guion. |
| csUriStem |El destino de la solicitud, como una página web. |
| csUriQuery |La consulta que el cliente intentaba realizar, si corresponde. |
| ManagementGroupName |Nombre del grupo de administración de agentes de Operations Manager.  En el caso de los otros agentes, es AOI-\<workspace ID\> |
| RemoteIPCountry |País o región de la dirección IP del cliente. |
| RemoteIPLatitude |La latitud de la dirección IP del cliente. |
| RemoteIPLongitude |La longitud de la dirección IP del cliente. |
| scStatus |Código de estado HTTP. |
| scSubStatus |Código de error de subestado. |
| scWin32Status |Código de estado de Windows. |
| sIP |Dirección IP del servidor web. |
| SourceSystem |OpsMgr |
| sPort |Puerto en el servidor al que está conectado el cliente. |
| sSiteName |Nombre del sitio IIS. |
| TimeGenerated |Fecha y hora en que se registró la entrada. |
| TimeTaken |Duración del procesamiento de la solicitud, en milisegundos. |
| csHost | Nombre de host. |
| csBytes | Número de bytes que el servidor recibió. |

## <a name="log-queries-with-iis-logs"></a>Consultas de registros con registros de IIS
La tabla siguiente proporciona ejemplos distintos de consultas de registro que recuperan registros de IIS.

| Consultar | Descripción |
|:--- |:--- |
| W3CIISLog |Todos los registros de IIS. |
| W3CIISLog &#124; where scStatus==500 |Todas las entradas de registro IIS con un estado de retorno de 500. |
| W3CIISLog &#124; summarize count() by cIP |Contador de entradas de registro de IIS por dirección IP del cliente. |
| W3CIISLog &#124; where csHost=="www\.contoso.com" &#124; summarize count() by csUriStem |Número de entradas de registro de IIS por dirección URL para el host www\.contoso.com. |
| W3CIISLog &#124; summarize sum(csBytes) by Computer &#124; take 500000 |Total de bytes recibidos por cada equipo de IIS |

## <a name="next-steps"></a>Pasos siguientes
* Configure Azure Monitor para recopilar otros [orígenes de datos](../agents/agent-data-sources.md) para su análisis.
* Obtenga información acerca de las [consultas de registros](../logs/log-query-overview.md) para analizar los datos recopilados de soluciones y orígenes de datos.
