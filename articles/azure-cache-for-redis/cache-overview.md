---
title: ¿Qué es Azure Cache for Redis?
description: Información sobre Azure Cache for Redis para permitir cache-aside, el almacenamiento en caché de contenido, el almacenamiento en caché de la sesión de usuario, el trabajo y la cola de mensajes, así como las transacciones distribuidas.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 02/08/2021
ms.openlocfilehash: 49a697e6c3a6a6c931f2bb9c545647e2d6f1322d
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106056664"
---
# <a name="about-azure-cache-for-redis"></a>Acerca de Azure Cache for Redis
Azure Cache for Redis proporciona un almacén de datos en memoria basado en el software de [Redis](https://redis.io/). Redis mejora el rendimiento y la escalabilidad de una aplicación que use en gran medida los almacenes de datos de back-end. Es capaz de procesar grandes volúmenes de solicitudes de aplicación al mantener los datos a los que se accede con frecuencia en la memoria del servidor en la que se puede escribir y leer rápidamente. Redis incorpora una solución crítica de almacenamiento de datos de baja latencia y alto rendimiento en las aplicaciones modernas.

Azure Cache for Redis ofrece tanto el producto de código abierto (OSS Redis) como el producto comercial de Redis Labs (Redis Enterprise) como servicio administrado. Proporciona instancias de servidor Redis seguras y dedicadas, así como una compatibilidad completa con la API de Redis. Microsoft ofrece el servicio, que se hospeda en Azure, y cualquier aplicación de dentro o fuera de Azure puede acceder a él.

Azure Cache for Redis se puede usar como una memoria caché de datos distribuidos o de contenido, un almacén de sesión, un agente de mensajes, entre otras opciones. Se puede implementar de forma independiente o junto con otro servicio de base de datos de Azure, como Azure SQL o Cosmos DB.

## <a name="key-scenarios"></a>Escenarios principales
Azure Cache for Redis mejora el rendimiento de las aplicaciones, ya que admite patrones de arquitectura de aplicaciones comunes. Estos son algunos de los más comunes:

| Patrón      | Descripción                                        |
| ------------ | -------------------------------------------------- |
| [Caché de datos](cache-web-app-cache-aside-leaderboard.md) | Las bases de datos suelen ser demasiado grandes para cargarlas directamente en una caché. Es habitual usar el patrón [cache-aside](/azure/architecture/patterns/cache-aside) para cargar datos en la memoria caché solo cuando es necesario. Cuando el sistema realiza cambios en los datos, también puede actualizar la caché, que se distribuye luego a otros clientes. Además, el sistema puede establecer una fecha de expiración en los datos o usar una directiva de expulsión para desencadenar las actualizaciones de los datos en la memoria caché.|
| [Caché de contenido](cache-aspnet-output-cache-provider.md) | Muchas páginas web se generan a partir de plantillas que usan contenido estático como encabezados, pies de página y banners. Estos elementos estáticos no deberían cambiar a menudo. El uso de una caché en memoria proporciona acceso rápido a contenido estático en comparación con los almacenes de datos de back-end. Este patrón reduce el tiempo de procesamiento y la carga del servidor, lo que permite que los servidores web tengan mayor capacidad de respuesta. Puede permitirle reducir el número de servidores necesarios para administrar las cargas. Azure Cache for Redis proporciona el proveedor de caché de salida de Redis, que admite este patrón con ASP.NET.|
| [Almacén de sesión](cache-aspnet-session-state-provider.md) | Este patrón se utiliza normalmente con carros de la compra y otros datos de historial del usuario que una aplicación web puede desear asociar con las cookies del usuario. El almacenamiento de demasiados datos en una cookie puede tener un impacto negativo en el rendimiento, ya que aumenta su tamaño y no hay que olvidar que se pasa y se valida con cada solicitud. Una solución habitual usa la cookie como clave cuando se consultan datos en una base de datos. El uso de una memoria caché en memoria, como Azure Redis Cache, para asociar información a un usuario es mucho más rápido que su interacción con una base de datos relacional completa. |
| Almacenamiento en cola de trabajos y mensajes | Las aplicaciones agregan a menudo tareas a una cola cuando las operaciones asociadas a la solicitud tardan tiempo en ejecutarse. Las operaciones con ejecuciones más largas se ponen en cola para procesarse en secuencia, a menudo por parte de otro servidor.  Este método de aplazar trabajo se denomina puesta en cola de tareas. Azure Cache for Redis proporciona una cola distribuida que habilita este patrón en la aplicación.|
| Distributed transactions | A veces, las aplicaciones requieren una serie de comandos en un almacén de datos de back-end para ejecutarse como una única operación atómica. El resultado de todos los comandos debe ser satisfactorio, o todos deben revertirse al estado inicial. Azure Cache for Redis admite la ejecución de un lote de comandos como [transacción](https://redis.io/topics/transactions) única. |

## <a name="redis-versions"></a>Versiones de Redis

Azure Cache for Redis admite la versión 4.x de OSS Redis y la versión preliminar 6.0. Hemos decidido omitir Redis 5.0 para proporcionarle la versión más reciente. Anteriormente, Azure Cache for Redis mantenía una versión única de Redis. Con vistas al futuro, se proporcionará una nueva actualización de la versión principal y al menos una versión anterior estable. El usuario puede [elegir qué versión](cache-how-to-version.md) funciona mejor para su aplicación.


## <a name="service-tiers"></a>Niveles de servicio
Azure Redis Cache está disponible en los niveles siguientes:

| Nivel | Descripción |
|---|---|
| Básico | Una memoria caché de OSS Redis que se ejecuta en una sola máquina virtual. Este nivel no tiene contrato de nivel de servicio (SLA) y es ideal para las cargas de trabajo de desarrollo y pruebas y no críticas. |
| Estándar | Una memoria caché de OSS Redis que se ejecuta en dos máquinas virtuales en una configuración replicada. |
| Premium | Memorias caché de OSS Redis de alto rendimiento. Este nivel ofrece mayor rendimiento, menor latencia, mejor disponibilidad y más características. Las memorias caché Premium se implementan en máquinas virtuales más eficaces en comparación con las de las memorias caché del nivel Básico o Estándar. |
| Enterprise | Memorias caché de alto rendimiento con la tecnología del software Redis Enterprise de Redis Labs. Este nivel admite módulos de Redis, incluidos RediSearch, RedisBloom y RedisTimeSeries. Además, ofrece una disponibilidad aún mayor que el nivel Premium. |
| Enterprise Flash | Memorias caché de gran tamaño y rentabilidad basadas en el software Redis Enterprise de Redis Labs. Este nivel amplía el almacenamiento de datos de Redis en memoria no volátil, que es más barata que DRAM, en una máquina virtual. Reduce el costo total de memoria por GB. |

### <a name="feature-comparison"></a>Comparación de características
En [Precios de Azure Cache for Redis](https://azure.microsoft.com/pricing/details/cache/) encontrará una comparación detallada de cada nivel. La tabla siguiente le ayuda a describir algunas de las características que admite cada nivel:

| Descripción de la característica | Básico | Estándar | Premium | Enterprise | Enterprise Flash |
| ------------------- | :-----: | :------: | :---: | :---: | :---: |
| [Acuerdo de Nivel de Servicio (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |-|✔|✔|✔|✔|
| Cifrado de datos |✔|✔|✔|✔|✔|
| [Aislamiento de red](cache-how-to-premium-vnet.md) |✔|✔|✔|✔|✔|
| [Escalado](cache-how-to-scale.md) |✔|✔|✔|✔|✔|
| [Clúster de OSS](cache-how-to-premium-clustering.md) |-|-|✔|✔|✔|
| [Persistencia de los datos](cache-how-to-premium-persistence.md) |-|-|✔|Vista previa|Vista previa|
| [Redundancia de zona](cache-how-to-zone-redundancy.md) |-|-|Vista previa|✔|✔|
| [Replicación geográfica](cache-how-to-geo-replication.md) |-|-|✔|Vista previa|Vista previa|
| [Módulos](https://redis.io/modules) |-|-|-|✔|✔|
| [Import/Export](cache-how-to-import-export-data.md) |-|-|✔|✔|✔|
| [Actualizaciones programadas](cache-administration.md#schedule-updates) |✔|✔|✔|-|-|

### <a name="choosing-the-right-tier"></a>Elección del nivel correcto
Debe tener en cuenta lo siguiente al elegir un nivel de Azure Cache for Redis:

* **Memoria**: los niveles Básico y Estándar ofrecen de 250 MB a 53 GB, el nivel Premium de 6 GB a 1,2 TB y los niveles Enterprise de 12 GB a 14 TB.  Para crear una memoria caché de nivel Premium superior a 120 GB, puede usar la agrupación en clústeres de OSS Redis. Para más información, consulte [Precios de Azure Cache for Redis](https://azure.microsoft.com/pricing/details/cache/). Para más información, consulte [How to configure clustering for a Premium Azure Cache for Redis](cache-how-to-premium-clustering.md) (Configuración de la agrupación en clústeres para una instancia de Azure Cache for Redis Prémium).
* **Rendimiento**: las memorias caché de los niveles Premium y Enterprise se implementan en hardware que tiene procesadores más rápidos y ofrecen un mejor rendimiento en comparación con el nivel Básico o Estándar. Las cachés de nivel Premium tienen un mayor rendimiento y latencias más bajas. Para más información, consulte [Rendimiento de Azure Cache for Redis](cache-planning-faq.md#azure-cache-for-redis-performance).
* **Núcleo específico para el servidor de Redis**: Todas las memorias caché excepto C0 ejecutan núcleos de máquinas virtuales dedicados. Redis, por diseño, solo usa un subproceso para el procesamiento de comandos. Azure Cache for Redis emplea núcleos adicionales para el procesamiento de E/S. El hecho de tener más núcleos mejora el rendimiento del procesamiento, aunque pueda no producir un escalado lineal. Además, los tamaños de máquina virtual mayores suelen tener límites de ancho de banda también mayores. De este modo, contribuye a evitar la saturación de la red, lo que provocará tiempos de espera en la aplicación.
* **Rendimiento de la red**: si tiene una carga de trabajo que requiere un rendimiento alto, el nivel Premium y el nivel Enterprise ofrecen más ancho de banda en comparación con los niveles Estándar o Básico. También dentro de cada nivel, las cachés de mayor tamaño tienen más ancho de banda debido a la máquina virtual subyacente que hospeda la memoria caché. Para más información, consulte [Rendimiento de Azure Cache for Redis](cache-planning-faq.md#azure-cache-for-redis-performance).
* **Número máximo de conexiones de cliente**: el nivel Prémium ofrece el número máximo de clientes que se pueden conectar a Redis, con un número mayor de conexiones para memorias caché de mayor tamaño. La agrupación en clústeres no aumenta el número de conexiones disponibles para una caché en clúster. Para más información, consulte [Precios de Azure Cache for Redis](https://azure.microsoft.com/pricing/details/cache/).
* **Alta disponibilidad**: Azure Cache for Redis ofrece varias opciones de [alta disponibilidad](cache-high-availability.md), y garantiza la disponibilidad de una memoria caché Estándar, Premium o Enterprise de acuerdo con nuestro [Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). El Acuerdo de Nivel de Servicio solo cubre la conectividad para los puntos de conexión de la memoria caché. El SLA no cubre la protección frente a la pérdida de datos. Se recomienda usar la característica de persistencia de datos de Redis en los niveles Prémium y Enterprise para aumentar la resistencia contra la pérdida de datos.
* **Persistencia de los datos**: Los niveles Prémium y Enterprise permiten conservar los datos de la caché en una cuenta de Azure Storage y en un disco administrado, respectivamente. Los problemas con la infraestructura subyacente podrían provocar una pérdida de datos. Se recomienda usar la característica de persistencia de datos de Redis en estos niveles para aumentar la resistencia contra la pérdida de datos. Azure Cache for Redis ofrece las opciones RDB y AOF (versión preliminar). La persistencia de los datos se puede habilitar mediante Azure Portal y la CLI de Azure. Para obtener información acerca del nivel Prémium, consulte [Configuración de la persistencia de datos en el nivel Prémium de Azure Cache for Redis](cache-how-to-premium-persistence.md).
* **Aislamiento de red**: Las implementaciones de Azure Private Link y Virtual Network (VNET) proporcionan seguridad mejorada y aislamiento del tráfico para la instancia de Azure Cache for Redis. La red virtual le permite restringir aún más el acceso mediante directivas de control de acceso a la red. Para más información, consulte [Azure Cache for Redis con Azure Private Link (versión preliminar pública)](cache-private-link.md) y [Configuración de la compatibilidad de red virtual con el nivel Premium de Azure Cache for Redis](cache-how-to-premium-vnet.md).
* **Extensibilidad**: Los niveles Enterprise admiten [RediSearch](https://docs.redislabs.com/latest/modules/redisearch/), [RedisBloom](https://docs.redislabs.com/latest/modules/redisbloom/) y [RedisTimeSeries](https://docs.redislabs.com/latest/modules/redistimeseries/). Estos módulos agregan nuevos tipos de datos y funcionalidad a Redis.

Puede escalar la memoria caché desde el nivel Básico hasta el nivel Premium una vez que se haya creado. Sin embargo, actualmente no se admite su reducción vertical a un nivel inferior. Para instrucciones detalladas acerca del escalado, consulte [How to Scale Azure Cache for Redis](cache-how-to-scale.md) (Escalado de Azure Redis Cache) y [How to automate a scaling operation](cache-how-to-scale.md#how-to-automate-a-scaling-operation) (Automatización de una operación de escalado).

### <a name="special-considerations-for-enterprise-tiers"></a>Consideraciones especiales para los niveles Enterprise

Los niveles Enterprise usan Redis Enterprise, una variante comercial de Redis creada por Redis Labs. Los clientes obtendrán y pagarán una licencia para este software mediante una oferta de Azure Marketplace. Azure Cache for Redis facilitará la adquisición de licencias para que no tenga que hacerlo por separado. Para realizar la compra en Azure Marketplace, debe cumplir los siguientes requisitos previos:
* La suscripción de Azure tiene un instrumento de pago válido. No se admiten los créditos de Azure ni las suscripciones gratuitas de MSDN.
* Su organización permite [compras en Azure Marketplace](../cost-management-billing/manage/ea-azure-marketplace.md#enabling-azure-marketplace-purchases).
* Si usa un Marketplace privado, debe contener la oferta Enterprise de Redis Labs.

> [!IMPORTANT]
> La caché de Azure Cache for Redis Enterprise requiere equilibradores de carga de red estándar que se cobran por separado de las propias instancias de caché. Consulte los [precios de Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/) para obtener más detalles. Si se configura una caché de nivel Enterprise para varias zonas de disponibilidad, la transferencia de datos se facturará según las [tarifas de ancho de banda de red estándar](https://azure.microsoft.com/pricing/details/bandwidth/) a partir del 1 de julio de 2021.
>
> Además, la persistencia de datos incorpora Managed Disks. El uso de estos recursos será gratuito durante la versión preliminar pública de la persistencia de datos de nivel Enterprise. Esto puede cambiar cuando la característica esté disponible con carácter general.
>
>

## <a name="next-steps"></a>Pasos siguientes
* [Creación de una caché en Redis de código abierto](quickstart-create-redis.md)
* [Creación de una caché de Redis Enterprise](quickstart-create-redis-enterprise.md)
* [Uso de Azure Cache for Redis con una aplicación web de ASP.NET](cache-web-app-howto.md)
* [Uso de Azure Cache for Redis con .NET Core](cache-dotnet-core-quickstart.md)
* [Uso de Azure Cache for Redis con .NET Framework](cache-dotnet-how-to-use-azure-redis-cache.md)
* [Uso de Azure Cache for Redis con Node.js](cache-nodejs-get-started.md)
* [Uso de Azure Cache for Redis con Java](cache-java-get-started.md)
* [Uso de Azure Cache for Redis con Python](cache-python-get-started.md)