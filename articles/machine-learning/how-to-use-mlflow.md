---
title: Seguimiento de MLflow para experimentos de Machine Learning
titleSuffix: Azure Machine Learning
description: Configure MLflow con Azure Machine Learning para registrar métricas y artefactos de modelos de aprendizaje automático e implementar los modelos de aprendizaje automático como un servicio web.
services: machine-learning
author: shivp950
ms.author: shipatel
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.date: 12/23/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: ed9d41a84e455241ed3cfc41b905a671f2a2d499
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97912961"
---
# <a name="train-and-track-ml-models-with-mlflow-and-azure-machine-learning-preview"></a>Entrenamiento y seguimiento de modelos de Machine Learning con MLflow y Azure Machine Learning (versión preliminar)

En este artículo, obtendrá información sobre cómo habilitar el identificador URI de seguimiento y la API de registro de MLflow, que en conjunto se conocen como [MLflow Tracking](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api), para conectar Azure Machine Learning como el back-end de experimentos de MLflow. 

Las funcionalidades admitidas son: 

+ Realizar un seguimiento de las métricas y los artefactos de los experimentos, así como registrarlos, en el [área de trabajo de Azure Machine Learning](./concept-azure-machine-learning-architecture.md#workspace). Si ya usa Seguimiento de MLflow para los experimentos, el área de trabajo proporciona una ubicación centralizada, segura y escalable para almacenar los modelos y las métricas de entrenamiento.

+ Enviar trabajos de aprendizaje con [proyectos de MLflow](https://www.mlflow.org/docs/latest/projects.html) con compatibilidad de back-end con Azure Machine Learning (versión preliminar). Puede enviar trabajos localmente con el seguimiento de Azure Machine Learning o migrar las ejecuciones a la nube, por ejemplo, a través de un [Proceso de Azure Machine Learning](./how-to-create-attach-compute-cluster.md).

+ Hacer un seguimiento de modelos y administrarlos en MLflow y el registro de modelos de Azure Machine Learning.

[MLflow](https://www.mlflow.org) es una biblioteca de código abierto para administrar el ciclo de vida de los experimentos de aprendizaje automático. MLFlow Tracking es un componente de MLflow que lleva a cabo un registro y un seguimiento de las métricas de ejecución de entrenamiento y de los artefactos del modelo, independientemente del entorno del experimento (localmente en su equipo, en un destino de proceso remoto, en una máquina virtual o en un [clúster de Azure Databricks](how-to-use-mlflow-azure-databricks.md)). 

>[!NOTE]
> Como biblioteca de código abierto, MLflow cambia con frecuencia. Como tal, la funcionalidad que se pone a disposición a través de la integración de Azure Machine Learning y MLflow debe considerarse como una vista previa y no es totalmente compatible con Microsoft.

En el siguiente diagrama se ilustra que con Seguimiento de MLflow, se realiza un seguimiento de las métricas de ejecución de un experimento y se almacenan los artefactos del modelo en el área de trabajo de Azure Machine Learning.

![Diagrama de MLflow con Azure Machine Learning](./media/how-to-use-mlflow/mlflow-diagram-track.png)

> [!TIP]
> La información de este documento va destinada principalmente a aquellos científicos de datos y desarrolladores que deseen supervisar el proceso de entrenamiento del modelo. Los administradores que estén interesados en la supervisión del uso de recursos y eventos desde Azure Machine Learning, como cuotas, ejecuciones de entrenamiento completadas o implementaciones de modelos completadas pueden consultar [Supervisión de Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>Comparación entre los clientes de MLflow y Azure Machine Learning

 En la tabla siguiente se resumen los distintos clientes que pueden usar Azure Machine Learning y sus respectivas funcionalidades.

 El Seguimiento de MLflow ofrece funciones de registro de métricas y almacenamiento de artefactos que de otra manera solo están disponibles a través del [SDK de Python de Azure Machine Learning](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py).

| Capacidad | Implementación y seguimiento de MLflow | SDK de Python de Azure Machine Learning |  CLI de Azure Machine Learning | Azure Machine Learning Studio|
|---|---|---|---|---|
| Administración del área de trabajo |   | ✓ | ✓ | ✓ |
| Uso de almacenes de datos  |   | ✓ | ✓ | |
| Métricas de registro      | ✓ | ✓ |   | |
| Carga de artefactos | ✓ | ✓ |   | |
| Visualización de métricas     | ✓ | ✓ | ✓ | ✓ |
| Administración de procesos   |   | ✓ | ✓ | ✓ |
| Implementación de modelos    | ✓ | ✓ | ✓ | ✓ |
|Supervisión del rendimiento del modelo||✓|  |   |
| Detección del desfase de datos |   | ✓ |   | ✓ |

## <a name="prerequisites"></a>Requisitos previos

* Instale el paquete `azureml-mlflow`. 
    * Este paquete incorpora automáticamente `azureml-core` del [SDK de Azure Machine Learning de Python](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py), que proporciona la conectividad de MLflow para acceder al área de trabajo.
* [Cree un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).
    * Consulte qué [permisos de acceso necesita para realizar las operaciones de MLflow con el área de trabajo](how-to-assign-roles.md#mlflow-operations).

## <a name="track-local-runs"></a>Seguimiento de ejecuciones locales

El seguimiento de MLflow con Azure Machine Learning le permite almacenar las métricas y los artefactos registrados desde las ejecuciones locales en el área de trabajo de Azure Machine Learning.

Importe las clases `mlflow` y [`Workspace`](/python/api/azureml-core/azureml.core.workspace%28class%29?preserve-view=true&view=azure-ml-py) para acceder al URI de seguimiento de MLflow y configurar el área de trabajo.

En el código siguiente, el método `get_mlflow_tracking_uri()` asigna un dirección URI de seguimiento única al área de trabajo (`ws`) y `set_tracking_uri()` apunta el URI de seguimiento de MLflow a esa dirección.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>El URI de seguimiento es válido hasta una hora como máximo. Si reinicia el script después de un tiempo de inactividad, use la API get_mlflow_tracking_uri para obtener un nuevo URI.

Establezca el nombre del experimento de MLflow con `set_experiment()` y comience la ejecución de entrenamiento con `start_run()`. Después, use `log_metric()` para activar la API de registro de MLflow y empezar a registrar las métricas de la ejecución de entrenamiento.

```Python
experiment_name = 'experiment_with_mlflow'
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Seguimiento de ejecuciones remotas

Las ejecuciones remotas permiten entrenar modelos en procesos más eficaces, como máquinas virtuales habilitadas para GPU o clústeres de Proceso de Machine Learning. Consulte [Uso de destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md) para obtener información sobre las distintas opciones de proceso.

El seguimiento de MLflow con Azure Machine Learning le permite almacenar las métricas y los artefactos registrados desde las ejecuciones remotas en el área de trabajo de Azure Machine Learning. En cualquier ejecución que contenga código de seguimiento de MLflow se registrarán automáticamente las métricas en el área de trabajo. 

En el ejemplo siguiente, en el entorno de Conda se incluye `mlflow` y `azureml-mlflow` como paquetes PIP. 


```yaml
name: sklearn-example
dependencies:
  - python=3.6.2
  - scikit-learn
  - matplotlib
  - numpy
  - pip:
    - azureml-mlflow
    - numpy
```

En el script, configure el proceso y el entorno de ejecución de entrenamiento con la clase [`Environment`](/python/api/azureml-core/azureml.core.environment.environment?preserve-view=true&view=azure-ml-py). Después, construya [`ScriptRunConfig`](/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?preserve-view=true&view=azure-ml-py) con el proceso remoto como destino de proceso.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric('example', 1.23)
```

Con esta configuración del proceso y de la ejecución de entrenamiento, use el método `Experiment.submit()` para enviar una ejecución. Este método establece automáticamente el identificador URI de seguimiento de MLflow y dirige el registro de MLflow al área de trabajo.

```Python
run = exp.submit(src)
```

## <a name="train-with-mlflow-projects"></a>Entrenamiento con proyectos de MLflow

Los [proyectos de MLflow](https://mlflow.org/docs/latest/projects.html) permiten organizar y describir el código para permitir que otros científicos de datos (o herramientas automatizadas) lo ejecuten. Los proyectos de MLflow con Azure Machine Learning le permiten realizar un seguimiento de las ejecuciones de entrenamiento y administrarlas en el área de trabajo. 

En este ejemplo se muestra cómo enviar proyectos de MLflow localmente con seguimiento de Azure Machine Learning.

Instale el paquete `azureml-mlflow` que va a usar el seguimiento de MLflow con Azure Machine Learning en los experimentos localmente. Los experimentos pueden ejecutarse a través de una instancia de Jupyter Notebook o un editor de código.

```shell
pip install azureml-mlflow
```

Importe las clases `mlflow` y [`Workspace`](/python/api/azureml-core/azureml.core.workspace%28class%29?preserve-view=true&view=azure-ml-py) para acceder al URI de seguimiento de MLflow y configurar el área de trabajo.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

Establezca el nombre del experimento de MLflow con `set_experiment()` y comience la ejecución de entrenamiento con `start_run()`. Después, use `log_metric()` para activar la API de registro de MLflow y empezar a registrar las métricas de la ejecución de entrenamiento.

```Python
experiment_name = 'experiment-with-mlflow-projects'
mlflow.set_experiment(experiment_name)
```

Cree el objeto de configuración de back-end para almacenar la información necesaria para la integración, por ejemplo, el destino de proceso y el tipo de entorno administrado que se va a usar.

```python
backend_config = {"USE_CONDA": False}
```
Agregue el paquete `azureml-mlflow` como dependencia pip al archivo de configuración del entorno para realizar el seguimiento de las métricas y los artefactos clave en el área de trabajo. 

``` shell
name: mlflow-example
channels:
  - defaults
  - anaconda
  - conda-forge
dependencies:
  - python=3.6
  - scikit-learn=0.19.1
  - pip
  - pip:
    - mlflow
    - azureml-mlflow
```
Envíe la ejecución local y asegúrese de establecer el parámetro `backend = "azureml" `. Con esta configuración, puede enviar ejecuciones localmente y obtener la compatibilidad adicional del seguimiento de salida automático, archivos de registro, instantáneas y errores impresos en el área de trabajo. 

Vea las ejecuciones y métricas en [Azure Machine Learning Studio](overview-what-is-machine-learning-studio.md). 


```python
local_env_run = mlflow.projects.run(uri=".", 
                                    parameters={"alpha":0.3},
                                    backend = "azureml",
                                    use_conda=False,
                                    backend_config = backend_config, 
                                    )

```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Visualización de las métricas y los artefactos en el área de trabajo

Las métricas y los artefactos procedentes del registro de MLflow se conservan en el área de trabajo. Para verlos en cualquier momento, vaya al área de trabajo y busque el experimento por su nombre en el área de trabajo en [Azure Machine Learning Studio](https://ml.azure.com).  O bien, ejecute el código siguiente: 

```python
run.get_metrics()
```

## <a name="manage-models"></a>Administración de modelos 

Registre y realice un seguimiento de los modelos con el [registro de modelos de Azure Machine Learning](concept-model-management-and-deployment.md#register-package-and-deploy-models-from-anywhere), que admite el registro de modelos de MLflow. Los modelos de Azure Machine Learning se alinean con el esquema de modelos de MLflow, lo que facilita la exportación e importación de estos modelos en diferentes flujos de trabajo. Los metadatos relacionados con MLflow, como el id. de ejecución, también se etiquetan con el modelo registrado para la rastreabilidad. Los usuarios pueden enviar las ejecuciones de entrenamiento, registrar e implementar los modelos generados a partir de las ejecuciones de MLflow. 

Si quiere implementar y registrar el modelo preparado para producción en un solo paso, consulte [Implementación y registro de modelos de MLflow](how-to-deploy-mlflow-models.md).

Para registrar y ver un modelo a partir de una ejecución, siga estos pasos:

1. Una vez completada la ejecución, llame al método `register_model()`.

    ```python
    # the model folder produced from the run is registered. This includes the MLmodel file, model.pkl and the conda.yaml.
    run.register_model(model_name = 'my-model', model_path = 'model')
    ```

1. Vea el modelo registrado en su área de trabajo con [Azure Machine Learning Studio](overview-what-is-machine-learning-studio.md)

    En el ejemplo siguiente, el modelo registrado `my-model` tiene etiquetados metadatos de seguimiento de MLflow. 

    ![register-mlflow-model](./media/how-to-use-mlflow/registered-mlflow-model.png)

1. Seleccione la pestaña **Artefactos** para ver todos los archivos de modelo que se alinean con el esquema de modelos de MLflow (conda.yaml, MLmodel, model.pkl).

    ![model-schema](./media/how-to-use-mlflow/mlflow-model-schema.png)

1. Seleccione MLmodel para ver el archivo MLmodel generado por la ejecución.

    ![MLmodel-schema](./media/how-to-use-mlflow/mlmodel-view.png)


## <a name="clean-up-resources"></a>Limpieza de recursos

Si no tiene pensado usar los artefactos o las métricas registradas en el área de trabajo, la funcionalidad para eliminarlos de forma individual no está disponible actualmente. Por ello, deberá eliminar el grupo de recursos que contiene la cuenta de almacenamiento y el área de trabajo para no incurrir en cargos:

1. En Azure Portal, seleccione **Grupos de recursos** a la izquierda del todo.

   ![Eliminación en Azure Portal](./media/how-to-use-mlflow/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que creó.

1. Seleccione **Eliminar grupo de recursos**.

1. Escriba el nombre del grupo de recursos. A continuación, seleccione **Eliminar**.

## <a name="example-notebooks"></a>Cuadernos de ejemplo

En [MLflow con cuadernos de Azure ML](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/track-and-monitor-experiments/using-mlflow) se demuestran y se analizan con mayor profundidad los conceptos presentados en este artículo.

> [!NOTE]
> Puede encontrar un repositorio de ejemplos administrado por la comunidad mediante mlflow en https://github.com/Azure/azureml-examples.

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de modelos con MLflow](how-to-deploy-mlflow-models.md).
* Supervise los modelos de producción para el [desfase de datos](./how-to-enable-data-collection.md).
* [Seguimiento de las ejecuciones de Azure Databricks con MLflow](how-to-use-mlflow-azure-databricks.md).
* [Administra sus modelos](concept-model-management-and-deployment.md).