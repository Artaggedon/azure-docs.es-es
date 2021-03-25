---
title: Cómo crear un procesador de multimedia mediante el SDK de Azure Media Services para .NET| Microsoft Docs
description: Aprenda a crear un componente de procesador de multimedia para codificar, cifrar, descifrar o convertir el formato de contenido multimedia para Azure Media Services. Los ejemplos de código están escritos en C# y utilizan el SDK de Media Services para .NET.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 6ce2a28ff9b34373cc716ea397927ef160bd1097
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103013353"
---
# <a name="how-to-get-a-media-processor-instance"></a>Procedimientos: Obtención de una instancia del procesador de multimedia

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)

## <a name="overview"></a>Información general

En los Media Services, un procesador multimedia es un componente que controla una tarea de procesamiento específica, como codificación, conversión de formato, cifrado o descifrado de contenido multimedia. Normalmente crea un procesador multimedia cuando crea una tarea para codificar, cifrar o convertir el formato de contenido multimedia.

## <a name="azure-media-processors"></a>Procesadores de multimedia de Azure

En el siguiente tema se proporcionan listas de procesadores de multimedia:

* [Codificación de procesadores de multimedia](scenarios-and-availability.md)
* [Procesadores de multimedia de Analytics](scenarios-and-availability.md)

## <a name="get-media-processor"></a>Obtención de un procesador multimedia

El siguiente método muestra cómo obtener una instancia del procesador multimedia. El ejemplo de código supone el uso de una variable de nivel de módulo llamada **_context** para hacer referencia al contexto de servidor tal como se describe en la sección [Procedimientos: conexión con los Media Services mediante programación](media-services-use-aad-auth-to-access-ams-api.md).

```csharp
private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
{
    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

    if (processor == null)
    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

    return processor;
}
```

## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo obtener una instancia de procesador multimedia, consulte el tema sobre la [codificación de un recurso](media-services-dotnet-encode-with-media-encoder-standard.md) , que le mostrará cómo utilizar el servicio Media Encoder estándar para codificar un recurso.