---
title: Uso de listas de seguimiento de Azure Sentinel
description: En este artículo se describe cómo usar las listas de seguimiento de Azure Sentinel para investigar amenazas, importar datos empresariales, crear listas de permitidos y enriquecer datos de eventos.
services: sentinel
author: yelevin
ms.author: yelevin
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.custom: mvc
ms.date: 09/06/2020
ms.openlocfilehash: e31128687cfcc1f4e32879328ad3227182efb9ce
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98797349"
---
# <a name="use-azure-sentinel-watchlists"></a>Uso de listas de seguimiento de Azure Sentinel

> [!IMPORTANT]
> Esta característica de listas de reproducción está actualmente en **VERSIÓN PRELIMINAR**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

Las listas de seguimiento de Azure Sentinel permiten la recopilación de datos de orígenes de datos externos para la correlación con los eventos en el entorno de Azure Sentinel. Una vez que crea las listas de seguimiento, puede usarlas búsquedas, reglas de detección, búsqueda de amenazas y cuaderno de estrategias de respuesta. Las listas de seguimiento se almacenan en el área de trabajo de Azure Sentinel como pares de nombre-valor, y se almacenan en caché para obtener un rendimiento óptimo de las consultas y una baja latencia.

Algunos escenarios habituales para usar listas de seguimiento son:

- **Investigación de amenazas** y respuesta a los incidentes con una rápida importación de direcciones IP, hashes de archivo y otros datos de archivos CSV. Una vez que se importan, puede usar los pares nombre-valor de las listas de seguimiento para combinaciones y filtros en reglas de alerta, búsqueda de amenazas, libros, cuadernos y consultas generales.

- **Importación de datos comerciales** como listas de seguimiento. Por ejemplo, importe las listas de usuarios con privilegios de acceso del sistema, o empleados despedidos, y, a continuación, use la lista de seguimiento para crear listas de permitidos y de denegados, que se usan para detectar o impedir que los usuarios inicien sesión en la red.

- **Reducción de la fatiga por alerta**. Cree listas de permitidos para suprimir las alertas de un grupo de usuarios, como usuarios de direcciones IP autorizadas que realizan tareas que normalmente desencadenarían la alerta, y evitar que eventos benignos se conviertan en alertas.

- **Enriquecimiento de datos de eventos**. Use listas de seguimiento para enriquecer los datos de eventos con combinaciones de nombre-valor obtenidas de orígenes de datos externos.

## <a name="create-a-new-watchlist"></a>Creación de una nueva lista de seguimiento

1. En Azure Portal, vaya a **Azure Sentinel** > **Configuración** > **Watchlist** (Lista de seguimiento) y, a continuación, seleccione **Agregar nuevo**.

    > [!div class="mx-imgBorder"]
    > ![nueva lista de seguimiento](./media/watchlists/sentinel-watchlist-new.png)

1. En la página **General**, proporcione el nombre, la descripción y el alias de la lista de seguimiento y, a continuación, seleccione **Siguiente**.

    > [!div class="mx-imgBorder"]
    > ![página general de la lista de seguimiento](./media/watchlists/sentinel-watchlist-general.png)

1. En la página **Origen**, seleccione el tipo de conjunto de archivos, cargue un archivo y, a continuación, seleccione **Siguiente**.

    :::image type="content" source="./media/watchlists/sentinel-watchlist-source.png" alt-text="página de origen de la lista de seguimiento" lightbox="./media/watchlists/sentinel-watchlist-source.png":::

    > [!NOTE]
    >
    > Las cargas de archivos están limitadas actualmente a archivos de hasta 3,8 MB de tamaño.

1. Revise la información, compruebe que sea correcta y, después, seleccione **Crear**.

    > [!div class="mx-imgBorder"]
    > ![página de revisión de la lista de seguimiento](./media/watchlists/sentinel-watchlist-review.png)

    Una vez que se crea la lista de seguimiento, aparece una notificación.

    :::image type="content" source="./media/watchlists/sentinel-watchlist-complete.png" alt-text="notificación de creación correcta de la lista de seguimiento" lightbox="./media/watchlists/sentinel-watchlist-complete.png":::

