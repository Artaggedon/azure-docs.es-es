---
title: Solución de problemas de ParallelRunStep
titleSuffix: Azure Machine Learning
description: Sugerencias para solucionar problemas que se producen al usar ParallelRunStep en canalizaciones de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: troubleshooting
ms.custom: troubleshooting
ms.reviewer: larryfr, vaidyas, laobri, tracych
ms.author: trmccorm
author: tmccrmck
ms.date: 09/23/2020
ms.openlocfilehash: b5511c8ecc33238e0409b5ee4c1c7a11adddeac5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102522162"
---
# <a name="troubleshooting-the-parallelrunstep"></a>Solución de problemas de ParallelRunStep

En este artículo aprenderá a solucionar los errores que se producen al usar la clase [ParallelRunStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallel_run_step.parallelrunstep) con el [SDK de Azure Machine Learning](/python/api/overview/azure/ml/intro).

Para obtener sugerencias generales sobre la solución de problemas de una canalización, consulte [Solución de problemas de canalizaciones de aprendizaje automático](how-to-debug-pipelines.md).

## <a name="testing-scripts-locally"></a>Prueba de scripts de forma local

 La clase ParallelRunStep se ejecuta como un paso de las canalizaciones de Machine Learning. Es posible que desee [probar los scripts localmente](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-machine-learning-pipelines) como primer paso.

##  <a name="script-requirements"></a>Requisitos del script

El script para `ParallelRunStep` *debe contener* dos funciones:
- `init()`: utilice esta función para cualquier preparación costosa o común para la inferencia posterior. Por ejemplo, para cargar el modelo en un objeto global. Solo se llamará a esta función una vez al principio del proceso.
-  `run(mini_batch)`: la función se ejecutará para cada instancia de `mini_batch`.
    -  `mini_batch`: `ParallelRunStep` invocará el método run y pasará una lista o `DataFrame` de Pandas como argumento al método. Cada entrada de mini_batch será una ruta de acceso de archivo si la entrada es `FileDataset`, o `DataFrame` de Pandas si la entrada es `TabularDataset`.
    -  `response`: El método run() debe devolver `DataFrame` de Pandas o una matriz. Para append_row output_action, estos elementos devueltos se anexan al archivo de salida común. Para summary_only, se omite el contenido de los elementos. Para todas las acciones de salida, cada elemento de salida devuelto indica una ejecución correcta del elemento de entrada en el minilote de entrada. Asegúrese de que se incluyen suficientes datos en el resultado de la ejecución para asignar la entrada al resultado de la salida de la ejecución. La salida de la ejecución se escribirá en el archivo de salida y no se garantiza que esté en orden, por lo que debe usar alguna clave en la salida para asignarla a la entrada.

```python
%%writefile digit_identification.py
# Snippets from a sample script.
# Refer to the accompanying digit_identification.py
# (https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/parallel-run)
# for the implementation script.

import os
import numpy as np
import tensorflow as tf
from PIL import Image
from azureml.core import Model


def init():
    global g_tf_sess

    # Pull down the model from the workspace
    model_path = Model.get_model_path("mnist")

    # Construct a graph to execute
    tf.reset_default_graph()
    saver = tf.train.import_meta_graph(os.path.join(model_path, 'mnist-tf.model.meta'))
    g_tf_sess = tf.Session()
    saver.restore(g_tf_sess, os.path.join(model_path, 'mnist-tf.model'))


def run(mini_batch):
    print(f'run method start: {__file__}, run({mini_batch})')
    resultList = []
    in_tensor = g_tf_sess.graph.get_tensor_by_name("network/X:0")
    output = g_tf_sess.graph.get_tensor_by_name("network/output/MatMul:0")

    for image in mini_batch:
        # Prepare each image
        data = Image.open(image)
        np_im = np.array(data).reshape((1, 784))
        # Perform inference
        inference_result = output.eval(feed_dict={in_tensor: np_im}, session=g_tf_sess)
        # Find the best probability, and add it to the result list
        best_result = np.argmax(inference_result)
        resultList.append("{}: {}".format(os.path.basename(image), best_result))

    return resultList
```

Si tiene otro archivo o carpeta en el mismo directorio que el script de inferencia, puede hacer referencia a él buscando el directorio de trabajo actual.

```python
script_dir = os.path.realpath(os.path.join(__file__, '..',))
file_path = os.path.join(script_dir, "<file_name>")
```

### <a name="parameters-for-parallelrunconfig"></a>Parámetros para ParallelRunConfig

