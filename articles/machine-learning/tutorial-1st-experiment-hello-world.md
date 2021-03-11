---
title: 'Tutorial: Ejecución de un script "Hola mundo" de Python'
titleSuffix: Azure Machine Learning
description: La parte 2 de la serie de introducción de Azure Machine Learning muestra cómo enviar un script de Python "Hola mundo" trivial a la nube.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 02/11/2021
ms.custom: devx-track-python
ms.openlocfilehash: 4f2b01b7a04958c4bd1f97332b54a1ff4fc32356
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102522332"
---
# <a name="tutorial-run-a-hello-world-python-script-part-2-of-4"></a>Tutorial: Ejecución de un script "Hola mundo" (parte 2 de 4)

En este tutorial aprenderá a usar el SDK de Azure Machine Learning para Python para enviar y ejecutar un script "Hola mundo" de Python.

Este tutorial es la *parte 2 de una serie de tutoriales de cuatro partes* en la que aprenderá los aspectos básicos de Azure Machine Learning y completará tareas de aprendizaje automático basadas en trabajos en Azure. Este tutorial se basa en el trabajo completado en [Parte 1: Configuración de la máquina local para Azure Machine Learning](tutorial-1st-experiment-sdk-setup-local.md).

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Cómo crear y ejecutar de un script "Hola mundo" de Python de forma local.
> * Crear un script de control de Python para enviar "Hola mundo" a Azure Machine Learning.
> * Comprender los conceptos de Azure Machine Learning en el script de control.
> * Enviar y ejecutar el script "Hola mundo".
> * Ver la salida del código en la nube.

## <a name="prerequisites"></a>Requisitos previos

- Realización de [Parte 1](tutorial-1st-experiment-sdk-setup-local.md) si aún no tiene un área de trabajo de Azure Machine Learning.

## <a name="create-and-run-a-python-script-locally"></a>Creación y ejecución de un script de Python de forma local

Cree un nuevo subdirectorio llamado `src` en el directorio `tutorial` para almacenar el código que quiere ejecutar en un clúster de proceso de Azure Machine Learning. En el subdirectorio `src`, cree el script de Python `hello.py`:

```python
# src/hello.py
print("Hello world!")
```

Ahora la estructura de directorios del proyecto tendrá el siguiente aspecto:

:::image type="content" source="media/tutorial-1st-experiment-hello-world/directory-structure.png" alt-text="La estructura de directorios muestra hello.py en el subdirectorio src.":::


### <a name="test-your-script-locally"></a><a name="test"></a>Prueba del script de forma local

Puede ejecutar el código localmente mediante su IDE favorito o un terminal. Ejecutar el código localmente tiene la ventaja de la depuración interactiva del código.  En la ventana que tiene activado el entorno de Conda *tutorial1*, ejecute el archivo de Python:

```bash
cd <path/to/tutorial>
python ./src/hello.py
```