## <a name="use-watchlists-in-queries"></a>Uso de listas de seguimiento en consultas

1. En Azure Portal, vaya a **Azure Sentinel** > **Configuration** > **Watchlist** (Lista de seguimiento), seleccione la lista de seguimiento que quiere usar y, a continuación, seleccione **Ver en Log Analytics**.

    :::image type="content" source="./media/watchlists/sentinel-watchlist-queries-list.png" alt-text="uso de listas de seguimiento en consultas" lightbox="./media/watchlists/sentinel-watchlist-queries-list.png":::

1. Los elementos de la lista de seguimiento se extraen automáticamente para la consulta y aparecerán en la pestaña **Resultados**. En el ejemplo siguiente se muestran los resultados de la extracción de los campos **ServerName** y **IpAddress**.

    > [!NOTE]
    > La marca de tiempo en las consultas se omitirá tanto en la UI de la consulta como en las alertas programadas.

    :::image type="content" source="./media/watchlists/sentinel-watchlist-queries-fields.png" alt-text="consultas con campos de la lista de seguimiento" lightbox="./media/watchlists/sentinel-watchlist-queries-fields.png":::
    
1. Puede consultar los datos de cualquier tabla comparándolos con los datos de una lista de reproducción tratando a esta última como una tabla de combinaciones y búsquedas.

    ```kusto
    Heartbeat
    | lookup kind=leftouter _GetWatchlist('IPlist') 
     on $left.ComputerIP == $right.IPAddress
    ```
    :::image type="content" source="./media/watchlists/sentinel-watchlist-queries-join.png" alt-text="consultas en comparación con listas de reproducción como búsqueda":::

## <a name="use-watchlists-in-analytics-rules"></a>Uso de listas de seguimiento en reglas de análisis

Para usar listas de seguimiento en reglas de análisis, desde el Azure Portal, vaya a **Azure Sentinel** > **Configuración** > **Análisis** y cree una regla con la función `_GetWatchlist('<watchlist>')` en la consulta.

1. En este ejemplo, cree una lista de reproducción denominada "ipwatchlist" con los valores siguientes:

    :::image type="content" source="./media/watchlists/create-watchlist.png" alt-text="lista de cuatro elementos de la lista de reproducción":::

    :::image type="content" source="./media/watchlists/sentinel-watchlist-new-2.png" alt-text="crear lista de reproducción con cuatro elementos":::

1. A continuación, cree la regla de análisis.  En este ejemplo, solo se incluyen eventos de direcciones IP en la lista de reproducción:

    ```kusto
    //Watchlist as a variable
    let watchlist = (_GetWatchlist('ipwatchlist') | project IPAddress);
    Heartbeat
    | where ComputerIP in (watchlist)
    ```
    ```kusto
    //Watchlist inline with the query
    Heartbeat
    | where ComputerIP in ( 
        (_GetWatchlist('ipwatchlist')
        | project IPAddress)
    )
    ```

:::image type="content" source="./media/watchlists/sentinel-watchlist-analytics-rule-2.png" alt-text="uso de listas de seguimiento en reglas de análisis":::

## <a name="view-list-of-watchlists-aliases"></a>Visualización de la lista de alias de listas de seguimiento

Para obtener una lista de los alias de las listas de seguimiento, en Azure Portal, vaya a **Azure Sentinel** > **General** > **Registros** y ejecute la consulta siguiente: `_GetWatchlistAlias`. Puede ver la lista de alias en la pestaña **Resultados**.

> [!div class="mx-imgBorder"]
> ![enumeración de listas de seguimiento](./media/watchlists/sentinel-watchlist-alias.png)

## <a name="next-steps"></a>Pasos siguientes
En este documento, aprendió a usar las listas de seguimiento en Azure Sentinel para enriquecer los datos y mejorar las investigaciones. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](./tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.