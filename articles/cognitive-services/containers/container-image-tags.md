---
title: Etiquetas de imágenes de contenedor de Cognitive Services
titleSuffix: Azure Cognitive Services
description: Una lista completa de todas las etiquetas de imágenes de contenedor de Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/17/2020
ms.author: aahi
ms.openlocfilehash: c3023c32f2a31235f5a733639a548c190197b413
ms.sourcegitcommit: 706e7d3eaa27f242312d3d8e3ff072d2ae685956
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99981126"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Etiquetas de imágenes de contenedor de Azure Cognitive Services notas de la versión

Azure Cognitive Services ofrece muchas imágenes de contenedor. Los registros de contenedor y los repositorios correspondientes varían entre las imágenes de contenedor. Cada nombre de imagen de contenedor ofrece varias etiquetas. Una etiqueta de imagen de contenedor es un mecanismo de control de versiones de la imagen de contenedor. Este artículo está pensado para usarse como una referencia completa para mostrar todas las imágenes de contenedor de Cognitive Services y sus etiquetas disponibles.

> [!TIP]
> Al usar [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), preste especial atención a las mayúsculas y minúsculas del registro de contenedor, el repositorio, el nombre de la imagen del contenedor y la etiqueta correspondiente, ya que se **distingue mayúsculas de minúsculas**.

## <a name="anomaly-detector"></a>Anomaly Detector

La imagen del contenedor de [Anomaly Detector][ad-containers] se puede encontrar en el sindicato del registro de contenedor de `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/decision` y se denomina `anomaly-detector`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>Read OCR (reconocimiento óptico de caracteres)

La imagen de contenedor de [Computer Vision][cv-containers] Read OCR se puede encontrar en la distribución del registro de contenedor de `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services` y se denomina `read`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/vision/read`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Notas de la versión `3.2-preview.2`:
* Versión de Distroless
* Parámetro ReadingOrder para elegir entre el orden de la línea de texto en la respuesta JSON
* Registro mejorado
* Revisiones del modelo CJK

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
|3.2.2.014850001-49e0eac6-amd64-preview                     |       |

# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)

Notas de la versión `v2.0.013250001-amd64-preview`:

* Se reduce aún más el uso de memoria para el contenedor.
* La memoria caché externa es necesaria para la configuración de varios pods. Por ejemplo, configure Redis para el almacenamiento en caché.
* Se han corregido los resultados que faltan cuando se configura Redis Cache y `ResultExpirationPeriod` se establece en 0.
* Se elimina el límite de 26 MB para el tamaño del cuerpo de la solicitud. Ahora, el contenedor puede aceptar archivos de más de 26 MB.
* Se agrega una marca de tiempo y una versión de compilación al registro de la consola.

Notas de la versión `1.1.013050001-amd64-preview`

* Se ha agregado la configuración de inicialización de contenedores `ReadEngineConfig:ResultExpirationPeriod` para especificar el momento en que el sistema debe limpiar los resultados del reconocimiento. 
    * La configuración se encuentra en horas y el valor predeterminado es de 48 h.
    * La configuración puede reducir el uso de memoria para el almacenamiento de resultados, en especial cuando se usa el almacenamiento en memoria del contenedor.
    * Ejemplo 1. ReadEngineConfig:ResultExpirationPeriod=1: el sistema borrará el resultado del reconocimiento 1 h después del proceso.
    * Si este valor se establece en 0, el sistema borrará el resultado de la operación de reconocimiento una vez recuperado.

* Se ha corregido un error interno del servidor (500) cuando se pasa un formato de imagen no válido al sistema. Ahora se devolverá un error 400:

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `2.0.013250001-amd64-preview` |       |
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>Form Recognizer

La imagen del contenedor de [Form Recognizer][fr-containers] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/custom-form` y se denomina `labeltool`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

La imagen de contenedor [LUIS][lu-containers] se puede encontrar en el sindicato de registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/language` y se denomina `luis`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/language/luis`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>Conversión de voz a texto personalizada

La imagen de contenedor [Custom Speech-to-text][sp-cstt] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `custom-speech-to-text`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text`. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list).


# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Nota de la versión `2.9.0-amd64`:

**Característica**
* Más detalles de error de problemas al capturar modelos personalizados por identificador.
* Se admite la hipótesis de forma predeterminada en los resultados de la conversación.

Tenga en cuenta que debido a las listas de frases incluidas, el tamaño de esta imagen de contenedor ha aumentado.

| Etiquetas de imagen                    | Notas | Digest                                                                   |
|-------------------------------|:------|:-------------------------------------------------------------------------|
| `latest`                      |       | `sha256:bb1707cadba7add464a86df46d7d340c699692fe8df8d2222816189f06447a1b`|
| `2.9.0-amd64`                 |       | `sha256:bb1707cadba7add464a86df46d7d340c699692fe8df8d2222816189f06447a1b`|


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

Nota de la versión `2.7.0-amd64`:

**Características**
* La puntuación está habilitada de forma predeterminada.

Tenga en cuenta que debido a las listas de frases incluidas, el tamaño de esta imagen de contenedor ha aumentado.

Nota de la versión `2.6.0-amd64`:

**Características**
* Compatibilidad con Phraselist v2 
* Se admiten listas de frases en las siguientes configuraciones regionales:
    * en-au
    * en-ca
    * en-gb
    * en-in
    * es-es
    * zh-CN
* Compatibilidad con la descarga del modelo base personalizado. 
    * Use `BaseModelLocale=<locale>` con el comando `docker run`.
* Completamente migrado a .NET 3.1

**Correcciones**
* Se ha corregido un problema por el que la puntuación de confianza era siempre 1 en el modo Diarización.
* Se ha migrado a la API TextAnalytics 3.0

Tenga en cuenta que debido a las listas de frases incluidas, el tamaño de esta imagen de contenedor ha aumentado.

Nota de la versión `2.5.0-amd64`:

**Características**
* Admite la pronunciación personalizada en modelos personalizados
* Admite las nubes de Azure y Azure for US Government

**Correcciones**
* Se ha corregido un problema con la ejecución de un usuario no raíz en el modo Diarización.

| Etiquetas de imagen                    | Notas               |
|-------------------------------|:--------------------|
| `2.7.0-amd64`                 |                     |
| `2.6.0-amd64`                 |                     |
| `2.5.0-amd64`                 |   Primera versión con disponibilidad general    |

---

## <a name="custom-text-to-speech"></a>Conversión de texto a voz personalizada

La imagen de contenedor [Conversión de texto a voz personalizada][sp-ctts] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `custom-text-to-speech`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech`. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list).


# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Nota de la versión `1.11.0-amd64`:

**Característica**
* Más detalles de error de problemas al capturar modelos personalizados por identificador.

| Etiquetas de imagen                    | Notas | Digest                                                                   |
|-------------------------------|:------|:-------------------------------------------------------------------------|
| `latest`                      |       | `sha256:a4a5758a368bc56590eb1c1552fa2bda54f27be9cb0f491468388b9f90cb9110` |
| `1.11.0-amd64`                |       | `sha256:a4a5758a368bc56590eb1c1552fa2bda54f27be9cb0f491468388b9f90cb9110` |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

Nota de la versión `1.9.0-amd64`:

Versión mensual normal

Nota de la versión `1.8.0-amd64`:

**Características**
* Completamente migrado a .NET 3.1

Nota de la versión `1.7.0-amd64`:

**Característica**
* Parcialmente migrada a .NET 3.1

| Etiquetas de imagen                    | Notas               |
|-------------------------------|:--------------------|
| `1.9.0-amd64`                 |                     |
| `1.8.0-amd64`                 |                     |
| `1.7.0-amd64`                 |   Primera versión con disponibilidad general    |

---

## <a name="speech-to-text"></a>Voz a texto

