---
title: Procedimientos recomendados para el diseño de aplicaciones de Azure Service Fabric
description: Procedimientos recomendados y consideraciones de diseño para desarrollar aplicaciones y servicios con Azure Service Fabric.
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: ddf846e9e3ac6add7cf3f584b702de5accfb22af
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "91538505"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Procedimientos recomendados para el diseño de aplicaciones de Azure Service Fabric

En este artículo se proporciona una guía de procedimientos recomendados para compilar aplicaciones y servicios en Azure Service Fabric.
 
## <a name="get-familiar-with-service-fabric"></a>Familiarizarse con Service Fabric
* Vea el artículo [¿Qué desea saber sobre Service Fabric?](service-fabric-content-roadmap.md).
* Obtenga información sobre los [Escenarios de aplicación de Service Fabric](service-fabric-application-scenarios.md).
* Comprenda las opciones de modelos de programación leyendo la [introducción a los modelos de programación de Service Fabric](service-fabric-choose-framework.md).



## <a name="application-design-guidance"></a>Guía de diseño de aplicaciones
Familiarícese con la [arquitectura general](/azure/architecture/reference-architectures/microservices/service-fabric) de las aplicaciones de Service Fabric y sus [consideraciones de diseño](/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations).

### <a name="choose-an-api-gateway"></a>Elegir una puerta de enlace de API
Use un servicio de puerta de enlace de API que se comunique con servicios de back-end y que, a continuación, se pueda escalar horizontalmente. Los servicios de puerta de enlace de API más comúnmente utilizados son:

- [Azure API Management](./service-fabric-api-management-overview.md), que está [integrada con Service Fabric](./service-fabric-tutorial-deploy-api-management.md).
- [Azure IoT Hub](../iot-hub/index.yml) o [Azure Event Hubs](../event-hubs/index.yml) con [ServiceFabricProcessor](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Microsoft.Azure.EventHubs.ServiceFabricProcessor) para leer a partir de las particiones de Event Hubs.
- [Proxy inverso de Træfik](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric) con [proveedor de Azure Service Fabric](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/).
- [Azure Application Gateway](../application-gateway/index.yml).

   > [!NOTE] 
   > Azure Application Gateway no se integra directamente con Service Fabric. Azure API Management es normalmente la opción preferida.
- Su propia puerta de enlace de aplicación web [ASP.NET Core](./service-fabric-reliable-services-communication-aspnetcore.md).

### <a name="stateless-services"></a>Servicios sin estado
Se recomienda que siempre comience compilando servicios sin estado mediante el estado de almacenamiento [Reliable Services](./service-fabric-reliable-services-introduction.md) en una instancia de Azure Database, Azure CosmosDB o Azure Storage. El estado externalizado es el enfoque más familiar para la mayoría de los desarrolladores. Este enfoque también permite aprovechar las capacidades de consulta en el almacén.  

### <a name="when-to-use-stateful-services"></a>Cuándo usar servicios con estado
Considere la posibilidad de elegir servicios con estado cuando tenga un escenario de baja latencia y necesite mantener los datos cerca del proceso. Algunos escenarios de ejemplo incluyen dispositivos gemelos digitales de IoT, estado de juego, estado de sesión, almacenamiento en caché de datos de una base de datos y flujos de trabajo de larga duración para realizar el seguimiento de las llamadas a otros servicios.

Decida el período de tiempo de retención de los datos:

