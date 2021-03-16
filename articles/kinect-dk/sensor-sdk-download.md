---
title: Descarga de Sensor SDK de Azure Kinect
description: Obtenga información sobre cómo descargar e instalar Sensor SDK de Azure Kinect en Windows y Linux.
author: Brent-A
ms.author: brenta
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: azure, kinect,sdk, download update, latest, available, install
ms.openlocfilehash: 591fcba4c887e298cf667c5d95c19184bc213ffe
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102179636"
---
# <a name="azure-kinect-sensor-sdk-download"></a>Descarga de Sensor SDK de Azure Kinect

Esta página incluye vínculos de descarga para cada versión de Sensor SDK de Azure Kinect. El instalador proporciona todos los archivos necesarios para desarrollar para Azure Kinect.

## <a name="azure-kinect-sensor-sdk-contents"></a>Contenido de Sensor SDK de Azure Kinect

- Encabezados y bibliotecas para compilar una aplicación mediante Azure Kinect DK.
- Archivos DLL redistribuibles necesarios para las aplicaciones que usan Azure Kinect DK.
- [Azure Kinect Viewer](azure-kinect-viewer.md)
- [Azure Kinect Recorder](azure-kinect-recorder.md)
- [Azure Kinect Firmware Tool](azure-kinect-firmware-tool.md)

## <a name="windows-installation-instructions"></a>Instrucciones de instalación para Windows

Puede encontrar los detalles para la instalación de las versiones más recientes y anteriores del SDK y el firmware del sensor de Azure Kinect [aquí](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md).

Puede encontrar el código fuente [aquí](https://github.com/microsoft/Azure-Kinect-Sensor-SDK).

> [!NOTE]
> Cuando instale el SDK, recuerde la ruta de acceso de instalación. Por ejemplo, "C:\Archivos de programa\Azure Kinect SDK 1.2". Encontrará las herramientas a las que se hace referencia en los artículos en esta ruta de acceso.

## <a name="linux-installation-instructions"></a>Instrucciones de instalación para Linux

Actualmente, la única distribución admitida es Ubuntu 18.04. Para solicitar soporte técnico para otras distribuciones, consulte [esta página](https://aka.ms/azurekinectfeedback).

En primer lugar, deberá configurar el [repositorio de paquetes de Microsoft](https://packages.microsoft.com/), siguiendo las instrucciones de [este artículo](/windows-server/administration/linux-package-repository-for-microsoft-software).

Ahora puede instalar los paquetes necesarios. El paquete `k4a-tools` incluye [Azure Kinect Viewer](azure-kinect-viewer.md), [Azure Kinect Recorder](record-sensor-streams-file.md) y [Azure Kinect Firmware Tool](azure-kinect-firmware-tool.md). Para instalar el paquete, ejecute:

`sudo apt install k4a-tools`
 
Este comando instala los paquetes de dependencia necesarios para que las herramientas funcionen correctamente, incluida la versión más reciente de `libk4a<major>.<minor>`. Tendrá que agregar reglas udev para acceder a Azure Kinect DK sin ser el usuario raíz. Para obtener instrucciones, consulte la [configuración de dispositivos Linux](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md#linux-device-setup). Como alternativa, puede iniciar aplicaciones que usen el dispositivo como raíz.

El paquete `libk4a<major>.<minor>-dev` contiene los encabezados y los archivos CMake para crear sus aplicaciones o archivos ejecutables en `libk4a`.

El paquete `libk4a<major>.<minor>` contiene los objetos compartidos necesarios para ejecutar las aplicaciones o archivos ejecutables que dependen de `libk4a`.

Los tutoriales básicos requieren el paquete `libk4a<major>.<minor>-dev`. Para instalar el paquete, ejecute:

`sudo apt install libk4a<major>.<minor>-dev` 

Si el comando se ejecuta correctamente, el SDK está listo para su uso.

Asegúrese de instalar la versión correspondiente de `libk4a<major>.<minor>` con `libk4a<major>.<minor>-dev`. Por ejemplo, si instala el paquete `libk4a4.1-dev`, instale el paquete `libk4a4.1` correspondiente que contiene la versión correspondiente de los archivos objeto compartidos. Para obtener la versión más reciente de `libk4a`, consulte los vínculos de la sección siguiente.

## <a name="change-log-and-older-versions"></a>Registro de cambios y versiones anteriores

Puede encontrar el registro de cambios de Sensor SDK de Azure Kinect [aquí](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/CHANGELOG.md).

Si necesita una versión anterior de Sensor SDK de Azure Kinect, busque [aquí](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md).

## <a name="next-steps"></a>Pasos siguientes

[Configuración de Azure Kinect DK](set-up-azure-kinect-dk.md)