La imagen de contenedor [Speech-to-text][sp-stt] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `speech-to-text`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text`. Puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list).

Desde la versión 2.5.0 de Speech-to-text, se admiten imágenes en la región de *Virginia del gobierno de Estados Unidos*. Use el punto de conexión de facturación de la región de *Virginia del gobierno de Estados Unidos* y las claves de API al usar esta región.

# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Nota de la versión `2.9.0-amd64-<locale>`:

**Característica**
* Más detalles de error de problemas al capturar modelos personalizados por identificador.
* Se admite la hipótesis de forma predeterminada en los resultados de la conversación.

Tenga en cuenta que debido a las listas de frases incluidas, el tamaño de esta imagen de contenedor ha aumentado. 

| Etiquetas de imagen                    | Notas                                                                                                |
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | Imagen de contenedor con la configuración regional `en-US`.                                                             |
| `2.9.0-amd64-<locale>`        | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `2.9.0-amd64-en-us`. |

Este contenedor tiene las siguientes configuraciones regionales disponibles.

| Configuración regional para la versión v2.9.0           | Notas                                    | Digest                                                                    |
|-----------------------------|:-----------------------------------------|:--------------------------------------------------------------------------|
| `ar-ae`                     | Imagen de contenedor con la configuración regional `ar-AE`. | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-bh`                     | Imagen de contenedor con la configuración regional `ar-BH`. | `sha256:41e7942e4026beaad93e50f199a6a2d855f77c74e60bc9636bf2bf2c7d3bd482` |
| `ar-eg`                     | Imagen de contenedor con la configuración regional `ar-EG`. | `sha256:d27f383435770aa01bb4117ba2d50a05ec172a1da35c4920ab43cd0fb74f44c2` |
| `ar-iq`                     | Imagen de contenedor con la configuración regional `ar-IQ`. | `sha256:ca2734a6bfc562c4c07981358051d281fb5e089815b9eac14c66a0e6f92e9858` |
| `ar-jo`                     | Imagen de contenedor con la configuración regional `ar-JO`. | `sha256:57429ee8e95a76ec953f1b1f94b39a20507626cd7fe5431df826912e5b959e41` |
| `ar-kw`                     | Imagen de contenedor con la configuración regional `ar-KW`. | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-lb`                     | Imagen de contenedor con la configuración regional `ar-LB`. | `sha256:4c5fb6fdc08343e8640222583373effae3d03907cf1262a4fad3303df9385797` |
| `ar-om`                     | Imagen de contenedor con la configuración regional `ar-OM`. | `sha256:5ffd280908e3ee65fcb7bea0b532844f9d8510044ab4c2c612dc3c235938ad0a` |
| `ar-qa`                     | Imagen de contenedor con la configuración regional `ar-QA`. | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-sa`                     | Imagen de contenedor con la configuración regional `ar-SA`. | `sha256:08885bedb2993daf0c918ecdc6ec775f7982ffa5ca561e80ab9b8a103cde8194` |
| `ar-sy`                     | Imagen de contenedor con la configuración regional `ar-SY`. | `sha256:00f3d1fd6ccb857ccef8a72322336e7a097d04027411f0dcc5499b44229fb470` |
| `bg-bg`                     | Imagen de contenedor con la configuración regional `bg-BG`. | `sha256:aa6ae12f786dcaa028e5867abba198effed875b6bc4cbafd4be37349e95dceef` |
| `ca-es`                     | Imagen de contenedor con la configuración regional `ca-ES`. | `sha256:515a940ccd76ef1926bab3ad259e1cc7ac2bd90bb3860d28f83d0f6324b3f0fe` |
| `cs-cz`                     | Imagen de contenedor con la configuración regional `cs-CZ`. | `sha256:03f6242d73de64c3eb3347400ea6e7408a8816bd96f3d6368ea2a8193accd457` |
| `da-dk`                     | Imagen de contenedor con la configuración regional `da-DK`. | `sha256:ed6714e804ff2d1bbd41512c78906ad9b8827dfdfed0076a271817e075c2ec40` |
| `de-de`                     | Imagen de contenedor con la configuración regional `de-DE`. | `sha256:386f2bb4c4b6ba797919ddcb5bbc9942bf8a03e774f9b01438f9bae0928414ef` |
| `el-gr`                     | Imagen de contenedor con la configuración regional `el-GR`. | `sha256:28696d10c78404fec033794e6e6ae0bfd92b0dab5cf7eb1d24cc2cdfbfcb646d` |
| `en-au`                     | Imagen de contenedor con la configuración regional `en-AU`. | `sha256:dd9ce70f83767a5bdc52fd62b96e09ce6f79ecc1903ed8e116753099b06b03cd` |
| `en-ca`                     | Imagen de contenedor con la configuración regional `en-CA`. | `sha256:70095cf952565256f3a0927358d0fd802d28fe1c3b89b26ead31ba1127cd0b06` |
| `en-gb`                     | Imagen de contenedor con la configuración regional `en-GB`. | `sha256:836bc38328636799ec9c8717618d51ab8b50ea2f0dc9663f342c4454938c9b23` |
| `en-hk`                     | Imagen de contenedor con la configuración regional `en-HK`. | `sha256:eda3702d95d4ae3b64ceb93bda42e8522776e141a18b2a3dde3bc3fcf0e9a2b8` |
| `en-ie`                     | Imagen de contenedor con la configuración regional `en-IE`. | `sha256:bfc2126fffb947bf10ac379efb70db3d2c7ee2c16dd541a5b86e03e73d7d477c` |
| `en-in`                     | Imagen de contenedor con la configuración regional `en-IN`. | `sha256:5660d02eabf4e1e9f58e7993ed7e5917b1990b41ed35a484a715d7265400cd0b` |
| `en-nz`                     | Imagen de contenedor con la configuración regional `en-NZ`. | `sha256:891c1805fd8011865de7371ffd4bde85d879341f2100e8053bbbc722d7c792bc` |
| `en-ph`                     | Imagen de contenedor con la configuración regional `en-PH`. | `sha256:21d6d46398f940a769241fdfffec5658356e54b4127b44efe5e061724f7a7681` |
| `en-sg`                     | Imagen de contenedor con la configuración regional `en-SG`. | `sha256:6f473b8ba56bad098c21a0c0496cb312dafcfb83dc1a2e1aff21011f6b39321d` |
| `en-us`                     | Imagen de contenedor con la configuración regional `en-US`. | `sha256:20aa22d24e35f7d92ceac96d2cbab8ce46ee0ed7bb601f18fa867f1bd0bcf5ab` |
| `en-za`                     | Imagen de contenedor con la configuración regional `en-ZA`. | `sha256:5e5ad2b016a1ceac500813e0a68ff4108ddf5a4ca98cb0aed4930b6d1e8920dd` |
| `es-ar`                     | Imagen de contenedor con la configuración regional `es-AR`. | `sha256:b372d9e32e7b518bb9949d8db459bd4e300304e53aed1342aba65a054d4a4c25` |
| `es-bo`                     | Imagen de contenedor con la configuración regional `es-BO`. | `sha256:d3538f3834c554ebebbdfe75e261a06f104dfa27143353601c3a6a3d41025129` |
| `es-cl`                     | Imagen de contenedor con la configuración regional `es-CL`. | `sha256:0bb100ef5313b182a59c08949e4baf1086bde2c1a6bca3324c4e052f465f7632` |
| `es-co`                     | Imagen de contenedor con la configuración regional `es-CO`. | `sha256:cdab27080ef3ded55dcf89cf85bc2ae16de1372f84a42d836ff5f20612b68a61` |
| `es-cr`                     | Imagen de contenedor con la configuración regional `es-CR`. | `sha256:e4ea51ffa38f347adc7c0642d50237cfa045683f52b5e3e726e4c28688231d35` |
| `es-cu`                     | Imagen de contenedor con la configuración regional `es-CU`. | `sha256:f81c0b7f774d64e673a1311d00604f5e4837fdba4d8fb4a2ab0c8bb8b7fde87d` |
| `es-do`                     | Imagen de contenedor con la configuración regional `es-DO`. | `sha256:78035c54e649e34cd8276a402f9c9845e13bc40503da6c2f631698a16a049c67` |
| `es-ec`                     | Imagen de contenedor con la configuración regional `es-EC`. | `sha256:e4e4d9c123e452f8ae89bf6cc1292a406f7b482668e36b48ef2fbb29f14c4360` |
| `es-es`                     | Imagen de contenedor con la configuración regional `es-ES`. | `sha256:10a4ddd279633cc8696b00be77f6e9309494a560244a325982522aaa805806e7` |
| `es-gt`                     | Imagen de contenedor con la configuración regional `es-GT`. | `sha256:a603a8f9c1778808df5d14e3fa1c7e993ef9cca3e0b515a4d4586c2c3a1d14b6` |
| `es-hn`                     | Imagen de contenedor con la configuración regional `es-HN`. | `sha256:4f539f8019c489623868bf02f3c61ed4b66d3a85e89250a9b484717a91e9489e` |
| `es-mx`                     | Imagen de contenedor con la configuración regional `es-MX`. | `sha256:20fc3806f08ad4e6fd5fb1f71318f1f5b591e2085ee4cbba2f25ea06135e5f6a` |
| `es-ni`                     | Imagen de contenedor con la configuración regional `es-NI`. | `sha256:d65520a4f628f6a416171ac58341579fdffba97ddd2941a910bda385d31c735d` |
| `es-pa`                     | Imagen de contenedor con la configuración regional `es-PA`. | `sha256:d38ea88613f5db6d6d9f879ef92a204c524bb27766848b825d1e6ce2a9b13cf7` |
| `es-pe`                     | Imagen de contenedor con la configuración regional `es-PE`. | `sha256:02205d1ecc29feed3ac8442dbdc1855c419749d9dcbd98028a5d1619166f0328` |
| `es-pr`                     | Imagen de contenedor con la configuración regional `es-PR`. | `sha256:c9c3e1ac800120a14f472c8be62730a489e00f29df29fe770a56429ea1c09ef5` |
| `es-py`                     | Imagen de contenedor con la configuración regional `es-PY`. | `sha256:859c24c40e65bc19a866218466eb7678f71205bedfcb6ee3180b6cb721194b9a` |
| `es-sv`                     | Imagen de contenedor con la configuración regional `es-SV`. | `sha256:036f13d34005f5d6634387c9d13c3535724795b0d6cad832fc46363609fc2f11` |
| `es-us`                     | Imagen de contenedor con la configuración regional `es-US`. | `sha256:b8eb300d0a11dc397d0bab02e1f6b26de6091595fd052ebb607f196c28d16f1c` |
| `es-uy`                     | Imagen de contenedor con la configuración regional `es-UY`. | `sha256:0ffba124ecd79777ca08055689a1d853916ccd8c8f2806d0001edf5eb4aa42fa` |
| `es-ve`                     | Imagen de contenedor con la configuración regional `es-VE`. | `sha256:4d7caf48264eaf18bb2d07b0258d6f64b7c26815fdbdf812718dd8e88f1a6d1e` |
| `et-ee`                     | Imagen de contenedor con la configuración regional `et-EE`. | `sha256:310abdc1a8490990a99ce061f04c9d49cafb7a452fbfdc2790de6f60e1505c6c` |
| `fi-fi`                     | Imagen de contenedor con la configuración regional `fi-FI`. | `sha256:8f209d30b2d148224b296c2d2c204b5970fbe7aaf5eb3289cf8b6644bfd78373` |
| `fr-ca`                     | Imagen de contenedor con la configuración regional `fr-CA`. | `sha256:11b718d4b86d606b198e47deaa25f6ce164cfc53267048e3d2dbe1bc8500cc5a` |
| `fr-fr`                     | Imagen de contenedor con la configuración regional `fr-FR`. | `sha256:7a4264a0e9560e6aa3fdee80c3e3f55a0e26cddce8ebbeb7a9c87693ab451a25` |
| `ga-ie`                     | Imagen de contenedor con la configuración regional `ga-IE`. | `sha256:bbc764ac08b2ef10ac58a8f9534d4d375109fdf16ab75c8cdbf2d57aa692d3e2` |
| `gu-in`                     | Imagen de contenedor con la configuración regional `gu-IN`. | `sha256:2d0a83b7bcf1cfc50cf013c95442519e5236a146b7968e75e129b3a5c33ad3a1` |
| `hi-in`                     | Imagen de contenedor con la configuración regional `hi-IN`. | `sha256:f0ee8f259035ac5dd9ef38807495d0f8d989ddbb8eacf83893f1fea22265e6b4` |
| `hr-hr`                     | Imagen de contenedor con la configuración regional `hr-HR`. | `sha256:6101ecac9f5f35c1ea1b8cd8e52fdbbc1be2582e4f3e385c16509fd95a002217` |
| `hu-hu`                     | Imagen de contenedor con la configuración regional `hu-HU`. | `sha256:9e94c4d6fff73058ce4eef609b8404430a429c6961648655c915cb2fac10656f` |
| `it-it`                     | Imagen de contenedor con la configuración regional `it-IT`. | `sha256:44986ad44bb53eaf350e0865e62ea5ba7f37d1f5b52e388f61f56fd7afe8ff32` |
| `ja-jp`                     | Imagen de contenedor con la configuración regional `ja-JP`. | `sha256:6b7aaa828d1b2d2fce1831e540e08ba60307088b90ca32e96fd002a67aff926b` |
| `ko-kr`                     | Imagen de contenedor con la configuración regional `ko-KR`. | `sha256:1abeda544a7579daac7f8b8f8d34a2cc63b4bd3631e474315d424973ae024ab0` |
| `lt-lt`                     | Imagen de contenedor con la configuración regional `lt-LT`. | `sha256:455da50a7db591df7be69d7cd361a77734b9249101d8cf86b807f0350b5167ef` |
| `lv-lv`                     | Imagen de contenedor con la configuración regional `lv-LV`. | `sha256:676e17b6223e35d1897b46536e6f523e1d18b78f834b62ec00bb126ad3a2e71a` |
| `mr-in`                     | Imagen de contenedor con la configuración regional `mr-IN`. | `sha256:dbfb97e52dc4b4c71dec1a9e622714f004b1e59d7900260e09a85bf15912fccd` |
| `mt-mt`                     | Imagen de contenedor con la configuración regional `mt-MT`. | `sha256:19f7f644ae3a0639fdcc53acc065d0e534b74c07f8c095418d4d4d444c566bf1` |
| `nb-no`                     | Imagen de contenedor con la configuración regional `nb-NO`. | `sha256:d3a13ab6fa2eb5d5ca0e3281b1092452650e9ede8749f6edcab990e3bbb8d198` |
| `nl-nl`                     | Imagen de contenedor con la configuración regional `nl-NL`. | `sha256:7ad5e61f9a72c600bdc79e4c04ac63c239951ac4c0d44e02fe0607a6aff356cc` |
| `pl-pl`                     | Imagen de contenedor con la configuración regional `pl-PL`. | `sha256:fe6a4812534d704b145b84fd8857fb3d9052f67fcbbd5d490c5902082e295195` |
| `pt-br`                     | Imagen de contenedor con la configuración regional `pt-BR`. | `sha256:adcd34941d4ace7db01bd476d61c9bbafe071419932b4cfae5231cf202af3a14` |
| `pt-pt`                     | Imagen de contenedor con la configuración regional `pt-PT`. | `sha256:0534a7e4b391f1ee666b248a274879c081496ed4939b0ad33154d8a96fd67f94` |
| `ro-ro`                     | Imagen de contenedor con la configuración regional `ro-RO`. | `sha256:091ea4a31652ff9dbc6259636f6c12b0ceb79a269e2cf3cdec677a1914b6a64e` |
| `ru-ru`                     | Imagen de contenedor con la configuración regional `ru-RU`. | `sha256:5eef3ae8afb445e60bb913edd6eed1415abb0bfbc439978f69f4cba7b61c8e6e` |
| `sk-sk`                     | Imagen de contenedor con la configuración regional `sk-SK`. | `sha256:98709e9349d889b57933317005af42770e47ce8178a7d9c737d9fbdd81148478` |
| `sl-si`                     | Imagen de contenedor con la configuración regional `sl-SI`. | `sha256:3a9139334c4780dc6f6a9b0f15fba5292e16ecf1f5d45fe49a9c8ef3b0e110b3` |
| `sv-se`                     | Imagen de contenedor con la configuración regional `sv-SE`. | `sha256:b29b2a65d83c20d65ba4e4fbca66f9fc07e536e161f90448c2bb360eb8de1e55` |
| `ta-in`                     | Imagen de contenedor con la configuración regional `ta-IN`. | `sha256:4302e1d979b24a23595ee2b1fd074a57ee36166ce9ac400a3deb397341ae52b2` |
| `te-in`                     | Imagen de contenedor con la configuración regional `te-IN`. | `sha256:69be11a63199d9a6f63ac346e689051ba9cd5214894b110da2879aaa0f4a8e88` |
| `th-th`                     | Imagen de contenedor con la configuración regional `th-TH`. | `sha256:2e4167dacdcb2c9d91930356ebae311b6b33ceb3e85f908422e880edbd42da64` |
| `tr-tr`                     | Imagen de contenedor con la configuración regional `tr-TR`. | `sha256:d46289ee9ba71c9c1dbbefa5da439e71310af74633c9d6d6d448d2ebee60da02` |
| `zh-cn`                     | Imagen de contenedor con la configuración regional `zh-CN`. | `sha256:49eeee500e07ffd3056ba8aab314d6c8458399a8c0d6d44ce1d9aebf50ddca06` |
| `zh-hk`                     | Imagen de contenedor con la configuración regional `zh-HK`. | `sha256:5a3251ad6df9565d44dd422de4fa0d83a9b50c8a80ec15213403482940d2b2fc` |
| `zh-tw`                     | Imagen de contenedor con la configuración regional `zh-TW`. | `sha256:2c45dd90b0c19d7f12b1be44d3e85fe2603cea2389c2877b79d6de351839cf6a` |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

Nota de la versión `2.7.0-amd64-<locale>`:

**Características**
* Compatibilidad con las siguientes configuraciones regionales nuevas:
    * ar-bh, ar-iq, ar-jo, ar-lb, ar-om, ar-sy
    * bg-bg
    * el-gr
    * en-hk, en-ie, en-ph, en-sg, en-za
    * es-ar, es-bo, es-cl, es-co, es-cr, es-cu, es-do, es-ec, es-gt, es-pa, es-pe, es-pr, es-py, es-sv, es-us, es-uy, es-ve
    * et-ee
    * ga-ie
    * hr-hr
    * hu-HU
    * lt-lt
    * lv-lv
    * mt-mt
    * ro-ro
    * sk-sk
    * sl-sl
* La puntuación está habilitada de forma predeterminada.

Tenga en cuenta que debido a las listas de frases incluidas, el tamaño de esta imagen de contenedor ha aumentado. 

Nota de la versión `2.6.0-amd64-<locale>`:

**Características**
* Se ha actualizado a los modelos más recientes y se ha migrado completamente a .NET 3.1
* Compatibilidad con Phraselist v2
* Se admiten listas de frases en las siguientes configuraciones regionales:
    * en-au
    * en-ca
    * en-gb
    * en-in
    * es-es
    * zh-CN
* Se admite la nueva configuración regional `cs-CZ`. 
    * Actualmente, no se admite el uso de mayúsculas ni el de puntuación.

**Correcciones**
* Se ha corregido un problema por el que las puntuaciones de confianza eran siempre 1 en el modo Diarización.
* Se ha migrado a la API TextAnalytics 3.0

Tenga en cuenta que debido a las listas de frases incluidas, el tamaño de esta imagen de contenedor ha aumentado. 

