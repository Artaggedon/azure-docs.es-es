---
title: Seguimiento de archivos actualizados con una tarea de monitor de Azure Automation
description: En este artículo se describe cómo crear una tarea de monitor en la cuenta de Azure Automation para inspeccionar los nuevos archivos que se creen en una carpeta.
services: automation
ms.subservice: process-automation
ms.topic: conceptual
ms.date: 12/17/2020
ms.openlocfilehash: ca4c4013e0044811a5bac8786996761b8a5c45f1
ms.sourcegitcommit: e0ec3c06206ebd79195d12009fd21349de4a995d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97682757"
---
# <a name="track-updated-files-with-a-watcher-task"></a>Seguimiento de archivos actualizados con una tarea de monitor

Azure Automation usa una tarea de monitor para buscar eventos y desencadenar acciones con runbooks de PowerShell. La tarea de monitor contiene dos partes, el monitor y la acción. Un runbook de monitor se ejecuta en un intervalo definido en la tarea de monitor y envía los datos a un runbook de acción.

> [!NOTE]
> No se admiten tareas de monitor en Azure China 21Vianet.

> [!IMPORTANT]
> A partir de mayo de 2020, el uso de Azure Logic Apps es la forma recomendada y admitida de supervisar eventos, programar tareas periódicas y desencadenar acciones. Consulte [Programación y ejecución de tareas, procesos y flujos de trabajo automatizados y periódicos con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

En este artículo se le guía por la creación de una tarea de monitor para supervisar cuándo se agrega un archivo nuevo a un directorio. Aprenderá a:

* Importar un runbook de monitor
* Creación de una variable de Automation
* Crear un runbook de acción
* Crear una tarea de monitor
* Desencadenar un monitor
* Inspeccionar la salida

## <a name="prerequisites"></a>Prerrequisitos

Para completar este artículo se necesita lo siguiente:

* Suscripción de Azure. Si aún no tiene ninguna, puede [activar las ventajas de la suscripción a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o suscribirse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Cuenta de Automation](./index.yml) para contener los runbooks de monitor y de acción y la tarea de monitor.
* [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) donde se ejecuta la tarea de monitor.
* Runbooks de PowerShell. Los runbooks de flujo de trabajo de PowerShell no son compatibles con las tareas de monitor.

## <a name="import-a-watcher-runbook"></a>Importar un runbook de monitor

En este artículo se usa un runbook de monitor denominado **Watch-NewFile** para buscar archivos nuevos en un directorio. El runbook de monitor recupera la última hora de escritura conocida en los archivos de una carpeta y examina los archivos más recientes que esa marca de agua.

Puede descargar el runbook desde la [organización GitHub de Azure Automation](https://github.com/azureautomation).

1. Vaya a la página de la organización GitHub de Azure Automation para [Watch-NewFile.ps1](https://github.com/azureautomation/watcher-action-that-processes-events-triggerd-by-a-watcher-runbook).
2. Para descargar el runbook desde GitHub, seleccione **Code** (Código) en el lado derecho de la página y después **Download ZIP** (Descargar archivo ZIP) para descargar todo el código en un archivo ZIP.
3. Extraiga el contenido e [importe el runbook](manage-runbooks.md#import-a-runbook-from-the-azure-portal).

También puede importar este runbook a la cuenta de Automation desde el portal mediante los pasos siguientes.

1. Abra la cuenta de Automation y haga clic en la página Runbooks.
2. Haga clic en **Examinar galería** y, en la lista desplegable **Origen**, seleccione **GitHub**.
3. Busque **Watcher runbook**, seleccione **Watcher runbook that looks for new files in a directory** (Runbook de monitor que busca archivos nuevos en un directorio) y haga clic en **Importar**.
4. Asigne al runbook un nombre y una descripción y haga clic en **Aceptar** para importar el runbook en la cuenta de Automation.
5. Seleccione **Editar** y luego haga clic en **Publicar**. Cuando se le pida, haga clic en **Sí** para publicar el runbook.

## <a name="create-an-automation-variable"></a>Creación de una variable de Automation

Se usa una [variable de Automation](./shared-resources/variables.md) para almacenar las marcas de tiempo que el runbook anterior lee y almacena de cada archivo.

1. Seleccione **Variables** en **Recursos compartidos** y haga clic en **+ Agregar una variable**.
1. Escriba **Watch-NewFileTimestamp** para el nombre.
1. Seleccione DateTime como tipo.
1. Haga clic en **Crear** para crear la variable de Automation.

## <a name="create-an-action-runbook"></a>Crear un runbook de acción

Un runbook de acción se usa en una tarea de monitor para actuar sobre los datos transferidos a él desde un runbook de monitor. Debe importar un runbook de acción predefinido llamado **Process-NewFile** desde la [organización GitHub de Azure Automation](https://github.com/azureautomation).

Para crear un runbook de acción:

1. Vaya a la página de la organización GitHub de Azure Automation para [Process-NewFile.ps1](https://github.com/azureautomation/watcher-action-that-processes-events-triggerd-by-a-watcher-runbook).
2. Para descargar el runbook desde GitHub, seleccione **Code** (Código) en el lado derecho de la página y después **Download ZIP** (Descargar archivo ZIP) para descargar todo el código en un archivo ZIP.
3. Extraiga el contenido e [importe el runbook](manage-runbooks.md#import-a-runbook-from-the-azure-portal).

También puede importar este runbook a la cuenta de Automation desde Azure Portal:

1. Vaya a la cuenta de Automation y seleccione **Runbooks** en **Automatización de procesos**.
1. Haga clic en **Examinar galería** y, en la lista desplegable **Origen**, seleccione **GitHub**.
1. Busque **Watcher action**, seleccione **Watcher action that processes events triggered by a watcher runbook** (Acción de monitor que procesa eventos desencadenados por un runbook de monitor) y haga clic en **Importar**.
1. Asigne al runbook un nombre y una descripción y haga clic en **Aceptar** para importar el runbook en la cuenta de Automation.
1. Seleccione **Editar** y luego haga clic en **Publicar**. Cuando se le pida, haga clic en **Sí** para publicar el runbook.

## <a name="create-a-watcher-task"></a>Crear una tarea de monitor

En este paso, va a configurar la tarea de monitor que hace referencia a los runbooks de monitor y de acción definidos en las secciones anteriores.

1. Vaya a la cuenta de Automation y seleccione **Tareas de monitor** en **Automatización de procesos**.
1. Seleccione la página Tareas de monitor y haga clic en **+ Agregar una tarea de monitor**.
1. Escriba **WatchMyFolder** como nombre.

1. Seleccione **Configurar monitor** y elija el runbook **Watch-NewFile**.

1. Escriba los siguientes valores para los parámetros:

   * **FOLDERPATH**: carpeta de Hybrid Runbook Worker donde se crean los nuevos archivos, por ejemplo, **d:\examplefiles**.
   * **EXTENSION**: extensión para la configuración. deje este parámetro en blanco para procesar todas las extensiones de archivo.
   * **RECURSE**: operación recursiva. Deje este valor como el predeterminado.
   * **RUN SETTINGS**: configuración para ejecutar el runbook. seleccione Hybrid Worker.

1. Haga clic en **Aceptar** y, después, en **Seleccionar** para volver a la página Monitor.
1. Seleccione **Configurar acción** y elija el runbook **Process-NewFile**.
1. Escriba los siguientes valores para los parámetros:

   * **EVENTDATA**: datos del evento. déjelo en blanco. Los datos se pasan desde el runbook de monitor.
   * **Run Settings**: configuración para ejecutar el runbook. Déjelo como Azure porque este runbook se ejecuta en Azure Automation.

1. Haga clic en **Aceptar** y, después, en **Seleccionar** para volver a la página Monitor.
1. Haga clic en **Aceptar** para crear la tarea de monitor.

    ![Configurar acción de monitor desde la interfaz de usuario](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Desencadenar un monitor

Debe ejecutar una prueba como se describe a continuación para asegurarse de que la tarea de monitor funciona según lo previsto. 

1. Inicie una sesión remota en Hybrid Runbook Worker.
2. Abra **PowerShell** y cree un archivo de prueba en la carpeta.

```azurepowerShell-interactive
New-Item -Name ExampleFile1.txt
```

En el siguiente ejemplo se muestra la salida esperada.

```output
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>Inspeccionar la salida

1. Vaya a la cuenta de Automation y seleccione **Tareas de monitor** en **Automatización de procesos**.
1. Seleccione la tarea de monitor **WatchMyFolder**.
1. Haga clic en **Ver secuencias de monitor** en **Secuencias** para ver si el monitor ha encontrado el archivo nuevo y ha iniciado el runbook de acción.
1. Haga clic en **Ver trabajos de acción del monitor** para ver los trabajos del runbook de acción. Se puede seleccionar cada trabajo para ver los detalles del trabajo.

   ![Trabajos de acción de monitor desde la interfaz de usuario](media/automation-watchers-tutorial/WatcherActionJobs.png)

La salida esperada cuando se encuentra el archivo nuevo puede verse en el ejemplo siguiente:

```output
Message is Process new file...

Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la creación de un runbook propio, vea [Creación de un runbook de PowerShell](learn/automation-tutorial-runbook-textual-powershell.md).
