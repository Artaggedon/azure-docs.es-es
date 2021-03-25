---
title: 'Recomendaciones de rendimiento: Azure Database for MariaDB'
description: En este artículo se describe la característica Recomendaciones de rendimiento en Azure Database for MariaDB.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/3/2020
ms.openlocfilehash: a20b8a43c8955e1afea7a7157e3e73425fb0d806
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98664323"
---
# <a name="performance-recommendations-in-azure-database-for-mariadb"></a>Recomendaciones de rendimiento en Azure Database for MariaDB

**Se aplica a:** Azure Database for MariaDB 10.2

La característica Recomendaciones de rendimiento analiza las bases de datos para crear sugerencias personalizadas para mejorar el rendimiento. Para generar las recomendaciones, el análisis examina diversas características de bases de datos, incluido el esquema. Habilite [Almacén de consultas](concepts-query-store.md) en el servidor para poder usar por completo la característica Recomendaciones de rendimiento. Si el esquema de rendimiento está desactivado, al activarlo en el Almacén de consultas se activarán performance_schema y un subconjunto de los instrumentos del esquema de rendimiento necesarios para la característica. Después de implementar una recomendación de rendimiento, debe probar el rendimiento para evaluar el efecto de estos cambios.

## <a name="permissions"></a>Permisos

Los permisos **Propietario** o **Colaborador** necesarios para ejecutar el análisis con la característica Recomendaciones de rendimiento.

## <a name="performance-recommendations"></a>Recomendaciones de rendimiento

La característica [Recomendaciones de rendimiento](concepts-performance-recommendations.md) analiza las cargas de trabajo en el servidor para identificar los índices con el potencial de mejorar el rendimiento.

Abra **Recomendaciones de rendimiento** desde la sección **Rendimiento inteligente** de la barra de menús en la página de Azure Portal para el servidor MariaDB.

:::image type="content" source="./media/concepts-performance-recommendations/performance-recommendations-page.png" alt-text="Página de inicio de Recomendaciones de rendimiento":::

Seleccione **Analizar** y elija una base de datos, lo que iniciará el análisis. Según la carga de trabajo, el análisis puede tardar varios minutos. Una vez que se realice el análisis, habrá una notificación en el portal. El análisis realiza un examen en profundidad de la base de datos. Se recomienda realizar el análisis durante los períodos de poca actividad.

La ventana **Recomendaciones** ahora mostrará una lista de las recomendaciones que se encontraron y el identificador de la consulta relacionada que generó cada recomendación. Con el identificador de la consulta, puede usar la vista [mysql.query_store](concepts-query-store.md#mysqlquery_store) para obtener más información sobre la consulta.

:::image type="content" source="./media/concepts-performance-recommendations/performance-recommendations-result.png" alt-text="Nueva página de Recomendaciones de rendimiento":::

Las recomendaciones no se aplican automáticamente. Para aplicar la recomendación, copie el texto de la consulta y ejecútelo desde el cliente que prefiera. No olvide probar y supervisar para evaluar la recomendación.

## <a name="recommendation-types"></a>Tipos de recomendaciones

### <a name="index-recommendations"></a>Recomendaciones de índices

Las recomendaciones de *Crear índice* sugieren nuevos índices para acelerar las consultas que se ejecutan con mayor frecuencia o que consumen mucho tiempo en la carga de trabajo. Este tipo de recomendación requiere que [Almacén de consultas](concepts-query-store.md) esté habilitado. Almacén de consultas recopila información de consultas y proporciona estadísticas detalladas de tiempo de ejecución y frecuencia de las consultas, que el análisis usa para hacer la recomendación.

### <a name="query-recommendations"></a>Recomendaciones de consulta

Las recomendaciones de consulta sugieren optimizaciones y reescrituras para las consultas de la carga de trabajo. Mediante la identificación de los antipatrones de consulta MariaDB y su corrección sintáctica, se puede mejorar el rendimiento de las consultas que tardan mucho tiempo. Este tipo de recomendación requiere que Almacén de consultas esté habilitado. Almacén de consultas recopila información de consultas y proporciona estadísticas detalladas de tiempo de ejecución y frecuencia de las consultas, que el análisis usa para hacer la recomendación.
## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [supervisión y el ajuste](concepts-monitoring.md) en Azure Database for MariaDB.