Nota de la versión `2.5.0-amd64-<locale>`:

**Características**
* Compatibilidad con la nube de Azure for US Government

**Correcciones**
* Corrige un problema con la ejecución de un usuario no raíz en el modo Diarización

| Etiquetas de imagen                  | Notas                                    |
|-----------------------------|:-----------------------------------------|
| `2.7.0-amd64-<locale>`      | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `2.7.0-amd64-en-us`. |
| `2.6.0-amd64-<locale>`      | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `2.6.0-amd64-en-us`. |
| `2.5.0-amd64-<locale>`      | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `2.5.0-amd64-en-us`. |


Este contenedor tiene las siguientes configuraciones regionales disponibles.

| Configuración regional para la versión v2.7.0           | Notas                                    | Digest                                                                   |
|-----------------------------|:-----------------------------------------|:-------------------------------------------------------------------------|
| `ar-ae`                     | Imagen de contenedor con la configuración regional `ar-AE`. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-bh`                     | Imagen de contenedor con la configuración regional `ar-BH`. | `sha256:5a2c140661f50d0c95587121ec1ab8895289f4dda5b3ad14074413e869e6bd4` |
| `ar-eg`                     | Imagen de contenedor con la configuración regional `ar-EG`. | `sha256:783bb8321fcfb7890b0c99935099f7e84c85a698c2fe0031c661e265358d79c` |
| `ar-iq`                     | Imagen de contenedor con la configuración regional `ar-IQ`. | `sha256:abd0101f73c1cf71f30da7b11b93d2a7ac8877dbfcfc2d34553d20705aca7a2` |
| `ar-jo`                     | Imagen de contenedor con la configuración regional `ar-JO`. | `sha256:d4c7fd2a1637e163aa106c23b6a759e8c78366c60ece83b3aabfe93ebabae07` |
| `ar-kw`                     | Imagen de contenedor con la configuración regional `ar-KW`. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-lb`                     | Imagen de contenedor con la configuración regional `ar-LB`. | `sha256:20e5c9105e86625c72de54290a6eb07630d35c3760f729c4b855e3661583dfe` |
| `ar-om`                     | Imagen de contenedor con la configuración regional `ar-OM`. | `sha256:97f1b44f2cbb837a2ef86441a0a52a07f706240edb6ef6618ee4db8cbbe1c19` |
| `ar-qa`                     | Imagen de contenedor con la configuración regional `ar-QA`. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sa`                     | Imagen de contenedor con la configuración regional `ar-SA`. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sy`                     | Imagen de contenedor con la configuración regional `ar-SY`. | `sha256:51980a2e2c3dd3548deedcedaf5fc688db602a5eced1a4b7df7d10750393623` |
| `bg-bg`                     | Imagen de contenedor con la configuración regional `bg-BG`. | `sha256:1c1acf0fbb353ebb04692f37eb4d4cdf0b4e309720dd7e709001dada0d1ea81` |
| `ca-es`                     | Imagen de contenedor con la configuración regional `ca-ES`. | `sha256:c60baa0007f61c7652b97b49645215de63411125d627c974c09222e316df204` |
| `cs-cz`                     | Imagen de contenedor con la configuración regional `cs-CZ`. | `sha256:3fa09fc3a6bde6b77df2444aae8fc78b5f25fb9010171d1682db116ea5801f5` |
| `da-dk`                     | Imagen de contenedor con la configuración regional `da-DK`. | `sha256:4b26dbba50c2771943880b68e0e4ea0713d0e3bb8bad884454849bccc9e94a3` |
| `de-de`                     | Imagen de contenedor con la configuración regional `de-DE`. | `sha256:5109ed80b1fecf4db0328adcd50528d0aa9e726b5fc84587c40aaea4e91256d` |
| `el-gr`                     | Imagen de contenedor con la configuración regional `el-GR`. | `sha256:fc8b466c588bf097efac2b79454d5ac0df5c6990398f07ede9be7e1d536e4bd` |
| `en-au`                     | Imagen de contenedor con la configuración regional `en-AU`. | `sha256:3461892a27fc3eb3f9610b2def00bc15f380c6b9797c90ceca19e6abb55f6a6` |
| `en-ca`                     | Imagen de contenedor con la configuración regional `en-CA`. | `sha256:a0509be39785f1e869bd96ab10e7c07d3f4e61c9aa17ff5900076e7bd64ba11` |
| `en-gb`                     | Imagen de contenedor con la configuración regional `en-GB`. | `sha256:1b976fc7ac109e61dcf74af3652c12535e3db92931d2d0bb2ea59bd46f9efed` |
| `en-hk`                     | Imagen de contenedor con la configuración regional `en-HK`. | `sha256:0b1e1df101f978869c98f6e50632712016b8311fc89b334e7f44e968d64bf2f` |
| `en-ie`                     | Imagen de contenedor con la configuración regional `en-IE`. | `sha256:c5ba0d3c7219ce39f0b918a51a7cae8a65c277f564279cad920e068725aa39f` |
| `en-in`                     | Imagen de contenedor con la configuración regional `en-IN`. | `sha256:e907f07be498f024103f6fe6abffa23e242bf3585724741b29a2f3f41d0899c` |
| `en-nz`                     | Imagen de contenedor con la configuración regional `en-NZ`. | `sha256:66845f6ce20ae71d609867c6eb4772366ce042499e4bcdce4c1b579daf7fad7` |
| `en-ph`                     | Imagen de contenedor con la configuración regional `en-PH`. | `sha256:e7874653bf66b1a1ab344b3391eb8767be34260b7f11b62fd057cbe17b805b2` |
| `en-sg`                     | Imagen de contenedor con la configuración regional `en-SG`. | `sha256:827cdb158280e6f4037f4815410c7aa78abf9c6467876c1504aecfef787bdd7` |
| `en-us`                     | Imagen de contenedor con la configuración regional `en-US`. | `sha256:248d17340055e3e137219ddc234c605e6a53ceead136ea55c9697c352da6a8d` |
| `en-za`                     | Imagen de contenedor con la configuración regional `en-ZA`. | `sha256:a8abc99f498db7088bb25acec47da81e90b6a5eaa1c6f78e0f9a314d839d0ae` |
| `es-ar`                     | Imagen de contenedor con la configuración regional `es-AR`. | `sha256:edf78429630851b6eb01f54f8a8a1aeeda9971c6a834403a204662eda22b3b9` |
| `es-bo`                     | Imagen de contenedor con la configuración regional `es-BO`. | `sha256:5832b44f1da2f6b9a097c99babfbc370d8d0eabe1ff8daabec2c3f482dc9d63` |
| `es-cl`                     | Imagen de contenedor con la configuración regional `es-CL`. | `sha256:409a712b96235e154472134f96ff9272265f1e5b555e00ad03c2260b0781009` |
| `es-co`                     | Imagen de contenedor con la configuración regional `es-CO`. | `sha256:99792bc083dc16e0edf15491e6a840d786c9140b747551563a8d98f66f0b415` |
| `es-cr`                     | Imagen de contenedor con la configuración regional `es-CR`. | `sha256:21fe14a538e5b8b2d288b00b8f5a02d87469e285f32e725155042079f336ac9` |
| `es-cu`                     | Imagen de contenedor con la configuración regional `es-CU`. | `sha256:05d40eae01cec4c42c4febd379cd61373eb43d0aacfd47b988bb95e6a6ad216` |
| `es-do`                     | Imagen de contenedor con la configuración regional `es-DO`. | `sha256:73dd0e0d4f39a259563ee7cc18c2e72c9ab20c52905fe343e0413ca7c4b3f0d` |
| `es-ec`                     | Imagen de contenedor con la configuración regional `es-EC`. | `sha256:c3e69139ef365fe9332b5b68b43458242c7dad9d9f2b557431272306e81cb9e` |
| `es-es`                     | Imagen de contenedor con la configuración regional `es-ES`. | `sha256:bd83fcfc116ba645a0e12a7a93b6ada74a8f701172f826a91c5f223a1dbaa61` |
| `es-gt`                     | Imagen de contenedor con la configuración regional `es-GT`. | `sha256:5bb9b18b91b74e123e3720893d88bfcb0a87dac31a1f7171d23c7cb1fa09fee` |
| `es-hn`                     | Imagen de contenedor con la configuración regional `es-HN`. | `sha256:941d108a4b76eb554e8f13cf5090665a702de3ebf35b75e4350f0916dfccd72` |
| `es-mx`                     | Imagen de contenedor con la configuración regional `es-MX`. | `sha256:cebea03732781b4425500d162ae6580bbd7ce9b5f4ede988c4570fe311d8567` |
| `es-ni`                     | Imagen de contenedor con la configuración regional `es-NI`. | `sha256:8ba165f94ad840936ebd0af17a0a63aa08a6292e7ad9029f5b93eef41165eb9` |
| `es-pa`                     | Imagen de contenedor con la configuración regional `es-PA`. | `sha256:c61b7f1b6801a03c3eab0dd1aede87017a86bc7368ded2f8bad8d9e5f60d0d3` |
| `es-pe`                     | Imagen de contenedor con la configuración regional `es-PE`. | `sha256:447a3ab3f302aba24d201d9f5b2877ffcd64dfd5e9d6b88d9924847160b2de2` |
| `es-pr`                     | Imagen de contenedor con la configuración regional `es-PR`. | `sha256:a53b3295c986e91ee8cf93ebe1057b997c76ef7f99913508b859311a194fdd4` |
| `es-py`                     | Imagen de contenedor con la configuración regional `es-PY`. | `sha256:85b3f75e75e63e29521daf772ee68a59ac2428579512501aa81dc51a2315652` |
| `es-sv`                     | Imagen de contenedor con la configuración regional `es-SV`. | `sha256:db5ece7ba536e38d5de59cd37807630ab76589dcf1c97e253f98d7f44d9424e` |
| `es-us`                     | Imagen de contenedor con la configuración regional `es-US`. | `sha256:99f2743725bb71e25543484f49bcfde14584ccbbaaa912678938d69d965075a` |
| `es-uy`                     | Imagen de contenedor con la configuración regional `es-UY`. | `sha256:a3e11c16a97a1ae76408d812b2fee1e4b3ba07160bbcb62a22814523568ee5d` |
| `es-ve`                     | Imagen de contenedor con la configuración regional `es-VE`. | `sha256:8cb431aafd84263ead8de946377c1d3f2ddfa7e172b8a4c5aa7ba477c5b41f0` |
| `et-ee`                     | Imagen de contenedor con la configuración regional `et-EE`. | `sha256:943e7cf894e9d75341a58993104824c1c8cd8da1322cc5a732e9d53882c6523` |
| `fi-fi`                     | Imagen de contenedor con la configuración regional `fi-FI`. | `sha256:35658e9dce796cb96a1371f250398e86351ea1b5ada080da7ce8471b30c7cae` |
| `fr-ca`                     | Imagen de contenedor con la configuración regional `fr-CA`. | `sha256:62256cad671e8baa03fdd4c5f4eca7d5c5effedd64cafd9020ba72c9c4210e0` |
| `fr-fr`                     | Imagen de contenedor con la configuración regional `fr-FR`. | `sha256:b385993232d9daa327d1a7b067268927b17f36eed3e8d423748794544c62746` |
| `ga-ie`                     | Imagen de contenedor con la configuración regional `ga-IE`. | `sha256:ab9abdb993b0f7487edda8200f1393ac44ba4888c0f444a02afb6c85ca3e393` |
| `gu-in`                     | Imagen de contenedor con la configuración regional `gu-IN`. | `sha256:328e69488f2948722d7ccc97e266071f61a8c9f65cd671688490955806526de` |
| `hi-in`                     | Imagen de contenedor con la configuración regional `hi-IN`. | `sha256:b9b0bfec80aa53d06ea2cbd9097f753ec5caaf00ac2f00321ae7ad916fd7fa6` |
| `hr-hr`                     | Imagen de contenedor con la configuración regional `hr-HR`. | `sha256:ab849cd2eeea682f8958bba8986fe90f0f7bb3b447512a10cf464e8e1ce4ea5` |
| `hu-hu`                     | Imagen de contenedor con la configuración regional `hu-HU`. | `sha256:30f239b155d91523442cf74a1f2732304fa2b50ae7b786833bb6a020b982621` |
| `it-it`                     | Imagen de contenedor con la configuración regional `it-IT`. | `sha256:288f95413870eb9d33bf1dabfa6fbd6b55b0faa52e4d5face3171d1dd4ddbdd` |
| `ja-jp`                     | Imagen de contenedor con la configuración regional `ja-JP`. | `sha256:e3ab37a80c215dec565eca212f57eb81887fc2894452868dff92e3bd42c4bb9` |
| `ko-kr`                     | Imagen de contenedor con la configuración regional `ko-KR`. | `sha256:c1208b8459333b606af516cd7806e9d4d5e002247bb1225e1f246563b356890` |
| `lt-lt`                     | Imagen de contenedor con la configuración regional `lt-LT`. | `sha256:8dec331161d3c29fc65ba6651fcc6cfe69fa314519f408b5f9f8eb27da09830` |
| `lv-lv`                     | Imagen de contenedor con la configuración regional `lv-LV`. | `sha256:7cf31282910b339666bb2b0a555caa7fc6ae414eea4423a41f35c3527f83235` |
| `mr-in`                     | Imagen de contenedor con la configuración regional `mr-IN`. | `sha256:9cb012bd58ef7723d4905d6fa3c1fde96e33c354b3d96d4e3ff69cf6e1bfe3a` |
| `mt-mt`                     | Imagen de contenedor con la configuración regional `mt-MT`. | `sha256:a0094c032ea555b168ec5751ab3257337d902d526e9ae335671fb751a352378` |
| `nb-no`                     | Imagen de contenedor con la configuración regional `nb-NO`. | `sha256:6bbc326e20a6a785b1ca33143b42a060858efb67b863a267d6efb7aebb48f87` |
| `nl-nl`                     | Imagen de contenedor con la configuración regional `nl-NL`. | `sha256:94b4ddf4cc80fa666e422f8416aea3f98ebe4842dfe9b1f4bfea7c47eb61127` |
| `pl-pl`                     | Imagen de contenedor con la configuración regional `pl-PL`. | `sha256:58e5f78bf772c3c8cbd5f0c5d6e67f5348e04e3f893d84738a2a3e964bab256` |
| `pt-br`                     | Imagen de contenedor con la configuración regional `pt-BR`. | `sha256:f500ef956bd28807f40df1f9f0520e437c5084f61a3be6d1379e746887d5b7c` |
| `pt-pt`                     | Imagen de contenedor con la configuración regional `pt-PT`. | `sha256:c841d2dbe5f40adf6039242c106985febb1a44212feb55d9769fe31134ec116` |
| `ro-ro`                     | Imagen de contenedor con la configuración regional `ro-RO`. | `sha256:93271c39c0a134e987a069c2a65289acff9869ae0d90fdcb39928c9ef0fd86b` |
| `ru-ru`                     | Imagen de contenedor con la configuración regional `ru-RU`. | `sha256:8d6b3c600e56cc96813b8c14b7916c5539a20ba561dc1c6d5bbef6285d6eef6` |
| `sk-sk`                     | Imagen de contenedor con la configuración regional `sk-SK`. | `sha256:6d604092cc6c964663a1c97d91c8f1c8cf4b46d07427d03f7041c0cc55eb521` |
| `sl-si`                     | Imagen de contenedor con la configuración regional `sl-SI`. | `sha256:f237ed58fedefcc749e74be1258cc70e5a690ee6c5a6b6388bd24075faa61da` |
| `sv-se`                     | Imagen de contenedor con la configuración regional `sv-SE`. | `sha256:da4233e6658b00eefdadb9d4acd889c6550a5e2a4a7af7a9f915c878abd4c9c` |
| `ta-in`                     | Imagen de contenedor con la configuración regional `ta-IN`. | `sha256:22b77606d25e9c2f52bf3cad6218782b4719f6a9dcfadc770468d266758a56c` |
| `te-in`                     | Imagen de contenedor con la configuración regional `te-IN`. | `sha256:7f4d11372862ca1d65fc9b868e2d775701b8e6eabd786c90c4e9ab82ba86e88` |
| `th-th`                     | Imagen de contenedor con la configuración regional `th-TH`. | `sha256:69033bcd7c0f59d31bafec6c2b7a9ff343928cdd58c16105415c291d555d37b` |
| `tr-tr`                     | Imagen de contenedor con la configuración regional `tr-TR`. | `sha256:4b7d339846a0d371dfe25aa2e626f131003c01329c9a1da468eb3703ef176ea` |
| `zh-cn`                     | Imagen de contenedor con la configuración regional `zh-CN`. | `sha256:a428459830fb766083212f71c5638a65ce30d8dd84f6c624ae22768e8a76976` |
| `zh-hk`                     | Imagen de contenedor con la configuración regional `zh-HK`. | `sha256:7a2903462b67336a6ce4c8e2faac42052f0a4392d1d5eb3839758cc8d0429f1` |
| `zh-tw`                     | Imagen de contenedor con la configuración regional `zh-TW`. | `sha256:30fd2b3660e047d24a46fbba14ba282f15bc0339ec93f49afd0d02ff4069146` |

