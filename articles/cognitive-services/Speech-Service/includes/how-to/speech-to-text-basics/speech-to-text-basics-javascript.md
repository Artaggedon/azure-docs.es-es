---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/15/2020
ms.author: trbye
ms.custom: devx-track-js
ms.openlocfilehash: a27fba6e426b72d72160a9a238f68cf8cef5c73b
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98947557"
---
Una de las características principales del servicio de voz es la capacidad para reconocer y transcribir la voz humana (que a menudo se denomina "conversión de voz en texto"). En este inicio rápido, aprenderá a usar el SDK de voz en sus aplicaciones y productos para realizar una conversión de voz en texto de alta calidad.

## <a name="skip-to-samples-on-github"></a>Pasar a los ejemplos en GitHub

Si quiere pasar directamente al código de ejemplo, consulte los [ejemplos del inicio rápido de JavaScript](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node) en GitHub.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por sentado que tiene una cuenta de Azure y una suscripción al servicio de voz. Si no dispone de una cuenta y una suscripción, [pruebe el servicio de voz de forma gratuita](../../../overview.md#try-the-speech-service-for-free).

## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

En primer lugar, deberá instalar <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">Speech SDK para JavaScript<span class="docon docon-navigate-external x-hidden-focus"></span></a>. Utilice las siguientes instrucciones en función de la plataforma:

- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs#get-the-speech-sdk" target="_blank">Node.js <span 
class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=browser#get-the-speech-sdk" target="_blank">Explorador web <span class="docon docon-navigate-external x-hidden-focus"></span></a>

Además, en función del entorno de destino, use una de las siguientes:

# <a name="script"></a>[script](#tab/script)

Descargue y extraiga el archivo *microsoft.cognitiveservices.speech.sdk.bundle.js* de <a href="https://aka.ms/csspeech/jsbrowserpackage" target="_blank">Speech SDK para JavaScript <span class="docon docon-navigate-external x-hidden-focus"></span></a> y colóquelo en una carpeta accesible para el archivo HTML.

```html
<script src="microsoft.cognitiveservices.speech.sdk.bundle.js"></script>;
```

> [!TIP]
> Si el destino es un explorador web y se usa la etiqueta `<script>`, el prefijo `sdk` no es necesario al referenciar clases. El prefijo `sdk` es un alias que se usa para asignar un nombre al módulo `require`.

# <a name="require"></a>[require](#tab/require)

```javascript
const sdk = require("microsoft-cognitiveservices-speech-sdk");
```

Para más información sobre `require`, consulte <a href="https://nodejs.org/en/knowledge/getting-started/what-is-require/" target="_blank">what is require? <span class="docon docon-navigate-external x-hidden-focus"></span></a> (¿Qué es require?).

---

## <a name="create-a-speech-configuration"></a>Creación de una configuración de voz

Para llamar al servicio de voz con Speech SDK, debe crear un elemento [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig). Esta clase incluye información sobre la suscripción, como la clave, la región asociada, el punto de conexión, el host o el token de autorización. Cree una clase [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) mediante su clave y región. Consulte la página [Búsqueda de las claves y la región](../../../overview.md#find-keys-and-region) para encontrar el par clave-región.

```javascript
const speechConfig = sdk.SpeechConfig.fromSubscription("<paste-your-subscription-key>", "<paste-your-region>");
```

Existen otras maneras de inicializar una clase [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig):

* Con un punto de conexión: pase un punto de conexión del servicio de voz. La clave y el token de autorización son opcionales.
* Con un host: pase una dirección de host. La clave y el token de autorización son opcionales.
* Con un token de autorización: pase el token de autorización y la región asociada.

> [!NOTE]
> Debe crear siempre una configuración, independientemente de si va a realizar reconocimiento de voz, síntesis de voz, traducción o reconocimiento de intenciones.

## <a name="recognize-from-microphone-browser-only"></a>Reconocimiento desde un micrófono (solo explorador)

Para reconocer la voz desde el micrófono del dispositivo, cree una clase `AudioConfig` mediante `fromDefaultMicrophoneInput()`. A continuación, inicialice una clase [`SpeechRecognizer`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer), y pase `speechConfig` y `audioConfig`.

```javascript
const sdk = require("microsoft-cognitiveservices-speech-sdk");
const speechConfig = sdk.SpeechConfig.fromSubscription("<paste-your-subscription-key>", "<paste-your-region>");

function fromMic() {
    let audioConfig = sdk.AudioConfig.fromDefaultMicrophoneInput();
    let recognizer = new sdk.SpeechRecognizer(speechConfig, audioConfig);
    
    console.log('Speak into your microphone.');
    recognizer.recognizeOnceAsync(result => {
        console.log(`RECOGNIZED: Text=${result.text}`);
    });
}
fromMic();
```

Si desea usar un dispositivo de entrada de audio *específico*, debe especificar el identificador de dispositivo en la clase `AudioConfig`. Obtenga información sobre [cómo obtener el identificador del dispositivo](../../../how-to-select-audio-input-devices.md) de entrada de audio.

## <a name="recognize-from-file"></a>Reconocimiento desde un archivo 

# <a name="browser"></a>[Browser](#tab/browser)

Para reconocer la voz desde un archivo de audio en un entorno de JavaScript basado en explorador, use la función `fromWavFileInput()` para crear una clase [`AudioConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig). La función `fromWavFileInput()` espera un objeto [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File/File) de JavaScript como un parámetro.

```javascript
const sdk = require("microsoft-cognitiveservices-speech-sdk");
const speechConfig = sdk.SpeechConfig.fromSubscription("<paste-your-subscription-key>", "<paste-your-region>");

function fromFile() {
    // wavByteContent should be a byte array of the raw wav content
    let file = new File([wavByteContent]);
    let audioConfig = sdk.AudioConfig.fromWavFileInput(file);
    let recognizer = new sdk.SpeechRecognizer(speechConfig, audioConfig);
    
    recognizer.recognizeOnceAsync(result => {
        console.log(`RECOGNIZED: Text=${result.text}`);
    });
}
fromFile();
```

# <a name="nodejs"></a>[Node.js](#tab/node)

Para reconocer la voz desde un archivo de audio de Node.js, se requiere un patrón de diseño alternativo que use una secuencia de entrada, ya que el objeto [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File/File) de JavaScript no se puede usar en un entorno de ejecución de Node.js. El código siguiente:

* Crea una secuencia de entrada mediante `createPushStream()`.
* Abre el archivo `.wav` al crear un flujo de lectura y lo escribe en el flujo de entrada.
* Crea una configuración de audio mediante el flujo de entrada.

```javascript
const fs = require('fs');
const sdk = require("microsoft-cognitiveservices-speech-sdk");
const speechConfig = sdk.SpeechConfig.fromSubscription("<paste-your-subscription-key>", "<paste-your-region>");

function fromFile() {
    let pushStream = sdk.AudioInputStream.createPushStream();

    fs.createReadStream("YourAudioFile.wav").on('data', function(arrayBuffer) {
        pushStream.write(arrayBuffer.slice());
    }).on('end', function() {
        pushStream.close();
    });
 
    let audioConfig = sdk.AudioConfig.fromStreamInput(pushStream);
    let recognizer = new sdk.SpeechRecognizer(speechConfig, audioConfig);
    recognizer.recognizeOnceAsync(result => {
        console.log(`RECOGNIZED: Text=${result.text}`);
        recognizer.close();
    });
}
fromFile();
```

El uso de una secuencia de extracción como entrada interpreta que los datos de audio son un PCM sin formato; es decir, se omiten los encabezados.
La API seguirá funcionando en determinados casos si no se ha omitido el encabezado, pero para obtener mejores resultados considere la posibilidad de aplicar la lógica para leer los encabezados, de manera que `fs` se inicie al *principio de los datos de audio*.

---

## <a name="error-handling"></a>Control de errores

Los ejemplos anteriores sencillamente obtienen el texto reconocido de `result.text`, pero para gestionar los errores y otras respuestas, deberá escribir código para determinar el resultado. En el código siguiente se evalúa la propiedad [`result.reason`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognitionresult#reason) y:

* Imprime el resultado del reconocimiento: `ResultReason.RecognizedSpeech`.
* Si no hay ninguna coincidencia de reconocimiento, se informa al usuario: `ResultReason.NoMatch`.
* Si se produce un error, se imprime el mensaje de error: `ResultReason.Canceled`.

```javascript
switch (result.reason) {
    case ResultReason.RecognizedSpeech:
        console.log(`RECOGNIZED: Text=${result.text}`);
        break;
    case ResultReason.NoMatch:
        console.log("NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled:
        const cancellation = CancellationDetails.fromResult(result);
        console.log(`CANCELED: Reason=${cancellation.reason}`);

        if (cancellation.reason == CancellationReason.Error) {
            console.log(`CANCELED: ErrorCode=${cancellation.ErrorCode}`);
            console.log(`CANCELED: ErrorDetails=${cancellation.errorDetails}`);
            console.log("CANCELED: Did you update the subscription info?");
        }
        break;
    }
```

## <a name="continuous-recognition"></a>Reconocimiento continuo

En los ejemplos anteriores se usa el reconocimiento de una sola captura, que reconoce una única expresión. El final de una expresión única se determina mediante la escucha de un silencio al final o hasta que se procesa un máximo de 15 segundos de audio.

En cambio, el reconocimiento continuo se usa para **establecer** cuándo cesar el reconocimiento. Requiere que se suscriba a los eventos `Recognizing`, `Recognized` y `Canceled` para obtener los resultados del reconocimiento. Para detener el reconocimiento, debe llamar a [`stopContinuousRecognitionAsync`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#stopcontinuousrecognitionasync). Este es un ejemplo de cómo se realiza el reconocimiento continuo en un archivo de entrada de audio.

Comience por definir la entrada e inicializar una clase [`SpeechRecognizer`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer):

```javascript
const recognizer = new sdk.SpeechRecognizer(speechConfig);
```

A continuación, suscríbase a los eventos enviados desde [`SpeechRecognizer`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer).

* [`recognizing`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#recognizing): se señalizan los eventos que contienen los resultados intermedios del reconocimiento.
* [`recognized`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#recognized): se señalizan los eventos que contienen los resultados finales del reconocimiento (lo que indica un intento de reconocimiento correcto).
* [`sessionStopped`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#sessionstopped): se señalizan los eventos que indican el final de una sesión de reconocimiento (operación).
* [`canceled`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#canceled): se señalizan los eventos que contienen los resultados de reconocimiento cancelados (que indican un intento de reconocimiento que se canceló como resultado de una solicitud de cancelación directa o, de forma alternativa, por un error de protocolo o transporte).

```javascript
recognizer.recognizing = (s, e) => {
    console.log(`RECOGNIZING: Text=${e.result.text}`);
};

recognizer.recognized = (s, e) => {
    if (e.result.reason == ResultReason.RecognizedSpeech) {
        console.log(`RECOGNIZED: Text=${e.result.text}`);
    }
    else if (e.result.reason == ResultReason.NoMatch) {
        console.log("NOMATCH: Speech could not be recognized.");
    }
};

recognizer.canceled = (s, e) => {
    console.log(`CANCELED: Reason=${e.reason}`);

    if (e.reason == CancellationReason.Error) {
        console.log(`"CANCELED: ErrorCode=${e.errorCode}`);
        console.log(`"CANCELED: ErrorDetails=${e.errorDetails}`);
        console.log("CANCELED: Did you update the subscription info?");
    }

    recognizer.stopContinuousRecognitionAsync();
};

recognizer.sessionStopped = (s, e) => {
    console.log("\n    Session stopped event.");
    recognizer.stopContinuousRecognitionAsync();
};
```

Cuando todo esté configurado, llame a [`startContinuousRecognitionAsync`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#startcontinuousrecognitionasync) para iniciar el reconocimiento.

```javascript
recognizer.startContinuousRecognitionAsync();

// make the following call at some point to stop recognition.
// recognizer.StopContinuousRecognitionAsync();
```

### <a name="dictation-mode"></a>Modo de dictado

Al usar el reconocimiento continuo, puede habilitar el procesamiento de dictado mediante la función "habilitar dictado" correspondiente. Este modo hará que la instancia de configuración de voz interprete las descripciones de palabras de estructuras de oraciones como puntuación. Por ejemplo, la expresión "Interrogación de apertura vive en la ciudad interrogación de cierre" se interpretaría como el texto "¿Vive en la ciudad?".

Para habilitar el modo de dictado, use el método [`enableDictation`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig#enabledictation--) del elemento [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig).

```javascript
speechConfig.enableDictation();
```

## <a name="change-source-language"></a>Cambio del idioma de origen

Una tarea común en el reconocimiento de voz es especificar el idioma de entrada (u origen). Echemos un vistazo a cómo se cambiaría el idioma de entrada a italiano. En el código, busque [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) y, a continuación, agregue esta línea directamente debajo.

```javascript
speechConfig.speechRecognitionLanguage = "it-IT";
```

La propiedad [`speechRecognitionLanguage`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig#speechrecognitionlanguage) espera una cadena con formato de configuración regional de idioma. En la columna **Locale**, puede proporcionar cualquier valor de la lista de idiomas o configuraciones regionales [compatibles](../../../language-support.md).

## <a name="improve-recognition-accuracy"></a>Mejora de la precisión del reconocimiento

Las listas de frases se usan para identificar frases conocidas en datos de audio, como el nombre de una persona o una ubicación específica. Al proporcionar una lista de frases, mejorará la precisión del reconocimiento de voz.

Por ejemplo, si tiene el comando "Mover a" y "Cerca" como posible destino que se puede decir, puede añadir la entrada "Mover a Cerca". Al agregar una frase, aumentará la probabilidad de que, cuando se reconozca el audio, se reconozca "Mover a Cerca" en lugar de "Mover acerca".

A una lista de frases se pueden agregar palabras solas o frases completas. Durante el reconocimiento, se usa una entrada de una lista de frases para mejorar el reconocimiento de las palabras y frases de la lista incluso cuando las entradas aparecen en medio de la expresión. 

> [!IMPORTANT]
> La característica de lista de frases está disponible en los siguientes idiomas: en-US, de-DE, en-AU, en-CA, en-GB, es-ES, es-MX, fr-CA, fr-FR, it-IT, ja-JP, ko-KR, pt-BR, zh-CN

Para usar una lista de frases, primero debe crear un objeto [`PhraseListGrammar`](/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar) y, a continuación, agregar palabras y frases específicas con [`addPhrase`](/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar#addphrase-string-).

Los cambios realizados en el objeto [`PhraseListGrammar`](/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar) se aplican en el siguiente reconocimiento o después de una reconexión al servicio de voz.

```javascript
const phraseList = sdk.PhraseListGrammar.fromRecognizer(recognizer);
phraseList.addPhrase("Supercalifragilisticexpialidocious");
```

Si necesita borrar la lista de frases:

```javascript
phraseList.clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Otras opciones para mejorar la precisión del reconocimiento

Las listas de frases son solo una opción para mejorar la precisión del reconocimiento. También puede: 

* [Mejora de la precisión con Habla personalizada](../../../custom-speech-overview.md)
* [Mejora de la precisión con modelos de inquilino](../../../tutorial-tenant-model.md)