`ParallelRunConfig` es la configuración principal de la instancia de `ParallelRunStep` dentro de la canalización de Azure Machine Learning. Se usa para encapsular el script y configurar los parámetros necesarios, incluidos los de las siguientes entradas:
- `entry_script`: script de usuario como ruta de acceso de archivo local que se ejecutará en paralelo en varios nodos. Si `source_directory` está presente, utilice una ruta de acceso relativa. De lo contrario, use cualquier ruta de acceso accesible desde la máquina.
- `mini_batch_size`: tamaño del minilote que se pasa a una sola llamada de `run()`. (Opcional; el valor predeterminado es `10` archivos para `FileDataset`, y `1MB` para `TabularDataset`).
    - En el caso de `FileDataset`, es el número de archivos con un valor mínimo de `1`. Puede combinar varios archivos en un solo minilote.
    - En el caso de `TabularDataset`, es el tamaño de los datos. Los valores posibles son `1024`, `1024KB`, `10MB` y `1GB`. `1MB` es el valor recomendado. El minilote de `TabularDataset` nunca cruzará los límites de los archivos. Por ejemplo, si tiene archivos. csv de varios tamaños, el menor es de 100 KB y el mayor es de 10 MB. Si establece `mini_batch_size = 1MB`, los archivos con un tamaño menor que 1 MB se tratarán como un solo minilote. Los archivos de tamaño mayor que 1 MB se dividirán en varios minilotes.
- `error_threshold`: número de errores de registro para `TabularDataset` y errores de archivo para `FileDataset` que se deben omitir durante el procesamiento. Si el recuento de errores de la entrada supera este valor, el trabajo se anulará. El umbral de error es para toda la entrada y no para los minilotes individuales que se envían al método `run()`. El intervalo es `[-1, int.max]`. La parte `-1` indica que se omitirán todos los errores durante el procesamiento.
- `output_action`: uno de los valores siguientes indica cómo se organizará la salida:
    - `summary_only`: el script de usuario almacenará la salida. `ParallelRunStep` usará la salida solo para el cálculo del umbral de error.
    - `append_row`: en las entradas, solo se creará un archivo en la carpeta de salida para anexar todas las salidas separadas por líneas.
- `append_row_file_name`: para personalizar el nombre del archivo de salida de append_row output_action (opcional; el valor predeterminado es `parallel_run_step.txt`).
- `source_directory`: rutas de acceso a las carpetas que contienen todos los archivos que se van a ejecutar en el destino de proceso (opcional).
- `compute_target`: Solo se admite `AmlCompute`.
- `node_count`: número de nodos de proceso que se usarán para ejecutar el script de usuario.
- `process_count_per_node`: número de procesos por nodo. El procedimiento recomendado es establecerlo en el número de GPU o CPU que tenga un nodo (opcional; el valor predeterminado es `1`).
- `environment`: definición del entorno de Python. Puede configurarla para usar un entorno de Python existente o un entorno temporal. La definición también es responsable de establecer las dependencias de la aplicación necesarias (opcional).
- `logging_level`: nivel de detalle del registro. Los valores con nivel de detalle en aumento son: `WARNING`, `INFO` y `DEBUG`. (Opcional; el valor predeterminado es `INFO`).
- `run_invocation_timeout`: tiempo de espera de invocación del método `run()` en segundos. (Opcional; el valor predeterminado es `60`).
- `run_max_try`: número máximo de intentos de `run()` para un minilote. Se produce un error en `run()` si se genera una excepción o no se devuelve nada cuando se alcanza `run_invocation_timeout` (opcional; el valor predeterminado es `3`). 

Puede especificar `mini_batch_size`, `node_count`, `process_count_per_node`, `logging_level`, `run_invocation_timeout` y `run_max_try` como `PipelineParameter`, de modo que, cuando vuelva a enviar una ejecución de la canalización, pueda optimizar los valores de los parámetros. En este ejemplo, se usa `PipelineParameter` para `mini_batch_size` y `Process_count_per_node`, y se cambiarán estos valores cuando se vuelva a enviar una ejecución más tarde. 

### <a name="parameters-for-creating-the-parallelrunstep"></a>Parámetros para crear ParallelRunStep

