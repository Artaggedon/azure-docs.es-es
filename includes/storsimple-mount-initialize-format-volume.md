---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 7c4392aa67fd5e995e93b2e5b0c188f07f598aa4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "95556556"
---
#### <a name="to-mount-initialize-and-format-a-volume"></a>Para montar, inicializar y formatear un volumen
1. Inicie el iniciador iSCSI de Microsoft.
2. En la ventana **Propiedades del iniciador iSCSI**, en la pestaña **Detección**, haga clic en **Detectar portal**.
3. En el cuadro de diálogo **Detectar portal de destino**, proporcione la dirección IP de la interfaz de red habilitada para iSCSI y, a continuación, haga clic en **Aceptar**. 
4. En la ventana **Propiedades del iniciador iSCSI**, en la pestaña **Destinos**, busque los **Destinos detectados**. El estado del dispositivo debe aparecer como **Inactivo**.
5. Seleccione el dispositivo de destino y, a continuación, haga clic en **Conectar**. Después de conectar el dispositivo, el estado debería cambiar a **Conectado**. Para más información sobre el uso del iniciador iSCSI de Microsoft, consulte [Instalación y configuración del iniciador iSCSI de Microsoft][1].
6. En el host de Windows, presione la tecla del logotipo de Windows + X y, a continuación, haga clic en **Ejecutar**. 
7. En el cuadro de diálogo **Ejecutar**, escriba **Diskmgmt.msc**. Haga clic en **Aceptar**, y aparecerá el cuadro de diálogo **Administración de discos**. El panel derecho mostrará los volúmenes del host.
8. En la ventana **Administración de discos** , aparecerán los volúmenes montados, tal como se muestra en la siguiente ilustración. Haga clic con el botón derecho en el volumen detectado (haga clic en el nombre del disco) y, a continuación, haga clic en **Conectado**.
   
     ![Inicializar el formateo del volumen](./media/storsimple-mount-initialize-format-volume/HCS_InitializeFormatVolume-include.png) 
9. Haga clic con el botón secundario en el volumen (haga clic en el nombre del disco) de nuevo y después haga clic en **Inicializar**.
10. Para formatear un volumen simple, siga estos pasos:
    
    1. Seleccione el volumen, haga clic con el botón secundario en él (haga clic en la zona derecha) y haga clic en **Nuevo volumen simple**.
    2. En el Asistente para nuevo volumen simple, especifique el tamaño y la letra de unidad del volumen y configure el volumen como un sistema de archivos NTFS.
    3. Especifique un tamaño de unidad de asignación de 64 KB. Este tamaño de unidad de asignación funciona bien con los algoritmos de desduplicación usados en la solución de StorSimple.
    4. Realice un formateo rápido.

![Vídeo disponible](./media/storsimple-mount-initialize-format-volume/Video_icon.png) **Vídeo disponible**

Para ver un vídeo que muestra cómo montar, inicializar y formatear un volumen de StorSimple, haga clic [aquí](https://azure.microsoft.com/documentation/videos/mount-initialize-and-format-a-storsimple-volume/).

<!--Link references-->
[1]: /previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee338480(v=ws.10)