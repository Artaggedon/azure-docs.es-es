---
title: Configuración de la recopilación de datos para el agente de Azure Monitor (versión preliminar)
description: Se describe cómo crear una regla de recopilación de datos para recopilar datos de máquinas virtuales mediante el agente de Azure Monitor.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/19/2020
ms.openlocfilehash: 93e244706d6d478155ac001d20fa3ce74fa6a887
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101723646"
---
# <a name="configure-data-collection-for-the-azure-monitor-agent-preview"></a>Configuración de la recopilación de datos para el agente de Azure Monitor (versión preliminar)

Las reglas de recopilación de datos (DCR) definen los datos que entran en Azure Monitor y especifican dónde se deben enviar. En este artículo se describe cómo crear una regla de recopilación de datos para recopilar datos de máquinas virtuales mediante el agente de Azure Monitor.

Para obtener una descripción completa de las reglas de recopilación de datos, consulte [Reglas de recopilación de datos en Azure Monitor (versión preliminar)](data-collection-rule-overview.md).

> [!NOTE]
> En este artículo se describe cómo configurar los datos de las máquinas virtuales con el agente de Azure Monitor que se encuentra actualmente en versión preliminar. Consulte [Información general sobre los agentes de Azure Monitor](agents-overview.md) para obtener una descripción de los agentes que están disponibles con carácter general y cómo usarlos para recopilar datos.

## <a name="data-collection-rule-associations"></a>Asociaciones de reglas de recopilación de datos

Para aplicar una reglas de recopilación de datos a una máquina virtual, cree una asociación para la máquina virtual. Una máquina virtual puede tener una asociación a varias reglas de recopilación de datos y una de estas reglas puede tener varias máquinas virtuales asociadas. Esto le permite definir un conjunto de reglas de recopilación de datos, cada una de las cuales coincide con un requisito determinado y aplicarlas solo a las máquinas virtuales en las que se aplican. 

Por ejemplo, imagine un entorno con un conjunto de máquinas virtuales que ejecutan una aplicación de línea de negocio y otras que se ejecutan SQL Server. Es posible que tenga una regla de recopilación de datos predeterminada que se aplique a todas las máquinas virtuales y a reglas de recopilación de datos independientes que recopilen datos específicamente para la aplicación de línea de negocio y SQL Server. Las asociaciones de las máquinas virtuales con las reglas de recopilación de datos tendrían un aspecto similar al siguiente diagrama.

![En el diagrama se muestran las máquinas virtuales que hospedan la aplicación de línea de negocio y SQL Server asociadas a las reglas de recopilación de datos denominadas central-i t-default y lob-app para la aplicación de línea de negocio y central-i t-default y s q l para SQL Server.](media/data-collection-rule-azure-monitor-agent/associations.png)



## <a name="create-rule-and-association-in-azure-portal"></a>Creación de reglas y asociaciones en Azure Portal

Puede usar Azure Portal para crear una regla de recopilación de datos y asociar las máquinas virtuales de su suscripción a esa regla. El agente de Azure Monitor se instalará automáticamente y se creará una identidad administrada para las máquinas virtuales que no lo tengan instalado.

> [!IMPORTANT]
> Actualmente hay un problema conocido por el que, si la regla de recopilación de datos crea una identidad administrada en una máquina virtual que ya tiene una identidad administrada asignada por el usuario, la identidad asignada por el usuario se deshabilita.

En el menú **Azure Monitor** de Azure Portal, seleccione las **reglas de recopilación de datos** en la sección **Configuración**. Haga clic en **Agregar** para agregar una nueva asignación y regla de recopilación de datos.

