---
title: Representación de la depuración
description: Información general sobre los efectos de representación de la depuración en el lado del servidor
author: jumeder
ms.author: jumeder
ms.date: 06/15/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: f6d79f41843069fe6cafe1fa1358ac6c1aab12e6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99591742"
---
# <a name="debug-rendering"></a>Representación de la depuración

La API de representación de la depuración proporciona una variedad de opciones globales para modificar la representación en el lado del servidor con diferentes efectos de depuración.

## <a name="available-debug-rendering-effects"></a>Efectos de representación de la depuración disponibles

|Configuración                          | Efecto                               |
|---------------------------------|:-------------------------------------|
|Contador de fotogramas                    | Representa una superposición de texto en la esquina superior izquierda del fotograma. El texto muestra el identificador de fotograma actual en el lado del servidor, identificador que aumenta continuamente a medida que continúa la representación. |
|Número de polígonos                    | Representa una superposición de texto en la esquina superior izquierda del fotograma. En el texto se muestra la cantidad de polígonos representados actualmente, el mismo valor que consultan las [consultas de rendimiento del servidor](performance-queries.md).| 
|Tramas de alambres                        | Si está habilitado, toda la geometría de objetos cargada en el servidor se representará en modo de trama de alambres. En este modo solo se rasterizarán los bordes de los polígonos. |

El código siguiente habilita estos efectos de depuración:

```cs
void EnableDebugRenderingEffects(RenderingSession session, bool highlight)
{
    DebugRenderingSettings settings = session.Connection.DebugRenderingSettings;

    // Enable frame counter text overlay on the server side rendering
    settings.RenderFrameCount = true;

    // Enable polygon count text overlay on the server side rendering
    settings.RenderPolygonCount = true;

    // Enable wireframe rendering of object geometry on the server
    settings.RenderWireframe = true;
}
```

```cpp
void EnableDebugRenderingEffects(ApiHandle<RenderingSession> session, bool highlight)
{
    ApiHandle<DebugRenderingSettings> settings = session->Connection()->GetDebugRenderingSettings();

    // Enable frame counter text overlay on the server side rendering
    settings->SetRenderFrameCount(true);

    // Enable polygon count text overlay on the server side rendering
    settings->SetRenderPolygonCount(true);

    // Enable wireframe rendering of object geometry on the server
    settings->SetRenderWireframe(true);
}
```

![Representación de la depuración](./media/debug-rendering.png)

> [!NOTE]
> Todos los efectos de representación de la depuración son valores globales que afectan a todo el fotograma.

## <a name="use-cases"></a>Casos de uso

La API de representación de la depuración está pensada para tareas sencillas de depuración, como comprobar que la conexión del servicio está realmente en funcionamiento y sin problemas. Las opciones de representación de texto afectan directamente a los fotogramas de vídeo transmitidos hacia un nivel inferior. Al habilitarlas, se comprueba si se reciben nuevos fotogramas y se descodifica correctamente el vídeo.

Sin embargo, los efectos proporcionados no proporcionan ninguna introspección detallada sobre el estado del servicio. En este caso de uso, se recomiendan las [consultas de rendimiento en el lado del servidor](performance-queries.md).

## <a name="performance-considerations"></a>Consideraciones de rendimiento

* La habilitación de las superposiciones de texto no ocasiona apenas sobrecarga del rendimiento.
* La habilitación del modo Wireframe conlleva una sobrecarga de rendimiento considerable, aunque puede variar en función de la escena. En el caso de escenas complejas, este modo puede hacer que la velocidad de fotogramas caiga por debajo del objetivo de 60 Hz.

## <a name="api-documentation"></a>Documentación de la API

* [RenderingConnection::DebugRenderingSettings() de C++](/cpp/api/remote-rendering/renderingconnection#debugrenderingsettings)

## <a name="next-steps"></a>Pasos siguientes

* [Modos de representación](../../concepts/rendering-modes.md)
* [Consultas de rendimiento en el lado servidor](performance-queries.md)