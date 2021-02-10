---
title: Espacio provisional
description: Describe la configuración del espacio provisional y casos de uso
author: christophermanthei
ms.author: chmant
ms.date: 03/07/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: 4a739907ce0a3b0b6dfcb8791b51d0ea5e7e76e7
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99594017"
---
# <a name="stage-space"></a>Espacio provisional

Cuando se ejecuta ARR en un dispositivo que proporciona datos de seguimiento de la cabeza, como HoloLens 2, la posición de la cabeza se envía tanto a la aplicación de usuario como al servidor. El espacio en el que se define la transformación de la cabeza se llama *espacio provisional*.

Para alinear el contenido local y remoto, se supone que el espacio provisional y el espacio mundial son idénticos en el cliente y el servidor. Si el usuario decide agregar una transformación adicional en la parte superior de la cámara, debe enviarse al servidor también para alinear el contenido local y remoto correctamente.

Los motivos típicos para mover el espacio provisional son las [herramientas de bloqueo de mundo](https://microsoft.github.io/MixedReality-WorldLockingTools-Unity/README.html) u otras técnicas de delimitación del mundo real, así como mover un personaje virtual en la realidad virtual.

## <a name="stage-space-settings"></a>Configuración del espacio provisional

> [!CAUTION]
> EXPERIMENTAL: Esta característica es experimental y cambiará con el tiempo. Por lo tanto, la actualización a versiones más recientes del SDK de cliente puede requerir trabajo adicional para actualizar el código. La implementación actual interrumpirá la alineación del contenido local o remoto durante un breve período de tiempo al cambiar el origen del espacio provisional.
Por lo tanto, actualmente solo está pensada para usarse con fines de bloqueo de mundo, como los delimitadores que solo muestran cambios muy pequeños a lo largo del tiempo.

Para informar al servidor de que se aplica una transformación adicional al espacio provisional, se debe enviar el origen definido por una posición y una rotación en el espacio mundial. Se puede tener acceso a esta configuración a través de la *configuración del espacio provisional*.

> [!IMPORTANT]
> En la [simulación de escritorio](../../concepts/graphics-bindings.md), la ubicación de espacio mundial de la cámara se proporciona por la aplicación de usuario. En este caso, se debe omitir el establecimiento del origen del espacio provisional, ya que ya se ha multiplicado en la transformación de la cámara.

```cs
void ChangeStageSpace(RenderingSession session)
{
    StageSpaceSettings settings = session.Connection.StageSpaceSettings;

    // Set position and rotation to the world-space transform of the stage space.
    settings.Position = new Double3(0, 0, 0);
    settings.Rotation = new Quaternion(0, 0, 0, 1);
}
```

```cpp
void ChangeStageSpace(ApiHandle<RenderingSession> session)
{
    ApiHandle<StageSpaceSettings> settings = session->Connection()->GetStageSpaceSettings();

    // Set position and rotation to the world-space transform of the stage space.
    settings->SetPosition({0, 0, 0});
    settings->SetRotation({0, 0, 0, 1});
}
```

## <a name="unity-stage-space-script"></a>Script de espacio provisional de Unity

La integración de Unity proporciona un script denominado **StageSpace** que se puede agregar al elemento GameObject principal de la cámara. Una vez presente, este script se encargará de establecer el origen del espacio provisional.

## <a name="next-steps"></a>Pasos siguientes

* [Enlace de gráficos](../../concepts/graphics-bindings.md)
* [Reproyección de fase tardía](late-stage-reprojection.md)