| Configuración regional para v2.6.0           | Notas                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Imagen de contenedor con la configuración regional `ar-AE`. |
| `ar-eg`                     | Imagen de contenedor con la configuración regional `ar-EG`. |
| `ar-kw`                     | Imagen de contenedor con la configuración regional `ar-KW`. |
| `ar-qa`                     | Imagen de contenedor con la configuración regional `ar-QA`. |
| `ar-sa`                     | Imagen de contenedor con la configuración regional `ar-SA`. |
| `ca-es`                     | Imagen de contenedor con la configuración regional `ca-ES`. |
| `cs-cz`                     | Imagen de contenedor con la configuración regional `cs-CZ`. |
| `da-dk`                     | Imagen de contenedor con la configuración regional `da-DK`. |
| `de-de`                     | Imagen de contenedor con la configuración regional `de-DE`. |
| `en-au`                     | Imagen de contenedor con la configuración regional `en-AU`. |
| `en-ca`                     | Imagen de contenedor con la configuración regional `en-CA`. |
| `en-gb`                     | Imagen de contenedor con la configuración regional `en-GB`. |
| `en-in`                     | Imagen de contenedor con la configuración regional `en-IN`. |
| `en-nz`                     | Imagen de contenedor con la configuración regional `en-NZ`. |
| `en-us`                     | Imagen de contenedor con la configuración regional `en-US`. |
| `es-es`                     | Imagen de contenedor con la configuración regional `es-ES`. |
| `es-mx`                     | Imagen de contenedor con la configuración regional `es-MX`. |
| `fi-fi`                     | Imagen de contenedor con la configuración regional `fi-FI`. |
| `fr-ca`                     | Imagen de contenedor con la configuración regional `fr-CA`. |
| `fr-fr`                     | Imagen de contenedor con la configuración regional `fr-FR`. |
| `gu-in`                     | Imagen de contenedor con la configuración regional `gu-IN`. |
| `hi-in`                     | Imagen de contenedor con la configuración regional `hi-IN`. |
| `it-it`                     | Imagen de contenedor con la configuración regional `it-IT`. |
| `ja-jp`                     | Imagen de contenedor con la configuración regional `ja-JP`. |
| `ko-kr`                     | Imagen de contenedor con la configuración regional `ko-KR`. |
| `mr-in`                     | Imagen de contenedor con la configuración regional `mr-IN`. |
| `nb-no`                     | Imagen de contenedor con la configuración regional `nb-NO`. |
| `nl-nl`                     | Imagen de contenedor con la configuración regional `nl-NL`. |
| `pl-pl`                     | Imagen de contenedor con la configuración regional `pl-PL`. |
| `pt-br`                     | Imagen de contenedor con la configuración regional `pt-BR`. |
| `pt-pt`                     | Imagen de contenedor con la configuración regional `pt-PT`. |
| `ru-ru`                     | Imagen de contenedor con la configuración regional `ru-RU`. |
| `sv-se`                     | Imagen de contenedor con la configuración regional `sv-SE`. |
| `ta-in`                     | Imagen de contenedor con la configuración regional `ta-IN`. |
| `te-in`                     | Imagen de contenedor con la configuración regional `te-IN`. |
| `th-th`                     | Imagen de contenedor con la configuración regional `th-TH`. |
| `tr-tr`                     | Imagen de contenedor con la configuración regional `tr-TR`. |
| `zh-cn`                     | Imagen de contenedor con la configuración regional `zh-CN`. |
| `zh-hk`                     | Imagen de contenedor con la configuración regional `zh-HK`. |
| `zh-tw`                     | Imagen de contenedor con la configuración regional `zh-TW`. |

| Configuración regional para v2.5.0           | Notas                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Imagen de contenedor con la configuración regional `ar-AE`. |
| `ar-eg`                     | Imagen de contenedor con la configuración regional `ar-EG`. |
| `ar-kw`                     | Imagen de contenedor con la configuración regional `ar-KW`. |
| `ar-qa`                     | Imagen de contenedor con la configuración regional `ar-QA`. |
| `ar-sa`                     | Imagen de contenedor con la configuración regional `ar-SA`. |
| `ca-es`                     | Imagen de contenedor con la configuración regional `ca-ES`. |
| `da-dk`                     | Imagen de contenedor con la configuración regional `da-DK`. |
| `de-de`                     | Imagen de contenedor con la configuración regional `de-DE`. |
| `en-au`                     | Imagen de contenedor con la configuración regional `en-AU`. |
| `en-ca`                     | Imagen de contenedor con la configuración regional `en-CA`. |
| `en-gb`                     | Imagen de contenedor con la configuración regional `en-GB`. |
| `en-in`                     | Imagen de contenedor con la configuración regional `en-IN`. |
| `en-nz`                     | Imagen de contenedor con la configuración regional `en-NZ`. |
| `en-us`                     | Imagen de contenedor con la configuración regional `en-US`. |
| `es-es`                     | Imagen de contenedor con la configuración regional `es-ES`. |
| `es-mx`                     | Imagen de contenedor con la configuración regional `es-MX`. |
| `fi-fi`                     | Imagen de contenedor con la configuración regional `fi-FI`. |
| `fr-ca`                     | Imagen de contenedor con la configuración regional `fr-CA`. |
| `fr-fr`                     | Imagen de contenedor con la configuración regional `fr-FR`. |
| `gu-in`                     | Imagen de contenedor con la configuración regional `gu-IN`. |
| `hi-in`                     | Imagen de contenedor con la configuración regional `hi-IN`. |
| `it-it`                     | Imagen de contenedor con la configuración regional `it-IT`. |
| `ja-jp`                     | Imagen de contenedor con la configuración regional `ja-JP`. |
| `ko-kr`                     | Imagen de contenedor con la configuración regional `ko-KR`. |
| `mr-in`                     | Imagen de contenedor con la configuración regional `mr-IN`. |
| `nb-no`                     | Imagen de contenedor con la configuración regional `nb-NO`. |
| `nl-nl`                     | Imagen de contenedor con la configuración regional `nl-NL`. |
| `pl-pl`                     | Imagen de contenedor con la configuración regional `pl-PL`. |
| `pt-br`                     | Imagen de contenedor con la configuración regional `pt-BR`. |
| `pt-pt`                     | Imagen de contenedor con la configuración regional `pt-PT`. |
| `ru-ru`                     | Imagen de contenedor con la configuración regional `ru-RU`. |
| `sv-se`                     | Imagen de contenedor con la configuración regional `sv-SE`. |
| `ta-in`                     | Imagen de contenedor con la configuración regional `ta-IN`. |
| `te-in`                     | Imagen de contenedor con la configuración regional `te-IN`. |
| `th-th`                     | Imagen de contenedor con la configuración regional `th-TH`. |
| `tr-tr`                     | Imagen de contenedor con la configuración regional `tr-TR`. |
| `zh-cn`                     | Imagen de contenedor con la configuración regional `zh-CN`. |
| `zh-hk`                     | Imagen de contenedor con la configuración regional `zh-HK`. |
| `zh-tw`                     | Imagen de contenedor con la configuración regional `zh-TW`. |

---

## <a name="text-to-speech"></a>Texto a voz

La imagen de contenedor [Text-to-speech][sp-tts] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `text-to-speech`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list).


# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Nota de la versión `1.11.0-amd64-<locale-and-voice>`:

**Característica**
* Más detalles de error de problemas al capturar modelos personalizados por identificador.

| Etiquetas de imagen                                  | Notas                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaRUS`.                                            | 
| `1.11.0-amd64-<locale-and-voice>`           | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.11.0-amd64-en-us-ariarus`. |

