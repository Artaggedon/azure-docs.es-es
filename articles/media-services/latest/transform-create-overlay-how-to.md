---
title: Creación de una superposición con Media Encoder Standard
description: Aprenda a crear una superposición con Media Encoder Standard.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 08/31/2020
ms.openlocfilehash: 6d68f35cd7eff745a3d17009b65b1ed73a190173
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2021
ms.locfileid: "106490257"
---
# <a name="how-to-create-an-overlay-with-media-encoder-standard"></a>Creación de una superposición con Media Encoder Standard

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Media Encoder Standard permite superponer una imagen, un archivo de audio u otro vídeo a otro vídeo. La entrada debe especificar exactamente un archivo. Puede especificar un archivo de imagen en formato JPG, PNG, GIF o BMP, un archivo de audio (por ejemplo, un archivo WAV, MP3, WMA o M4A) o un archivo de vídeo.


## <a name="prerequisites"></a>Prerrequisitos

* Recopile la información de la cuenta que necesita para configurar el archivo *appsettings.json* en el ejemplo. Si no está seguro de cómo hacerlo, consulte [Inicio rápido: Registro de una aplicación en la plataforma de identidad de Microsoft](../../active-directory/develop/quickstart-register-app.md). Se esperan los valores siguientes en el archivo *appsettings.json*.

    ```json
    {
    "AadClientId": "",
    "AadEndpoint": "https://login.microsoftonline.com",
    "AadSecret": "",
    "AadTenantId": "",
    "AccountName": "",
    "ArmAadAudience": "https://management.core.windows.net/",
    "ArmEndpoint": "https://management.azure.com/",
    "Location": "",
    "ResourceGroup": "",
    "SubscriptionId": ""
    }
    ```

Si todavía no está familiarizado con las Transformaciones, se recomienda que complete las actividades siguientes:

* Lea [Codificación de vídeo y audio con Media Services](encode-concept.md).
* Lea [Procedimiento de codificación con una transformación personalizada - .NET](transform-custom-presets-how-to.md). Siga los pasos que aparecen en ese artículo para configurar la instancia de .NET necesaria para trabajar con las transformaciones y, a continuación, vuelva aquí para probar un ejemplo de valor preestablecido de superposiciones.
* Consulte el [documento de referencia de Transformaciones](/rest/api/media/transforms).

Una vez que esté familiarizado con las Transformaciones, descargue el ejemplo de superposiciones.

## <a name="overlays-preset-sample"></a>Ejemplo de valor preestablecido de superposiciones

Descargue el [ejemplo de media-services-overlay](https://github.com/Azure-Samples/media-services-overlays) para empezar a trabajar con las superposiciones.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [transforms next steps](./includes/transforms-next-steps.md)]
