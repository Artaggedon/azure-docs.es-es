---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: a8cfdc76694d52acee70cde0e3f1697cd8129d06
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97691928"
---
## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Un recurso implementado de Communication Services. [Cree un recurso de Communication Services](../../create-communication-resource.md).
- `User Access Token` para habilitar el cliente de llamada. Para más información sobre [cómo obtener un `User Access Token`](../../access-tokens.md)
- Opcional: Complete el inicio rápido para [empezar a agregar llamadas a su aplicación](../getting-started-with-calling.md)

## <a name="setting-up"></a>Instalación

### <a name="creating-the-xcode-project"></a>Creación del proyecto de Xcode

En Xcode, cree un proyecto de iOS nuevo y seleccione la plantilla **Aplicación de vista única**. En este inicio rápido se usa el [marco SwiftUI](https://developer.apple.com/xcode/swiftui/), por lo que debe establecer el **lenguaje** en **Swift** y la **interfaz de usuario** en **SwiftUI**. No va a crear pruebas unitarias ni pruebas de interfaz de usuario durante este inicio rápido. No dude en desactivar la opción **Include Unit Tests** (Incluir pruebas unitarias) y desactive también la opción **Include UI Tests** (Incluir pruebas de interfaz de usuario).

:::image type="content" source="../media/ios/xcode-new-ios-project.png" alt-text="Captura de pantalla que muestra la ventana de creación del proyecto nuevo dentro de Xcode.":::

### <a name="install-the-package-and-dependencies-with-cocoapods"></a>Instalación del paquete y las dependencias con CocoaPods

1. Cree un podfile para la aplicación, de la siguiente manera:

   ```
   platform :ios, '13.0'
   use_frameworks!
   target 'AzureCommunicationCallingSample' do
     pod 'AzureCommunicationCalling', '~> 1.0.0-beta.5'
     pod 'AzureCommunication', '~> 1.0.0-beta.5'
     pod 'AzureCore', '~> 1.0.0-beta.5'
   end
   ```

2. Ejecute `pod install`.
3. Abra `.xcworkspace` con XCode.

### <a name="request-access-to-the-microphone"></a>Solicitud de acceso al micrófono

Para acceder al micrófono del dispositivo, debe actualizar la lista de propiedades de información de la aplicación con el elemento `NSMicrophoneUsageDescription`. Puede establecer el valor asociado al elemento `string` que se incluirá en el cuadro de diálogo que el sistema usa para solicitar acceso al usuario.

Haga clic con el botón derecho en la entrada `Info.plist` del árbol del proyecto y seleccione **Abrir como** > **Código fuente**. Agregue las líneas siguientes a la sección `<dict>` de nivel superior y guarde el archivo.

```xml
<key>NSMicrophoneUsageDescription</key>
<string>Need microphone access for VOIP calling.</string>
```

### <a name="set-up-the-app-framework"></a>Instalación del marco de la aplicación

Abra el archivo **ContentView.swift** del proyecto y agregue una declaración `import` en la parte superior del archivo para importar la `AzureCommunicationCalling library`. Además, importe `AVFoundation`; necesitaremos esta información para la solicitud de permiso de audio en el código.

```swift
import AzureCommunicationCalling
import AVFoundation
```

## <a name="object-model"></a>Modelo de objetos

Las clases e interfaces siguientes controlan algunas de las características principales de la biblioteca cliente para llamadas de Azure Communication Services para iOS.


| Nombre                                  | Descripción                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ACSCallClient | ACSCallClient es el punto de entrada principal a la biblioteca cliente para llamadas.|
| ACSCallAgent | ACSCallAgent se usa para iniciar y administrar llamadas. |
| CommunicationUserCredential | CommunicationUserCredential se usa como la credencial de token para crear una instancia de CallAgent.| 
| CommunicationIndentifier | CommunicationIndentifier se usa para representar la identidad del usuario, que puede ser una de las opciones siguientes: CommunicationUser/PhoneNumber/CallingApplication. |

> [!NOTE]
> Al implementar delegados de eventos, la aplicación tiene que mantener una referencia segura a los objetos que requieren suscripciones a eventos. Por ejemplo, cuando se devuelve un objeto `ACSRemoteParticipant` al invocar el método `call.addParticipant` y la aplicación establece que el delegado escuche en `ACSRemoteParticipantDelegate`, la aplicación debe contener una referencia segura al objeto `ACSRemoteParticipant`. De lo contrario, si se recopila este objeto, el delegado generará una excepción irrecuperable cuando el SDK de llamadas intente invocar el objeto.

## <a name="initialize-the-acscallagent"></a>Inicialización de ACSCallAgent

Para crear una instancia de `ACSCallAgent` a partir de `ACSCallClient`, debe usar el método `callClient.createCallAgent` que devuelve de manera asincrónica un objeto `ACSCallAgent` una vez que se inicializa.

Para crear un cliente de llamada, debe pasar un objeto `CommunicationUserCredential`.

```swift

import AzureCommunication

let tokenString = "token_string"
var userCredential: CommunicationUserCredential?
do {
    userCredential = try CommunicationUserCredential(
        initialToken: tokenString, refreshProactively: true,
        tokenRefresher: self.fetchTokenSync
    )
} catch {
    print("Failed to create CommunicationCredential object")
}

// tokenProvider needs to be implemented by contoso which fetches new token
public func fetchTokenSync(then onCompletion: TokenRefreshOnCompletion) {
    let newToken = self.tokenProvider!.fetchNewToken()
    onCompletion(newToken, nil)
}
```

Pase el objeto CommunicationUserCredential creado anteriormente a ACSCallClient y establezca el nombre para mostrar.

```swift

callClient = CallClient()
let callAgentOptions:CallAgentOptions = CallAgentOptions()
options.displayName = "ACS iOS User"

callClient?.createCallAgent(userCredential: userCredential!,
    options: callAgentOptions,
    completionHandler: { (callAgent, error) in
        if error == nil {
            print("Create agent succeeded")
            self.callAgent = callAgent
        } else {
            print("Create agent failed")
        }
})

```

## <a name="place-an-outgoing-call"></a>Realización de una llamada saliente

Para crear e iniciar una llamada, debe llamar a una de las API en `ACSCallAgent` y proporcionar la identidad de Communication Services de un usuario que haya aprovisionado a través de la biblioteca cliente de administración de Communication Services.

La creación y el inicio de la llamada es sincrónico. Recibirá una instancia de llamada que le permitirá suscribirse a todos los eventos de la llamada.

### <a name="place-a-11-call-to-a-user-or-a-1n-call-with-users-and-pstn"></a>Realice una llamada de uno a uno a un usuario o una llamada de uno a varios con usuarios y RTC

```swift

let callees = [CommunicationUser(identifier: 'acsUserId')]
let oneToOneCall = self.callAgent.call(participants: callees, options: StartCallOptions())

```

### <a name="place-a-1n-call-with-users-and-pstn"></a>Realice una llamada de uno a varios con usuarios y RTC
Para realizar la llamada a RTC, debe especificar el número de teléfono adquirido con Communication Services.
```swift

let pstnCallee = PhoneNumber('+1999999999')
let callee = CommunicationUser(identifier: 'acsUserId')
let groupCall = self.callAgent.call(participants: [pstnCallee, callee], options: StartCallOptions())

```

### <a name="place-a-11-call-with-with-video"></a>Realice una llamada de uno a uno con vídeo
Para obtener una instancia de administrador de dispositivos, consulte [aquí](#device-management).

```swift

let camera = self.deviceManager!.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: camera)
let videoOptions = VideoOptions(localVideoStream: localVideoStream)

let startCallOptions = StartCallOptions()
startCallOptions?.videoOptions = videoOptions

let callee = CommunicationUser(identifier: 'acsUserId')
let call = self.callAgent?.call(participants: [callee], options: startCallOptions)

```

### <a name="join-a-group-call"></a>Unión a una llamada grupal
Para unirse a una llamada, debe llamar a una de las API de *CallAgent*.

```swift

let groupCallContext = GroupCallContext()
groupCallContext?.groupId = UUID(uuidString: "uuid_string")!
let call = self.callAgent?.join(with: groupCallContext, joinCallOptions: JoinCallOptions())

```

### <a name="accept-an-incoming-call"></a>Aceptar una llamada entrante
Para aceptar una llamada, llame al método "accept" en un objeto de llamada.
Establezca un delegado a CallAgent. 
```swift
final class CallHandler: NSObject, CallAgentDelegate
{
    public var incomingCall: Call?
 
    public func onCallsUpdated(_ callAgent: CallAgent!, args: CallsUpdatedEventArgs!) {
        if let incomingCall = args.addedCalls?.first(where: { $0.isIncoming }) {
            self.incomingCall = incomingCall
        }
    }
}

let firstCamera: VideoDeviceInfo? = self.deviceManager?.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: firstCamera)
let acceptCallOptions = AcceptCallOptions()
acceptCallOptions!.videoOptions = VideoOptions(localVideoStream:localVideoStream!)
if let incomingCall = CallHandler().incomingCall {
   incomingCall.accept(options: acceptCallOptions,
                          completionHandler: { (error) in
                           if error == nil {
                               print("Incoming call accepted")
                           } else {
                               print("Failed to accept incoming call")
                           }
                       })
} else {
   print("No incoming call found to accept")
}
```

## <a name="push-notification"></a>Notificación push

Una notificación push móvil es la notificación emergente que recibe en el dispositivo móvil. En el caso de las llamadas, nos centraremos en las notificaciones push VoIP (voz sobre IP). Ofrecemos las funcionalidades necesarias para registrarse para recibir notificaciones push, para administrar las notificaciones push y para anular el registro y dejar de recibir notificaciones push.

### <a name="prerequisite"></a>Requisito previo

- Paso 1: Xcode -> Signing & Capabilities (Firma y funcionalidades) -> Add Capability (Agregar funcionalidad) -> "Push Notifications" (Notificaciones push)
- Paso 2: Xcode -> Signing & Capabilities (Firma y funcionalidades)-> Add Capability (Agregar funcionalidad) -> "Background Modes" (Modos en segundo plano)
- Paso 3: "Background Modes" (Modos en segundo plano) -> seleccione "Voice over IP" (Voz sobre IP) y "Remote notifications" (Notificaciones remotas)

:::image type="content" source="../media/ios/xcode-push-notification.png" alt-text="Captura de pantalla que muestra cómo agregar funcionalidades en Xcode." lightbox="../media/ios/xcode-push-notification.png":::

#### <a name="register-for-push-notifications"></a>Registro de notificaciones push

Para registrarse para recibir notificaciones push, llame a registerPushNotification() en una instancia de *CallAgent* con un token de registro del dispositivo.

Se debe llamar al registro para recibir notificaciones push después de que la inicialización se realiza correctamente. Cuando se destruya el objeto `callAgent`, se llamará a `logout`, lo que anulará automáticamente el registro para así dejar de recibir notificaciones push.


```swift

let deviceToken: Data = pushRegistry?.pushToken(for: PKPushType.voIP)
callAgent.registerPushNotifications(deviceToken: deviceToken,
                completionHandler: { (error) in
    if(error == nil) {
        print("Successfully registered to push notification.")
    } else {
        print("Failed to register push notification.")
    }
})

```

#### <a name="push-notification-handling"></a>Manejo de notificaciones push
Para recibir notificaciones push sobre llamadas entrantes, llame a *handlePushNotification()* en una instancia de *CallAgent* con una carga de diccionario.

```swift

let dictionaryPayload = pushPayload?.dictionaryPayload
callAgent.handlePushNotification(payload: dictionaryPayload, completionHandler: { (error) in
    if (error != nil) {
        print("Handling of push notification failed")
    } else {
        print("Handling of push notification was successful")
    }
})

```
#### <a name="unregister-push-notification"></a>Anulación del registro de notificaciones push

Las aplicaciones pueden anular el registro de notificaciones push en cualquier momento. Solo debe llamar al método `unRegisterPushNotification` en *CallAgent*.
> [!NOTE]
> El registro para que las aplicaciones dejen de recibir notificaciones push no se anula de manera automática al cerrar la sesión.

```swift

callAgent.unRegisterPushNotifications(completionHandler: { (error) in
    if (error != nil) {
        print("Unregister of push notification failed, please try again")
    } else {
        print("Unregister of push notification was successful")
    }
})

```

## <a name="mid-call-operations"></a>Operaciones durante la llamada

Durante una llamada, puede realizar varias operaciones para administrar la configuración relacionada con el vídeo y el audio.

### <a name="mute-and-unmute"></a>Silencio y reactivación del sonido

Para silenciar o reactivar el sonido del punto de conexión local, puede usar las API asincrónicas `mute` y `unmute`:

```swift
call!.mute(completionHandler: { (error) in
    if error == nil {
        print("Successfully muted")
    } else {
        print("Failed to mute")
    }
})

```

[Asincrónico] Reactivación del sonido local

```swift
call!.unmute(completionHandler:{ (error) in
    if error == nil {
        print("Successfully un-muted")
    } else {
        print("Failed to unmute")
    }
})
```

### <a name="start-and-stop-sending-local-video"></a>Inicio y detención del envío de vídeo local

Para empezar a enviar vídeo local a otros participantes de la llamada, use la API `startVideo` y pase `localVideoStream` con `camera`.

```swift

let firstCamera: VideoDeviceInfo? = self.deviceManager?.getCameraList()![0]
let localVideoStream = LocalVideoStream(camera: firstCamera)

call!.startVideo(stream: localVideoStream) { (error) in
    if (error == nil) {
        print("Local video started successfully")
    }
    else {
        print("Local video failed to start")
    }
}

```

Una vez que se inicia el envío de vídeo, la instancia `ACSLocalVideoStream` se agrega a la colección `localVideoStreams` en una instancia de llamada:

```swift

call.localVideoStreams[0]

```

[Asincrónico] Para detener el vídeo local, pase el objeto `localVideoStream` devuelto a partir de la invocación de `call.startVideo`:

```swift

call!.stopVideo(stream: localVideoStream) { (error) in
    if (error == nil) {
        print("Local video stopped successfully")
    } else {
        print("Local video failed to stop")
    }
}

```

## <a name="remote-participants-management"></a>Administración de participantes remotos

El tipo `ACSRemoteParticipant` representa a todos los participantes remotos y estos están disponibles a través de la colección `remoteParticipants` en una instancia de llamada:

### <a name="list-participants-in-a-call"></a>Lista de los participantes en una llamada

```swift

call.remoteParticipants

```

### <a name="remote-participant-properties"></a>Propiedades de los participantes remotos

```swift

// [ACSRemoteParticipantDelegate] delegate - an object you provide to receive events from this ACSRemoteParticipant instance
var remoteParticipantDelegate = remoteParticipant.delegate

// [CommunicationIdentifier] identity - same as the one used to provision token for another user
var identity = remoteParticipant.identity

// ACSParticipantStateIdle = 0, ACSParticipantStateEarlyMedia = 1, ACSParticipantStateConnecting = 2, ACSParticipantStateConnected = 3, ACSParticipantStateOnHold = 4, ACSParticipantStateInLobby = 5, ACSParticipantStateDisconnected = 6
var state = remoteParticipant.state

// [ACSError] callEndReason - reason why participant left the call, contains code/subcode/message
var callEndReason = remoteParticipant.callEndReason

// [Bool] isMuted - indicating if participant is muted
var isMuted = remoteParticipant.isMuted

// [Bool] isSpeaking - indicating if participant is currently speaking
var isSpeaking = remoteParticipant.isSpeaking

// ACSRemoteVideoStream[] - collection of video streams this participants has
var videoStreams = remoteParticipant.videoStreams // [ACSRemoteVideoStream, ACSRemoteVideoStream, ...]

```

### <a name="add-a-participant-to-a-call"></a>Incorporación de un participante a una llamada

Para agregar un participante a una llamada (ya sea un usuario o un número de teléfono), puede invocar `addParticipant`. Esto devolverá de manera sincrónica la instancia de un participante remoto.

```swift

let remoteParticipantAdded: RemoteParticipant = call.add(participant: CommunicationUser(identifier: "userId"))

```

### <a name="remove-a-participant-from-a-call"></a>Eliminación de un participante de una llamada
Para quitar un participante de una llamada (ya sea un usuario o un número de teléfono), puede invocar la API `removeParticipant`. Esto se resolverá de manera asincrónica.

```swift

call!.remove(participant: remoteParticipantAdded) { (error) in
    if (error == nil) {
        print("Successfully removed participant")
    } else {
        print("Failed to remove participant")
    }
}

```

## <a name="render-remote-participant-video-streams"></a>Representación de secuencias de vídeo de participantes remotos

Los participantes remotos pueden iniciar el uso compartido de pantalla o vídeo durante una llamada.

### <a name="handle-remote-participant-videoscreen-sharing-streams"></a>Administración de secuencias de uso compartido de pantalla o vídeo de participantes remotos

Para enumerar las secuencias de los participantes remotos, revise las colecciones `videoStreams`:

```swift

var remoteParticipantVideoStream = call.remoteParticipants[0].videoStreams[0]

```

### <a name="remote-video-stream-properties"></a>Propiedades de secuencias de vídeo remotas

```swift

var type: MediaStreamType = remoteParticipantVideoStream.type // 'ACSMediaStreamTypeVideo'

var isAvailable: Bool = remoteParticipantVideoStream.isAvailable // indicates if remote stream is available

var id: Int = remoteParticipantVideoStream.id // id of remoteParticipantStream

```

### <a name="render-remote-participant-stream"></a>Representación de secuencias de participantes remotos

Para iniciar la representación de secuencias de participantes remotos:

```swift

let renderer: Renderer? = Renderer(remoteVideoStream: remoteParticipantVideoStream)
let targetRemoteParticipantView: RendererView? = renderer?.createView(with: RenderingOptions(scalingMode: ScalingMode.crop))
// To update the scaling mode later
targetRemoteParticipantView.update(scalingMode: ScalingMode.fit)

```

### <a name="remote-video-renderer-methods-and-properties"></a>Propiedades y métodos de representación de vídeo remoto

```swift
// [Bool] isRendering - indicating if stream is being rendered
remoteVideoRenderer.isRendering()
// [Synchronous] dispose() - dispose renderer and all `RendererView` associated with this renderer. To be called when you have removed all associated views from the UI.
remoteVideoRenderer.dispose()
```

## <a name="device-management"></a>Administración de dispositivos

`DeviceManager` permite enumerar los dispositivos locales que se pueden usar en una llamada para transmitir secuencias de audio o vídeo. También permite solicitar permiso a un usuario para acceder al micrófono o a la cámara. Puede acceder a `deviceManager` en el objeto `callClient`:

```swift

self.callClient!.getDeviceManager(
    completionHandler: { (deviceManager, error) in
        if (error == nil) {
            print("Got device manager instance")
            self.deviceManager = deviceManager
        } else {
            print("Failed to get device manager instance")
        }
    })
```

### <a name="enumerate-local-devices"></a>Enumeración de los dispositivos locales

Para acceder a los dispositivos locales, puede usar métodos de enumeración en el Administrador de dispositivos. La enumeración es una acción sincrónica.

```swift
// enumerate local cameras
var localCameras = deviceManager.getCameraList() // [ACSVideoDeviceInfo, ACSVideoDeviceInfo...]
// enumerate local cameras
var localMicrophones = deviceManager.getMicrophoneList() // [ACSAudioDeviceInfo, ACSAudioDeviceInfo...]
// enumerate local cameras
var localSpeakers = deviceManager.getSpeakerList() // [ACSAudioDeviceInfo, ACSAudioDeviceInfo...]
``` 

### <a name="set-default-microphonespeaker"></a>Establecimiento de micrófono/altavoz predeterminados

El administrador de dispositivos le permite establecer un dispositivo predeterminado que se usará al iniciar una llamada. Si no se establecen los valores predeterminados de la pila, Communication Services revertirá a los valores predeterminados del sistema operativo.

```swift
// get first microphone
var firstMicrophone = self.deviceManager!.getMicrophoneList()![0]
// [Synchronous] set microphone
deviceManager.setMicrophone(microphoneDevice: firstMicrophone)
// get first speaker
var firstSpeaker = self.deviceManager!.getSpeakerList()![0]
// [Synchronous] set speaker
deviceManager.setSpeaker(speakerDevice: firstSpeaker)
```

### <a name="local-camera-preview"></a>Vista previa de la cámara local

Puede usar `ACSRenderer` para empezar a representar una secuencia desde su cámara local. Esta secuencia no se enviará a los demás participantes, porque es una fuente de vista previa local. Se trata de una acción asincrónica.

```swift

let camera: VideoDeviceInfo = self.deviceManager!.getCameraList()![0]
let localVideoStream: LocalVideoStream = LocalVideoStream(camera: camera)
let renderer: Renderer = Renderer(localVideoStream: localVideoStream)
self.view = renderer!.createView()

```

### <a name="local-camera-preview-properties"></a>Propiedades de la vista previa de la cámara local

El representador tiene un conjunto de propiedades y métodos que le permiten controlar la representación:

```swift

// Constructor can take in ACSLocalVideoStream or ACSRemoteVideoStream
let localRenderer = Renderer(localVideoStream:localVideoStream)
let remoteRenderer = Renderer(remoteVideoStream:remoteVideoStream)

// [ACSStreamSize] size of the rendering view
localRenderer.size

// [ACSRendererDelegate] an object you provide to receive events from this ACSRenderer instance
localRenderer.delegate

// [Synchronous] create view
try! localRenderer.createView()

// [Synchronous] create view with rendering options
try! localRenderer.createView(with: RenderingOptions(scalingMode: ScalingMode.fit))

// [Synchronous] dispose rendering view
localRenderer.dispose()

```

## <a name="eventing-model"></a>Modelo de eventos

Puede suscribirse a la mayoría de las propiedades y colecciones que se van a notificar al cambiar los valores.

### <a name="properties"></a>Propiedades
Para suscribirse a eventos `property changed`:

```swift
call.delegate = self
// Get the property of the call state by doing get on the call's state member
public func onCallStateChanged(_ call: Call!,
                               args: PropertyChangedEventArgs!)
{
    print("Callback from SDK when the call state changes, current state: " + call.state.rawValue)
}

 // to unsubscribe
 self.call.delegate = nil

```

### <a name="collections"></a>Colecciones
Para suscribirse a eventos `collection updated`:

```swift
call.delegate = self
// Collection contains the streams that were added or removed only
public func onLocalVideoStreamsChanged(_ call: Call!,
                                       args: LocalVideoStreamsUpdatedEventArgs!)
{
    print(args.addedStreams.count)
    print(args.removedStreams.count)
}
// to unsubscribe
self.call.delegate = nil
```