- **Datos en caché**. Usa el almacenamiento en caché cuando la latencia a los almacenes externos es un problema. Use un servicio con estado como caché de datos propia o considere la posibilidad de usar [caché distribuida de SoCreate Service Fabric de código abierto](https://github.com/SoCreate/service-fabric-distributed-cache). En este escenario, no necesita preocuparse por si pierde todos los datos de la memoria caché.
- **Datos enlazados en tiempo**. En este escenario, necesita mantener los datos cerca de proceso debido a la latencia durante un período de tiempo, pero puede permitirse perder dichos datos en un escenario de *desastre*. Por ejemplo, en muchas soluciones de IoT, los datos deben estar cerca del proceso, por ejemplo, al calcular la temperatura media durante los últimos días; sin embargo, si estos datos se pierden, los puntos de datos concretos registrados no son tan importantes. Además, en este escenario normalmente no le interesa realizar una copia de seguridad de los puntos de datos individuales. Solo realiza copias de seguridad del promedio de los valores calculados que periódicamente se escriben en un almacenamiento externo.  
- **Datos a largo plazo**. Las colecciones de confianza pueden almacenar sus datos permanentemente. Sin embargo, en este caso deberá [prepararse para la recuperación ante desastres](./service-fabric-disaster-recovery.md), incluida la [configuración de directivas de copia de seguridad periódicas](./service-fabric-backuprestoreservice-configure-periodic-backup.md) para sus clusters. En efecto, configura qué pasará si su clúster se destruye en un escenario de desastre, en el que necesitaría crear un nuevo clúster, y cómo implementar nuevas instancias de aplicación y recuperar los datos de la copia de seguridad más reciente.

Ahorrar costos y mejorar la disponibilidad:
- Puede reducir los costos con servicios con estado, ya que no se incurre en costos de acceso y transacciones de datos desde el almacén remoto, y porque no es necesario usar ningún otro servicio como Azure Cache for Redis.
- Usar servicios con estado principalmente para el almacenamiento y no para el proceso es costoso y no se recomienda. Considere la posibilidad de utilizar los servicios con estado como proceso con un almacenamiento local económico.
- Al quitar las dependencias en otros servicios, puede mejorar la disponibilidad del servicio. Tener un estado administrado con alta disponibilidad en el clúster le protege de los tiempos de inactividad o problemas de latencia de otros servicios.

## <a name="how-to-work-with-reliable-services"></a>Cómo trabajar con Reliable Services
Service Fabric Reliable Services le permite crear fácilmente servicios con estado y sin estado. Para más información, consulte [Introducción a Reliable Services](./service-fabric-reliable-services-introduction.md).
- Respete siempre el [token de cancelación](./service-fabric-reliable-services-lifecycle.md#stateful-service-primary-swaps) en el método `RunAsync()` para los servicios con estado y sin estado y el método `ChangeRole()` para los servicios con estado. En caso contrario, Service Fabric no sabe si se puede cerrar el servicio. Por ejemplo, no respetar el token de cancelación puede dar lugar a tiempos de actualización de aplicaciones mucho más prolongados.
-    Abra y cierre los [clientes de escucha de comunicación](./service-fabric-reliable-services-communication.md) de manera oportuna y respete los tokens de cancelación.
-    Nunca mezcle código sincrónico con código asincrónico. Por ejemplo, no use `.GetAwaiter().GetResult()` en las llamadas asincrónicas. Use la asincronía *totalmente* en toda la pila de llamadas.

## <a name="how-to-work-with-reliable-actors"></a>Cómo trabajar con Reliable Actors
Service Fabric Reliable Actors le permite crear fácilmente actores con estado virtuales. Para más información, consulte [Introducción a Reliable Actors](./service-fabric-reliable-actors-introduction.md).

- Considere seriamente la posibilidad de utilizar la mensajería publicada y enviada entre los actores para escalar la aplicación. Entre las herramientas que ofrecen este servicio se incluyen [SoCreate Service Fabric Pub/Sub de código abierto](https://service-fabric-pub-sub.socreate.it/) y [Azure Service Bus](/azure/service-bus/).
- Haga que el estado de los actores sea [lo más pormenorizado posible](./service-fabric-reliable-actors-state-management.md#best-practices).
- Administre el [ciclo de vida del actor](./service-fabric-reliable-actors-state-management.md#best-practices). Elimine actores si no piensa usarlos nunca más. La eliminación de actores innecesarios es especialmente importante cuando se usa el [proveedor de estado volátil](./service-fabric-reliable-actors-state-management.md#state-persistence-and-replication), ya que todos los estados se almacenan en memoria.
- Debido a su [simultaneidad basada en turnos](./service-fabric-reliable-actors-introduction.md#concurrency), los actores son más útiles como objetos independientes. No cree gráficos de llamadas al método multiactor sincrónico (cada una de las cuales probablemente se convierte en una llamada de red independiente) ni solicitudes de actor circular. Esto afectará significativamente al rendimiento y la escala.
- No mezcle código sincrónico con código asincrónico. Usar la asincronía de una manera coherente para evitar problemas de rendimiento.
- No realice llamadas de larga duración en actores. Las llamadas de larga duración en actores bloquearán otras llamadas para el mismo actor debido a la simultaneidad basada en turnos.
- Si se está comunicando con otros servicios mediante la [comunicación remota de Service Fabric](./service-fabric-reliable-services-communication-remoting.md) y está creando un `ServiceProxyFactory`, cree la fábrica al nivel del [servicio de actores](./service-fabric-reliable-actors-using.md) y *no* al nivel del actor.


## <a name="application-diagnostics"></a>Diagnósticos de aplicaciones
Sea exhaustivo sobre la adición del [registro de aplicaciones](./service-fabric-diagnostics-event-generation-app.md) en las llamadas al servicio. Le ayudará a diagnosticar escenarios de servicios que se llaman entre sí. Por ejemplo, cuando A llama a B que llama a C que llama a D, podría generarse un error en la llamada en cualquier lugar. Si no tiene suficiente registro, los errores son difíciles de diagnosticar. Si los servicios están realizando demasiados registros debido al volumen de llamadas, al menos asegúrese de registrar los errores y las advertencias.

## <a name="iot-and-messaging-applications"></a>IoT y aplicaciones de mensajería
Cuando esté leyendo los mensajes de [Azure IoT Hub](../iot-hub/index.yml) o [Azure Event Hubs](../event-hubs/index.yml), use [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/ServiceFabricProcessor). ServiceFabricProcessor se integra con Service Fabric Reliable Services para mantener el estado de lectura desde las particiones de Event Hub e inserta los nuevos mensajes a los servicios mediante el método `IEventProcessor::ProcessEventsAsync()`.


## <a name="design-guidance-on-azure"></a>Guía de diseño sobre Azure
* Visite el [Centro de arquitectura de Azure](/azure/architecture/microservices/) para consultar una guía de diseño sobre cómo [compilar microservicios en Azure](/azure/architecture/microservices/).

* Visite [Introducción a Azure para Juegos](/gaming/azure/) para consultar una guía de diseño sobre [el uso de Service Fabric en servicios de juegos](/gaming/azure/reference-architectures/multiplayer-synchronous-sf).
