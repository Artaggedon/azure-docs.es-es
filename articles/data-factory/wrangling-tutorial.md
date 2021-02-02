---
title: Introducción al flujo de datos de limpieza y transformación en Azure Data Factory
description: Tutorial sobre cómo preparar los datos en Azure Data Factory mediante el flujo de datos de limpieza y transformación
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/19/2021
ms.openlocfilehash: 3d43a532f57feab361c6a3de79269991f46fc55d
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98684029"
---
# <a name="prepare-data-with-data-wrangling"></a>Preparación de los datos con una limpieza y transformación de datos

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

La limpieza y transformación de datos en una factoría de datos permite crear mashups de Power Query interactivas de forma nativa en ADF y, después, ejecutarlas a escala dentro de una canalización de ADF.

> [!NOTE]
> La actividad de Power Query en ADF está actualmente disponible en versión preliminar pública.

## <a name="create-a-power-query-activity"></a>Creación de una actividad de Power Query

Hay dos maneras de crear una Power Query en Azure Data Factory. Una forma consiste en hacer clic en el icono del signo más y seleccionar **Flujo de datos** en el panel de recursos de Factory.

> [!NOTE]
> Anteriormente, la característica de limpieza y transformación de datos se encontraba en el flujo de trabajo del flujo de datos. Ahora, creará el mashup de limpieza y transformación de datos a partir de ```New > Power query```

![Captura de pantalla que muestra Power Query en el panel de recursos de fábrica.](media/data-flow/power-query-wrangling.png)

El otro método está en el panel de actividades del lienzo de la canalización. Abra el acordeón **Power Query** y arrastre la actividad **Power Query** al lienzo.

![Captura de pantalla que resalta la opción de limpieza y transformación de datos.](media/data-flow/power-query-activity.png)

## <a name="author-a-power-query-data-wrangling-activity"></a>Creación de una actividad de limpieza y transformación de datos de Power Query

Agregue un **Conjunto de datos de origen** para el mashup de Power Query. Puede elegir un conjunto de datos existente o crear uno. También puede seleccionar un conjunto de datos de receptores. Puede elegir uno o más conjuntos de datos de origen, pero en este momento solo se permite un receptor. Elegir un conjunto de datos de receptor es opcional, pero se requiere al menos un conjunto de datos de origen.

![Limpieza y transformación](media/wrangling-data-flow/tutorial4.png)

Haga clic en **Crear** para abrir el editor de mashup Power Query Online.

![Captura de pantalla que muestra el botón Crear que abre el editor de mashup de Power Query Online.](media/wrangling-data-flow/tutorial5.png)

Cree la Power Query de limpieza y transformación con la preparación de datos sin código. Para ver la lista de funciones disponibles, consulte las [funciones de transformación](wrangling-functions.md). ADF traduce el script M en un script de flujo de datos para que pueda ejecutar Power Query a escala mediante el entorno Spark de flujo de datos de Azure Data Factory.

![Captura de pantalla que muestra el proceso para creación de Power Query de limpieza y transformación de datos.](media/wrangling-data-flow/tutorial6.png)

## <a name="running-and-monitoring-a-power-query-data-wrangling-activity"></a>Ejecución y supervisión de una actividad de limpieza y transformación de datos de Power Query

Para una ejecución de depuración de canalización de una actividad de Power Query, haga clic en **Depurar** en el lienzo de la canalización. Una vez publicada la canalización, **Trigger now** (Desencadenar ahora) ejecuta a petición la última canalización publicada. Las canalizaciones de Power Query se pueden programar con todos los desencadenadores de Azure Data Factory existentes.

![Captura de pantalla que muestra cómo agregar una actividad de limpieza y transformación de datos de Power Query.](media/wrangling-data-flow/tutorial3.png)

Vaya a la pestaña **Supervisión** para visualizar la salida de una ejecución de actividad de Power Query desencadenada.

![Captura de pantalla que muestra la salida de una ejecución de actividad de limpieza y transformación de datos de Power Query desencadenada.](media/wrangling-data-flow/tutorial2.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [crear un flujo de datos de asignación](tutorial-data-flow.md).
