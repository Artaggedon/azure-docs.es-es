---
title: Actualización del firmware de Azure Kinect DK
description: Obtenga información sobre cómo actualizar el firmware del dispositivo Azure Kinect DK mediante la herramienta de firmware de Azure Kinect.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: kinect, firmware, actualización, recuperación
ms.openlocfilehash: 71557798a584635356cc47fd7c4a0309df4d018a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "94356233"
---
# <a name="update-azure-kinect-dk-firmware"></a>Actualización del firmware de Azure Kinect DK

En este documento se proporcionan instrucciones sobre cómo actualizar el firmware del dispositivo en Azure Kinect DK.

Azure Kinect DK no actualiza el firmware automáticamente. Puede usar la [Herramienta de firmware de Azure Kinect](azure-kinect-firmware-tool.md) para actualizar el firmware manualmente a la versión más reciente disponible.

## <a name="prepare-for-firmware-update"></a>Preparación para la actualización del firmware

1. [Descargue el SDK](sensor-sdk-download.md).
2. Instale el SDK.
3. En la ubicación de instalación del SDK en (ubicación de instalación del SDK)\tools\ debe encontrar:

    - AzureKinectFirmwareTool.exe
    - Un archivo de firmware .bin en la carpeta de firmware, como *AzureKinectDK_Fw_1.5.926614.bin*.

4. Conecte el dispositivo al equipo host y también enciéndalo.

> [!IMPORTANT]
> Mantenga conectado el USB y la fuente de alimentación durante la actualización del firmware. Si quita cualquiera de estas conexiones durante la actualización, es posible que dañe el firmware.

## <a name="update-device-firmware"></a>Actualización del firmware del dispositivo

1. Abra un símbolo del sistema en la carpeta (ubicación de instalación de SDK)\tools\.
2. Actualización del firmware con la herramienta de firmware de Azure Kinect

    `AzureKinectFirmwareTool.exe -u <device_firmware_file.bin>`

    Ejemplo:

    `AzureKinectFirmwareTool.exe -u firmware\AzureKinectDK_Fw_1.5.926614.bin`

3. Espere hasta que finalice la actualización del firmware. Puede tardar unos minutos, en función del tamaño de la imagen.

### <a name="verify-device-firmware-version"></a>Verificación de la versión de firmware del dispositivo

1. Verifique que el firmware esté actualizado.

    `AzureKinectFirmwareTool.exe -q`

2. Vea el ejemplo siguiente.

    ```console
       >AzureKinectFirmwareTool.exe -q
       == Azure Kinect DK Firmware Tool ==
        Device Serial Number: 000805192412
        Current Firmware Versions:
        RGB camera firmware:      1.6.102
        Depth camera firmware:    1.6.75
        Depth config file:        6109.7
        Audio firmware:           1.6.14
        Build Config:             Production
        Certificate Type:         Microsoft
    ```

3. Si ve la salida anterior, el firmware está actualizado.

4. Después de la actualización del firmware, puede ejecutar [Azure Kinect Viewer](azure-kinect-viewer.md) para verificar que todos los sensores funcionen según lo previsto.

## <a name="troubleshooting"></a>Solución de problemas

Las actualizaciones de firmware pueden no completarse por varios motivos. Cuando se produzca un error en una actualización de firmware, pruebe los siguientes pasos de mitigación:

1. Intente ejecutar el comando de actualización de firmware una segunda vez.

2. Consulte la versión del firmware para confirmar que el dispositivo siga conectado.        AzureKinectFirmareTool.exe

3. Si nada más funciona, siga los pasos de [recuperación](https://support.microsoft.com/help/4494277/reset-azure-kinect-dk) para revertir el firmware a la versión de fábrica e inténtelo de nuevo.

Por cualquier otro problema, consulte las [páginas de soporte técnico de Microsoft](./index.yml).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Herramienta de firmware de Azure Kinect](azure-kinect-firmware-tool.md)