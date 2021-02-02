---
title: 'Suscripción a Video Indexer y carga del primer vídeo: Azure'
titleSuffix: Azure Media Services
description: Obtenga información acerca de cómo suscribirse y cargar el primer vídeo mediante el portal de Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: quickstart
ms.date: 01/25/2021
ms.author: juliako
ms.openlocfilehash: 5b38c731db141052e6700472020cd60b6a4d13a5
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98797797"
---
# <a name="quickstart-how-to-sign-up-and-upload-your-first-video"></a>Inicio rápido: Suscripción y carga del primer vídeo

En este inicio rápido introductorio se muestra cómo iniciar sesión en el sitio web de Video Indexer y cómo cargar su primer vídeo.

Al crear una cuenta de Video Indexer, puede elegir una cuenta de evaluación gratuita (donde obtendrá un número determinado de minutos gratuitos de indexación) o una opción de pago (donde no está limitado por la cuota). Con la versión de evaluación gratuita, Video Indexer proporciona hasta 600 horas de indexación gratuita a los usuarios de sitios web y hasta 2400 horas de indexación gratuita a los usuarios de API. Con la opción de pago, se crea una cuenta de Video Indexer que está [conectada a su suscripción de Azure y a una cuenta de Azure Media Services](connect-to-azure.md). Se paga por minutos de indexación. Para más información, consulte [Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/). 

## <a name="sign-up-for-video-indexer"></a>Suscripción a Video Indexer

Para empezar a desarrollar con Video Indexer, vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) y suscríbase.

Una vez que empiece a usar Video Indexer, todos los datos almacenados y el contenido cargado se cifran en reposo con una clave administrada de Microsoft.

> [!NOTE]
> Consulte [Cambios planeados en la autenticación del sitio web de Video Indexer](release-notes.md#planned-video-indexer-website-authenticatication-changes).

## <a name="upload-a-video-using-the-video-indexer-website"></a>Carga de un vídeo mediante el sitio web de Video Indexer

### <a name="supported-file-formats-for-video-indexer"></a>Formatos de archivo admitidos para Video Indexer

Consulte la sección [Formatos de archivo/contenedor de entrada](../latest/media-encoder-standard-formats.md#input-containerfile-formats) para una lista de los formatos de archivo que puede usar con Video Indexer.

### <a name="upload-a-video"></a>Carga de un vídeo

1. Inicie sesión en el sitio web de [Video Indexer](https://www.videoindexer.ai/).
1. Para cargar un vídeo, presione el botón **Upload** (Cargar) o el vínculo.

    > [!NOTE]
    > El nombre del vídeo no debe superar los 80 caracteres.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/video-indexer-get-started/video-indexer-upload.png" alt-text="Cargar":::
1. Una vez cargado el vídeo, Video Indexer inicia la indexación y el análisis del vídeo. Verá el progreso. 

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/video-indexer-get-started/progress.png" alt-text="Progreso de la carga":::
1. Cuando Video Indexer haya terminado el análisis, recibirá un correo electrónico con un vínculo a su vídeo y una breve descripción de lo que se ha encontrado en él. Por ejemplo: personas, palabras habladas y escritas, temas y entidades con nombre.
1. Más adelante, puede buscar el vídeo en la lista de bibliotecas y realizar diferentes operaciones. Por ejemplo: buscar, volver a indexar o editar.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/video-indexer-get-started/uploaded.png" alt-text="Realización de la carga":::

## <a name="supported-browsers"></a>Exploradores compatibles

Para más información, consulte los [exploradores compatibles](video-indexer-overview.md#supported-browsers).

## <a name="see-also"></a>Consulte también

Consulte [Carga e indexación de los vídeos](upload-index-videos.md) para más detalles.

Después de cargar e indexar un vídeo, puede empezar a usar el [sitio web de Video Indexer](video-indexer-view-edit.md) o el [Portal para desarrolladores de Video Indexer](video-indexer-use-apis.md) para ver la información detallada del vídeo. 

[Comience a usar las API](video-indexer-use-apis.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener una introducción detallada, visite nuestro [laboratorio introductorio](https://github.com/Azure-Samples/media-services-video-indexer/blob/master/IntroToVideoIndexer.md). 

Al final del taller, tendrá un buen conocimiento del tipo de información que se puede extraer del contenido de vídeo y audio, estará mejor preparado para identificar las oportunidades relacionadas con la inteligencia de contenidos, el tono de IA del vídeo en Azure y la demostración de varios escenarios en Video Indexer.