> [!div class="nextstepaction"]
> [He ejecutado el script localmente](?success=run-local#control-script) [He tenido un problema](https://www.research.net/r/7C2NTH7?issue=run-local)

## <a name="create-a-control-script"></a><a name="control-script"></a> Creación de un script de control

Un *script de control* le permite ejecutar el script de `hello.py` en la nube. El script de control se usa para controlar cómo y dónde se ejecuta el código de aprendizaje automático.  

En el directorio del tutorial, cree un nuevo archivo de Python denominado `03-run-hello.py`, y copie y pegue el código siguiente en ese archivo:

```python
# tutorial/03-run-hello.py
from azureml.core import Workspace, Experiment, Environment, ScriptRunConfig

ws = Workspace.from_config()
experiment = Experiment(workspace=ws, name='day1-experiment-hello')

config = ScriptRunConfig(source_directory='./src', script='hello.py', compute_target='cpu-cluster')

run = experiment.submit(config)
aml_url = run.get_portal_url()
print(aml_url)
```

### <a name="understand-the-code"></a>Comprendiendo el código

Esta es una descripción de cómo funciona el script de control:

:::row:::
   :::column span="":::
      `ws = Workspace.from_config()`
   :::column-end:::
   :::column span="2":::
      El [área de trabajo](/python/api/azureml-core/azureml.core.workspace.workspace) se conecta al área de trabajo de Azure Machine Learning para que pueda comunicarse con los recursos de Azure Machine Learning.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `experiment =  Experiment( ... )`
   :::column-end:::
   :::column span="2":::
      El [experimento](/python/api/azureml-core/azureml.core.experiment.experiment) proporciona una manera sencilla de organizar varias ejecuciones con un solo nombre. Más adelante, podrá ver cómo los experimentos facilitan la comparación de las métricas entre docenas de ejecuciones.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config = ScriptRunConfig( ... )` 
   :::column-end:::
   :::column span="2":::
      [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig) ajusta el código `hello.py` y lo pasa al área de trabajo. Como sugiere su nombre, puede usar esta clase para _configurar_ cómo quiera el _script_ para su _ejecución_ en Azure Machine Learning. También especifica el destino de proceso en el que se ejecutará el script. En este código, el destino es el clúster de proceso que creó en el [tutorial de configuración](tutorial-1st-experiment-sdk-setup-local.md).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `run = experiment.submit(config)`
   :::column-end:::
   :::column span="2":::
       Envía el script. Este envío se denomina [ejecución](/python/api/azureml-core/azureml.core.run%28class%29). Una ejecución encapsula una ejecución única del código. Use una ejecución para supervisar el progreso del script, capturar la salida, analizar los resultados, visualizar las métricas y mucho más.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `aml_url = run.get_portal_url()` 
   :::column-end:::
   :::column span="2":::
        El objeto `run` proporciona un identificador en la ejecución del código. Supervise el progreso de Azure Machine Learning Studio con la dirección URL que se imprime desde el script de Python.  
   :::column-end:::
:::row-end:::

> [!div class="nextstepaction"]
> [He creado el script de control](?success=create-control-script#submit) [He tenido un problema](https://www.research.net/r/7C2NTH7?issue=create-control-script)

## <a name="submit-and-run-your-code-in-the-cloud"></a><a name="submit"></a> Envío y ejecución del código en la nube

Ejecute el script de control, que a su vez ejecuta `hello.py` en el clúster de proceso que creó en el [tutorial de configuración](tutorial-1st-experiment-sdk-setup-local.md).


```bash
python 03-run-hello.py
```

> [!TIP]
> Si la ejecución de este código genera un error que indica que no tiene acceso a la suscripción, consulte [Conexión a un área de trabajo](how-to-manage-workspace.md?tab=python#connect-multi-tenant) para información sobre las opciones de autenticación.

> [!div class="nextstepaction"]
> [He enviado código en la nube](?success=submit-to-cloud#monitor) [He tenido un problema](https://www.research.net/r/7C2NTH7?issue=submit-to-cloud)

## <a name="monitor-your-code-in-the-cloud-by-using-the-studio"></a><a name="monitor"></a>Supervisión del código en la nube con Studio

La salida del script contendrá un vínculo a Studio que tiene un aspecto similar al siguiente: `https://ml.azure.com/experiments/hello-world/runs/<run-id>?wsid=/subscriptions/<subscription-id>/resourcegroups/<resource-group>/workspaces/<workspace-name>`.

Siga el vínculo.  En primer lugar, verá el estado **En preparación**.  La primera ejecución tardará entre 5 y 10 minutos en completarse. Esto se debe a lo siguiente:

* Se crea una imagen de Docker en la nube.
* Se cambia el tamaño del clúster de proceso de 0 a 1 nodo.
* La imagen de Docker se descarga en el proceso. 

Las ejecuciones posteriores son mucho más rápidas (unos 15 segundos), ya que la imagen de Docker se almacena en caché en el proceso. Para probar esto, reenvíe el código siguiente después de que se haya completado la primera ejecución.

Una vez completado el trabajo, vaya a la pestaña **Salidas y registros**. Allí puede ver un archivo `70_driver_log.txt` similar al siguiente:

```txt
 1: [2020-08-04T22:15:44.407305] Entering context manager injector.
 2: [context_manager_injector.py] Command line Options: Namespace(inject=['ProjectPythonPath:context_managers.ProjectPythonPath', 'RunHistory:context_managers.RunHistory', 'TrackUserError:context_managers.TrackUserError', 'UserExceptions:context_managers.UserExceptions'], invocation=['hello.py'])
 3: Starting the daemon thread to refresh tokens in background for process with pid = 31263
 4: Entering Run History Context Manager.
 5: Preparing to call script [ hello.py ] with arguments: []
 6: After variable expansion, calling script [ hello.py ] with arguments: []
 7:
 8: Hello world!
 9: Starting the daemon thread to refresh tokens in background for process with pid = 31263
10:
11:
12: The experiment completed successfully. Finalizing run...
13: Logging experiment finalizing status in history service.
14: [2020-08-04T22:15:46.541334] TimeoutHandler __init__
15: [2020-08-04T22:15:46.541396] TimeoutHandler __enter__
16: Cleaning up all outstanding Run operations, waiting 300.0 seconds
17: 1 items cleaning up...
18: Cleanup took 0.1812913417816162 seconds
19: [2020-08-04T22:15:47.040203] TimeoutHandler __exit__
```

En la línea 8, verá el resultado "Hola mundo" .

El archivo de `70_driver_log.txt` contiene la salida estándar de una ejecución. Este archivo puede ser útil al depurar ejecuciones remotas en la nube.

> [!div class="nextstepaction"]
> [He visto el registro en Studio](?success=monitor-in-studio#next-steps) [He tenido un problema](https://www.research.net/r/7C2NTH7?issue=monitor-in-studio)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, tomó un script "Hola mundo" sencillo y lo ejecutó en Azure. Vio cómo conectarse al área de trabajo de Azure Machine Learning, crear un experimento y enviar el código `hello.py` a la nube.

En el siguiente tutorial, se basará en estos aprendizajes para ejecutar algo más interesante que `print("Hello world!")`.

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de un modelo](tutorial-1st-experiment-sdk-train.md)

>[!NOTE] 
> Si quiere finalizar la serie de tutoriales aquí y no avanzar al paso siguiente, recuerde [limpiar los recursos](tutorial-1st-experiment-bring-data.md#clean-up-resources).
