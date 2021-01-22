---
title: Implementación de un ejemplo de Unity en HoloLens
description: Artículo de inicio rápido que muestra cómo obtener el ejemplo de Unity en el dispositivo HoloLens
author: jakrams
ms.author: jakras
ms.date: 02/14/2020
ms.topic: quickstart
ms.openlocfilehash: a5a44b9d27901fcd00f8813096de6bbae5f2310d
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98179952"
---
# <a name="quickstart-deploy-unity-sample-to-hololens"></a>Inicio rápido: Implementación de un ejemplo de Unity en HoloLens

En este artículo de inicio rápido se explica cómo implementar y ejecutar la aplicación de ejemplo del artículo de inicio rápido para Unity en un dispositivo HoloLens2.

En este artículo de inicio rápido aprenderá lo siguiente:

> [!div class="checklist"]
>
>* Creación de la aplicación de ejemplo del artículo de inicio rápido para un dispositivo HoloLens
>* Implementación del ejemplo en el dispositivo
>* Ejecución del ejemplo en el dispositivo

## <a name="prerequisites"></a>Requisitos previos

En este inicio rápido, se va a implementar el proyecto de ejemplo de [Inicio rápido: Representación de un modelo con Unity](render-model.md).

Asegúrese de que las credenciales se guardan correctamente con la escena y que puede conectarse a una sesión desde el editor de Unity.

## <a name="build-the-sample-project"></a>Creación del proyecto de ejemplo

1. Abra *File -> Build Settings* (Archivo -> Configuración de la compilación).
1. Cambie *Platform* (Plataforma) a **Universal Windows Platform** (Plataforma universal de Windows).
1. Establezca *Target Device* (Dispositivo de destino) en **HoloLens**.
1. Establezca *Architecture* (Arquitectura) en **ARM64**.
1. Establezca *Build Type* (Tipo de compilación) en **D3D Project** (Proyecto de D3D).\
    ![Configuración de compilación](./media/unity-build-settings.png)
1. Seleccione **Switch to Platform** (Cambiar a Plataforma).
1. Al presionar **Build** (Compilar) (o "Build And Run" [Compilar y ejecutar]), se le pedirá que seleccione una carpeta donde guardar la solución.
1. Abra el archivo **Quickstart.sln** generado con Visual Studio.
1. Cambie la configuración a **Release** (Versión) y **ARM64**.
1. Cambie el modo del depurador a **Remote Machine** (Máquina remota).\
    ![Configuración de la solución](media/unity-deploy-config.png)
1. Compilar la solución
1. Para el proyecto "Quickstart" (Inicio rápido), vaya a *Properties > Debugging* (Propiedades > Depuración).
    1. Asegúrese de que la *versión* de configuración está activa.
    1. Establezca *Debugger to Launch* (Depurador para iniciar) en **Remote Machine** (Máquina remota).
    1. Cambie el *nombre de la máquina* por la **IP de HoloLens**

## <a name="launch-the-sample-project"></a>Inicio del proyecto de ejemplo

1. Conecte el dispositivo HoloLens con un cable USB al equipo.
1. Inicie el depurador en Visual Studio (F5). La aplicación se implementará automáticamente en el dispositivo.

Se iniciará la aplicación de ejemplo y comenzará una nueva sesión. Al cabo de un rato, la sesión está lista y el modelo representado de forma remota le aparecerá delante.
Si quiere volver a iniciar el ejemplo más tarde, también puede encontrarlo ahora en el menú Inicio del dispositivo HoloLens.

## <a name="next-steps"></a>Pasos siguientes

En el siguiente inicio rápido, echaremos un vistazo a la conversión de un modelo personalizado.

> [!div class="nextstepaction"]
> [Inicio rápido: Conversión de un modelo para su representación](convert-model.md)