| Configuraciones regionales para la versión v1.11.0                         | Notas                                                                      | Digest                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | Imagen de contenedor con la configuración regional `ar-EG` y voz `ar-EG-Hoda`.            | `sha256:7ba558f444ea482eca87b3e850e9b416c71391282b26a590d1ee3d9a81350188` |
| `ar-sa-naayf`                               | Imagen de contenedor con la configuración regional `ar-SA` y voz `ar-SA-Naayf`.           | `sha256:7f0afcc205340dea7ffd959812dcba6a11448f6c5c1ab55c1422a360bd876137` |
| `bg-bg-ivan`                                | Imagen de contenedor con la configuración regional `bg-BG` y voz `bg-BG-Ivan`.            | `sha256:fde80af0e2e8e49b49ddec5f1502a246cf308328738d6f572f0043e625673782` |
| `ca-es-herenarus`                           | Imagen de contenedor con la configuración regional `ca-ES` y voz `ca-ES-HerenaRUS`.       | `sha256:fb2b50b128aa84ad0cd05db2462337d316ff2d2d78f393c5a9dece588a80654e` |
| `cs-cz-jakub`                               | Imagen de contenedor con la configuración regional `cs-CZ` y voz `cs-CZ-Jakub`.           | `sha256:9dde22e5e2164bee77aaf9fe4e8fc141d9dfbe3c92c4b07da969d34aa14f7fd0` |
| `da-dk-hellerus`                            | Imagen de contenedor con la configuración regional `da-DK` y voz `da-DK-HelleRUS`.        | `sha256:4a756cd10ad21dcc2b1c7006ec961f7e267f6d2204d9ad4efd6d4730d67a4ccc` |
| `de-at-michael`                             | Imagen de contenedor con la configuración regional `de-AT` y voz `de-AT-Michael`.         | `sha256:9d531c162c4279830f99ef0d44a506a023a0137723aab3adff7a663043a1c576` |
| `de-ch-karsten`                             | Imagen de contenedor con la configuración regional `de-CH` y voz `de-CH-Karsten`.         | `sha256:353d07168b4a44fcc12a0239f5bf20e2d29365b9abe26b9b844fb6194e7c9bcc` |
| `de-de-heddarus`                            | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           | `sha256:d76ff817fc154ba0f5ce1abb93c5a0269fe5bf7b4feb3b3fe9fe8ffe6fd4fee4` |
| `de-de-hedda`                               | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           | `sha256:d76ff817fc154ba0f5ce1abb93c5a0269fe5bf7b4feb3b3fe9fe8ffe6fd4fee4` |
| `de-de-stefan-apollo`                       | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Stefan-Apollo`.   | `sha256:8e22964dc4b77c05f602f72b0e706a534a89a271c4d17b5117af122c34df9a18` |
| `el-gr-stefanos`                            | Imagen de contenedor con la configuración regional `el-GR` y voz `el-GR-Stefanos`.        | `sha256:fcd6288d5fd4ddfe3d3e65e860895f6f7a7e81216c7113f71e7b1b01eb501150` |
| `en-au-catherine`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-Catherine`.       | `sha256:e49a5ec17b696a3a73d10383d369a2ff88ccddb812898a2eedefe6e6a009ce5a` |
| `en-au-hayleyrus`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-HayleyRUS`.       | `sha256:b7fb06bd992982c7e2e71da217898da45b742aab08e901bfcef9c43acf546bc0` |
| `en-ca-heatherrus`                          | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-HeatherRUS`.      | `sha256:efd7d85845ca597937b8cbea7724cf31797855e0de5f30d66984ab9bac688152` |
| `en-ca-linda`                               | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-Linda`.           | `sha256:8211077d55b440dbb26e42db6322b35ef6ec88e8c2ec6647831e0046668ed8a4` |
| `en-gb-george-apollo`                       | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-George-Apollo`.   | `sha256:f6e924720b71d8f9a1edd4f5f2280e9054263eb79ce5364e03c9b802ad92f2dd` |
| `en-gb-hazelrus`                            | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-HazelRUS`.        | `sha256:de702f70c53e4c1647e5fdd3432d37dc8972e069fcc103a1fc2b0be70f0d6d71` |
| `en-gb-susan-apollo`                        | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-Susan-Apollo`.    | `sha256:5077cb575ffeb64e3d70184a68259438821891f6c9865350d2f887ea43ee99c1` |
| `en-ie-sean`                                | Imagen de contenedor con la configuración regional `en-IE` y voz `en-IE-Sean`.            | `sha256:c6f734cc12f04697a4d9b2003c46c5a4efd8c68da90838debb5628d9f8e70104` |
| `en-in-heera-apollo`                        | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Heera-Apollo`.    | `sha256:f5a78e857bc1563cbcd74f7b856bc2e4bd981675b397aeccfa134137f1cd3392` |
| `en-in-priyarus`                            | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-PriyaRUS`.        | `sha256:667729cafd6bf5afe071a0a2989f836943e3bb6d3d1ebe35b7fab9bb311bfebc` |
| `en-in-ravi-apollo`                         | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Ravi-Apollo`.     | `sha256:e46533f972235f297dd31fd338638f5117e3f04fa4a434d678d1cecc76db023b` |
| `en-us-aria24krus`                          | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Aria24kRUS`.      | `sha256:a8f881b60021468dbd96d9733606bd00f7f889ccb523d1773492a8301128e596` |
| `en-us-ariarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaRUS`.         | `sha256:a8f881b60021468dbd96d9733606bd00f7f889ccb523d1773492a8301128e596` |
| `en-us-benjaminrus`                         | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-BenjaminRUS`.     | `sha256:53ee105977b6440f1a7fe5088255a9c6e437c39b7c66e5cd4aba984a1667b25c` |
| `en-us-guy24krus`                           | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Guy24kRUS`.       | `sha256:537d2018f414b825aa9995d2e15e0bdb0119e45f2c6fc10d326e3df6f49ef713` |
| `en-us-zirarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-ZiraRUS`.         | `sha256:05da3347d457ca040cbe9b3e3d586d298a844f906b34ef7b6d768c247274ff1f` |
| `es-es-helenarus`                           | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-HelenaRUS`.       | `sha256:481cc43ba896a0d3291903af84120fa618130e2a2c8dce9b0ef23172b66858a8` |
| `es-es-laura-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Laura-Apollo`.    | `sha256:8cb9d071a1e01dc3e63d5f1b1c040aa6fee94488a5bbd60f2c91704abfd921cc` |
| `es-es-pablo-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Pablo-Apollo`.    | `sha256:da293ff5c49435c020044614962382040f41b6339ec83677301921a6dabbafb7` |
| `es-mx-hildarus`                            | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-HildaRUS`.        | `sha256:9677d5bbbbe0c73df93948d4ecf3f367830ef9e7cfb3b42557cf94ec514b6c68` |
| `es-mx-raul-apollo`                         | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-Raul-Apollo`.     | `sha256:a5109a6a659aa321892d4c6844e102ac72990fc2d58f32e45a072b291849fee8` |
| `fi-fi-heidirus`                            | Imagen de contenedor con la configuración regional `fi-FI` y voz `fi-FI-HeidiRUS`.        | `sha256:f8f1aa8168660ee1c21dfa4a92530bcba6f1aeb765cee9087a6cc29d7c332a8a` |
| `fr-ca-caroline`                            | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-Caroline`.        | `sha256:450f0f75f26299a89a80efc3ce93b42d6447a32022aaf4f88edc935e56100191` |
| `fr-ca-harmonierus`                         | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-HarmonieRUS`.     | `sha256:7b18adf90e6db8f8e2c5955f38aa0adfbdbd10a9a95e2cf13035b9c5416000e8` |
| `fr-ch-guillaume`                           | Imagen de contenedor con la configuración regional `fr-CH` y voz `fr-CH-Guillaume`.       | `sha256:ec3c238d0bfc3d26f20349ade1c4e19805b796f4bb3d5bf1fe4a9801b1ea1471` |
| `fr-fr-hortenserus`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-HortenseRUS`.     | `sha256:7b13613a9c5260e03ed831c79e5538633b4201867068ca0e1624b2c39fa8cf39` |
| `fr-fr-julie-apollo`                        | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Julie-Apollo`.    | `sha256:162c777447e3077438865332ac34df956be43c0429ce9962bcf5df9b210dbf01` |
| `fr-fr-paul-apollo`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Paul-Apollo`.     | `sha256:8cdf28dc31d40a69eb6720fd42b8c19792f973c4e58760abbb6573c6129c81c1` |
| `he-il-asaf`                                | Imagen de contenedor con la configuración regional `he-IL` y voz `he-IL-Asaf`.            | `sha256:3f9ec9201deca21f5e3e561d6dd673ee6fb2a7f13b4cae2985ffb69622994b99` |
| `hi-in-hemant`                              | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Hemant`.          | `sha256:c6de645816587116384ada93c02257f257a13a4b696e1bd8aeecebb9a9668f15` |
| `hi-in-kalpana-apollo`                      | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana-Apollo`.  | `sha256:455ab4c9bc7c2457e2e48265065789a54513e07a1dc9e4bc108651f118f1570d` |
| `hi-in-kalpana`                             | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana`.         | `sha256:455ab4c9bc7c2457e2e48265065789a54513e07a1dc9e4bc108651f118f1570d` |
| `hr-hr-matej`                               | Imagen de contenedor con la configuración regional `hr-HR` y voz `hr-HR-Matej`.           | `sha256:6ac24252194f91cd815736bd8be03fb95e0b965fabed5de4c631e99cd917da97` |
| `hu-hu-szabolcs`                            | Imagen de contenedor con la configuración regional `hu-HU` y voz `hu-HU-Szabolcs`.        | `sha256:bf20ea91d922beb682e321a31cabb11ebec474f47edcf4e3787882e2a204b3b5` |
| `id-id-andika`                              | Imagen de contenedor con la configuración regional `id-ID` y voz `id-ID-Andika`.          | `sha256:859bef31e5d882b508154ec00632e5e1e95bc8ea2dde6198f157703d759746c7` |
| `it-it-cosimo-apollo`                       | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-Cosimo-Apollo`.   | `sha256:b6c81ab4bd0aba217977b0bd83a8a65f7c09b5954cda0870dea15aec0dbbe1ed` |
| `it-it-luciarus`                            | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-LuciaRUS`.        | `sha256:e216a1390a0d4d9f111c56c1d655f36614947eea18d6ec91a9f6d050048b1ad4` |
| `ja-jp-ayumi-apollo`                        | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ayumi-Apollo`.    | `sha256:ba2042523ea1fff9d2c8b805ac36075169c3aecce0c965d09e326c06eab5a36f` |
| `ja-jp-harukarus`                           | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-HarukaRUS`.       | `sha256:fdbc8f59fc1c4b52c11d248ee9a5d7fe4e58343f036e558fbb33282e24d5b71f` |
| `ja-jp-ichiro-apollo`                       | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ichiro-Apollo`.   | `sha256:08ea0ed61ac152dc5caea2d4cacc81175c272cb4a835eecaa7f8e7c5485740b7` |
| `ko-kr-heamirus`                            | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-HeamiRUS`.        | `sha256:40ff95e5fb92278e369b4f37d7dbb109431ecb115b1b9516aa887e6bb4fd030b` |
| `ms-my-rizwan`                              | Imagen de contenedor con la configuración regional `ms-MY` y voz `ms-MY-Rizwan`.          | `sha256:70cfe68a81ee860136cfaed35909f522c28c20ef5514c2d9d96c283892f8b7f5` |
| `nb-no-huldarus`                            | Imagen de contenedor con la configuración regional `nb-NO` y voz `nb-NO-HuldaRUS`.        | `sha256:9941cda0e65884900532e6a0ba68e475f373277105594bf09e67225450192d3c` |
| `nl-nl-hannarus`                            | Imagen de contenedor con la configuración regional `nl-NL` y voz `nl-NL-HannaRUS`.        | `sha256:c71d980dfc70575421d1589c74e8b3e7cc036551412d0ad0f89dbc543252a405` |
| `pl-pl-paulinarus`                          | Imagen de contenedor con la configuración regional `pl-PL` y voz `pl-PL-PaulinaRUS`.      | `sha256:e5fbd98a70eb1dcf80c446b48b8f17e47ac12853bb255f0aed174c78196de257` |
| `pt-br-daniel-apollo`                       | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-Daniel-Apollo`.   | `sha256:9f57f9847f2372fa341cf037410ac68ada1c3075ab9b77cffbcf01d199f7c1f5` |
| `pt-br-heloisarus`                          | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-HeloisaRUS`.      | `sha256:ef546532c582392e6ed47df55c0fbfa6dca6d3e523547089263b57354a4efb1a` |
| `pt-pt-heliarus`                            | Imagen de contenedor con la configuración regional `pt-PT` y voz `pt-PT-HeliaRUS`.        | `sha256:116aefb76ddf39bed379c023c8260d2607314ad1b31ddef83ec2818ad9805a0b` |
| `ro-ro-andrei`                              | Imagen de contenedor con la configuración regional `ro-RO` y voz `ro-RO-Andrei`.          | `sha256:6968fdefdd798adab48faeb40857c8cdca55712dbf4806703e11ccdfab874051` |
| `ru-ru-ekaterinarus`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-EkaterinaRUS`.    | `sha256:48add20e3c147fb4be26c948841a12736c8b10d053aa7d25984df8e4016e939f` |
| `ru-ru-irina-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Irina-Apollo`.    | `sha256:ce5c055aedb3f9323f41a9de8d8f3dd23fb2ad0621d499f914f5cb3856e995f3` |
| `ru-ru-pavel-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Pavel-Apollo`.    | `sha256:badc02f9ccdee13ab7dbd4e178bd5c57d332cc3acd2d4a9a3f889d317e0517be` |
| `sk-sk-filip`                               | Imagen de contenedor con la configuración regional `sk-SK` y voz `sk-SK-Filip`.           | `sha256:763d4fe74b6f04a976482880eed76175854f659bb5bfcb315dce8ef69acead2e` |
| `sl-si-lado`                                | Imagen de contenedor con la configuración regional `sl-SI` y voz `sl-SI-Lado`.            | `sha256:73374363f9b69e03b8b9de34b319d7797876a3dae40bdce0830a67cf4bb4d4f2` |
| `sv-se-hedvigrus`                           | Imagen de contenedor con la configuración regional `sv-SE` y voz `sv-SE-HedvigRUS`.       | `sha256:317d6b5d69f56c9087cd1e8004e60a48841b997937dcdccc97e7c0b2e2ffb631` |
| `ta-in-valluvar`                            | Imagen de contenedor con la configuración regional `ta-IN` y voz `ta-IN-Valluvar`.        | `sha256:d1aaad1d5f32a910e245e6c117178c0703d39035e4053fe2dd2bb646fc02f7b8` |
| `te-in-chitra`                              | Imagen de contenedor con la configuración regional `te-IN` y voz `te-IN-Chitra`.          | `sha256:0224ac3b2de11c4f6ef65ce0bdcd1b9c4112ea472b3bd5626fdff47a5185f54c` |
| `th-th-pattara`                             | Imagen de contenedor con la configuración regional `th-TH` y voz `th-TH-Pattara`.         | `sha256:16c7384bfe210f30e09eae3542a58ff9bdbfa9253fdf4d380a53b37809f82c7d` |
| `tr-tr-sedarus`                             | Imagen de contenedor con la configuración regional `tr-TR` y voz `tr-TR-SedaRUS`.         | `sha256:5c7786c00a66346438ee4065e3eaa03ef9f8323ba839068344492b8a3b6d997a` |
| `vi-vn-an`                                  | Imagen de contenedor con la configuración regional `vi-VN` y voz `vi-VN-An`.              | `sha256:6925744597c45eed8761a9597f3525f435dd420b67ff775a73211fdef9cd9cb2` |
| `zh-cn-huihuirus`                           | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-HuihuiRUS`.       | `sha256:b38a3f465062853b171d2bce6c6d8afa14d223e24bfd5ea0827e34c26a09a2c8` |
| `zh-cn-kangkang-apollo`                     | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Kangkang-Apollo`. | `sha256:fa9555e2f520340457d5cebe469af40516237fb9398a5f90046565655b2862f8` |
| `zh-cn-yaoyao-apollo`                       | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Yaoyao-Apollo`.   | `sha256:d7eeca43e45d09a1c22611f865fb1f8b42673688a11a2acffd37a4e08a7fd8c4` |
| `zh-hk-danny-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Danny-Apollo`.    | `sha256:ee7257c0179fbe015324b4d29f16fe93964e5f1901906240477fb1d820a500f2` |
| `zh-hk-tracy-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Tracy-Apollo`.    | `sha256:dfa4effbf7d0ec6c9130c142241b3e247e226e13dc218fd44f986ca1c7fff2ed` |
| `zh-hk-tracyrus`                            | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-TracyRUS`.        | `sha256:dfa4effbf7d0ec6c9130c142241b3e247e226e13dc218fd44f986ca1c7fff2ed` |
| `zh-tw-hanhanrus`                           | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-HanHanRUS`.       | `sha256:263153fd6e05970e04af9a9bd95fb13591f0138ac030a632a6a78d95936afa4b` |
| `zh-tw-yating-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Yating-Apollo`.   | `sha256:b8289bb550b9328d83d6a7ec93bdf9524087222f537a55db0b2eb5402c2bf663` |
| `zh-tw-zhiwei-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Zhiwei-Apollo`.   | `sha256:af4bc0ef2211f69a92541bb14596341375e1003aef541aefcea7843192046b4c` |


# <a name="previous-version"></a>[Versión anterior](#tab/previous)

Nota de la versión `1.9.0-amd64-<locale-and-voice>`:

* Versión mensual normal

Nota de la versión `1.8.0-amd64-<locale-and-voice>`:

**Característica**

* Completamente migrado a .NET 3.1

Nota de la versión `1.7.0-amd64-<locale-and-voice>`:

**Característica**

* Componentes actualizados a .NET 3.1

| Etiquetas de imagen                                  | Notas                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.9.0-amd64-<locale-and-voice>`            | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.9.0-amd64-en-us-ariarus`.  |
| `1.8.0-amd64-<locale-and-voice>`            | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.8.0-amd64-en-us-ariarus`.  |
| `1.7.0-amd64-<locale-and-voice>`            | Primera versión con disponibilidad general. Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.7.0-amd64-en-us-ariarus`.  |

| Configuraciones regionales para la versión v1.9.0                          | Notas                                                                      | Digest                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | Imagen de contenedor con la configuración regional `ar-EG` y voz `ar-EG-Hoda`.            | `sha256:2b19cfd2212d6517b286aa18617d2f9d1dd1520078b559cbbf9240599270d10` | 
| `ar-sa-naayf`                               | Imagen de contenedor con la configuración regional `ar-SA` y voz `ar-SA-Naayf`.           | `sha256:6063aae5fb15c62b234cf945220916516a06ca81354c5311dee02af4d8cb0d3` |
| `bg-bg-ivan`                                | Imagen de contenedor con la configuración regional `bg-BG` y voz `bg-BG-Ivan`.            | `sha256:c6786916464755e64ffa64e69e8f3e7ef16115bac00bb6ea1e45368c42c58d1` |
| `ca-es-herenarus`                           | Imagen de contenedor con la configuración regional `ca-ES` y voz `ca-ES-HerenaRUS`.       | `sha256:2a8a1accbf99e2746c9345b77e2f261e0111227312c402cc2e1cd8760cdc82a` |
| `cs-cz-jakub`                               | Imagen de contenedor con la configuración regional `cs-CZ` y voz `cs-CZ-Jakub`.           | `sha256:3e464356bb08c9c966af2b28a88ccafd591aecd2e37a0fedb356bd443720e8d` |
| `da-dk-hellerus`                            | Imagen de contenedor con la configuración regional `da-DK` y voz `da-DK-HelleRUS`.        | `sha256:b85c43080804103673ff99dddea644a516c4103e8b1f11fa3dd34857492cd40` |
| `de-at-michael`                             | Imagen de contenedor con la configuración regional `de-AT` y voz `de-AT-Michael`.         | `sha256:87b57ee61f964e4d72e75d860c499fa3b3d8dbda6a96c97d696beb20aa8b2a9` |
| `de-ch-karsten`                             | Imagen de contenedor con la configuración regional `de-CH` y voz `de-CH-Karsten`.         | `sha256:ab1385b9746f4f054204302b9d564a433ae03748021b8ed71b4a3a224af1e9b` |
| `de-de-heddarus`                            | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-hedda`                               | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-stefan-apollo`                       | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Stefan-Apollo`.   | `sha256:56a1c63e7e6a0f5623ddc1f6a44ac6e51471d073e02e14e8c8b1e577930d816` |
| `el-gr-stefanos`                            | Imagen de contenedor con la configuración regional `el-GR` y voz `el-GR-Stefanos`.        | `sha256:ccbbb09f29ff8f276e246037183c7a3e9a3eb5bf33a942b22205cce3c6857f2` |
| `en-au-catherine`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-Catherine`.       | `sha256:0c7374890f963e1ae9507e89dc9965a94723bd57802826c0677cd5262189783` |
| `en-au-hayleyrus`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-HayleyRUS`.       | `sha256:7430bf8eace8294ca085f36ea56399261b2b4f69027e86649e8f3868fc3d811` |
| `en-ca-heatherrus`                          | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-HeatherRUS`.      | `sha256:0166ce1de3d669ea4ad80738c63369b7032125a54ecabade07241d740a94cfe` |
| `en-ca-linda`                               | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-Linda`.           | `sha256:50bed6a7bde9b793d307bcc3ace4c0f28d4a33c7a4dad9b3a394dc39a3e1c28` |
| `en-gb-george-apollo`                       | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-George-Apollo`.   | `sha256:50b800c0018a39609ddb1cee1b10062bf38a907644c393d20786db7c3ade748` |
| `en-gb-hazelrus`                            | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-HazelRUS`.        | `sha256:2aa79394dfeac8cec0cc1704a5199949cfccf347fe61161d02c7000c4ffcfa6` |
| `en-gb-susan-apollo`                        | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-Susan-Apollo`.    | `sha256:7a3174b3aae5f10241e731d392b56f124808cdd506f881ced919ced73d836c0` |
| `en-ie-sean`                                | Imagen de contenedor con la configuración regional `en-IE` y voz `en-IE-Sean`.            | `sha256:2457202fadb2354fc8d3666432096bd87c07760a4e3f4dbcc49853fff658577` |
| `en-in-heera-apollo`                        | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Heera-Apollo`.    | `sha256:e4068cd7ca4272ea94819e2ba8743d2a76c8710b162db5e9ecbde6c92c12877` |
| `en-in-priyarus`                            | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-PriyaRUS`.        | `sha256:9d63a0ed53ac06178ab84588551421c0e1d04b8bad3321410ebb99c3ca2a9e8` |
| `en-in-ravi-apollo`                         | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Ravi-Apollo`.     | `sha256:67049c9ce591336655943f5030afcfdaa150a8aace7b372425a69cc33a6b7b9` |
| `en-us-aria24krus`                          | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Aria24kRUS`.      | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-ariarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaRUS`.         | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-benjaminrus`                         | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-BenjaminRUS`.     | `sha256:93cd49adaaa2a1bdfb06ab655be164ae66f206cb7c03a2cbd59e5fba70610ab` |
| `en-us-guy24krus`                           | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Guy24kRUS`.       | `sha256:7b788bfcaae4c63c274ca15924bfd861cfcafd5fec13f685d80babc25b2949d` |
| `en-us-zirarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-ZiraRUS`.         | `sha256:bfc87a77df5695ad43481348500fba8f6a7b495708fba200706049469b5ba97` |
| `es-es-helenarus`                           | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-HelenaRUS`.       | `sha256:0b6c17aca75efb64aa9bfc0d83303038fe58d4b2fb1fc94c9380a4335b80796` |
| `es-es-laura-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Laura-Apollo`.    | `sha256:d6fcffc944c37a2dd0de29c39b82f3f8cce3a95ad925d2814ed7538335d5d4f` |
| `es-es-pablo-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Pablo-Apollo`.    | `sha256:a460bc53d9083d3c3770129995cf96cc1069ae4e8101f1739d304fe210f0af0` |
| `es-mx-hildarus`                            | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-HildaRUS`.        | `sha256:5b7578fc5b00158dfa674d95a3f1d57f22eb285e8333b4006d1fe1808bda7ba` |
| `es-mx-raul-apollo`                         | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-Raul-Apollo`.     | `sha256:03922fb017783c86d788c72e01c7ede440f8f3c913c86cab19bad4dfc2e4a2b` |
| `fi-fi-heidirus`                            | Imagen de contenedor con la configuración regional `fi-FI` y voz `fi-FI-HeidiRUS`.        | `sha256:146c1f98d6fa061016eba41db6e7b654eef222d37f35406d4b43477bb2ff897` |
| `fr-ca-caroline`                            | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-Caroline`.        | `sha256:1ee2e53f12ad1c72665d2aef64e9d4a7f9ea05670cad84dcae5e75409494f32` |
| `fr-ca-harmonierus`                         | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-HarmonieRUS`.     | `sha256:a21d25d3ac699af4e9ba9194aadd9b45f35fd9205224f3429a4c7da41fc38fe` |
| `fr-ch-guillaume`                           | Imagen de contenedor con la configuración regional `fr-CH` y voz `fr-CH-Guillaume`.       | `sha256:216125a9bd89a95d3c4dc2d7e031398659427b3aa7d4663d23a65737972e42b` |
| `fr-fr-hortenserus`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-HortenseRUS`.     | `sha256:795a698120eecbd80c48e738f73300739c1698ca859130ddb4236317bcdf70f` |
| `fr-fr-julie-apollo`                        | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Julie-Apollo`.    | `sha256:f6eb70d523c435c2e3a713b32a8af4a781df7ec043caad2fc7f458ee341eb2f` |
| `fr-fr-paul-apollo`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Paul-Apollo`.     | `sha256:28864c662a20f459b3051b1da2967a605e06267e6408285f7c2552748cf4eed` |
| `he-il-asaf`                                | Imagen de contenedor con la configuración regional `he-IL` y voz `he-IL-Asaf`.            | `sha256:eaa834bac6b69abef096b36a8baead741db78fe438af3d30f60abde3631d639` |
| `hi-in-hemant`                              | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Hemant`.          | `sha256:cfea0fa7cce9cc512f2fbb8b76f1c00fe5c32fad853c90b15934cf4ee6262fa` |
| `hi-in-kalpana-apollo`                      | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana-Apollo`.  | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hi-in-kalpana`                             | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana`.         | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hr-hr-matej`                               | Imagen de contenedor con la configuración regional `hr-HR` y voz `hr-HR-Matej`.           | `sha256:86683597c62752b4d769b69e5294979fafd4c277aaef1536e1cb19f9f06c0bf` |
| `hu-hu-szabolcs`                            | Imagen de contenedor con la configuración regional `hu-HU` y voz `hu-HU-Szabolcs`.        | `sha256:aa64eed28ca2ad060e2e02188e0401bf34e4caf7e2182b70a30ce33b3c11c9c` |
| `id-id-andika`                              | Imagen de contenedor con la configuración regional `id-ID` y voz `id-ID-Andika`.          | `sha256:0e1394d231a57a1df8163ccb634dc2ef2f8103b10608a40ab3efc5c0fbe9ded` |
| `it-it-cosimo-apollo`                       | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-Cosimo-Apollo`.   | `sha256:eef97f2817fc24405823a5fe4e825244db32279b44c0e6631e8ad9a5c1acf40` |
| `it-it-luciarus`                            | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-LuciaRUS`.        | `sha256:ebc331b0685f482d2f55619fa81fd451fd7c8f107f9cd7ad159bc6213ae4e33` |
| `ja-jp-ayumi-apollo`                        | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ayumi-Apollo`.    | `sha256:e9cb7dfd2eec154c8f3d530c16b66e8558c5955a2edaede69740067f00e43cf` |
| `ja-jp-harukarus`                           | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-HarukaRUS`.       | `sha256:93ce2ef6177c0d8ac70b61df8b11fcbcdfd3c0be0cc51cd8644f26679a741c2` |
| `ja-jp-ichiro-apollo`                       | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ichiro-Apollo`.   | `sha256:6a18bae69ac63b42ba992b8b74d8d31d91ca984d61b5f62f38be988cf38645e` |
| `ko-kr-heamirus`                            | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-HeamiRUS`.        | `sha256:7a48252d4ada2af43f9266a70113426d330bac192348cbdc929022295a0e727` |
| `ms-my-rizwan`                              | Imagen de contenedor con la configuración regional `ms-MY` y voz `ms-MY-Rizwan`.          | `sha256:90e2ecac14f8e960934fd013d208fc2a0afe1bfff037d5648d422bda8d8a76e` |
| `nb-no-huldarus`                            | Imagen de contenedor con la configuración regional `nb-NO` y voz `nb-NO-HuldaRUS`.        | `sha256:217b61bd6244b5effda8f12a2c563ce1b4572e9c5b8a08df143665f9ff754e4` |
| `nl-nl-hannarus`                            | Imagen de contenedor con la configuración regional `nl-NL` y voz `nl-NL-HannaRUS`.        | `sha256:fbff48dfc9dfadadf377867b28f6e3a3bd605e59da20f77a531efcc7d85d16e` |
| `pl-pl-paulinarus`                          | Imagen de contenedor con la configuración regional `pl-PL` y voz `pl-PL-PaulinaRUS`.      | `sha256:856a033a09925773fa4b4531e199ab7c03c537f366acecbda60f8d21735725e` |
| `pt-br-daniel-apollo`                       | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-Daniel-Apollo`.   | `sha256:2d1ec975f1aee56a6fc6039d154fb3f2fbeb4636f7078c5dfe99aeddb6a3634` |
| `pt-br-heloisarus`                          | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-HeloisaRUS`.      | `sha256:b7d629f37ab3305274764264dc08fab5236e60ef18d40e987618115db67ce44` |
| `pt-pt-heliarus`                            | Imagen de contenedor con la configuración regional `pt-PT` y voz `pt-PT-HeliaRUS`.        | `sha256:8b380ae7e4aac9d4ada4d15fa9e667387bc9ca038796d9b6999953bfbc97259` |
| `ro-ro-andrei`                              | Imagen de contenedor con la configuración regional `ro-RO` y voz `ro-RO-Andrei`.          | `sha256:b00ca7f1411169a5baf7263a8d7e5eed1a72084d9489eaf458429dfc338564a` |
| `ru-ru-ekaterinarus`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-EkaterinaRUS`.    | `sha256:31c588c31e3ac67305af66091e7756dfc4ca454317d0228116ea0b2fedf5d71` |
| `ru-ru-irina-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Irina-Apollo`.    | `sha256:e76437f8da7c279b38d2643defc997a13b4a364e9a212895cdb33a9a3f6457f` |
| `ru-ru-pavel-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Pavel-Apollo`.    | `sha256:461c1efa6cce0b10a87f338bc637aca76aef8458061a688870fb3343d682da0` |
| `sk-sk-filip`                               | Imagen de contenedor con la configuración regional `sk-SK` y voz `sk-SK-Filip`.           | `sha256:7fb0cfab4c0fe2913eb20f28a25c6663015d62f82e7e7864d9f7fac2d27697b` |
| `sl-si-lado`                                | Imagen de contenedor con la configuración regional `sl-SI` y voz `sl-SI-Lado`.            | `sha256:5336173d410e10ffeb5dc211a583887e33754319c757914955057d398dfbb0a` |
| `sv-se-hedvigrus`                           | Imagen de contenedor con la configuración regional `sv-SE` y voz `sv-SE-HedvigRUS`.       | `sha256:5dc8cdcc3054386bf69596707d9d261d4db5bfd09f1882ceb4e29238a34b24e` |
| `ta-in-valluvar`                            | Imagen de contenedor con la configuración regional `ta-IN` y voz `ta-IN-Valluvar`.        | `sha256:74ea485f23e4c1fe0029e06894860aa0188c36c0e14ea3584a06d4216ccef56` |
| `te-in-chitra`                              | Imagen de contenedor con la configuración regional `te-IN` y voz `te-IN-Chitra`.          | `sha256:ff2977a98ef691da543db08be9cfe04d7fc3bf8f78b29310c163e47303b2ddd` |
| `th-th-pattara`                             | Imagen de contenedor con la configuración regional `th-TH` y voz `th-TH-Pattara`.         | `sha256:ba7e2c0e5e75d9f2b52aa50c97728616c43e81f48c15e24665e4c2ea5770a8f` |
| `tr-tr-sedarus`                             | Imagen de contenedor con la configuración regional `tr-TR` y voz `tr-TR-SedaRUS`.         | `sha256:375a8ceae89ea1f0dda551feff30ae3679231189b527992edbc49988d042d66` |
| `vi-vn-an`                                  | Imagen de contenedor con la configuración regional `vi-VN` y voz `vi-VN-An`.              | `sha256:b6f82148295b38b4039c45c48695ec50b4e97cd02b18d49c39bf9fca3bec958` |
| `zh-cn-huihuirus`                           | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-HuihuiRUS`.       | `sha256:3e773931f3adaac92cba43773a241692a2b471ebe73ec51c475df8ff63b7ee1` |
| `zh-cn-kangkang-apollo`                     | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Kangkang-Apollo`. | `sha256:05fc0d5075a1094caf70d98b4a9469952be52cb6eb4d9f7b9ff4ae961100c7b` |
| `zh-cn-yaoyao-apollo`                       | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Yaoyao-Apollo`.   | `sha256:d7613bcefc48e85b9d6f07c8cd223c16d4958bcf7f24087575250e97c593ac1` |
| `zh-hk-danny-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Danny-Apollo`.    | `sha256:efe22bc123dac9312dcaeb859a377d81f61fbb25ef46e4678d36ec6bebc5d32` |
| `zh-hk-tracy-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Tracy-Apollo`.    | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-hk-tracyrus`                            | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-TracyRUS`.        | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-tw-hanhanrus`                           | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-HanHanRUS`.       | `sha256:95d58922463d577d4c4722ab722a5768af35fb62236d47f6709717dea758909` |
| `zh-tw-yating-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Yating-Apollo`.   | `sha256:33eec6e3aaaedafaf3969746eeaf97a1760e763505decfe2abaa03f5054bfd2` |
| `zh-tw-zhiwei-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Zhiwei-Apollo`.   | `sha256:456db2898b2e5a9c30b7071ce6ea3f141438cbf1aa4899c7ffccfc2f0dde5bd` |

