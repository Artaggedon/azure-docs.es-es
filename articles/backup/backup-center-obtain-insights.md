---
title: Obtención de conclusiones mediante el Centro de copias de seguridad
description: Obtenga información sobre cómo analizar las tendencias históricas y sacar conclusiones más profundas sobre las copias de seguridad con el Centro de copias de seguridad.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: c5f707ce3bc8d1172805ac14958a50bf59d1412d
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98893700"
---
# <a name="obtain-insights-using-backup-center-preview"></a>Obtención de conclusiones mediante el Centro de copias de seguridad (versión preliminar)

Para analizar las tendencias históricas y obtener información más detallada sobre las copias de seguridad, el Centro de copias de seguridad proporciona una interfaz para [Informes de Backup (versión preliminar)](configure-reports.md), que usa [Registros de Azure Monitor](../azure-monitor/platform/data-platform-logs.md) y [Libros de Azure](../azure-monitor/platform/workbooks-overview.md). Informes de Backup ofrece las siguientes funcionalidades:

- Asignación y previsión del almacenamiento en la nube consumido.

- Auditoría de copias de seguridad y restauraciones.

- Identificación de las tendencias clave con diferentes niveles de granularidad.

- Obtención de visibilidad y conclusiones sobre las oportunidades de optimización de costos para las copias de seguridad.

## <a name="supported-scenarios"></a>Escenarios admitidos

- Informes de Backup (versión preliminar) no está disponible actualmente para copias de seguridad del servidor de Azure Database for PostgreSQL.

- Consulte la [matriz de compatibilidad](backup-center-support-matrix.md) para obtener una lista detallada de escenarios admitidos y no admitidos.

## <a name="get-started"></a>Introducción

### <a name="configure-your-vaults-to-send-data-to-a-log-analytics-workspace"></a>Configuración de almacenes para enviar datos a un área de trabajo de Log Analytics

[Obtenga información sobre cómo configurar los valores de diagnóstico a gran escala para los almacenes](./configure-reports.md#get-started)

### <a name="view-backup-reports-in-the-backup-center-portal"></a>Visualización de Informes de Backup en el portal del Centro de copias de seguridad

Al seleccionar el elemento de menú **Informes de Backup** en el Centro de copias de seguridad, se abren los informes. Elija una o varias áreas de trabajo de Log Analytics para ver y analizar la información clave de las copias de seguridad.

![Informes de Backup en el Centro de copias de seguridad](./media/backup-center-obtain-insights/backup-center-backup-reports.png)

A continuación se muestran las vistas disponibles:

1. **Resumen**: use esta pestaña para obtener información general sobre el conjunto de copias de seguridad. [Más información](./configure-reports.md#summary)

1. **Elementos de copia de seguridad**: use esta pestaña para ver información y tendencias sobre el almacenamiento en la nube consumido en el nivel de elemento de Backup. [Más información](./configure-reports.md#backup-items)

1. **Uso**: use esta pestaña para ver los parámetros de facturación principales de las copias de seguridad. [Más información](./configure-reports.md#usage)

1. **Trabajos**: use esta pestaña para ver tendencias de ejecución prolongada de los trabajos, como el número de trabajos con errores por día y las causas principales de los errores de los trabajos. [Más información](./configure-reports.md#jobs)

1. **Directivas**: use esta pestaña para ver información sobre todas las directivas activas, como el número de elementos asociados y el almacenamiento en la nube total consumido por los elementos de los que se ha realizado una copia de seguridad por una directiva determinada. [Más información](./configure-reports.md#policies)

1. **Optimizar**: use esta pestaña para obtener visibilidad sobre las posibles oportunidades de optimización de costos para las copias de seguridad. [Más información](./configure-reports.md#optimize)

## <a name="next-steps"></a>Pasos siguientes

- [Supervisión y funcionamiento de las copias de seguridad](backup-center-monitor-operate.md)
- [Control del conjunto de copias de seguridad](backup-center-govern-environment.md)
- [Acciones con el Centro de copias de seguridad](backup-center-actions.md)