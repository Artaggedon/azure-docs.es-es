---
title: Creación de claves de contenido con .NET
description: En este artículo se muestra cómo crear claves de contenido mediante .NET. Estas claves proporcionan un acceso seguro a los recursos.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 05bf928490e94f43b755e1958213899e9e1e98e9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103014186"
---
# <a name="create-contentkeys-with-net"></a>Creación de claves de contenido con .NET

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]
 
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Media Services permite crear nuevos recursos y entregar recursos cifrados. Una **ContentKey** proporciona acceso seguro a los **recursos**. 

Al crear un nuevo recurso (por ejemplo, antes de [cargar archivos](media-services-dotnet-upload-files.md)), puede especificar las siguientes opciones de cifrado: **StorageEncrypted**, **CommonEncryptionProtected** o **EnvelopeEncryptionProtected**. 

Al entregar recursos a los clientes, puede [configurar que los recursos se cifren de forma dinámica](media-services-dotnet-configure-asset-delivery-policy.md) con uno de los dos cifrados siguientes: **DynamicEnvelopeEncryption** o **DynamicCommonEncryption**.

Los recursos cifrados tienen que estar asociados con **ContentKey**. En este artículo se describe cómo crear una clave de contenido.

> [!NOTE]
> Al crear un nuevo recurso **StorageEncrypted** con el SDK de Media Services para .NET, se crea automáticamente el valor de **ContentKey** y se vincula al recurso.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
Uno de los valores que debe configurar al crear una clave de contenido es el tipo de clave de contenido. Elija uno de los valores siguientes. 

```csharp
    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }
```

## <a name="create-envelope-type-contentkey"></a><a id="envelope_contentkey"></a>Crear ContentKey de tipo de sobre
El siguiente fragmento de código crea una clave de contenido del tipo de cifrado de sobre. A continuación, asocia la clave con el recurso especificado.

```csharp
    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

call

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);
```


## <a name="create-common-type-contentkey"></a><a id="common_contentkey"></a>Crear ContentKey de tipo común
El fragmento de código siguiente crea una clave de contenido del tipo de cifrado común. A continuación, asocia la clave con el recurso especificado.

```csharp
    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
call

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 
```

## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

