---
title: Escalado del procesamiento de medios mediante Azure Portal | Microsoft Docs
description: Este tutorial lo guiará a través de los pasos de escalado de procesamiento de medios con Azure Portal.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 49c3899b912a88605e9269cdb1c34c7e18ed5247
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103009732"
---
# <a name="change-the-reserved-unit-type"></a>Cambio del tipo de unidad reservada

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/rnrneverdies/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Información general

Una cuenta de Media Services está asociada con un tipo de unidad reservada que determina la rapidez con la que se procesan las tareas de procesamiento multimedia. Puede elegir uno de los siguientes tipos de unidad reservada: **S1**, **S2** o **S3**. Por ejemplo, el mismo trabajo de codificación se ejecuta más rápido cuando se usa el tipo de unidad reservada **S2** en comparación con el tipo **S1**.

Además de especificar el tipo de unidad reservada, puede especificar el aprovisionamiento de su cuenta con **unidades reservadas**. El número de unidades reservadas aprovisionadas determina el número de tareas de medios que se pueden procesar de forma simultánea en una cuenta determinada.

>[!NOTE]
>Las unidades reservadas sirven para establecer paralelismos en todo el procesamiento multimedia, incluida la indexación de trabajos mediante Azure Media Indexer. Sin embargo, a diferencia de la codificación, la indexación de los trabajos no se procesará más rápido con unidades reservadas de mayor rapidez.

> [!IMPORTANT]
> Para más información sobre cómo escalar el procesamiento de medios, consulte este tema de [introducción](media-services-scale-media-processing-overview.md) .
> 
> 

## <a name="scale-media-processing"></a>Escalar el procesamiento multimedia
Para cambiar el tipo de unidad reservada y el número de unidades reservadas, haga lo siguiente:

1. En [Azure Portal](https://portal.azure.com/), seleccione la cuenta de Azure Media Services.
2. En la ventana **Configuración**, seleccione **Unidades reservadas de medios**.
   
    Para cambiar el número de unidades reservadas para el tipo de unidad reservada seleccionada, use el control deslizante **Unidades reservadas de medios** que se encuentra en la parte superior de la pantalla.
   
    Para cambiar el valor de **RESERVED UNIT TYPE**, haga clic en la barra **Velocidad de las unidades de procesamiento reservadas**. Luego, seleccione el plan de tarifa que necesita: S1, S2 o S3.
   
3. Presione el botón GUARDAR para guardar los cambios.
   
    Las nuevas unidades reservadas se asignan en cuanto presiona GUARDAR.

## <a name="next-steps"></a>Pasos siguientes
Consulte las rutas de aprendizaje de Media Services.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
