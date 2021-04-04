---
title: ¿Qué es Azure Scheduler?
description: Creación, programación y ejecución de trabajos automatizados que llamen a servicios dentro y fuera de Azure
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 66ec285554299214122a4093837d3506bf642b13
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92368135"
---
# <a name="what-is-azure-scheduler"></a>¿Qué es Azure Scheduler?

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes. 
>
> Scheduler ya no está disponible en Azure portal, pero la [API REST](/rest/api/scheduler) y los [cmdlets de PowerShell para Azure Scheduler](scheduler-powershell-reference.md) siguen disponibles en la actualidad para que pueda administrar los trabajos y las colecciones de trabajos.

[Azure Scheduler](https://azure.microsoft.com/services/scheduler/) le ayuda a crear [trabajos](../scheduler/scheduler-concepts-terms.md) que se ejecutan en la nube mediante la descripción declarativa de acciones. Posteriormente, el servicio programa y ejecuta esas acciones de forma automática. Por ejemplo, puede llamar a servicios dentro y fuera de Azure, como llamar a puntos de conexión HTTP o HTTPS, y también publicar mensajes en colas de Azure Storage y colas o temas de Azure Service Bus. Puede ejecutar trabajos de inmediato o en un momento posterior. Scheduler admite [programaciones complejas y periodicidad avanzada](../scheduler/scheduler-advanced-complexity.md) con facilidad. Scheduler especifica cuándo se debe ejecutar los trabajos, mantiene un historial con los resultados del trabajo que puede revisar y, a continuación, programa de forma predecible y confiable cargas de trabajo para que se ejecuten.

Otras funcionalidades de Azure también usan Scheduler en segundo plano, por ejemplo, [Azure WebJobs](../app-service/webjobs-create.md), que es una característica de [Web Apps](https://azure.microsoft.com/services/app-service/web/) en Azure App Service. Puede administrar la comunicación para estas acciones mediante la [API REST de Scheduler](/rest/api/scheduler/).

Estos son algunos escenarios en los que Scheduler puede ayudarle:

* Ejecución de acciones de aplicación periódicas: por ejemplo, recopilar periódicamente datos de Twitter en una fuente.

* Realización del mantenimiento diario: eliminación diaria de registros, realización de copias de seguridad y otras tareas de mantenimiento.

  Por ejemplo, como administrador, es posible que desee realizar una copia de seguridad de la base de datos a la 1:00 a.m. cada día durante los próximos 9 meses.

Aunque puede usar Scheduler para crear, mantener y ejecutar cargas de trabajo programadas, Scheduler no hospeda las cargas de trabajo ni ejecuta código. El servicio solo *invoca* los servicios o el código hospedado en cualquier otro lugar, por ejemplo, en Azure, localmente o con otro proveedor. Scheduler puede realizar la invocación mediante HTTP, HTTPS, una cola de Storage, una cola de Service Bus o un tema de Service Bus.

Para crear, programar, administrar, actualizar o eliminar trabajos y [colecciones de trabajos](../scheduler/scheduler-concepts-terms.md), puede usar código, la [API REST de Scheduler](/rest/api/scheduler/) o los [cmdlets de PowerShell de Azure Scheduler](scheduler-powershell-reference.md).

## <a name="next-steps"></a>Pasos siguientes

* [Conceptos, terminología y jerarquía de entidades de Azure Scheduler](scheduler-concepts-terms.md)
* [Planes y facturación de Azure Scheduler](scheduler-plans-billing.md)
* [Creación de programaciones complejas y periodicidad avanzada con Azure Scheduler](scheduler-advanced-complexity.md)
* [Referencia de API de REST de Azure Scheduler](/rest/api/scheduler)
* [Referencia de cmdlets de PowerShell de Azure Scheduler](scheduler-powershell-reference.md)