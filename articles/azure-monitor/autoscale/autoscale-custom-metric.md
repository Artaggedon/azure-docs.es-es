---
title: Escalado automático con una métrica personalizada en Azure
description: Obtenga información sobre cómo escalar recursos mediante métricas personalizadas en Azure.
ms.topic: conceptual
ms.date: 05/07/2017
ms.subservice: autoscale
ms.openlocfilehash: 8e744e6a91eb6fbe23a6b45f95c39b1acfdcb61f
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100603449"
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Introducción al escalado automático mediante métricas personalizadas en Azure
En este artículo se describe cómo escalar el recurso mediante una métrica personalizada en Azure Portal.

La escalabilidad automática de Azure Monitor solo se aplica a [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), [clústeres de Azure Data Explorer](https://azure.microsoft.com/services/data-explorer/),   
Entorno del servicio de integración y los [servicios de API Management](../../api-management/api-management-key-concepts.md).

## <a name="lets-get-started"></a>Introducción
En este artículo se presupone que tiene una aplicación web con Application Insights configurado. Si aún no la tiene, puede [configurar Application Insights para el sitio web ASP.NET][1].

- Abra [Azure Portal][2]
- Haga clic en el icono de Azure Monitor en el panel de navegación izquierdo.
  ![Inicio de Azure Monitor][3]
- Haga clic en la opción Escalado automático para ver todos los recursos a los que se aplica el escalado automático, junto con su estado de escalado automático actual ![Detección de escalado automático en Azure Monitor][4]
- Abra la hoja "Escalado automático" en Azure Monitor y seleccione un recurso que desee escalar.
  > Nota: En los pasos siguientes se usa un plan de App Service asociado con una aplicación web con Application Insights configurado.
- En la hoja de configuración de escalado del recurso, tenga en cuenta que el recuento de la instancia actual es 1. Haga clic en "Enable autoscale" (Habilitar escalado automático).
  ![Configuración de escalado para la nueva aplicación web][5]
- Proporcione un nombre para la configuración de escalado y haga clic en "Agregar una regla". Tenga en cuenta que las opciones de la regla de escalado se abren como un panel Contexto en el lado derecho. De forma predeterminada, establece la opción para escalar el recuento de instancias en 1 si el porcentaje de CPU del recurso supera el 70 %. Cambie el origen de métricas en la parte superior a "Application Insights", seleccione el recurso de Application Insights en el menú desplegable "Recurso" y luego seleccione la métrica personalizada en la que quiere basar el escalado.
  ![Escalar por métrica personalizada][6]
- De forma similar al paso anterior, agregue una regla de escalado que reduzca horizontalmente y disminuya el recuento de escala en 1 si la métrica personalizada está por debajo del umbral.
  ![Escala en función de la CPU][7]
- Establezca los límites de la instancia. Por ejemplo, si desea escalar entre 2-5 instancias en función de las fluctuaciones de la métrica personalizada, establezca el "mínimo" en "2", el "máximo" en "5" y el "valor predeterminado" en "2".
  > Nota: En caso de que haya algún problema al leer las métricas de recursos y la capacidad actual sea inferior a la predeterminada, el escalado automático escalará horizontalmente al valor predeterminado a fin de garantizar la disponibilidad del recurso. Si la capacidad actual ya es mayor que la predeterminada, el escalado automático no reducirá horizontalmente.
- Haga clic en "Guardar".

¡Enhorabuena! Ya ha establecido correctamente la configuración del escalado para escalar automáticamente la aplicación web en función de una métrica personalizada.

> Nota: Los mismos pasos son aplicables para empezar a trabajar con un rol de servicio en la nube o VMSS.

<!--Reference-->
[1]: ../app/asp-net.md
[2]: https://portal.azure.com
[3]: ./media/autoscale-custom-metric/azure-monitor-launch.png
[4]: ./media/autoscale-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/autoscale-custom-metric/scale-setting-new-web-app.png
[6]: ./media/autoscale-custom-metric/scale-by-custom-metric.png
[7]: ./media/autoscale-custom-metric/autoscale-setting-custom-metrics-ai.png
