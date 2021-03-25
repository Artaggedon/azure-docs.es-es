---
title: Trabajos de copia de seguridad de StorSimple Snapshot Manager | Microsoft Docs
description: Describe cómo usar el complemento MMC de Administrador de instantáneas StorSimple para ver y administrar trabajos de copia de seguridad programados, actualmente en ejecución y completados.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 3c26a84e32a17cba83b5ca895f146e561072fa62
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "95998199"
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Uso de Administrador de instantáneas StorSimple para ver y administrar trabajos de copia de seguridad

## <a name="overview"></a>Información general
En el panel **Ámbito**, El nodo **Trabajos** muestra las tareas de copia de seguridad **Programada**, **Últimas 24 horas** y **En ejecución** que se han iniciado de forma interactiva o mediante una directiva configurada. 

Este tutorial explica cómo se puede usar el nodo **Trabajos** para mostrar información sobre los trabajos de copia de seguridad programados, recientes y en ejecución. (La lista de trabajos y la información correspondiente aparecen en el panel **Resultados**). Además, puede es posible hacer clic en un trabajo de la lista y ver un menú contextual que enumera las acciones disponibles.

## <a name="view-scheduled-jobs"></a>Visualización de los trabajos programados
Use el procedimiento siguiente para ver los trabajos de copia de seguridad programados.

#### <a name="to-view-scheduled-jobs"></a>Para ver los trabajos programados
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple. 
2. En el panel **Ámbito**, expanda el nodo **Trabajos** y luego haga clic en **Programado**. La siguiente información aparece en el panel **Resultados**:
   
   * **Nombre**: el nombre de la instantánea programada
   * **Siguiente ejecución**: la fecha y hora de la siguiente instantánea programada
   * **Última ejecución**: la fecha y hora de la instantánea programada más reciente
     
     > [!NOTE]
     > Solo para las instantáneas de un solo uso, **Siguiente ejecución** y **Última ejecución** serán iguales.
     
     ![Trabajos de copia de seguridad programados](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. Para realizar acciones adicionales en un trabajo específico, haga clic con el botón derecho en el nombre del trabajo en el panel **Resultados** y realice su selección entre las opciones de menú.

## <a name="view-recent-jobs"></a>Visualización de trabajos recientes
Utilice el procedimiento siguiente para ver la copia de seguridad y restaurar trabajos que se completaron en las últimas 24 horas.

#### <a name="to-view-recent-jobs"></a>Para ver los trabajos recientes
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple.
2. En el panel **Ámbito**, expanda el nodo **Trabajos** y haga clic en **Últimas 24 horas**. El panel **Resultados** muestra los trabajos de copia de seguridad de las últimas 24 horas (hasta un máximo de 64 trabajos). La siguiente información aparece en el panel **Resultados**, según las opciones de **Ver** que especifique:
   
   * **Nombre**: el nombre de la instantánea programada.
   * **Iniciado**: la fecha y hora en que comenzó la instantánea.
   * **Detenido**: la fecha y hora en las que se finalizó o canceló la instantánea.
   * **Transcurrido**: la cantidad de tiempo entre las horas **Iniciado** y **Detenido**.
   * **Estado**: el estado del trabajo completado recientemente. **Correcto** indica que la copia de seguridad se creó correctamente. **Error** indica que el trabajo no se ejecutó correctamente.
   * **Información**: el motivo del error.
   * **Bytes procesados (MB)**: la cantidad de datos del grupo de volúmenes que se procesó (en MB) 
     
     ![Trabajos ejecutados en las últimas 24 horas](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. Para realizar acciones adicionales en un trabajo específico, haga clic con el botón derecho en el nombre del trabajo en el panel **Resultados** y realice su selección entre las opciones de menú.
   
    ![Eliminación de un trabajo](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Visualización de los trabajos en ejecución
Utilice el procedimiento siguiente para ver los trabajos que se están ejecutando en el momento presente.

#### <a name="to-view-currently-running-jobs"></a>Para ver los trabajos en ejecución
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple.
2. En el panel **Ámbito**, expanda el nodo **Trabajos** y luego haga clic en **En ejecución**. Según las opciones de **Ver** que especifique, la siguiente información aparece en el panel **Resultados**:
   
   * **Nombre**: el nombre de la instantánea programada.
   * **Iniciado**: la fecha y hora en que comenzó la instantánea.
   * **Punto de control**: la acción actual de la copia de seguridad.
   * **Estado**: el porcentaje de finalización.
   * **Transcurrido**: la cantidad de tiempo transcurrido desde que comenzó la copia de seguridad 
   * **Rendimiento medio (MB)** : la relación del total de bytes de datos procesados con el tiempo total necesario para realizar el procesamiento (MB).
   * **Bytes procesados (MB)** : total de bytes de los datos procesados (en MB).
   * **Bytes escritos (MB)** : total de bytes de los datos escritos (en MB). Incluye los datos, así como los metadatos y, por tanto, es normalmente mayor que los bytes procesados.
     
     ![Trabajos en ejecución](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. Para realizar acciones adicionales en un trabajo específico, haga clic con el botón derecho en el nombre del trabajo en el panel **Resultados** y realice su selección entre las opciones de menú.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre el [uso de Snapshot Manager de StorSimple para administrar la solución de StorSimple](storsimple-snapshot-manager-admin.md).
* Obtenga más información sobre el [uso de Snapshot Manager de StorSimple para administrar el catálogo de copias de seguridad](storsimple-snapshot-manager-manage-backup-catalog.md).