Cree ParallelRunStep mediante el script, la configuración del entorno y los parámetros. Especifique el destino de proceso que ya adjuntó a su área de trabajo como destino de ejecución del script de inferencia. Use `ParallelRunStep` para crear el paso de canalización de inferencias por lotes, que toma todos los parámetros siguientes:
- `name`: nombre del paso, con las siguientes restricciones de nomenclatura: unique, 3-32 caracteres y regex ^\[a-z\]([-a-z0-9]*[a-z0-9])?$.
- `parallel_run_config`: objeto `ParallelRunConfig`, tal y como se definió anteriormente.
- `inputs`: uno o varios conjuntos de datos de Azure Machine Learning de tipo único que se van a particionar para el procesamiento en paralelo.
- `side_inputs`: uno o varios datos de referencia o conjuntos de datos que se usan como entradas laterales sin necesidad de crear particiones.
- `output`: un objeto `OutputFileDatasetConfig` que representa la ruta de acceso al directorio en el que se almacenarán los datos de salida.
- `arguments`: lista de los argumentos pasados al script de usuario. Use unknown_args para recuperarlos en el script de entrada (opcional).
- `allow_reuse`: sirve para decidir si el paso debe volver a usar los resultados anteriores cuando se ejecuta con la misma configuración o entrada. Si este parámetro es `False`, siempre se generará una nueva ejecución para este paso durante la ejecución de la canalización. (Opcional; el valor predeterminado es `True`).

```python
from azureml.pipeline.steps import ParallelRunStep

parallelrun_step = ParallelRunStep(
    name="predict-digits-mnist",
    parallel_run_config=parallel_run_config,
    inputs=[input_mnist_ds_consumption],
    output=output_dir,
    allow_reuse=True
)
```

## <a name="debugging-scripts-from-remote-context"></a>Depuración de scripts desde un contexto remoto

La transición de la depuración de un script de puntuación de forma local, a la depuración de un script de puntuación en una canalización real puede resultar difícil. Para información sobre cómo buscar los registros en el portal, consulte la [sección de canalizaciones de aprendizaje automático en la depuración de scripts desde un contexto remoto](how-to-debug-pipelines.md). La información de esa sección también se aplica a ParallelRunStep.

Por ejemplo, el archivo de registro `70_driver_log.txt` contiene información del controlador que inicia el código de ParallelRunStep.

Dada la naturaleza distribuida de los trabajos de ParallelRunStep, hay registros de distintos orígenes. Sin embargo, se crean dos archivos consolidados que proporcionan información de alto nivel:

- `~/logs/job_progress_overview.txt`: Este archivo proporciona información de alto nivel sobre el número de minilotes (también conocidos como tareas) que se han creado hasta el momento y el número de minilotes que se han procesado hasta ahora. En este extremo, se muestra el resultado del trabajo. Si se produjo un error en el trabajo, se mostrará el mensaje de error y dónde se debe iniciar la solución de problemas.

- `~/logs/sys/master_role.txt`: Este archivo proporciona la vista del nodo principal (también conocido como orquestador) del trabajo en ejecución. Incluye la creación de tareas, la supervisión del progreso y el resultado de la ejecución.

Los registros generados a partir del script de entrada mediante el asistente EntryScript y las instrucciones print se encuentran en los siguientes archivos:

- `~/logs/user/entry_script_log/<ip_address>/<process_name>.log.txt`: estos archivos son los registros escritos desde entry_script con el asistente EntryScript.

- `~/logs/user/stdout/<ip_address>/<process_name>.stdout.txt`: estos archivos son los registros de stdout (por ejemplo, la instrucción print) de entry_script.

- `~/logs/user/stderr/<ip_address>/<process_name>.stderr.txt`: estos archivos son los registros de stderr de entry_script.

Para obtener una descripción concisa de los errores del script, hay lo siguiente:

- `~/logs/user/error.txt`: este archivo intentará resumir los errores del script.

Para obtener más información sobre los errores del script, hay lo siguiente:

- `~/logs/user/error/`: contiene seguimientos de pila completos de las excepciones producidas al cargar y ejecutar el script de entrada.

Cuando necesite comprender en detalle cómo ejecuta cada nodo el script de puntuación, examine los registros de proceso individuales para cada nodo. Los registros de proceso se pueden encontrar en la carpeta `sys/node`, agrupados por nodos de trabajo:

- `~/logs/sys/node/<ip_address>/<process_name>.txt`: este archivo proporciona información detallada sobre cada uno de los minilotes a medida que un trabajo lo elige o lo completa. Para cada minilote, este archivo incluye:

    - La dirección IP y el PID del proceso de trabajo. 
    - El número total de elementos, el número de elementos procesados correctamente y el número de elementos con errores.
    - Hora de inicio, duración, tiempo de proceso y tiempo del método de ejecución.

También puede ver los resultados de las comprobaciones periódicas del uso de recursos de cada nodo. Los archivos de registro y los archivos de configuración se encuentran en esta carpeta:

