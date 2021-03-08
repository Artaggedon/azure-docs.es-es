---
title: Novedades del servicio Face
titleSuffix: Azure Cognitive Services
description: Las notas de la versión del servicio Face incluyen un historial de cambios de versión para varias versiones.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 12/11/2020
ms.author: pafarley
ms.openlocfilehash: a66f65d48a14853ec1da08f5d83b777f4fdac846
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102183325"
---
# <a name="whats-new-in-face-service"></a>Novedades del servicio Face

El servicio Azure Face se actualiza de forma continua. Use este artículo para mantenerse al día con mejoras de características, correcciones y actualizaciones de la documentación.

## <a name="february-2021"></a>Febrero de 2021

* Nuevo modelo de detección de Face API: el nuevo modelo de detección 03 es el modelo de detección más preciso disponible actualmente. Si es un nuevo cliente, se recomienda usar este modelo. La detección 03 mejora la recuperación y la precisión en caras más pequeñas que se encuentran dentro de las imágenes (64 x 64 píxeles). Entre las mejoras adicionales se incluyen una reducción general de falsos positivos y una detección mejorada en las orientaciones de caras giradas. La combinación de la detección 03 con el nuevo reconocimiento 04 también proporcionará un reconocimiento más preciso. Consulte [Especificación de un modelo de detección de caras](https://docs.microsoft.com/azure/cognitive-services/face/face-api-how-to-topics/specify-detection-model) para más información.
* Atributo de mascarilla facial: el atributo de mascarilla facial está disponible con el último modelo detección 03, junto con el atributo adicional `"noseAndMouthCovered"` que detecta si la mascarilla facial está bien colocada (tapa la nariz y la boca). Para usar la funcionalidad más reciente de detección de mascarillas, los usuarios deben especificar el modelo de detección en la solicitud de API: asignar la versión del modelo con el parámetro _detectionModel_ a `detection_03`. Consulte [Especificación de un modelo de detección de caras](https://docs.microsoft.com/azure/cognitive-services/face/face-api-how-to-topics/specify-detection-model) para más información.
* Nuevo modelo de reconocimiento de Face API: el nuevo modelo reconocimiento 04 es el modelo de reconocimiento más preciso disponible actualmente. Si es un nuevo cliente, se recomienda usar este modelo para la verificación y la identificación. Este modelo mejora la precisión del reconocimiento 03, como el reconocimiento de los usuarios inscritos que llevan mascarillas faciales (mascarillas quirúrgicas, mascarillas n95, mascarillas de tela). Ahora, los clientes pueden crear experiencias de usuario seguras y eficaces que detecten si un usuario inscrito lleva una mascarilla facial con el último modelo detección 03, y reconocer quién es con el último modelo reconocimiento 04. Consulte [Especificación de un modelo de reconocimiento](https://docs.microsoft.com/azure/cognitive-services/face/face-api-how-to-topics/specify-recognition-model) para más información.


## <a name="january-2021"></a>Enero de 2021
* Mitigación de la latencia cuando se usa Face API: El equipo de Face ha publicado un nuevo artículo en el que se detallan las posibles causas de latencia al usar el servicio y las posibles estrategias de mitigación. Vea [Mitigación de la latencia cuando se usa el servicio Face](./face-api-how-to-topics/how-to-mitigate-latency.md).

## <a name="december-2020"></a>Diciembre de 2020
* Configuración del cliente para el almacenamiento de Face ID: Aunque el servicio Face no almacena imágenes de clientes, las características de las caras extraídas se almacenarán en el servidor. Face ID es un identificador de la característica de caras y se usará en [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Face: Identificar), [Face - Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) (Face: Comprobar) y [Face - Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) (Face: Buscar similar). Las características de las caras almacenadas expirarán y se eliminarán 24 horas después de la llamada de detección original. Ahora los clientes pueden determinar el período de tiempo en que se almacenan en caché estos valores de Face ID. El valor máximo sigue siendo de 24 horas, pero ahora se puede establecer un valor mínimo de 60 segundos. Los nuevos intervalos de tiempo para los valores de Face ID que se almacenan en caché son cualquier valor entre 60 segundos y 24 horas. Puede encontrar más información en la referencia de la API [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) (el parámetro *faceIdTimeToLive*).

## <a name="november-2020"></a>Noviembre de 2020
* Se ha publicado una aplicación de ejemplo para realizar una inscripción mediante el reconocimiento facial y así demostrar los procedimientos recomendados para establecer un consentimiento significativo y crear sistemas de reconocimiento facial de alta precisión mediante inscripciones de alta calidad. El ejemplo de código abierto se puede encontrar en la guía para [Compilar una aplicación de inscripción](build-enrollment-app.md) y en [GitHub](https://github.com/Azure-Samples/cognitive-services-FaceAPIEnrollmentSample); está listo para que los desarrolladores lo implementen o personalicen. 

## <a name="august-2020"></a>Agosto de 2020
* Cifrado administrado por el cliente de datos en reposo: El servicio Face cifra automáticamente los datos al guardarlos en la nube. El cifrado del servicio Face protege los datos para ayudarle a cumplir los requisitos de cumplimiento y de seguridad de la organización. De forma predeterminada, su suscripción usa claves de cifrado administradas por Microsoft. También hay una opción nueva para administrar la suscripción con claves propias, que se denominan claves administradas por el cliente (CMK). Puede encontrar más detalles en [Claves administradas por el cliente](./encrypt-data-at-rest.md).

## <a name="april-2020"></a>Abril de 2020
* Nuevo modelo de reconocimiento de Face API: El modelo recognition_03 es el más preciso disponible actualmente. Si recién comienza a usar el servicio, se recomienda usar este modelo. Recognition_03 proporcionará una precisión mejorada tanto para las comparaciones de similitud como para las de coincidencia de personas. Puede encontrar más detalles en [Especificación de un modelo de reconocimiento facial](./face-api-how-to-topics/specify-recognition-model.md).

## <a name="june-2019"></a>Junio de 2019

* Se ha agregado un nuevo modelo de detección de caras con una mayor precisión para las caras pequeñas, ocluidas, desenfocadas y perfiles. Úselo en [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) y [LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42) especificando el nombre del nuevo modelo de detección de caras `detection_02` en el parámetro `detectionModel`. Puede encontrar más información en [Especificación de un modelo de detección](Face-API-How-to-Topics/specify-detection-model.md).

## <a name="april-2019"></a>Abril de 2019

* Se ha mejorado la precisión global de los atributos `age` y `headPose`. El atributo `headPose` también se ha actualizado con el valor `pitch` habilitado ahora. Use estos atributos especificándolos en el parámetro `returnFaceAttributes` del parámetro [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes`. 

* Se ha mejorado la velocidad de los parámetros [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) y [LargePersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

## <a name="march-2019"></a>Marzo de 2019

* Se ha agregado un nuevo modelo de reconocimiento facial con una mayor precisión. Úselo con los parámetros [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [LargeFaceList - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) y [LargePersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d) especificando el nombre del nuevo modelo de reconocimiento facial `recognition_02` en el parámetro `recognitionModel`. Puede encontrar más información en [Especificación de un modelo de reconocimiento](Face-API-How-to-Topics/specify-recognition-model.md).

## <a name="january-2019"></a>Enero de 2019

* Se ha agregado la característica de instantánea para admitir la migración de datos entre suscripciones: [Instantánea](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/snapshot-get). Para más información, consulte [Migración de los datos de caras a una suscripción de Face distinta](Face-API-How-to-Topics/how-to-migrate-face-data.md).

## <a name="october-2018"></a>Octubre de 2018

* Se ha refinado la descripción de `status`, `createdDateTime`, `lastActionDateTime` y `lastSuccessfulTrainingDateTime` en [PersonGroup: obtener estado de entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247), [LargePersonGroup: obtener estado de entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae32c6ac60f11b48b5aa5) y [LargeFaceList: obtener estado de entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a1582f8d2de3616c086f2cf).

## <a name="may-2018"></a>Mayo de 2018

* Se mejoró significativamente el atributo `gender` y también se mejoraron los atributos `age`, `glasses`, `facialHair`, `hair` y `makeup`. Puede usarlos mediante el parámetro `returnFaceAttributes` de [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236). 

* Se ha aumentado el límite de tamaño del archivo de imagen de entrada de 4 MB a 6 MB en [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [Persona de PersonGroup: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) y [Persona de LargePersonGroup: agregar cara ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

## <a name="march-2018"></a>Marzo de 2018

* Se agregó el contenedor a escala de millón: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) y [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Puede obtener más detalles en [How to use the large-scale feature](Face-API-How-to-Topics/how-to-use-large-scale.md) (Cómo usar la característica a gran escala).

* Se aumentó el parámetro `maxNumOfCandidatesReturned` de [Face: identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) de [1, 5] a [1, 100], y el valor predeterminado a 10.

## <a name="may-2017"></a>Mayo de 2017

* Se agregaron los atributos `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure` y `noise` atributos en el parámetro `returnFaceAttributes` de [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

* Se admiten 10 mil personas en un elemento PersonGroup y en [Face: identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Se admite la paginación en [Persona de PersonGroup: lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) con los parámetros opcionales: `start` y `top`.

* Simultaneidad compatible para agregar y eliminar caras de FaceLists diferentes y distintas personas en PersonGroup.

## <a name="march-2017"></a>Marzo de 2017
* Se agregó el atributo `emotion` atributos en el parámetro `returnFaceAttributes` de [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

* Se corrigió que la cara no se pudiera volver a detectar con el rectángulo devuelto desde [Face: detectar ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como `targetFace` en [FaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) y [Persona de PersonGroup: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Se corrigió el tamaño de la cara detectable para garantizar que esté estrictamente entre 36 x 36 y 4096 x 4096 píxeles.

## <a name="november-2016"></a>Noviembre de 2016
* Se agregó una suscripción a la versión estándar de Almacenamiento de caras, para almacenar caras persistentes adicionales al usar [Persona de PersonGroup: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) o [FaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) para la identificación o coincidencia de similitudes. Las imágenes almacenadas se cobran a 0,5 USD por 1000 caras y esta tarifa se prorratea por días. Las suscripciones del nivel Gratis siguen estando limitadas a un total de 1000 personas.

## <a name="october-2016"></a>Octubre de 2016
* Se modificó el mensaje de error de más de una cara en targetFace de "Hay más caras en la imagen" a "Hay más de una cara en la imagen" en [FaceList: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) y [Persona de PersonGroup: agregar cara](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

## <a name="july-2016"></a>Julio de 2016
* Se admite la autenticación del objeto Face to Person en [Face:comprobar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Se agregó un parámetro `mode` opcional que permite seleccionar dos modos de trabajo: `matchPerson` y `matchFace` en [Face: buscar similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), y el valor predeterminado es `matchPerson`.

* Se agregó un parámetro `confidenceThreshold` opcional para que el usuario establezca el umbral que indicará si una cara pertenece a un objeto Person en [Face: identificar ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Se agregaron los parámetros `start` y `top` opcionales en [PersonGroup: lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) para permitir que el usuario especifique el punto inicial y el número total de elementos PersonGroups que se van a enumerar.

## <a name="v10-changes-from-v0"></a>Cambios en V1.0 desde la versión V0

* Se actualizó el punto de conexión raíz del servicio de ```https://westus.api.cognitive.microsoft.com/face/v0/``` a ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Cambios aplicados a: [Face: detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [Face: identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face:buscar similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) y [Face: grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* Se actualizó el tamaño mínimo detectable de la cara a 36 x 36 píxeles. No se detectarán caras que tengan menos de 36 x 36 píxeles.

* Los datos de PersonGroup y Person en Face V0 están en desuso. No se puede acceder a esos datos con el servicio Face V1.0.

* El punto de conexión V0 de Face API quedó en desuso el 30 de junio de 2016.