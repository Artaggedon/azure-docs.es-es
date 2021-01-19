---
title: Preguntas más frecuentes sobre Azure Event Hubs | Microsoft Docs
description: En este artículo se proporciona una lista de las preguntas más frecuentes (P+F) acerca de Azure Event Hubs y sus respuestas.
ms.topic: article
ms.date: 10/27/2020
ms.openlocfilehash: e7a34fe0f2ef04fffeeddc5615d3ac1749467902
ms.sourcegitcommit: 19ffdad48bc4caca8f93c3b067d1cf29234fef47
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97955423"
---
# <a name="event-hubs-frequently-asked-questions"></a>Preguntas frecuentes sobre Event Hubs

## <a name="general"></a>General

### <a name="what-is-an-event-hubs-namespace"></a>¿Qué es un espacio de nombres de Event Hubs?
Un espacio de nombres es un contenedor de temas de Event Hubs y Kafka. Proporciona un [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) único. Un espacio de nombres actúa como un contenedor de aplicaciones que puede alojar varios temas de Event Hubs y Kafka. 

### <a name="when-do-i-create-a-new-namespace-vs-use-an-existing-namespace"></a>¿Cuando crear un nuevo espacio de nombres o cuándo usar un espacio de nombres existente?
Las asignaciones de capacidad ([unidades de procesamiento (TU)](#throughput-units)) se facturan según el espacio de nombres. Un espacio de nombres también se asocia a una región.

Es posible que desee crear un nuevo espacio de nombres en lugar de usar uno existente en alguno de los siguientes escenarios: 

- Necesita un centro de eventos asociado a una nueva región.
- Necesita un centro de eventos asociado a otra suscripción.
- Necesita un centro de eventos con una asignación de capacidad diferente (es decir, la capacidad necesaria para el espacio de nombres con el centro de eventos agregado superaría el umbral de 40 TU y no sería conveniente el clúster dedicado)  

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>¿Cuál es la diferencia entre los niveles Basic y Standard de Event Hubs?

El nivel Estándar de Azure Event Hubs proporciona más características de las disponibles en el nivel Básico. Estas son las características disponibles en el nivel Estándar:

* Retención de eventos más prolongada
* Más conexiones asíncronas, con un cargo por uso por encima del límite de más del número incluido
* Más de un único [grupo de consumidores](event-hubs-features.md#consumer-groups)
* [Capture](event-hubs-capture-overview.md)
* [Integración de Kafka](event-hubs-for-kafka-ecosystem-overview.md)

Para más información sobre los planes de tarifa, incluido Event Hubs dedicado, vea los [detalles de los precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="where-is-azure-event-hubs-available"></a>¿Dónde está disponible Azure Event Hubs?

Azure Event Hubs está disponible en todas las regiones de Azure admitidas. Para obtener una lista, visite la página [Regiones de Azure](https://azure.microsoft.com/regions/).  

### <a name="can-i-use-a-single-advanced-message-queuing-protocol-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>¿Se puede usar una sola conexión de Advanced Message Queuing Protocol (AMQP) para enviar y recibir en varios centros de eventos?

Sí, siempre y cuando todos los centros de eventos se encuentren en el mismo espacio de nombres.

### <a name="what-is-the-maximum-retention-period-for-events"></a>¿Cuál es el período de retención máximo para eventos?

El nivel Event Hubs estándar admite actualmente un período de retención máximo de siete días. Event Hubs no está concebido como un almacén de datos permanente. Los períodos de retención superiores a 24 horas están pensados para escenarios en los que es útil volver a reproducir un flujo de eventos en los mismos sistemas. Por ejemplo, para entrenar o comprobar un nuevo modelo de aprendizaje automático con datos existentes. Si necesita conservar los mensajes más de siete días, habilite [Event Hubs Capture](event-hubs-capture-overview.md) en el centro de eventos para extraer los datos de este a la cuenta de almacenamiento o la cuenta del servicio Azure Data Lake que prefiera. Al habilita Capture se le cobrará un cargo en función de las unidades de procesamiento compradas.

Puede configurar el período de retención de los datos capturados en su cuenta de almacenamiento. La característica de **administración del ciclo de vida** de Azure Storage ofrece una útil directiva basada en reglas para las cuentas de uso general v2 y de Blob Storage. Use la directiva para realizar la transición de los datos a los niveles de acceso adecuados o hacer que expiren al final de su ciclo de vida. Para más información, consulte [Administración del ciclo de vida de Azure Blob Storage](../storage/blobs/storage-lifecycle-management-concepts.md). 

### <a name="how-do-i-monitor-my-event-hubs"></a>¿Cómo puedo supervisar mi instancia de Event Hubs?
Event Hubs emite métricas exhaustivas que proporcionan el estado de los recursos a [Azure Monitor](../azure-monitor/overview.md). También permite evaluar el estado general del servicio Event Hubs, no solo en el nivel de espacio de nombres, sino también en el nivel de entidad. Obtenga información sobre la supervisión que se ofrece para [Azure Event Hubs](event-hubs-metrics-azure-monitor.md).

### <a name="where-does-azure-event-hubs-store-customer-data"></a><a name="in-region-data-residency"></a>¿Dónde se almacenan los datos de los clientes en Azure Event Hubs?
Azure Event Hubs almacena datos de los clientes. Event Hubs almacena estos datos automáticamente en una sola región, por lo que este servicio satisface los requisitos de residencia de datos de la región, incluidos los especificados en el [centro de confianza](https://azuredatacentermap.azurewebsites.net/).

[!INCLUDE [event-hubs-connectivity](../../includes/event-hubs-connectivity.md)]

## <a name="apache-kafka-integration"></a>Integración de Apache Kafka

### <a name="how-do-i-integrate-my-existing-kafka-application-with-event-hubs"></a>¿Cómo se puede integrar una aplicación existente de Kafka con Event Hubs?
Event Hubs proporciona un punto de conexión de Kafka que las aplicaciones basadas en Apache Kafka existentes pueden usar. Basta con un cambio de configuración para poder disfrutar de la experiencia de PaaS Kafka. Proporciona una alternativa a ejecutar su propio clúster de Kafka. Event Hubs admite Apache Kafka 1.0 y versiones del cliente más recientes y funciona con marcos, herramientas y aplicaciones de Kafka existentes. Para obtener más información, consulte [Event Hubs for Kafka repo](https://github.com/Azure/azure-event-hubs-for-kafka) (Event Hubs para el repositorio de Kafka).

### <a name="what-configuration-changes-need-to-be-done-for-my-existing-application-to-talk-to-event-hubs"></a>¿Qué cambios de configuración deben realizarse para que mi aplicación existente se comunique con Event Hubs?
Para conectarse a un centro de eventos, debe actualizar las configuraciones de cliente de Kafka. Se hace mediante la creación de un espacio de nombres de Event Hubs y la obtención de la [cadena de conexión](event-hubs-get-connection-string.md). Cambie bootstrap.servers para que señale al FQDN de Event Hubs y el puerto, a 9093. Actualice sasl.jaas.config para dirigir el cliente de Kafka al punto de conexión de Event Hubs (que es la cadena de conexión que ha obtenido) con la autenticación correcta, tal y como se muestra aquí:

```properties
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
request.timeout.ms=60000
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

Ejemplo:

```properties
bootstrap.servers=dummynamespace.servicebus.windows.net:9093
request.timeout.ms=60000
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXX";
```
Nota: Si sasl.jaas.config no es una configuración admitida en su marco, busque las configuraciones que se usan para establecer el nombre de usuario y la contraseña de SASL y úselas en su lugar. Establezca el nombre de usuario para $ConnectionString y la contraseña para la cadena de conexión de Event Hubs.

### <a name="what-is-the-messageevent-size-for-event-hubs"></a>¿Cuál es el tamaño de mensaje o evento de Event Hubs?
El tamaño máximo de mensaje permitido en Event Hubs es 1 MB.

## <a name="throughput-units"></a>Unidades de procesamiento

### <a name="what-are-event-hubs-throughput-units"></a>¿Qué son las unidades de procesamiento de Event Hubs?
El rendimiento de Event Hubs define la cantidad de datos en megabytes o el número (en miles) de eventos de 1 KB que entran y salen a través de Event Hubs. Este rendimiento se mide en unidades de procesamiento (TU). Compre TU antes de empezar a usar el servicio Event Hubs. Las TU de Event Hubs se pueden seleccionar explícitamente mediante el portal o las plantillas de Resource Manager para Event Hubs. 


### <a name="do-throughput-units-apply-to-all-event-hubs-in-a-namespace"></a>¿Se aplican las unidades de procesamiento a todos los centros de eventos de un espacio de nombres?
Sí, las unidades de procesamiento (TU) se aplican a todos los centros de eventos de un espacio de nombres de Event Hubs. Significa que las TU se compran en el nivel de espacio de nombres y se comparten entre los centros de eventos de ese espacio de nombres. Cada TU da al espacio de nombres derecho a las siguientes funcionalidades:

- Hasta 1 MB por segundo de eventos de entrada (eventos enviados a un centro de eventos), pero no más de 1000 eventos de entrada, operaciones de administración o llamadas a la API de control por segundo.
- Hasta 2 MB por segundo de eventos de salida (eventos consumidos de un centro de eventos), pero no más de 4096 eventos de salida.
- Hasta 84 GB de almacenamiento de eventos (suficiente para el período de retención predeterminado de 24 horas).

### <a name="how-are-throughput-units-billed"></a>¿Cómo se facturan las unidades de procesamiento?
Las unidades de procesamiento (TU) se facturan por hora. La facturación se basa en el número máximo de unidades que se seleccionaron durante la hora en cuestión. 

### <a name="how-can-i-optimize-the-usage-on-my-throughput-units"></a>¿Cómo puedo optimizar el uso de las unidades de procesamiento?
Puede empezar con solo una unidad de procesamiento (TU) y activar el [inflado automático](event-hubs-auto-inflate.md). La función de inflado automático le permite aumentar las TU a medida que incrementa la carga y el tráfico. También puede establecer un límite superior en el número de TU.

### <a name="how-does-auto-inflate-feature-of-event-hubs-work"></a>¿Cómo funciona la característica de inflado automático de Event Hubs?
La función de inflado automático le permite escalar verticalmente las unidades de procesamiento (TU). Significa que, para empezar, puede comprar un número bajo de TU y el inflado automático escalará verticalmente las TU a medida que aumente la entrada. Le ofrece una opción rentable y el control total del número de TU que se van a administrar. Esta característica permite **solo escalar verticalmente** y puede controlar por completo la reducción vertical del número de TU mediante su actualización. 

Es recomendable empezar con un número bajo de unidades de procesamiento (TU); por ejemplo, 2 TU. Si prevé que el tráfico puede crecer hasta 15 TU, active la función de inflado automático en el espacio de nombres y establezca el límite máximo en 15 TU. Ahora podrá aumentar automáticamente las TU a medida que crezca el tráfico.

### <a name="is-there-a-cost-associated-when-i-turn-on-the-auto-inflate-feature"></a>¿Hay algún costo asociado a la activación de la característica de inflado automático?
No hay **ningún costo** asociado con esta característica. 

### <a name="how-are-throughput-limits-enforced"></a>¿Cómo se aplican los límites de procesamiento?
Si el rendimiento total de **entrada** o la tasa total de eventos de entrada en todos centros de eventos de un espacio de nombres superan las asignaciones de unidades de procesamiento agregado, los emisores quedan sujetos a limitaciones y reciben errores que indican que se superó la cuota de entrada.

Si el rendimiento total de **salida** o la tasa total de eventos de salida en todos centros de eventos de un espacio de nombres superan las asignaciones de unidades de procesamiento agregado, los receptores quedan sujetos a limitaciones pero no se generan errores de limitación. 

Las cuotas de entrada y de salida se aplican por separado, por lo que ningún remitente puede provocar que se ralentice el consumo de eventos ni ningún receptor puede impedir que los eventos se envíen a un centro de eventos.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-reservedselected"></a>¿Hay algún límite en el número de unidades de procesamiento que se pueden reservar o seleccionar?

Al crear un espacio de nombres de nivel Básico o Estándar en Azure Portal, puede seleccionar hasta 20 unidades de procesamiento para el espacio de nombres. Para elevarlo a **exactamente** 40 unidades de procesamiento, envíe una [solicitud de soporte técnico](../azure-portal/supportability/how-to-create-azure-support-request.md).  

1. En la página **Espacio de nombres del bus de eventos**, seleccione **Nueva solicitud de soporte técnico** en el menú de la izquierda. 
1. En la página **Nueva solicitud de soporte técnico**, siga estos pasos:
    1. En el campo **Resumen**, describa el problema en pocas palabras. 
    1. Para **Tipo de problema**, seleccione **Cuota**. 
    1. En el campo **Subtipo de problema**, seleccione **Solicitud de aumento o disminución de las unidades de procesamiento**. 
    
        :::image type="content" source="./media/event-hubs-faq/support-request-throughput-units.png" alt-text="Página de solicitud de soporte técnico":::

Más allá de 40 TU, Event Hubs ofrece el modelo basado en recursos o capacidad denominado clústeres de Event Hubs dedicado. Los clústeres dedicados se venden en unidades de capacidad (CU). Para obtener más información, consulte [Introducción a Event Hubs dedicado](event-hubs-dedicated-overview.md).

## <a name="dedicated-clusters"></a>Clústeres dedicados

### <a name="what-are-event-hubs-dedicated-clusters"></a>¿Qué son los clústeres de Event Hubs dedicado?
Los clústeres de Event Hubs dedicado ofrecen implementaciones de un único inquilino para los clientes con los requisitos más exigentes. Esta oferta construye un clúster basado en la capacidad que no está limitado por las unidades de procesamiento. Esto significa que podría utilizar el clúster para la ingesta y transmisión de los datos tal y como indica el uso de CPU y memoria del clúster. Para obtener más información, consulte [Event Hubs Dedicated clusters](event-hubs-dedicated-overview.md) (Clústeres de Event Hubs dedicado).

### <a name="how-do-i-create-an-event-hubs-dedicated-cluster"></a>¿Cómo puedo crear un clúster de Event Hubs dedicado?
Para obtener instrucciones paso a paso y más información sobre la configuración de un clúster de Event Hubs dedicado, consulte la [guía de inicio rápido: Creación de un clúster de Event Hubs dedicado mediante Azure Portal](event-hubs-dedicated-cluster-create-portal.md). 


[!INCLUDE [event-hubs-dedicated-clusters-faq](../../includes/event-hubs-dedicated-clusters-faq.md)]


## <a name="partitions"></a>Particiones

### <a name="how-many-partitions-do-i-need"></a>¿Cuántas particiones necesito?
El número de particiones se especifica en el momento de la creación y debe estar comprendido entre 1 y 32. El número de particiones no es modificable en ninguno de los niveles, excepto en el [nivel dedicado](event-hubs-dedicated-overview.md), por lo que debería tener en cuenta el escalado a largo plazo a la hora de configurar este número. Las particiones son un mecanismo de organización de datos relacionado con el paralelismo de bajada necesario para consumir las aplicaciones. El número de particiones de un centro de eventos está directamente relacionado con el número de lectores simultáneos que espera tener. Para más información sobre las particiones, consulte [Particiones](event-hubs-features.md#partitions).

Es posible que quiera establecer el valor lo más alto posible, que es 32, en el momento de la creación. Recuerde que, si hay más de una partición, los eventos se enviarán a varias particiones sin conservar el orden, a menos que configure los remitentes para que solo realicen el envío a una única partición de las 32, lo que hará que las 31 restantes sean redundantes. En el primer caso, tendrá que leer eventos en las 32 particiones. En el último caso, no hay ningún costo adicional obvio aparte de la configuración adicional que debe realizar en el host del procesador de eventos.

Event Hubs está diseñado para permitir un lector de partición única por grupo de consumidores. En la mayoría de los casos de uso, el valor predeterminado de cuatro particiones es suficiente. Si desea escalar el procesamiento de eventos, tal vez desee agregar particiones adicionales. No hay ningún límite de procesamiento específico en una partición, pero el número total de unidades de procesamiento limita el procesamiento agregado en el espacio de nombres. A medida que aumenta el número de unidades de procesamiento en el espacio de nombres, puede que necesite particiones adicionales para permitir que los lectores simultáneos logren su propio procesamiento máximo.

Pero si tiene un modelo en el que su aplicación tiene afinidad con una partición determinada, aumentar el número de particiones puede que no suponga ventaja alguna. Para más información, vea [Disponibilidad y coherencia](event-hubs-availability-and-consistency.md).

### <a name="increase-partitions"></a>Aumento de particiones
Puede enviar una solicitud de soporte técnico para solicitar que el número de particiones aumente a 40 (exacto). 

1. En la página **Espacio de nombres del bus de eventos**, seleccione **Nueva solicitud de soporte técnico** en el menú de la izquierda. 
1. En la página **Nueva solicitud de soporte técnico**, siga estos pasos:
    1. En el campo **Resumen**, describa el problema en pocas palabras. 
    1. Para **Tipo de problema**, seleccione **Cuota**. 
    1. En el campo **Subtipo de problema**, seleccione **Solicitud de cambio de partición**. 
    
        :::image type="content" source="./media/event-hubs-faq/support-request-increase-partitions.png" alt-text="Aumentar el recuento de particiones":::

El número de particiones se puede aumentar hasta 40 exactamente. En este caso, el número de unidades de procesamiento también debe aumentarse a 40. Si más adelante decide reducir el límite de unidades de procesamiento a un número menor que 20, el límite máximo de particiones también se reducirá a 32. 

La disminución en el número de particiones no afecta a los centros de eventos existentes, ya que las particiones se aplican en el nivel de centro de eventos y son inmutables después de crear el centro. 

## <a name="pricing"></a>Precios

### <a name="where-can-i-find-more-pricing-information"></a>¿Dónde puedo encontrar más información sobre precios?

Para obtener una completa información sobre los precios de Event Hubs, consulte los [detalles de precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>¿Hay un cargo por retener eventos de Event Hubs durante más de 24 horas?

El nivel Event Hubs estándar permite períodos de retención de mensajes superiores a 24 horas, hasta un máximo de siete días. Si el tamaño del número total de eventos almacenados supera la asignación de almacenamiento para el número de unidades de procesamiento seleccionadas (84 GB por unidad de procesamiento), el tamaño que supere la asignación se cargará con la tarifa publicada de almacenamiento de blobs de Azure. La asignación de almacenamiento en cada unidad de procesamiento cubre todos los costos de almacenamiento de los períodos de retención de 24 horas (valor predeterminado), incluso aunque la unidad de procesamiento se consuma hasta la asignación de entrada máxima.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>¿Cómo se calcula y se cobra el tamaño de almacenamiento de Event Hubs?

El tamaño total de todos los eventos almacenados, incluida la sobrecarga interna de encabezados de eventos o las estructuras de almacenamiento en disco de todos los centros de eventos, se mide a lo largo del día. Al final del día, se calcula el tamaño máximo de almacenamiento. La asignación de almacenamiento diario se calcula basándose en el número mínimo de unidades de procesamiento seleccionadas durante el día (cada unidad de procesamiento ofrece una asignación de 84 GB). Si el tamaño total supera la asignación de almacenamiento diaria calculada, el exceso de almacenamiento se factura con las tarifas de Azure Blob Storage (con la tarifa de **almacenamiento con redundancia local** ).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>¿Cómo se calculan los eventos de entrada de Event Hubs?

Cada evento enviado a un centro de eventos cuenta como mensaje facturable. Un *evento de entrada* se define como una unidad de datos que es menor o igual que 64 KB. Cualquier evento que tenga un tamaño menor o igual que 64 KB se considera un evento facturable. Si el evento es mayor de 64 KB, el número de eventos facturables se calcula según el tamaño del evento, en múltiplos de 64 KB. Por ejemplo, un evento de 8 KB enviado al centro de eventos se factura como un evento, pero un mensaje de 96 KB enviado al centro de eventos se factura como dos eventos.

Los eventos consumidos en un centro de eventos, así como las operaciones de administración y las llamadas de control (como los puntos de comprobación), no se cuentan como eventos de entrada facturables, pero se acumulan hasta llegar a las unidades de procesamiento permitidas.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>¿Los cargos por conexión desacoplada se aplican a Event Hubs?

Los cargos de conexión se aplican solo cuando se usa el protocolo AMQP. No hay ningún cargo de conexión por el envío de eventos mediante HTTP, independientemente del número de sistemas o dispositivos emisores. Si tiene previsto usar AMQP (por ejemplo, para conseguir un flujo de eventos más eficiente o para habilitar la comunicación bidireccional en escenarios de comando y control de IoT), vea la página de [información de precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) para obtener información sobre el número de conexiones incluidas en cada nivel de servicio.

### <a name="how-is-event-hubs-capture-billed"></a>¿Cómo se factura Event Hubs Capture?

Capture se habilita cuando algún centro de eventos del espacio de nombres tiene la opción Capture habilitada. Event Hubs Capture se factura por horas y por unidad de rendimiento comprada. A medida que el número de unidades de rendimiento aumenta o disminuye, la facturación de Event Hubs Capture refleja estos cambios en incrementos de horas completas. Para más información sobre Event Hubs Capture, vea los [detalles de los precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="do-i-get-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>¿Se me cobrará la cuenta de almacenamiento que seleccione para Event Hubs Capture?

Capture usa la cuenta de almacenamiento que se especifique al habilitarlo en un servicio centro de eventos. Dado que esta es su cuenta de almacenamiento, los cambios efectuados en esta configuración se facturan en su suscripción a Azure.

## <a name="quotas"></a>Cuotas

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>¿Hay alguna cuota asociada a los centros Event Hubs?

Para obtener una lista de todas las cuotas de los centros Event Hubs, consulte [Cuotas](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Solución de problemas

### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>¿Por qué no puedo crear un espacio de nombres después de eliminarlo de otra suscripción? 
Cuando elimine un espacio de nombres de una suscripción, espere 4 horas antes de volver a crearlo con el mismo nombre en otra suscripción. En caso contrario, es posible que reciba el mensaje de error siguiente: `Namespace already exists`. 

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>¿Cuáles son algunas de las excepciones generadas por los centros Event Hubs y sus acciones sugeridas?

Para obtener una lista de posibles excepciones de Event Hubs, consulte [Información general sobre excepciones](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Registros de diagnóstico

Event Hubs admite dos tipos de [registros de diagnóstico](event-hubs-diagnostic-logs.md): los registros de errores de Capture y los registros operativos. Ambos, se representan en JSON y se pueden activar a través de Azure Portal.

### <a name="support-and-sla"></a>SLA y soporte técnico

El soporte técnico para Event Hubs está disponible a través de la [página de preguntas y respuestas de Microsoft sobre Azure Service Bus](/answers/topics/azure-service-bus.html). Se ofrece de forma gratuita soporte técnico para la administración de suscripciones y la facturación.

Para más información sobre nuestro SLA, consulte la página [Acuerdos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/) .

## <a name="azure-stack-hub"></a>Azure Stack Hub

### <a name="how-can-i-target-a-specific-version-of-azure-storage-sdk-when-using-azure-blob-storage-as-a-checkpoint-store"></a>¿Cómo se puede establecer como destino una versión específica del SDK de Azure Storage cuando se usa Azure Blob Storage como un almacén de puntos de comprobación?
Si ejecuta este código en Azure Stack Hub, experimentará errores en tiempo de ejecución a menos que tenga como destino una versión específica de la API de Storage. Esto se debe a que el SDK de Event Hubs usa la API de Azure Storage más reciente disponible en Azure, que puede que no esté disponible en la plataforma de Azure Stack Hub. Azure Stack Hub puede admitir una versión diferente del SDK de Storage Blob que las que suelen estar disponibles en Azure. Si usa Azure Blob Storage como almacén de puntos de comprobación, confirme la [versión de la API de Azure Storage admitida para la compilación de Azure Stack Hub](/azure-stack/user/azure-stack-acs-differences?#api-version) y establezca esa versión como destino en el código. 

Por ejemplo, si trabaja en la versión 2005 de Azure Stack Hub, la versión más reciente disponible para el servicio Storage es la 2019-02-02. De forma predeterminada, la biblioteca de cliente del SDK de Event Hubs usa la versión más reciente disponible en Azure (2019-07-07 en el momento de la versión del SDK). En este caso, además de seguir los pasos de esta sección, también tendrá que agregar código para usar como destino la versión 2019-02-02 de la API del servicio Storage. Para ver un ejemplo de cómo establecer como destino una versión específica de la API de Storage, vea los ejemplos de C#, Java, Python y JavaScript/TypeScript.  

Para obtener un ejemplo de cómo establecer como destino una versión específica de la API de Storage, vea los ejemplos siguientes en GitHub: 

- [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/)
- [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithCustomStorageVersion.java)
- Python: [sincrónico](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/receive_events_using_checkpoint_store_storage_api_version.py), [asincrónico](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/receive_events_using_checkpoint_store_storage_api_version_async.py)
- [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript/receiveEventsWithApiSpecificStorage.js) y [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/typescript/src/receiveEventsWithApiSpecificStorage.ts)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Event Hubs, visite los vínculos siguientes:

* [Información general de Event Hubs](./event-hubs-about.md)
* [Creación de un Centro de eventos](event-hubs-create.md)
* [Inflado automático de Event Hubs](event-hubs-auto-inflate.md)