- `~/logs/perf`: establezca `--resource_monitor_interval` para cambiar el intervalo de comprobación en segundos. El intervalo predeterminado es `600`, que son aproximadamente 10 minutos. Para detener la supervisión, establezca el valor en `0`. Cada carpeta `<ip_address>` incluye:

    - `os/`: información acerca de todos los procesos en ejecución en el nodo. Una comprobación ejecuta un comando del sistema operativo y guarda el resultado en un archivo. En Linux, el comando es `ps`. Para Windows, use `tasklist`.
        - `%Y%m%d%H`: el nombre de la subcarpeta es la fecha y hora.
            - `processes_%M`: el archivo finaliza con el minuto de la hora de la comprobación.
    - `node_disk_usage.csv`: detalles de uso del disco del nodo.
    - `node_resource_usage.csv`: información general sobre el uso de recursos del nodo.
    - `processes_resource_usage.csv`: información general sobre el uso de recursos de cada proceso.

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>¿Cómo me puedo registrar desde mi script de usuario en un contexto remoto?

ParallelRunStep puede ejecutar varios procesos en un nodo basado en process_count_per_node. Para organizar los registros de cada proceso en el nodo y combinar las instrucciones print y log, se recomienda usar el registrador ParallelRunStep como se muestra a continuación. Puede obtener un registrador de EntryScript, y hacer que los registros aparezcan en la carpeta **logs/user** del portal.

**Uso del registrador por un script de entrada de ejemplo:**
```python
from azureml_user.parallel_run import EntryScript

def init():
    """ Initialize the node."""
    entry_script = EntryScript()
    logger = entry_script.logger
    logger.debug("This will show up in files under logs/user on the Azure portal.")


def run(mini_batch):
    """ Accept and return the list back."""
    # This class is in singleton pattern and will return same instance as the one in init()
    entry_script = EntryScript()
    logger = entry_script.logger
    logger.debug(f"{__file__}: {mini_batch}.")
    ...

    return mini_batch
```

### <a name="how-could-i-pass-a-side-input-such-as-a-file-or-files-containing-a-lookup-table-to-all-my-workers"></a>¿Cómo puedo pasar una entrada lateral, como un archivo o archivos que contienen una tabla de búsqueda, a todos los trabajadores?

El usuario puede pasar datos de referencia al script mediante el parámetro side_inputs de ParalleRunStep. Todos los conjuntos de datos proporcionados como side_inputs se montarán en cada nodo de trabajo. El usuario puede obtener la ubicación del montaje pasando el argumento.

Construya un [conjunto de datos](/python/api/azureml-core/azureml.core.dataset.dataset) que contenga los datos de referencia y regístrelo con su área de trabajo. Páselo al parámetro `side_inputs` de `ParallelRunStep`. Además, puede agregar su ruta de acceso en la sección `arguments` para acceder fácilmente a su ruta de acceso montada:

```python
label_config = label_ds.as_named_input("labels_input")
batch_score_step = ParallelRunStep(
    name=parallel_step_name,
    inputs=[input_images.as_named_input("input_images")],
    output=output_dir,
    arguments=["--labels_dir", label_config],
    side_inputs=[label_config],
    parallel_run_config=parallel_run_config,
)
```

Una vez que pueda acceder a él en su script inferencia [por ejemplo, en el método init()] como sigue:

```python
parser = argparse.ArgumentParser()
parser.add_argument('--labels_dir', dest="labels_dir", required=True)
args, _ = parser.parse_known_args()

labels_path = args.labels_dir
```

### <a name="how-to-use-input-datasets-with-service-principal-authentication"></a>¿Cómo se usan los conjuntos de datos de entrada con la autenticación de entidad de servicio?

El usuario puede pasar conjuntos de datos de entrada con la autenticación de entidad de servicio usada en el área de trabajo. El uso de un conjunto de datos de este tipo en ParallelRunStep requiere que se registre el conjunto para que construya la configuración de ParallelRunStep.

```python
service_principal = ServicePrincipalAuthentication(
    tenant_id="***",
    service_principal_id="***",
    service_principal_password="***")
 
ws = Workspace(
    subscription_id="***",
    resource_group="***",
    workspace_name="***",
    auth=service_principal
    )
 
default_blob_store = ws.get_default_datastore() # or Datastore(ws, '***datastore-name***') 
ds = Dataset.File.from_files(default_blob_store, '**path***')
registered_ds = ds.register(ws, '***dataset-name***', create_new_version=True)
```

## <a name="next-steps"></a>Pasos siguientes

* Consulte estos [cuadernos de Jupyter Notebook que muestran canalizaciones de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines).

* Vea la referencia del SDK para obtener ayuda con el paquete [azureml-pipeline-steps](/python/api/azureml-pipeline-steps/azureml.pipeline.steps). Vea la [documentación](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallelrunstep) de referencia de la clase ParallelRunStep.

* Realice el [tutorial avanzado](tutorial-pipeline-batch-scoring-classification.md) sobre cómo usar canalizaciones con ParallelRunStep. En este tutorial se explica cómo pasar otro archivo como entrada lateral.
