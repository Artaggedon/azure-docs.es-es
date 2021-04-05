---
title: Notas de Reliable Actors sobre la serialización del tipo de actor
description: Examina los requisitos básicos para la definición de clases serializables que se pueden usar para definir interfaces y estados de Reliable Actors de Service Fabric
ms.topic: conceptual
ms.date: 11/02/2017
ms.custom: devx-track-csharp
ms.openlocfilehash: 2a1118e394046b217a288663659a2c910098e992
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96576016"
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Notas sobre la serialización del tipo de Reliable Actors de Service Fabric
Los argumentos de todos los métodos, los tipos de resultados de las tareas que devuelve cada método de una interfaz de actor y los objetos almacenados en el administrador de estados de un actor deben ser [serializables de contratos de datos](/dotnet/framework/wcf/feature-details/types-supported-by-the-data-contract-serializer). Esto también se aplica a los argumentos de los métodos definidos en [interfaces de eventos de actor](service-fabric-reliable-actors-events.md). (Los métodos de interfaz de eventos de actor siempre devuelven void).

## <a name="custom-data-types"></a>Tipos de datos personalizados
En este ejemplo, la siguiente interfaz de actor define un método que devuelve un tipo de datos personalizado denominado `VoicemailBox`:

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

```Java
public interface VoiceMailBoxActor extends Actor
{
    CompletableFuture<VoicemailBox> getMailBoxAsync();
}
```

La interfaz se implementa mediante un actor que utiliza el administrador de estado para almacenar un objeto `VoicemailBox`:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class VoiceMailBoxActorImpl extends FabricActor implements VoicemailBoxActor
{
    public VoiceMailBoxActorImpl(ActorService actorService, ActorId actorId)
    {
         super(actorService, actorId);
    }

    public CompletableFuture<VoicemailBox> getMailBoxAsync()
    {
         return this.stateManager().getStateAsync("Mailbox");
    }
}

```

En este ejemplo, el objeto `VoicemailBox` se serializa en los siguientes casos:

* El objeto se transmite entre una instancia de actor y un llamador.
* El objeto se guarda en el administrador de estados, donde se almacena en un disco y se replica en otros nodos.

El marco de Reliable Actors usa la serialización DataContract. Por lo tanto, los objetos de datos personalizados y sus miembros se deben anotar con los atributos **DataContract** y **DataMember**, respectivamente.

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```
```Java
public class Voicemail implements Serializable
{
    private static final long serialVersionUID = 42L;

    private UUID id;                    //getUUID() and setUUID()

    private String message;             //getMessage() and setMessage()

    private GregorianCalendar receivedAt; //getReceivedAt() and setReceivedAt()
}
```


```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```
```Java
public class VoicemailBox implements Serializable
{
    static final long serialVersionUID = 42L;
    
    public VoicemailBox()
    {
        this.messageList = new ArrayList<Voicemail>();
    }

    private List<Voicemail> messageList;   //getMessageList() and setMessageList()

    private String greeting;               //getGreeting() and setGreeting()
}
```


## <a name="next-steps"></a>Pasos siguientes
* [Ciclo de vida de un actor y recolección de elementos no utilizados](service-fabric-reliable-actors-lifecycle.md)
* [Recordatorios y temporizadores de los actores](service-fabric-reliable-actors-timers-reminders.md)
* [Eventos de actor](service-fabric-reliable-actors-events.md)
* [Reentrada de actor](service-fabric-reliable-actors-reentrancy.md)
* [Polimorfismo de actores y patrones de diseño orientado a objetos](service-fabric-reliable-actors-polymorphism.md)
* [Supervisión del rendimiento y diagnósticos de los actores](service-fabric-reliable-actors-diagnostics.md)