| Configuraciones regionales para v1.8.0                          | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Imagen de contenedor con la configuración regional `ar-EG` y voz `ar-EG-Hoda`.            |
| `ar-sa-naayf`                               | Imagen de contenedor con la configuración regional `ar-SA` y voz `ar-SA-Naayf`.           |
| `bg-bg-ivan`                                | Imagen de contenedor con la configuración regional `bg-BG` y voz `bg-BG-Ivan`.            |
| `ca-es-herenarus`                           | Imagen de contenedor con la configuración regional `ca-ES` y voz `ca-ES-HerenaRUS`.       |
| `cs-cz-jakub`                               | Imagen de contenedor con la configuración regional `cs-CZ` y voz `cs-CZ-Jakub`.           |
| `da-dk-hellerus`                            | Imagen de contenedor con la configuración regional `da-DK` y voz `da-DK-HelleRUS`.        |
| `de-at-michael`                             | Imagen de contenedor con la configuración regional `de-AT` y voz `de-AT-Michael`.         |
| `de-ch-karsten`                             | Imagen de contenedor con la configuración regional `de-CH` y voz `de-CH-Karsten`.         |
| `de-de-hedda`                               | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           |
| `de-de-heddarus`                            | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           |
| `de-de-stefan-apollo`                       | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Stefan-Apollo`.   |
| `el-gr-stefanos`                            | Imagen de contenedor con la configuración regional `el-GR` y voz `el-GR-Stefanos`.        |
| `en-au-catherine`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-Catherine`.       |
| `en-au-hayleyrus`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-HayleyRUS`.       |
| `en-ca-heatherrus`                          | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-HeatherRUS`.      |
| `en-ca-linda`                               | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-Linda`.           |
| `en-gb-george-apollo`                       | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-George-Apollo`.   |
| `en-gb-hazelrus`                            | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-HazelRUS`.        |
| `en-gb-susan-apollo`                        | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-Susan-Apollo`.    |
| `en-ie-sean`                                | Imagen de contenedor con la configuración regional `en-IE` y voz `en-IE-Sean`.            |
| `en-in-heera-apollo`                        | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Heera-Apollo`.    |
| `en-in-priyarus`                            | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-PriyaRUS`.        |
| `en-in-ravi-apollo`                         | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Ravi-Apollo`.     |
| `en-us-benjaminrus`                         | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-BenjaminRUS`.     |
| `en-us-guy24krus`                           | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Guy24kRUS`.       |
| `en-us-aria24krus`                          | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Aria24kRUS`.      |
| `en-us-ariarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaRUS`.         |
| `en-us-zirarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-ZiraRUS`.         |
| `es-es-helenarus`                           | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-HelenaRUS`.       |
| `es-es-laura-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Laura-Apollo`.    |
| `es-es-pablo-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Pablo-Apollo`.    |
| `es-mx-hildarus`                            | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-HildaRUS`.        |
| `es-mx-raul-apollo`                         | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-Raul-Apollo`.     |
| `fi-fi-heidirus`                            | Imagen de contenedor con la configuración regional `fi-FI` y voz `fi-FI-HeidiRUS`.        |
| `fr-ca-caroline`                            | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-Caroline`.        |
| `fr-ca-harmonierus`                         | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-HarmonieRUS`.     |
| `fr-ch-guillaume`                           | Imagen de contenedor con la configuración regional `fr-CH` y voz `fr-CH-Guillaume`.       |
| `fr-fr-hortenserus`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-HortenseRUS`.     |
| `fr-fr-julie-apollo`                        | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Julie-Apollo`.    |
| `fr-fr-paul-apollo`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Paul-Apollo`.     |
| `he-il-asaf`                                | Imagen de contenedor con la configuración regional `he-IL` y voz `he-IL-Asaf`.            |
| `hi-in-hemant`                              | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Hemant`.          |
| `hi-in-kalpana-apollo`                      | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana-Apollo`.  |
| `hi-in-kalpana`                             | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana`.         |
| `hr-hr-matej`                               | Imagen de contenedor con la configuración regional `hr-HR` y voz `hr-HR-Matej`.           |
| `hu-hu-szabolcs`                            | Imagen de contenedor con la configuración regional `hu-HU` y voz `hu-HU-Szabolcs`.        |
| `id-id-andika`                              | Imagen de contenedor con la configuración regional `id-ID` y voz `id-ID-Andika`.          |
| `it-it-cosimo-apollo`                       | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-Cosimo-Apollo`.   |
| `it-it-luciarus`                            | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-LuciaRUS`.        |
| `ja-jp-ayumi-apollo`                        | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ayumi-Apollo`.    |
| `ja-jp-harukarus`                           | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-HarukaRUS`.       |
| `ja-jp-ichiro-apollo`                       | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ichiro-Apollo`.   |
| `ko-kr-heamirus`                            | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-HeamiRUS`.        |
| `ms-my-rizwan`                              | Imagen de contenedor con la configuración regional `ms-MY` y voz `ms-MY-Rizwan`.          |
| `nb-no-huldarus`                            | Imagen de contenedor con la configuración regional `nb-NO` y voz `nb-NO-HuldaRUS`.        |
| `nl-nl-hannarus`                            | Imagen de contenedor con la configuración regional `nl-NL` y voz `nl-NL-HannaRUS`.        |
| `pl-pl-paulinarus`                          | Imagen de contenedor con la configuración regional `pl-PL` y voz `pl-PL-PaulinaRUS`.      |
| `pt-br-daniel-apollo`                       | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-Daniel-Apollo`.   |
| `pt-br-heloisarus`                          | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-HeloisaRUS`.      |
| `pt-pt-heliarus`                            | Imagen de contenedor con la configuración regional `pt-PT` y voz `pt-PT-HeliaRUS`.        |
| `ro-ro-andrei`                              | Imagen de contenedor con la configuración regional `ro-RO` y voz `ro-RO-Andrei`.          |
| `ru-ru-ekaterinarus`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-EkaterinaRUS`.    |
| `ru-ru-irina-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Irina-Apollo`.    |
| `ru-ru-pavel-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Pavel-Apollo`.    |
| `sk-sk-filip`                               | Imagen de contenedor con la configuración regional `sk-SK` y voz `sk-SK-Filip`.           |
| `sl-si-lado`                                | Imagen de contenedor con la configuración regional `sl-SI` y voz `sl-SI-Lado`.            |
| `sv-se-hedvigrus`                           | Imagen de contenedor con la configuración regional `sv-SE` y voz `sv-SE-HedvigRUS`.       |
| `ta-in-valluvar`                            | Imagen de contenedor con la configuración regional `ta-IN` y voz `ta-IN-Valluvar`.        |
| `te-in-chitra`                              | Imagen de contenedor con la configuración regional `te-IN` y voz `te-IN-Chitra`.          |
| `th-th-pattara`                             | Imagen de contenedor con la configuración regional `th-TH` y voz `th-TH-Pattara`.         |
| `tr-tr-sedarus`                             | Imagen de contenedor con la configuración regional `tr-TR` y voz `tr-TR-SedaRUS`.         |
| `vi-vn-an`                                  | Imagen de contenedor con la configuración regional `vi-VN` y voz `vi-VN-An`.              |
| `zh-cn-huihuirus`                           | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-HuihuiRUS`.       |
| `zh-cn-kangkang-apollo`                     | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Kangkang-Apollo`. |
| `zh-cn-yaoyao-apollo`                       | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Yaoyao-Apollo`.   |
| `zh-hk-danny-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Danny-Apollo`.    |
| `zh-hk-tracy-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Tracy-Apollo`.    |
| `zh-hk-tracyrus`                            | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-TracyRUS`.        |
| `zh-tw-hanhanrus`                           | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-HanHanRUS`.       |
| `zh-tw-yating-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Yating-Apollo`.   |
| `zh-tw-zhiwei-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Zhiwei-Apollo`.   |

| Configuraciones regionales para v1.7.0                          | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Imagen de contenedor con la configuración regional `ar-EG` y voz `ar-EG-Hoda`.            |
| `ar-sa-naayf`                               | Imagen de contenedor con la configuración regional `ar-SA` y voz `ar-SA-Naayf`.           |
| `bg-bg-ivan`                                | Imagen de contenedor con la configuración regional `bg-BG` y voz `bg-BG-Ivan`.            |
| `ca-es-herenarus`                           | Imagen de contenedor con la configuración regional `ca-ES` y voz `ca-ES-HerenaRUS`.       |
| `cs-cz-jakub`                               | Imagen de contenedor con la configuración regional `cs-CZ` y voz `cs-CZ-Jakub`.           |
| `da-dk-hellerus`                            | Imagen de contenedor con la configuración regional `da-DK` y voz `da-DK-HelleRUS`.        |
| `de-at-michael`                             | Imagen de contenedor con la configuración regional `de-AT` y voz `de-AT-Michael`.         |
| `de-ch-karsten`                             | Imagen de contenedor con la configuración regional `de-CH` y voz `de-CH-Karsten`.         |
| `de-de-hedda`                               | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           |
| `de-de-heddarus`                            | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Hedda`.           |
| `de-de-stefan-apollo`                       | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-Stefan-Apollo`.   |
| `el-gr-stefanos`                            | Imagen de contenedor con la configuración regional `el-GR` y voz `el-GR-Stefanos`.        |
| `en-au-catherine`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-Catherine`.       |
| `en-au-hayleyrus`                           | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-HayleyRUS`.       |
| `en-ca-heatherrus`                          | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-HeatherRUS`.      |
| `en-ca-linda`                               | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-Linda`.           |
| `en-gb-george-apollo`                       | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-George-Apollo`.   |
| `en-gb-hazelrus`                            | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-HazelRUS`.        |
| `en-gb-susan-apollo`                        | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-Susan-Apollo`.    |
| `en-ie-sean`                                | Imagen de contenedor con la configuración regional `en-IE` y voz `en-IE-Sean`.            |
| `en-in-heera-apollo`                        | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Heera-Apollo`.    |
| `en-in-priyarus`                            | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-PriyaRUS`.        |
| `en-in-ravi-apollo`                         | Imagen de contenedor con la configuración regional `en-IN` y voz `en-IN-Ravi-Apollo`.     |
| `en-us-benjaminrus`                         | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-BenjaminRUS`.     |
| `en-us-guy24krus`                           | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Guy24kRUS`.       |
| `en-us-aria24krus`                          | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-Aria24kRUS`.      |
| `en-us-ariarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaRUS`.         |
| `en-us-zirarus`                             | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-ZiraRUS`.         |
| `es-es-helenarus`                           | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-HelenaRUS`.       |
| `es-es-laura-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Laura-Apollo`.    |
| `es-es-pablo-apollo`                        | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-Pablo-Apollo`.    |
| `es-mx-hildarus`                            | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-HildaRUS`.        |
| `es-mx-raul-apollo`                         | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-Raul-Apollo`.     |
| `fi-fi-heidirus`                            | Imagen de contenedor con la configuración regional `fi-FI` y voz `fi-FI-HeidiRUS`.        |
| `fr-ca-caroline`                            | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-Caroline`.        |
| `fr-ca-harmonierus`                         | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-HarmonieRUS`.     |
| `fr-ch-guillaume`                           | Imagen de contenedor con la configuración regional `fr-CH` y voz `fr-CH-Guillaume`.       |
| `fr-fr-hortenserus`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-HortenseRUS`.     |
| `fr-fr-julie-apollo`                        | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Julie-Apollo`.    |
| `fr-fr-paul-apollo`                         | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-Paul-Apollo`.     |
| `he-il-asaf`                                | Imagen de contenedor con la configuración regional `he-IL` y voz `he-IL-Asaf`.            |
| `hi-in-hemant`                              | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Hemant`.          |
| `hi-in-kalpana-apollo`                      | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana-Apollo`.  |
| `hi-in-kalpana`                             | Imagen de contenedor con la configuración regional `hi-IN` y voz `hi-IN-Kalpana`.         |
| `hr-hr-matej`                               | Imagen de contenedor con la configuración regional `hr-HR` y voz `hr-HR-Matej`.           |
| `hu-hu-szabolcs`                            | Imagen de contenedor con la configuración regional `hu-HU` y voz `hu-HU-Szabolcs`.        |
| `id-id-andika`                              | Imagen de contenedor con la configuración regional `id-ID` y voz `id-ID-Andika`.          |
| `it-it-cosimo-apollo`                       | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-Cosimo-Apollo`.   |
| `it-it-luciarus`                            | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-LuciaRUS`.        |
| `ja-jp-ayumi-apollo`                        | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ayumi-Apollo`.    |
| `ja-jp-harukarus`                           | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-HarukaRUS`.       |
| `ja-jp-ichiro-apollo`                       | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-Ichiro-Apollo`.   |
| `ko-kr-heamirus`                            | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-HeamiRUS`.        |
| `ms-my-rizwan`                              | Imagen de contenedor con la configuración regional `ms-MY` y voz `ms-MY-Rizwan`.          |
| `nb-no-huldarus`                            | Imagen de contenedor con la configuración regional `nb-NO` y voz `nb-NO-HuldaRUS`.        |
| `nl-nl-hannarus`                            | Imagen de contenedor con la configuración regional `nl-NL` y voz `nl-NL-HannaRUS`.        |
| `pl-pl-paulinarus`                          | Imagen de contenedor con la configuración regional `pl-PL` y voz `pl-PL-PaulinaRUS`.      |
| `pt-br-daniel-apollo`                       | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-Daniel-Apollo`.   |
| `pt-br-heloisarus`                          | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-HeloisaRUS`.      |
| `pt-pt-heliarus`                            | Imagen de contenedor con la configuración regional `pt-PT` y voz `pt-PT-HeliaRUS`.        |
| `ro-ro-andrei`                              | Imagen de contenedor con la configuración regional `ro-RO` y voz `ro-RO-Andrei`.          |
| `ru-ru-ekaterinarus`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-EkaterinaRUS`.    |
| `ru-ru-irina-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Irina-Apollo`.    |
| `ru-ru-pavel-apollo`                        | Imagen de contenedor con la configuración regional `ru-RU` y voz `ru-RU-Pavel-Apollo`.    |
| `sk-sk-filip`                               | Imagen de contenedor con la configuración regional `sk-SK` y voz `sk-SK-Filip`.           |
| `sl-si-lado`                                | Imagen de contenedor con la configuración regional `sl-SI` y voz `sl-SI-Lado`.            |
| `sv-se-hedvigrus`                           | Imagen de contenedor con la configuración regional `sv-SE` y voz `sv-SE-HedvigRUS`.       |
| `ta-in-valluvar`                            | Imagen de contenedor con la configuración regional `ta-IN` y voz `ta-IN-Valluvar`.        |
| `te-in-chitra`                              | Imagen de contenedor con la configuración regional `te-IN` y voz `te-IN-Chitra`.          |
| `th-th-pattara`                             | Imagen de contenedor con la configuración regional `th-TH` y voz `th-TH-Pattara`.         |
| `tr-tr-sedarus`                             | Imagen de contenedor con la configuración regional `tr-TR` y voz `tr-TR-SedaRUS`.         |
| `vi-vn-an`                                  | Imagen de contenedor con la configuración regional `vi-VN` y voz `vi-VN-An`.              |
| `zh-cn-huihuirus`                           | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-HuihuiRUS`.       |
| `zh-cn-kangkang-apollo`                     | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Kangkang-Apollo`. |
| `zh-cn-yaoyao-apollo`                       | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-Yaoyao-Apollo`.   |
| `zh-hk-danny-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Danny-Apollo`.    |
| `zh-hk-tracy-apollo`                        | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-Tracy-Apollo`.    |
| `zh-hk-tracyrus`                            | Imagen de contenedor con la configuración regional `zh-HK` y voz `zh-HK-TracyRUS`.        |
| `zh-tw-hanhanrus`                           | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-HanHanRUS`.       |
| `zh-tw-yating-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Yating-Apollo`.   |
| `zh-tw-zhiwei-apollo`                       | Imagen de contenedor con la configuración regional `zh-TW` y voz `zh-TW-Zhiwei-Apollo`.   |

---

## <a name="neural-text-to-speech"></a>Texto a voz neuronal

La imagen de contenedor [Neural Text-to-speech][sp-ntts] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `neural-text-to-speech`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list).


# <a name="latest-version"></a>[La versión más reciente](#tab/current)

Notas de la versión `v1.3.0`:
* El contenedor de Neural Text to Speech ahora está disponible con carácter general. 

| Etiquetas de imagen                                  | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaNeural`.      |
| `1.3.0-amd64-<locale-and-voice>`    | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.3.0-amd64-en-us-arianeural`. |


| Configuraciones regionales y voces para la versión v1.3.0           | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-KatjaNeural`.     |
| `en-au-natashaneural`               | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-NatashaNeural`.   |
| `en-ca-claraneural`                 | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-ClaraNeural`.     |
| `en-gb-libbyneural`                 | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-LibbyNeural`.     |
| `en-gb-mianeural`                   | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-MiaNeural`.       |
| `en-us-arianeural`                  | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaNeural`.      |
| `en-us-guyneural`                   | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-GuyNeural`.       |
| `es-es-elviraneural`                | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural`                 | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-DaliaNeural`.     |
| `fr-ca-sylvieneural`                | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural`                | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-DeniseNeural`.    |
| `it-it-elsaneural`                  | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-ElsaNeural`.      |
| `ja-jp-nanamineural`                | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-NanamiNeural`.    |
| `ko-kr-sunhineural`                 | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-SunHiNeural`.     |
| `pt-br-franciscaneural`             | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-FranciscaNeural`. |
| `zh-cn-xiaoxiaoneural`              | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-XiaoxiaoNeural`.  |

# <a name="previous-version"></a>[Versión anterior](#tab/previous)

| Etiquetas de imagen                                  | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaNeural`.      |
| `1.2.0-amd64-<locale-and-voice>-preview`    | Sustituya `<locale>` por una de las configuraciones regionales disponibles que se muestran a continuación. Por ejemplo, `1.2.0-amd64-en-us-arianeural-preview`. |


