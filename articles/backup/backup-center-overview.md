---
title: Información general del Centro de copias de seguridad
description: En este artículo se proporciona información general sobre el Centro de copias de seguridad para Azure.
ms.topic: conceptual
ms.date: 09/30/2020
ms.openlocfilehash: 93f9e5dc064d264e0cc111d8ddb093639307c55b
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100592032"
---
# <a name="overview-of-backup-center-preview"></a>Información general del Centro de copias de seguridad (versión preliminar)

El Centro de copias de seguridad proporciona una **experiencia única de administración unificada** en Azure para que las empresas controlen, supervisen, operen y analicen las copias de seguridad a gran escala. Como tal, es coherente con las experiencias de administración nativas de Azure.

Entre algunas de las ventajas principales del Centro de copias de seguridad se incluyen:

* **Un solo panel de visualización para administrar las copias de seguridad**: el Centro de copias de seguridad está diseñado para funcionar bien en un entorno de Azure grande y distribuido. Puede usar el Centro de copias de seguridad para administrar de forma eficaz las copias de seguridad que abarcan varios tipos de cargas de trabajo, almacenes, suscripciones, regiones e inquilinos de [Azure Lighthouse](../lighthouse/overview.md).
* **Administración centrada en el origen de datos**: el Centro de copias de seguridad proporciona vistas y filtros que se centran en los orígenes de datos de los que se realiza la copia de seguridad (por ejemplo, VM y bases de datos). Esto permite que un propietario de recursos o un administrador de copias de seguridad supervisen y operen las copias de seguridad de los elementos sin necesidad de centrarse en el almacén en el que se realiza la copia de seguridad de un elemento. Una característica principal de este diseño es la capacidad de filtrar las vistas por propiedades específicas del origen de datos, como la suscripción del origen de datos, el grupo de recursos del origen de datos y las etiquetas del origen de datos. Por ejemplo, si su organización sigue una práctica de asignar distintas etiquetas a las VM que pertenecen a diferentes departamentos, puede usar el Centro de copias de seguridad para filtrar la información de copia de seguridad en función de las etiquetas de las VM subyacentes de las que se hace una copia de seguridad sin necesidad de centrarse en la etiqueta del almacén.
* **Experiencias conectadas**: el Centro de copias de seguridad proporciona integraciones nativas a los servicios de Azure existentes que permiten la administración a gran escala. Por ejemplo, el Centro de copias de seguridad usa la experiencia de [Azure Policy](../governance/policy/overview.md) para ayudarle a controlar las copias de seguridad. También aprovecha los [libros de Azure](../azure-monitor/visualize/workbooks-overview.md) y los [registros de Azure Monitor](../azure-monitor/logs/data-platform-logs.md) para ayudarle a ver informes detallados sobre las copias de seguridad. Por lo tanto, no es necesario conocer los nuevos principios para usar las diversas características que ofrece el Centro de copias de seguridad.

## <a name="supported-scenarios"></a>Escenarios admitidos

* El Centro de copias de seguridad admite actualmente copias de seguridad de máquinas virtuales de Azure, SQL en máquinas virtuales de Azure, SAP HANA en máquinas virtuales de Azure, Azure Files y servidor de Azure Database for PostgreSQL.
* Consulte la [matriz de compatibilidad](backup-center-support-matrix.md) para obtener una lista detallada de escenarios admitidos y no admitidos.

## <a name="get-started"></a>Introducción

Para empezar a usar el Centro de copias de seguridad, busque **Centro de copias de seguridad** en Azure Portal y desplácese hasta el panel **Centro de copias de seguridad (versión preliminar**).

![Búsqueda del Centro de copias de seguridad](./media/backup-center-overview/backup-center-search.png)

La primera pantalla que se ve es **Información general**. Contiene dos mosaicos: **Trabajos** e **Instancias de Backup**.

![Mosaicos del Centro de copias de seguridad](./media/backup-center-overview/backup-center-overview-widgets.png)

En el mosaico **Trabajos**, se obtiene una vista resumida de todos los trabajos relacionados con copias de seguridad y restauraciones, que se desencadenaron en el conjunto de copias de seguridad en las últimas 24 horas. Puede ver información sobre el número de trabajos completados, con errores y en curso. La selección de cualquiera de los números en este mosaico le permite ver más información sobre los trabajos de un tipo de origen de datos, tipo de operación y estado en particular.

En el mosaico **Instancias de Backup**, se obtiene una vista resumida de todas las instancias de copia de seguridad en todo el conjunto de copias de seguridad. Por ejemplo, puede ver el número de instancias de copia de seguridad que se encuentran en estado de eliminación temporal en comparación con el número de instancias que todavía están configuradas para protección. La selección de cualquiera de los números en este mosaico le permite ver más información sobre las instancias de copia de seguridad para un tipo de origen de datos y estado de protección en particular.

Vea el siguiente vídeo para comprender las funcionalidades del Centro de copias de seguridad:

> [!VIDEO https://www.youtube.com/embed/pFRMBSXZcUk?t=497]

Realice los [pasos siguientes](#next-steps) para conocer las distintas funcionalidades que proporciona el Centro de copias de seguridad y cómo puede usarlas para administrar de forma eficaz su conjunto de copias de seguridad.

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión y funcionamiento de las copias de seguridad](backup-center-monitor-operate.md)
* [Control del conjunto de copias de seguridad](backup-center-govern-environment.md)
* [Obtención de conclusiones sobre las copias de seguridad](backup-center-obtain-insights.md)
* [Acciones con el Centro de copias de seguridad](backup-center-actions.md)