[![Reglas de recopilación de datos](media/data-collection-rule-azure-monitor-agent/data-collection-rules.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rules.png#lightbox)

Haga clic en **Agregar** para crear una nueva regla y un conjunto de asociaciones. Dé un **nombre de regla** y especifique una **suscripción** y **un grupo de recursos**. Esto especifica dónde se creará la regla de recopilación de datos. Las máquinas virtuales y sus asociaciones pueden estar en cualquier suscripción o grupo de recursos del inquilino.

[![Nombre de la regla de recopilación de datos](media/data-collection-rule-azure-monitor-agent/data-collection-rule-basics.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-basics.png#lightbox)

En la pestaña **Máquinas virtuales**, agregue las máquinas virtuales que deben tener aplicada la regla de recopilación de datos. Deben mostrarse las máquinas virtuales de Azure y los servidores habilitados para Azure Arc en su entorno. El agente de Azure Monitor se instalará en las máquinas virtuales que no lo tengan instalado.

[![Máquinas virtuales de reglas de recopilación de datos](media/data-collection-rule-azure-monitor-agent/data-collection-rule-virtual-machines.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-virtual-machines.png#lightbox)

En la pestaña **Collect and deliver** (Recopilar y entregar), haga clic en **Add data source** (Agregar origen de datos) para agregar un origen de datos y un conjunto de destino. Seleccione un **tipo de origen de datos** y se mostrarán los detalles correspondientes. Para los contadores de rendimiento, puede seleccionar entre un conjunto predefinido de objetos y su frecuencia de muestreo. En el caso de los eventos, puede seleccionar entre un conjunto de registros o instalaciones y el nivel de gravedad. 

[![Origen de datos básico](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-basic.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-basic.png#lightbox)


Para especificar otros registros y contadores de rendimiento de los [orígenes de datos admitidos actualmente](azure-monitor-agent-overview.md#data-sources-and-destinations) o para filtrar eventos mediante consultas XPath, seleccione **Custom** (Personalizado). Después, puede especificar un [XPath](https://www.w3schools.com/xml/xpath_syntax.asp) para los valores específicos que se van a recopilar. Consulte la [regla de recopilación de datos de ejemplo](data-collection-rule-overview.md#sample-data-collection-rule) para ver ejemplos.

[![Origen de datos personalizado](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-custom.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-custom.png#lightbox)

En la pestaña **Destination** (Destino), agregue uno o varios destinos para el origen de datos. Los orígenes de datos de eventos de Windows y Syslog solo pueden enviar a registros de Azure Monitor. Los contadores de rendimiento pueden enviar a métricas de Azure Monitor y registros de Azure Monitor.

[![Destination](media/data-collection-rule-azure-monitor-agent/data-collection-rule-destination.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-destination.png#lightbox)

Haga clic en **Agregar origen de datos** (Agregar origen de datos) y, después, en **Revisar y crear** para revisar los detalles de la regla de recopilación de datos y la asociación con el conjunto de máquinas virtuales. Haga clic en **Crear** para crearlo.

> [!NOTE]
> Una vez creadas las asociaciones y la regla de recopilación de datos, los datos pueden tardar hasta cinco minutos en enviarse a los destinos.


## <a name="create-rule-and-association-using-rest-api"></a>Creación de reglas y asociaciones mediante una API REST

Siga los pasos que se indican a continuación para crear una regla de recopilación de datos y asociaciones mediante la API REST.

1. Cree manualmente el archivo de regla de recopilación de datos con el formato JSON que se muestra en [DCR de ejemplo](data-collection-rule-overview.md#sample-data-collection-rule).

2. Cree la regla con la [API REST](/rest/api/monitor/datacollectionrules/create#examples).

3. Cree una asociación para cada máquina virtual a la regla de recopilación de datos mediante la [API REST](/rest/api/monitor/datacollectionruleassociations/create#examples).


## <a name="create-association-using-resource-manager-template"></a>Creación de una asociación mediante plantillas de Resource Manager

No puede crear una regla de recopilación de datos mediante una plantilla de Resource Manager, pero puede crear una asociación entre una máquina virtual de Azure o un servidor habilitado para Azure Arc mediante una plantilla de Resource Manager. Consulte [Ejemplos de plantillas de Resource Manager para reglas de recopilación de datos en Azure Monitor](./resource-manager-data-collection-rules.md) para obtener plantillas de ejemplo.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [agente de Azure Monitor](azure-monitor-agent-overview.md).
- Más información sobre las [reglas de recopilación de datos](data-collection-rule-overview.md).