| Configuraciones regionales y voces en la versión preliminar de v1.2.0           | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaNeural`.      |
| `de-de-katjaneural-preview`                 | Imagen de contenedor con la configuración regional `de-DE` y voz `de-DE-KatjaNeural`.     |
| `en-au-natashaneural-preview`               | Imagen de contenedor con la configuración regional `en-AU` y voz `en-AU-NatashaNeural`.   |
| `en-ca-claraneural-preview`                 | Imagen de contenedor con la configuración regional `en-CA` y voz `en-CA-ClaraNeural`.     |
| `en-gb-libbyneural-preview`                 | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-LibbyNeural`.     |
| `en-gb-mianeural-preview`                   | Imagen de contenedor con la configuración regional `en-GB` y voz `en-GB-MiaNeural`.       |
| `en-us-arianeural-preview`                  | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-AriaNeural`.      |
| `en-us-guyneural-preview`                   | Imagen de contenedor con la configuración regional `en-US` y voz `en-US-GuyNeural`.       |
| `es-es-elviraneural-preview`                | Imagen de contenedor con la configuración regional `es-ES` y voz `es-ES-ElviraNeural`.    |
| `es-mx-dalianeural-preview`                 | Imagen de contenedor con la configuración regional `es-MX` y voz `es-MX-DaliaNeural`.     |
| `fr-ca-sylvieneural-preview`                | Imagen de contenedor con la configuración regional `fr-CA` y voz `fr-CA-SylvieNeural`.    |
| `fr-fr-deniseneural-preview`                | Imagen de contenedor con la configuración regional `fr-FR` y voz `fr-FR-DeniseNeural`.    |
| `it-it-elsaneural-preview`                  | Imagen de contenedor con la configuración regional `it-IT` y voz `it-IT-ElsaNeural`.      |
| `ja-jp-nanamineural-preview`                | Imagen de contenedor con la configuración regional `ja-JP` y voz `ja-JP-NanamiNeural`.    |
| `ko-kr-sunhineural-preview`                 | Imagen de contenedor con la configuración regional `ko-KR` y voz `ko-KR-SunHiNeural`.     |
| `pt-br-franciscaneural-preview`             | Imagen de contenedor con la configuración regional `pt-BR` y voz `pt-BR-FranciscaNeural`. |
| `zh-cn-xiaoxiaoneural-preview`              | Imagen de contenedor con la configuración regional `zh-CN` y voz `zh-CN-XiaoxiaoNeural`.  |

---

## <a name="speech-language-detection"></a>Detección de idioma de Voz

La imagen de contenedor [Speech language detection][sp-lid] se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/speechservices/` y se denomina `language-detection`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list).

