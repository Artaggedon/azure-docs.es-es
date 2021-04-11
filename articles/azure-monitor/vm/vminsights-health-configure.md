---
title: Configuración de la supervisión en el estado de invitado de VM Insights (versión preliminar)
description: Describe cómo modificar la supervisión predeterminada para el estado de invitado de VM Insights (versión preliminar) mediante Azure Portal.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/14/2020
ms.openlocfilehash: 6bda6c9e5f6e23e9e15c12fd507645fc72159302
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104583452"
---
# <a name="configure-monitoring-in-vm-insights-guest-health-preview"></a>Configuración de la supervisión en el estado de invitado de VM Insights (versión preliminar)
El estado de invitado de VM Insights permite ver el estado de una máquina virtual conforme a la definición de un conjunto de medidas de rendimiento que se muestrean a intervalos regulares. En este artículo se describe cómo puede modificar la supervisión predeterminada mediante Azure Portal. También se describen los conceptos básicos de los monitores necesarios para [configurar la supervisión mediante una regla de recopilación de datos](vminsights-health-configure-dcr.md).

## <a name="open-monitor-configuration"></a>Apertura de la configuración del monitor
Abra la configuración del monitor en Azure Portal; para ello, seleccione el monitor y, a continuación, la pestaña **Configuración**.

[![Configuración de la información del monitor](media/vminsights-health-overview/monitor-details-configuration.png)](media/vminsights-health-overview/monitor-details-configuration.png#lightbox)

## <a name="enable-or-disable-monitors"></a>Habilitación o deshabilitación de los monitores
Tanto los monitores de unidad como los monitores agregados tienen una opción de configuración de **Estado del monitor de mantenimiento** que permite habilitar o deshabilitar al monitor. Cuando un monitor está habilitado, su estado se muestra y se usa para establecer el estado de la máquina virtual. Cuando un monitor está deshabilitado, su estado no se calcula y no se usa para establecer el estado de la máquina virtual.

| Configuración | Descripción |
|:---|:---|
| habilitado | El monitor está habilitado, independientemente de la configuración de su elemento primario. |
| Disabled | El monitor está deshabilitado, independientemente de la configuración de su elemento primario. |
| Igual que el elemento primario | El monitor se habilitará o deshabilitará según la configuración de su elemento primario. |

Cuando un monitor está deshabilitado, los criterios se muestran como "no disponibles", como se muestra en el ejemplo siguiente.

![Monitor deshabilitado](media/vminsights-health-configure/disabled-monitor.png)


> [!NOTE]
> Si un monitor primario está deshabilitado, también se deshabilitan los monitores secundarios. Si habilita explícitamente el monitor secundario, el primario también se habilitará, pero su estado de configuración seguirá siendo el mismo. En este caso, recibirá el siguiente mensaje en el monitor primario.
>
> *Hay una discrepancia cuando el estado configurado del monitor es "deshabilitado", pero el estado de mantenimiento no lo refleja. Esto se debe a que los cambios configurados se propagan o cualquiera de sus monitores secundarios se ha habilitado explícitamente.*

## <a name="enable-or-disable-virtual-machine"></a>Habilitación o deshabilitación de la máquina virtual
Puede deshabilitar la supervisión de una máquina virtual para detener temporalmente a todos los monitores. Puede deshabilitar la supervisión de una máquina virtual, por ejemplo, cuando le está dando mantenimiento.

| Configuración | Descripción |
|:---|:---|
| habilitado  | Se muestra el estado de mantenimiento del equipo. |
| Disabled | El estado de mantenimiento del equipo se muestra como **deshabilitado**. No se crean las alertas. |

## <a name="health-state-logic"></a>Lógica del estado de mantenimiento
La lógica de estado de mantenimiento de un monitor de unidad define los criterios para establecer el estado de mantenimiento. Puede especificar el número de estados de mantenimiento que tiene un monitor, así como el umbral de cómo se calcula cada estado de mantenimiento.

![Criterios de mantenimiento de ejemplo](media/vminsights-health-configure/sample-health-criteria.png)

Los monitores agregados no especifican ninguna lógica de estado de mantenimiento. El estado de mantenimiento de dichos monitores lo establecen los monitores de unidad que les corresponden según su acumulación de estado.

Los monitores de unidad tienen dos o tres estados de mantenimiento cada uno. Cada uno tendrá siempre un estado correcto y, opcionalmente, un estado de advertencia, un estado crítico, o ambos. Tanto el estado de advertencia como el crítico requieren criterios únicos que definen cuándo se establecerá al monitor en dicho estado. El estado correcto no tiene criterios, ya que este estado se establece cuando no se cumplen los criterios de los demás estados.

En el ejemplo siguiente, el uso de CPU está establecido en los siguientes estados de mantenimiento:

- Crítico si el uso es mayor que 90 %.
- Advertencia si el uso es mayor o igual que 80 %.
- Correcto si el uso está por debajo del 80 %. Este se sobrentiende ya que es la condición en la que no se aplica ninguna de las otras condiciones.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de monitores a escala mediante reglas de recopilación de datos.](vminsights-health-configure-dcr.md)
