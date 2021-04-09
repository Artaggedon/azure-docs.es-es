---
title: Configuración de la lectura en voz alta
titleSuffix: Azure Cognitive Services
description: En este artículo se muestra cómo configurar las distintas opciones de la lectura en voz alta.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 8b45fe07b4bd42059199197bc5b99f20f6b2a8cf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "102608723"
---
# <a name="how-to-configure-read-aloud"></a>Configuración de la lectura en voz alta

En este artículo se muestra cómo configurar las distintas opciones de la lectura en voz alta en el Lector inmersivo.

## <a name="automatically-start-read-aloud"></a>Inicio automático de la lectura en voz alta

El parámetro `options` contiene todas las marcas que se pueden usar para configurar la lectura en voz alta. Establezca `autoplay` en `true` para habilitar el inicio automático de la lectura en voz alta después de iniciar el Lector inmersivo.

```typescript
const options = {
    readAloudOptions: {
        autoplay: true
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

> [!NOTE]
> Debido a las limitaciones del explorador, la reproducción automática no se admite en Safari.

## <a name="configure-the-voice"></a>Configuración de la voz

Establezca `voice` en `male` o `female`. No todos los idiomas admiten ambas voces. Para obtener más información, consulte la página [Compatibilidad con idiomas](./language-support.md).

```typescript
const options = {
    readAloudOptions: {
        voice: 'female'
    }
};
```

## <a name="configure-playback-speed"></a>Configuración de la velocidad de reproducción

Establezca `speed` en un número entre `0.5` (50 %) y `2.5` (250 %), inclusive. Los valores fuera de este rango se fijarán en 0,5 o 2,5.

```typescript
const options = {
    readAloudOptions: {
        speed: 1.5
    }
};
```

## <a name="next-steps"></a>Pasos siguientes

* Lea la [Referencia del SDK del Lector inmersivo](./reference.md)