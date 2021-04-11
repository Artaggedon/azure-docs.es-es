---
title: Introducción a Azure Spatial Anchors
description: Obtenga información sobre cómo Azure Spatial Anchors le ayuda a desarrollar experiencias de realidad mixta multiplataforma.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: 0b4c3b281f2eac6263ef578dee5419bd9f2eb8a7
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2021
ms.locfileid: "106058568"
---
# <a name="azure-spatial-anchors-overview"></a>Introducción a Azure Spatial Anchors

Le damos la bienvenida a Azure Spatial Anchors. Azure Spatial Anchors ofrece a los desarrolladores funcionalidades esenciales para compilar aplicaciones de realidad mixta con identificación del espacio. Estas aplicaciones pueden admitir Microsoft HoloLens, dispositivos iOS compatibles con ARKit y dispositivos Android compatibles con ARCore. Azure Spatial Anchors permite a los desarrolladores trabajar con plataformas de realidad mixta para percibir el espacio, designar puntos precisos de interés y volver a recuperar esos puntos de interés desde los dispositivos compatibles.
Estos puntos de interés precisos se conocen como delimitadores espaciales.

![Multiplataforma](./media/cross-platform.png)

## <a name="examples"></a>Ejemplos

Algunos ejemplos de uso de los delimitadores espaciales son:

- [Experiencias multiusuario](tutorials/tutorial-share-anchors-across-devices.md). Azure Spatial Anchors facilita a las personas que están en el mismo lugar participar en aplicaciones de realidad mixta multiusuario. Por ejemplo, dos personas pueden iniciar un juego de ajedrez de realidad mixta colocando un tablero de ajedrez virtual en una mesa. Después, pueden apuntar con su dispositivo a la mesa para ver e interactúan juntos con el tablero de ajedrez virtual.

- [Búsqueda de caminos](concepts/anchor-relationships-way-finding.md). Los desarrolladores también pueden conectar delimitadores espaciales entre sí para crear relaciones entre ellos. Por ejemplo, una aplicación puede incluir una experiencia que tenga dos o más puntos de interés con los que un usuario debe interactuar para completar una tarea. Los puntos de interés se pueden crear en un modo conectado. Más adelante, cuando el usuario esté completando la tarea en varios pasos, la aplicación puede pedir a los delimitadores que están cerca del actual que dirijan al usuario hacia el siguiente paso de la tarea.

- [Conservación del contenido virtual en el mundo real](how-tos/create-locate-anchors-unity.md#create-a-cloud-spatial-anchor). Una aplicación puede permitir que un usuario coloque un calendario virtual en la pared de una sala de conferencias, que los asistentes pueden ver con una aplicación de teléfono o un dispositivo HoloLens. En un entorno industrial, un usuario podría recibir información contextual sobre una máquina señalándola con la cámara de un dispositivo compatible.

Azure Spatial Anchors está compuesto por un servicio administrado y el SDK de cliente para las plataformas de dispositivos compatibles. En las secciones siguientes se proporciona información acerca de cómo empezar a crear aplicaciones con Azure Spatial Anchors.

## <a name="next-steps"></a>Pasos siguientes

Cree su primera aplicación con Azure Spatial Anchors.

> [!div class="nextstepaction"]
> [Unity (HoloLens)](quickstarts/get-started-unity-hololens.md)

> [!div class="nextstepaction"]
> [Unity (iOS)](quickstarts/get-started-unity-ios.md)

> [!div class="nextstepaction"]
> [Unity (Android)](quickstarts/get-started-unity-android.md)

> [!div class="nextstepaction"]
> [iOS](quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](quickstarts/get-started-hololens.md)

> [!div class="nextstepaction"]
> [Xamarin (Android)](quickstarts/get-started-xamarin-android.md)

> [!div class="nextstepaction"]
> [Xamarin (iOS)](quickstarts/get-started-xamarin-ios.md)
