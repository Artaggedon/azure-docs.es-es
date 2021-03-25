---
title: Información general sobre el ciclo de vida de Reliable Services
description: Conozca los eventos de ciclo de vida de una aplicación de Reliable Services en Azure Service Fabric para servicios con estado y sin estado.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 6ea8fa6933052374721d8d205d5b07386c807ae2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98784603"
---
# <a name="reliable-services-lifecycle-overview"></a>Información general del ciclo de vida de Reliable Services
> [!div class="op_single_selector"]
> * [C# en Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java en Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Al analizar los ciclos de vida de Reliable Services de Azure Service Fabric, los conceptos básicos del ciclo de vida son lo más importante. En general, el ciclo de vida incluye lo siguiente:

- Durante el inicio:
  - Se construyen los servicios.
  - Los servicios tienen la oportunidad de construir y devolver cero o más agentes de escucha.
  - Se abre cualquier agente de escucha devuelto, lo cual permite la comunicación con el servicio.
  - Se llama al método **RunAsync** del servicio, lo cual permite a este último realizar tareas de ejecución prolongada o trabajo en segundo plano.
- Durante el cierre:
  - Se cancela el token de cancelación pasado a **RunAsync** y se cierran los agentes de escucha.
  - Una vez cerrados los agentes de escucha, se destruye el propio objeto de servicio.

Hay información sobre la ordenación exacta de estos eventos. El orden de eventos puede cambiar ligeramente dependiendo de si Reliable Services tiene o no tiene estado. Además, para los servicios con estado, tenemos que tratar con el escenario de intercambio principal. Durante esta secuencia, el rol Principal se transfiere a otra réplica (o se devuelve) sin que el servicio tenga que cerrarse. Por último, hay que pensar en condiciones de error.

## <a name="stateless-service-startup"></a>Inicio de un servicio sin estado
El ciclo de vida de un servicio sin estado es sencillo. Este es el orden de los eventos:

1. Se construye el servicio.
2. A continuación, suceden dos cosas simultáneamente:
    - Se invoca `StatelessService.CreateServiceInstanceListeners()` y se abren los agentes de escucha devueltos. Se llama a `ICommunicationListener.OpenAsync()` en cada uno de los agentes de escucha.
    - Se llama al método `StatelessService.RunAsync()` del servicio.
3. Si está presente, se llama al método `StatelessService.OnOpenAsync()` del servicio. Esta llamada es una invalidación poco habitual, pero está disponible. En este momento, se pueden iniciar las tareas de inicialización del servicio ampliado.

Tenga en cuenta que no hay ninguna ordenación entre las llamadas para crear y abrir los agentes de escucha y **RunAsync**. Los agentes de escucha se pueden abrir antes de iniciar **RunAsync**. De forma similar, se puede invocar **RunAsync** antes de que se abran o incluso de que se construyan los agentes de escucha de comunicación. Si se necesita cualquier sincronización, se deja como un ejercicio para el implementador. Estas son algunas de las soluciones habituales:

  - En ocasiones, los agentes de escucha no pueden funcionar hasta que se crea otra información o se realiza un trabajo. En el caso de los servicios sin estado, normalmente, ese trabajo puede hacerse en otras ubicaciones, como las siguientes: 
    - en el constructor del servicio,
    - durante la llamada a `CreateServiceInstanceListeners()` o
    - durante la construcción del propio agente de escucha.
  - En ocasiones, el código de **RunAsync** no se inicia hasta que están abiertos los agentes de escucha. En este caso, hace falta coordinación adicional. Una solución habitual es incluir una marca dentro de los agentes de escucha que indique cuándo han finalizado. Esta marca se comprueba después en **RunAsync**, antes de continuar con el verdadero trabajo.

## <a name="stateless-service-shutdown"></a>Cierre de un servicio sin estado
Al cerrar un servicio sin estado, se sigue el mismo patrón, solo que en orden inverso:

1. En paralelo:
    - Todos los agentes de escucha abiertos se cierran. Se llama a `ICommunicationListener.CloseAsync()` en cada uno de los agentes de escucha.
    - Se cancela el token de cancelación que se pasó a `RunAsync()`. La comprobación de la propiedad `IsCancellationRequested` del token de cancelación devuelve el valor True y, si se llama al método `ThrowIfCancellationRequested` del token, se inicia una excepción `OperationCanceledException`.
2. Una vez que `CloseAsync()` finaliza en cada agente de escucha y tras finalizar también `RunAsync()`, se llama al método `StatelessService.OnCloseAsync()` del servicio, si está presente.  Se llama a OnCloseAsync cuando la instancia de servicio sin estado se va a apagar correctamente. Esto puede ocurrir cuando se está actualizando el código de servicio, cuando se está moviendo la instancia de servicio debido al equilibrio de carga o se detecta un error transitorio. No es frecuente invalidar `StatelessService.OnCloseAsync()`, pero puede utilizarse para cerrar de manera segura los recursos, detener el procesamiento en segundo plano, terminar de guardar el estado externo o cerrar las conexiones existentes.
3. Una vez finalizado `StatelessService.OnCloseAsync()`, se destruye el objeto de servicio.

## <a name="stateful-service-startup"></a>Inicio de un servicio con estado
Los servicios con estado tienen un patrón similar al de los servicios sin estado, con unos pocos cambios. Al iniciarse un servicio con estado, el orden de los eventos es el siguiente:

1. Se construye el servicio.
2. Se llama a `StatefulServiceBase.OnOpenAsync()`. Por lo general, la llamada no se invalida en el servicio.
3. Las siguientes operaciones tienen lugar en paralelo:
    - Se invoca a `StatefulServiceBase.CreateServiceReplicaListeners()`. 
      - Si se trata de un servicio principal, se abren todos los agentes de escucha devueltos. Se llama a `ICommunicationListener.OpenAsync()` en cada uno de los agentes de escucha.
      - Si se trata de un servicio secundario, solamente se abren los agentes de escucha con la marca `ListenOnSecondary = true`. No es muy frecuente tener agentes de escucha abiertos en servicios secundarios.
    - Si en la actualidad el servicio es un servicio principal, se llama al método `StatefulServiceBase.RunAsync()` del servicio.
4. Una vez que se han completado todas las llamadas a `OpenAsync()` de los agentes de escucha de la réplica y se ha llamando a `RunAsync()`, se llama a `StatefulServiceBase.OnChangeRoleAsync()`. Por lo general, la llamada no se invalida en el servicio.

Al igual que en los servicios sin estado, no existe relación entre el orden en el que se crean y se abren los agentes de escucha y el momento en que se llama a **RunAsync**. Si necesita que estas operaciones estén coordinadas, las soluciones serían prácticamente iguales. Hay un caso adicional para servicios con estado. Imaginemos que las llamadas que llegan a los agentes de escucha de comunicación requieren que la información se mantenga dentro de alguna instancia de [Reliable Collections](service-fabric-reliable-services-reliable-collections.md).

   > [!NOTE]  
   > Dado que es posible que los agentes de escucha de comunicación se abrieran antes de que se pudiera leer o escribir en las colecciones confiables, y antes de que se pudiera iniciar **RunAsync**, se necesita coordinación adicional. La solución más sencilla y habitual consiste en que los agentes de escucha de comunicación devuelvan un código de error que permita informar al cliente para que intente la solicitud de nuevo.

## <a name="stateful-service-shutdown"></a>Cierre de un servicio con estado
De forma similar a los servicios sin estado, los eventos de ciclo de vida durante el cierre son los mismos que durante el inicio, pero a la inversa. Cuando un servicio con estado se está cerrando, se producen los siguientes eventos:

1. En paralelo:
    - Todos los agentes de escucha abiertos se cierran. Se llama a `ICommunicationListener.CloseAsync()` en cada uno de los agentes de escucha.
    - Se cancela el token de cancelación que se pasó a `RunAsync()`. La comprobación de la propiedad `IsCancellationRequested` del token de cancelación devuelve el valor True y, si se llama al método `ThrowIfCancellationRequested` del token, se inicia una excepción `OperationCanceledException`.
2. Una vez que `CloseAsync()` finaliza en cada agente de escucha y tras finalizar también `RunAsync()`, se llama al método `StatefulServiceBase.OnChangeRoleAsync()` del servicio. Por lo general, la llamada no se invalida en el servicio.

   > [!NOTE]  
   > Solo es necesario esperar a que finalice **RunAsync** si esta réplica es una réplica principal.

3. Una vez que finaliza el método `StatefulServiceBase.OnChangeRoleAsync()`, se llama al método `StatefulServiceBase.OnCloseAsync()`. Esta llamada es una invalidación poco habitual, pero está disponible.
3. Una vez finalizado `StatefulServiceBase.OnCloseAsync()`, se destruye el objeto de servicio.

## <a name="stateful-service-primary-swaps"></a>Intercambios de servicios con estado principales
Durante la ejecución de un servicio con estado, solo se abren los agentes de escucha de comunicación y se llama al método **RunAsync** de los servicios con estado principales. Se construyen réplicas secundarias, pero no reciben más llamadas. Durante la ejecución de un servicio con estado, la réplica que es actualmente la principal puede cambiar como resultado de la optimización de equilibrio de clúster o un error. ¿Cómo afecta esto a los eventos de ciclo de vida que puede ver una réplica? El comportamiento de la réplica con estado depende de si es la réplica que está subiendo o bajando de categoría.

### <a name="for-the-primary-thats-demoted"></a>En el caso de que la principal baje de categoría
Si la réplica principal baja de categoría, Service Fabric necesita que esta réplica deje de procesar mensajes y que cierre cualquier trabajo que esté realizando en segundo plano. Como resultado, este paso es similar al del servicio cuando se cierra. Se diferencia en que el servicio no se destruye ni se cierra, puesto que permanece como un elemento secundario. Se llama a las API siguientes:

1. En paralelo:
    - Todos los agentes de escucha abiertos se cierran. Se llama a `ICommunicationListener.CloseAsync()` en cada uno de los agentes de escucha.
    - Se cancela el token de cancelación que se pasó a `RunAsync()`. La comprobación de la propiedad `IsCancellationRequested` del token de cancelación devuelve el valor True y, si se llama al método `ThrowIfCancellationRequested` del token, se inicia una excepción `OperationCanceledException`.
2. Una vez que `CloseAsync()` finaliza en cada agente de escucha y tras finalizar también `RunAsync()`, se llama al método `StatefulServiceBase.OnChangeRoleAsync()` del servicio. Por lo general, la llamada no se invalida en el servicio.

### <a name="for-the-secondary-thats-promoted"></a>En el caso de que la secundaria suba de categoría
De igual modo, Service Fabric necesita que la réplica secundaria que sube de categoría comience a escuchar los mensajes de la conexión e inicie las tareas en segundo plano que debe completar. Como resultado, este proceso es similar a cuando se crea el servicio, salvo que la propia réplica ya existe. Se llama a las API siguientes:

1. En paralelo:
    - Se invoca `StatefulServiceBase.CreateServiceReplicaListeners()` y se abren los agentes de escucha devueltos. Se llama a `ICommunicationListener.OpenAsync()` en cada uno de los agentes de escucha.
    - Se llama al método `StatefulServiceBase.RunAsync()` del servicio.
2. Una vez que se han completado todas las llamadas a `OpenAsync()` de los agentes de escucha de la réplica y se ha llamando a `RunAsync()`, se llama a `StatefulServiceBase.OnChangeRoleAsync()`. Por lo general, la llamada no se invalida en el servicio.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Problemas habituales durante la degradación de la categoría principal y el cierre de un servicio con estado
Service Fabric cambia la categoría principal del un servicio con estado por diferentes razones. La más frecuente es el [reequilibrio de clusteres](service-fabric-cluster-resource-manager-balancing.md) y la [actualización de aplicaciones](service-fabric-application-upgrade.md). Durante estas operaciones (así como durante el cierre normal del servicio, al igual que ocurriría si el servicio se eliminara), es importante que el servicio respete el objeto `CancellationToken`. 

Los servicios que no administren correctamente la cancelación pueden experimentar errores. El rendimiento de estas operaciones es lento, ya que Service Fabric espera que los servicios se detengan sin que se produzcan errores. En última instancia, esto puede producir errores en las actualizaciones, que pueden agotar el tiempo de espera o revertirse. Si no se respeta el token de cancelación, también puede producirse un desequilibrio en los clústeres. Este desequilibrio puede producirse porque los nodos pueden sobrecargarse sin que los servicios puedan reequilibrarse, dado que se tarda algún tiempo en transferirlos a otro sitio. 

Como los servicios tienen estado, también es probable que utilicen [Reliable Collections](service-fabric-reliable-services-reliable-collections.md). En Service Fabric, cuando un servicio principal se degrada de nivel, una de las primeras cosas que ocurren es que se revoca el acceso de escritura al estado subyacente. Esto genera una serie problemas secundarios que pueden afectar al ciclo de vida del servicio. Las colecciones devuelven excepciones en función del momento elegido y de si la réplica se está transfiriendo a otro lugar o cerrando. Estas excepciones deberían administrarse correctamente. Las excepciones iniciadas por Service Fabric pueden corresponder a dos categorías: permanentes [(`FabricException`)](/dotnet/api/system.fabric.fabricexception) y transitorias [(`FabricTransientException`)](/dotnet/api/system.fabric.fabrictransientexception). Las excepciones permanentes tienen que registrarse e iniciarse, mientras que las transitorias pueden volver a intentarse con arreglo a una determinada lógica de recuperación.

Administrar las excepciones que proceden del uso de `ReliableCollections` junto con los acontecimientos del ciclo de vida del servicio es una parte importante del proceso de comprobación y validación de un servicio de Reliable Services. Se recomienda ejecutar siempre el servicio con carga mientras se realizan actualizaciones y [pruebas de caos](service-fabric-controlled-chaos.md) antes de realizar una implementación en producción. Estos pasos básicos ayudan a garantizar que el servicio está implementado correctamente y puede administrar los eventos del ciclo de vida correctamente.


## <a name="notes-on-the-service-lifecycle"></a>Notas sobre el ciclo de vida del servicio
  - Tanto el método `RunAsync()` como las llamadas a `CreateServiceReplicaListeners/CreateServiceInstanceListeners` son opcionales. Un servicio puede tener uno, ambos o ninguno. Por ejemplo, si el servicio realiza todo su trabajo en respuesta a las llamadas del usuario, no es necesario que implemente `RunAsync()`. Solo son necesarios los agentes de escucha de comunicación y el código asociado. Del mismo modo, la creación y devolución de los agentes de escucha de comunicación es opcional, ya que el servicio tal vez solo tiene que realizar trabajo en segundo plano, por lo que solo necesita implementar `RunAsync()`.
  - Un servicio puede completar `RunAsync()` correctamente y volver desde este. La finalización no es una condición de error. La finalización de `RunAsync()` indica que el trabajo en segundo plano del servicio ha finalizado. En el caso de los servicios con estado de Reliable Services, se llamaría de nuevo a `RunAsync()` si la réplica se degradara de la categoría principal a la secundaria y se promoviera de nuevo a la categoría principal.
  - Si un servicio sale de `RunAsync()` iniciando una excepción inesperada, se considera un error. El objeto de servicio se cierra y se notifica el error de estado.
  - Aunque no hay un límite de tiempo para que se devuelvan resultados de estos métodos, se pierde inmediatamente la posibilidad de escribir en Reliable Collections y, por lo tanto, no se puede realizar ningún trabajo real. Se recomienda que devuelva resultados lo antes posible tras recibir la solicitud de cancelación. Si su servicio no responde a estas llamadas a la API en un intervalo de tiempo razonable, Service Fabric puede forzar la finalización del servicio. Esto solo suele pasar durante las actualizaciones de aplicación o cuando se elimina un servicio. Este tiempo de espera es de 15 minutos de manera predeterminada.
  - Los errores en la ruta `OnCloseAsync()` dan como resultado una llamada a `OnAbort()`, que es una oportunidad como último recurso para hacer todo lo posible para que el servicio limpie y libere cualquier recurso que haya reclamado. Normalmente se llama cuando se detecta un error permanente en el nodo o cuando Service Fabric no puede administrar el ciclo de vida de la instancia de servicio debido a errores internos.
  - Se llama a `OnChangeRoleAsync()` cuando la réplica del servicio con estado cambia de rol, por ejemplo, a principal o secundario. Las réplicas principales reciben el estado de escritura (se les permite crear y escribir en Reliable Collections). Las réplicas secundarias reciben el estado de lectura (solo pueden leer desde Reliable Collections existentes). La mayoría del trabajo en un servicio con estado se lleva a cabo en la réplica principal. Las réplicas secundarias pueden realizar validación de solo lectura, generación de informes, minería de datos u otros trabajos de solo lectura.

## <a name="next-steps"></a>Pasos siguientes
- [Introducción a Reliable Services](service-fabric-reliable-services-introduction.md)
- [Introducción a Reliable Services de Service Fabric de Microsoft Azure](service-fabric-reliable-services-quick-start.md)
- [Réplicas e instancias](service-fabric-concepts-replica-lifecycle.md)