| Etiquetas de imagen                                  | Notas                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>Extracción de frases clave

se puede encontrar en la distribución del registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/textanalytics/` y se denomina `keyphrase`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list).

# <a name="latest-version"></a>[La versión más reciente](#tab/current)


| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>Detección de idioma del texto

La imagen de contenedor [Detección de idioma][ta-la] se puede encontrar en el sindicato de registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/textanalytics/` y se denomina `language`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`.


Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list).

# <a name="latest-versions"></a>[Últimas versiones](#tab/current)

| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[Versiones anteriores](#tab/previous)


| Etiquetas de imagen                    | Notas |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>análisis de opiniones

La imagen de contenedor [Análisis de sentimiento][ta-se] se puede encontrar en el sindicato de registro de contenedor `mcr.microsoft.com`. Reside en el repositorio `azure-cognitive-services/textanalytics/` y se denomina `sentiment`. El nombre completo de la imagen de contenedor es `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`.

Esta imagen de contenedor tiene disponibles las siguientes etiquetas. También puede encontrar una lista completa de [etiquetas en el MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list).

| Etiquetas de imagen | Notas                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Análisis de sentimiento, versión 3 (inglés)               |
| `3.0-es`   | Análisis de sentimiento, versión 3 (español)               |
| `3.0-fr`   | Análisis de sentimiento, versión 3 (francés)                |
| `3.0-it`   | Análisis de sentimiento, versión 3 (italiano)               |
| `3.0-de`   | Análisis de sentimiento, versión 3 (alemán)                |
| `3.0-zh`   | Análisis de sentimiento, versión 3 (chino simplificado)  |
| `3.0-zht`  | Análisis de sentimiento, versión 3 (chino tradicional) |
| `3.0-ja`   | Análisis de sentimiento, versión 3 (japonés)              |
| `3.0-pt`   | Análisis de sentimiento, versión 3 (portugués)            |
| `3.0-nl`   | Análisis de sentimiento, versión 3 (holandés)                 |
| `2.1`    | Análisis de sentimiento, versión 2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
