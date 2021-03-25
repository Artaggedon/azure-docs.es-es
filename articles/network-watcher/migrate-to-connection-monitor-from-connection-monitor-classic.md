---
title: Migración a Connection Monitor desde Connection Monitor
titleSuffix: Azure Network Watcher
description: Aprenda a migrar a Connection Monitor desde Connection Monitor.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: vinigam
ms.openlocfilehash: d4ab5361d245ad1ee10d43184cc0a2d65fed2054
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101730038"
---
# <a name="migrate-to-connection-monitor-from-connection-monitor-classic"></a>Migración a Connection Monitor desde Connection Monitor (clásico)

> [!IMPORTANT]
> A partir del 1 de julio de 2021, no podrá agregar nuevos monitores de conexión en Connection Monitor (clásico), pero puede seguir usando los monitores de conexión existentes creados antes del 1 de julio de 2021. Para minimizar la interrupción del servicio en las cargas de trabajo actuales, [migre desde Connection Monitor (clásico) a la nueva versión de Connection Monitor](migrate-to-connection-monitor-from-connection-monitor-classic.md) de Azure Network Watcher antes del 29 de febrero de 2024.

Puede migrar los monitores de conexión existentes a la nueva y mejorada característica Connection Monitor con un solo clic y sin tiempo de inactividad. Para más información sobre las ventajas, consulte [Connection Monitor](./connection-monitor-overview.md).

## <a name="key-points-to-note"></a>Puntos clave a tener en cuenta

La migración ayuda a generar los siguientes resultados:

* Los agentes y la configuración del firewall funcionan tal cual. No es preciso realizar cambios. 
* Los monitores de conexión existentes se asignan a Connection Monitor -> Grupo de prueba -> Test format (Formato de prueba). Al hacer clic en **Editar**, puede ver y modificar las propiedades de la nueva característica Connection Monitor, descargar una plantilla para realizar cambios en ella y enviar la plantilla a través de Azure Resource Manager. 
* Las máquinas virtuales de Azure con la extensión Network Watcher envían datos tanto al área de trabajo como a las métricas. Connection Monitor permite que los datos estén disponibles mediante las nuevas métricas (ChecksFailedPercent y RoundTripTimeMs) en lugar de las anteriores (ProbesFailedPercent y AverageRoundtripMs). Las métricas anteriores se migrarán a las nuevas métricas como ProbesFailedPercent -> ChecksFailedPercent y AverageRoundtripMs -> RoundTripTimeMs.
* Supervisión de datos:
   * **Alertas**: se migran automáticamente a las nuevas métricas.
   * **Paneles e integraciones**: se requiere la edición manual del conjunto de métricas. 
    
## <a name="prerequisites"></a>Requisitos previos

Si usa un área de trabajo personalizada, asegúrese de que Network Watcher esté habilitado en la suscripción y en la región del área de trabajo de Log Analytics. 

## <a name="migrate-the-connection-monitors"></a>Migración de los monitores de conexión

1. Para migrar los monitores de conexión más antiguos a la versión más reciente, seleccione **Connection Monitor** y, luego, elija **Migrate Connection Monitors** (Migrar los monitores de conexión).

    ![Captura de pantalla que muestra la migración de los monitores de conexión a Connection Monitor](./media/connection-monitor-2-preview/migrate-cm-to-cm-preview.png)
    
1. Seleccione la suscripción y los monitores de conexión que quiere migrar y, luego, elija **Migrate selected** (Migrar seleccionados). 

Con solo unos clics, ha migrado los monitores de conexión existentes a Connection Monitor. Después de la migración de CM (clásico) a CM, no podrá ver el monitor en CM (clásico).

Ahora puede personalizar las propiedades de Connection Monitor, cambiar el área de trabajo predeterminada, descargar plantillas y comprobar el estado de la migración. 

Una vez iniciada la migración, tienen lugar los siguientes cambios: 
* El recurso de Azure Resource Manager cambia al monitor de conexión más reciente.
    * El nombre, la región y la suscripción del monitor de conexión no cambian. El identificador de recurso permanece igual.
    * A menos que el monitor de conexión se personalice, se crea un área de trabajo de Log Analytics predeterminada en la suscripción y la región del monitor de conexión. En esta área de trabajo se almacenan los datos de supervisión. Los datos del resultado de la prueba también se almacenan en las métricas.
    * Cada prueba se migra a un grupo de pruebas denominado *defaultTestGroup*.
    * Se crean puntos de conexión de origen y destino y se usan en el grupo de pruebas creado. Los nombres predeterminados son *defaultSourceEndpoint* y *defaultDestinationEndpoint*.
    * El puerto de destino y el intervalo de sondeo se trasladan a una configuración de prueba denominada *defaultTestConfiguration*. El protocolo se establece según los valores de puerto. Los umbrales de éxito y otras propiedades opcionales se dejan en blanco.
* Las alertas de métricas se migran a las alertas de métricas de Connection Monitor. Las métricas son distintas, de ahí el cambio. Para más información, consulte [Supervisión de conectividad de red con Connection Monitor](./connection-monitor-overview.md#metrics-in-azure-monitor).
* Los monitores de conexión migrados ya no se muestran como la solución de monitor de conexión anterior. Ahora están disponibles para su uso únicamente en Connection Monitor.
* Cualquier integración externa, como los paneles de Power BI y Grafana, y las integraciones con sistemas de Administración de eventos e información de seguridad (SIEM), se debe migrar manualmente. Este es el único paso manual que debe realizar para migrar la instalación.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Connection Monitor, consulte:
* [Migración de Network Performance Monitor a Connection Monitor](./migrate-to-connection-monitor-from-network-performance-monitor.md)
* [Creación de Connection Monitor mediante Azure Portal](./connection-monitor-create-using-portal.md)
