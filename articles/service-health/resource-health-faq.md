---
title: P+F sobre Azure Resource Health
description: Introducción a Azure Resource Health
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: dc24688d8393b7a13e8c93d3f4d0171cce4c213c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100570057"
---
# <a name="azure-resource-health-faq"></a>P+F sobre Azure Resource Health
Aprenda las respuestas a las preguntas más frecuentes sobre Azure Resource Health.

## <a name="what-is-azure-resource-health"></a>¿Qué es Azure Resource Health?
Resource Health le ayuda a diagnosticar y obtener soporte técnico cuando un problema de Azure afecta a sus recursos. Le informa acerca del mantenimiento actual y pasado de los recursos y le ayuda a mitigar los problemas. Resource Health le proporciona soporte técnico si necesita ayuda con los problemas de los servicios de Azure.  

## <a name="what-is-the-resource-health-intended-for"></a>¿Para qué está pensado Resource Health?
Cuando se detecta un problema con un recurso, Resource Health puede ayudarle a diagnosticar la causa principal. Proporciona ayuda para mitigar el problema y el soporte técnico cuando necesita más ayuda para solucionar los problemas con el servicio de Azure.

## <a name="what-health-checks-are-performed-by-resource-health"></a>¿Qué comprobaciones de estado realiza Resource Health?
Resource Health realiza varias comprobaciones según el [tipo de recurso](resource-health-checks-resource-types.md). Estas comprobaciones están diseñadas para implementar tres tipos de problemas: 
- Eventos no planeados, por ejemplo, un reinicio inesperado del host
- Eventos planeados, como actualizaciones programadas del sistema operativo del host
- Eventos desencadenados por acciones de usuario, por ejemplo, un usuario que reinicia una máquina virtual

## <a name="what-does-each-of-the-health-status-mean"></a>¿Qué significa cada uno de los estados de mantenimiento?
Hay tres estados de mantenimiento distintos:
- Disponible: no hay problemas conocidos en la plataforma de Azure que pudieran afectar a este recurso.
- No disponible: Resource Health detectó problemas que afectan al recurso.
- Desconocido: Resource Health no puede determinar el estado de un recurso porque dejó de recibir información sobre el mismo. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>¿Qué significa el estado desconocido? ¿Hay problemas con mi recurso?
El estado es desconocido cuando Resource Health deja de recibir información sobre un recurso específico. Si bien este estado no es una indicación definitiva del estado del recurso, en casos donde experimenta problemas podría indicar que hay un problema con Azure.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>¿Cómo puedo obtener ayuda para un recurso no disponible?
Puede enviar una solicitud de soporte técnico desde la hoja Resource Health. No necesita tener un contrato de soporte técnico con Microsoft para abrir una solicitud cuando el recurso no está disponible debido a eventos de plataforma.

## <a name="does-resource-health-differentiate-between-unavailability-caused-by-platform-problems-versus-something-i-did"></a>¿Resource Health diferencia entre la no disponibilidad a causa de problemas de plataforma y por alguna acción que yo hice?
Sí, cuando un recurso no está disponible, Resource Health incluye la causa principal en una de estas categorías: 
-   Acción iniciada por el usuario
-   Evento planeado 
-   Evento no planeado

En el portal, las acciones iniciadas por el usuario aparecen con un icono de notificación azul, mientras que los eventos planeados y no planeados se muestran con un icono de advertencia de color rojo. Se proporcionan más detalles en [Información general sobre Resource Health](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>¿Puedo integrar Resource Health con mis herramientas de supervisión?
Resource Health [admite](resource-health-alert-arm-template-guide.md) las alertas basadas en el registro de actividad. Las alertas del registro de actividad usan [grupos de acciones](../azure-monitor/alerts/action-groups.md) para notificar a los usuarios que se ha desencadenado una alerta. Los grupos de acciones son compatibles con una variedad de canales de notificación, como correo electrónico, SMS, webhook y acciones de ITSM.

## <a name="where-do-i-find-resource-health"></a>¿Dónde se encuentra Resource Health?
Después de iniciar sesión en Azure Portal, puede acceder a Resource Health de varias maneras:
- Vaya al recurso. En el panel de navegación de la izquierda, seleccione **Resource Health**
- Vaya a la hoja de Azure Service Health.  En el panel de navegación de la izquierda, seleccione **Resource Health**.

También puede usar la API de Resource Health para obtener información sobre el estado de los recursos.

## <a name="is-resource-health-available-for-all-resource-types"></a>¿Está Resource Health disponible para todos los tipos de recursos?
La lista de comprobaciones de estado y los tipos de recursos compatibles con Resource Health se encuentran [aquí](resource-health-checks-resource-types.md).

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>¿Qué debo hacer si mi recurso aparece como disponible pero creo que no lo está?
Cuando se comprueba el estado de un recurso, justo debajo del estado de mantenimiento puede hacer clic en **Informe de estado de mantenimiento incorrecto**. Antes de enviar el informe, tiene la opción de proporcionar detalles adicionales sobre por qué cree que el estado de mantenimiento actual no es correcto.

## <a name="is-resource-health-available-for-all-azure-regions"></a>¿Está Resource Health disponible en todas las regiones de Azure? 
Resource Health está disponible en todas las zonas geográficas de Azure.

## <a name="how-is-resource-health-different-from-azure-status-or-the-service-health-dashboard"></a>¿Cuál es la diferencia entre Resource Health, estado de Azure y el panel de Service Health?
La información que Resource Health proporciona es más específica que la que proporciona el estado de Azure o el panel de Service Health.

Mientras que el [estado de Azure](https://status.azure.com) y el panel de Service Health le informan sobre problemas de servicio que afectan a un amplio conjunto de clientes (por ejemplo, una región de Azure), Resource Health expone eventos más pormenorizados que solo afectan a un recurso específico. Por ejemplo, si un host se reinicia de improviso, Resource Health solo alerta a los clientes cuyas máquinas virtuales se ejecutaban en ese host.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>¿Es necesario activar Resource Health para cada recurso?
No, la información de estado está disponible para todos los tipos de recursos a través de Resource Health. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>¿Es necesario habilitar Resource Health para la organización?
No.  Azure Resource Health es accesible desde Azure Portal, sin requisitos de configuración.

## <a name="is-resource-health-available-free-of-charge"></a>¿Está Resource Health disponible de forma gratuita?
Sí.  Azure Resource Health es gratuito.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>¿Qué recomendaciones proporciona Resource Health?
Según el estado, Resource Health proporciona recomendaciones con el objetivo de disminuir el tiempo dedicado a solucionar problemas. En el caso de los recursos disponibles, las recomendaciones se centran en cómo solucionar los problemas más comunes que encuentran los clientes. Si el recurso no está disponible debido a un evento no planeado de Azure, el foco estará en ayudarle durante el proceso de recuperación y después de este. 

## <a name="next-steps"></a>Pasos siguientes

Más información sobre Resource Health:
-  [Información general sobre Azure Resource Health](Resource-health-overview.md)
-  [Tipos de recursos y comprobaciones de mantenimiento disponibles a través de Azure Resource Health](resource-health-checks-resource-types.md)
