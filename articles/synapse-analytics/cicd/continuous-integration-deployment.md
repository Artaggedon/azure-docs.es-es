---
title: Integración y entrega continuas para las áreas de trabajo de Synapse
description: Aprenda a usar la integración y la entrega continuas para implementar los cambios en el área de trabajo de un entorno (desarrollo, prueba o producción) a otro.
services: synapse-analytics
author: liud
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: liud
ms.reviewer: pimorano
ms.openlocfilehash: de3738573bb9bb6f045a45d290c74ba9e6902a5e
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103561964"
---
# <a name="continuous-integration-and-delivery-for-azure-synapse-workspace"></a>Integración y entrega continuas para las áreas de trabajo de Azure Synapse

## <a name="overview"></a>Información general

La integración continua (CI) es el proceso de automatizar la compilación y las pruebas de código cada vez que un miembro del equipo confirma cambios en el control de versiones. La implementación continua (CD) es el proceso de compilar, probar, configurar e implementar desde varios entornos de pruebas o ensayo en un entorno de producción.

En las áreas de trabajo de Azure Synapse, la integración y la entrega continuas (CI/CD) mueven todas las entidades de un entorno (desarrollo, prueba, producción) a otro. Para promover el área de trabajo a otra área de trabajo, hay dos partes: use [plantillas de Azure Resource Manager](../../azure-resource-manager/templates/overview.md) para crear o actualizar los recursos del área de trabajo (grupos y área de trabajo); migrar artefactos (scripts de SQL, cuadernos, definiciones de trabajos de Spark, canalizaciones, conjuntos de datos, flujos de datos, etc.) con herramientas de CI/CD de Synapse en Azure DevOps. 

En este artículo se describe el uso de canalizaciones de versión de Azure para automatizar la implementación de un área de trabajo de Synapse en varios entornos.

## <a name="prerequisites"></a>Requisitos previos

-   El área de trabajo que se usa para el desarrollo se ha configurado con un repositorio de Git en Studio. Consulte [Control de código fuente en Synapse Studio](source-control.md).
-   Un proyecto de Azure DevOps se ha preparado para ejecutar la canalización de versión.

## <a name="set-up-a-release-pipelines"></a>Configuración una canalización de versión

1.  En [Azure DevOps](https://dev.azure.com/), abra el proyecto creado para la versión.

1.  En el lado izquierdo de la página, seleccione **Canalizaciones** y después **Versiones**.

    ![Selección de Canalizaciones, Versiones](media/create-release-1.png)

1.  Seleccione **Nueva canalización** o, si tiene canalizaciones existentes, seleccione **Nueva** y, luego **Nueva canalización de versión**.

1.  Seleccione la plantilla **Fase vacía**.

    ![Selección de Trabajo vacío](media/create-release-select-empty.png)

1.  En el cuadro **Nombre de la fase**, escriba el nombre del entorno.

1.  Seleccione **Agregar artefacto** y, luego, elija el mismo repositorio de Git configurado con la instancia de Synapse Studio de desarrollo. Seleccione el repositorio de Git que usó para administrar la plantilla de Resource Manager de grupos y áreas de trabajo. Si usa GitHub como origen, debe crear una conexión de servicio para la cuenta de GitHub y los repositorios de incorporación de cambios. Para más información, consulte [Conexiones de servicio](/azure/devops/pipelines/library/service-endpoints). 

    ![Adición de una rama de publicación](media/release-creation-github.png)

1.  Seleccione la rama de la plantilla de Resource Manager para la actualización de recursos. En **Versión predeterminada**, seleccione **Más reciente de la rama predeterminada**.

    ![Adición de una plantilla de Resource Manager](media/release-creation-arm-branch.png)

1.  Seleccione la [rama de publicación](source-control.md#configure-publishing-settings) del repositorio en **Rama predeterminada**. De forma predeterminada, esta rama de publicación es `workspace_publish`. En **Versión predeterminada**, seleccione **Más reciente de la rama predeterminada**.

    ![Agregar un artefacto](media/release-creation-publish-branch.png)

## <a name="set-up-a-stage-task-for-arm-resource-create-and-update"></a>Configuración de una tarea de fase para la creación y actualización de recursos de Azure Resource Manager 

Agregue una tarea de implementación de Azure Resource Manager para crear o actualizar recursos, incluidos los grupos y las áreas de trabajo:

1. En la vista de fase, seleccione **Ver tareas de la fase**.

    ![Vista de fase](media/release-creation-stage-view.png)

1. Cree una nueva tarea. Busque **Implementación de una plantilla de Resource Manager** y, después, seleccione **Agregar**.

1. En la tarea de implementación, seleccione la suscripción, el grupo de recursos y la ubicación del área de trabajo de destino. Proporcione las credenciales si es necesario.

1. En la lista **Acción**, seleccione **Create or update resource group** (Crear o actualizar grupo de recursos).

1. Seleccione el botón de puntos suspensivos ( **...** ) situado junto al cuadro **Plantilla**. Busque la plantilla de Azure Resource Manager de su área de trabajo de destino.

1. Seleccione **…** junto al cuadro **Parámetros de plantilla** para elegir el archivo de parámetros.

1. Seleccione **…** junto al cuadro **Reemplazar parámetros de plantilla** y escriba los valores de los parámetros deseados del área de trabajo de destino. 

1. Seleccione **Incremental** para el **Modo de implementación**.
    
    ![Implementación de áreas de trabajo y grupos](media/pools-resource-deploy.png)

1. (Opcional) Agregue **Azure PowerShell** para la asignación de roles de área de trabajo de concesión y actualización. Si usa la canalización de versión para crear un área de trabajo de Synapse, la entidad de servicio de la canalización se agrega como administrador del área de trabajo predeterminada. Puede ejecutar PowerShell para conceder a otras cuentas acceso al área de trabajo. 
    
    ![Concesión de permisos](media/release-creation-grant-permission.png)

 > [!WARNING]
> En el modo de implementación completa, se **eliminarán** aquellos recursos que existan en el grupo de recursos, pero no estén especificados en la nueva plantilla de Resource Manager. Para más información, consulte [Modos de implementación de Azure Resource Manager](../../azure-resource-manager/templates/deployment-modes.md).

## <a name="set-up-a-stage-task-for-artifacts-deployment"></a>Configuración de una tarea de fase para la implementación de artefactos 

Use la extensión [Synapse workspace deployment](https://marketplace.visualstudio.com/items?itemName=AzureSynapseWorkspace.synapsecicd-deploy) para implementar otros elementos del área de trabajo de Synapse, como conjuntos de datos, scripts de SQL, cuadernos, definiciones de trabajos de Spark, flujos de datos, canalizaciones, servicios vinculados, credenciales e instancias de IR (Integration Runtime).  

1. Busque y obtenga la extensión en el **marketplace de Azure DevOps**(https://marketplace.visualstudio.com/azuredevops) 

     ![Obtención de una extensión](media/get-extension-from-market.png)

1. Seleccione una organización para instalar la extensión. 

     ![Instalación de la extensión](media/install-extension.png)

1. Asegúrese de que se haya concedido a la entidad de servicio de la canalización de Azure DevOps el permiso de suscripción y de que esté asignada como administrador del área de trabajo de destino. 

1. Cree una nueva tarea. Busque **Synapse workspace deployment** y, luego, seleccione **Agregar**.

     ![Adición de extensiones](media/add-extension-task.png)

1.  En la tarea, seleccione **...** junto al cuadro **Plantilla** para elegir el archivo de plantilla.

1. Seleccione **…** junto al cuadro **Parámetros de plantilla** para elegir el archivo de parámetros.

1. Seleccione la conexión, el grupo de recursos y el nombre del área de trabajo de destino. 

1. Seleccione **…** junto al cuadro **Reemplazar parámetros de plantilla** y escriba los valores de los parámetros deseados del área de trabajo de destino. 

    ![Implementación del área de trabajo de Synapse](media/create-release-artifacts-deployment.png)

> [!IMPORTANT]
> En escenarios de CI/CD, el tipo de entorno de ejecución de integración (IR) en otros entornos debe ser el mismo. Por ejemplo, si tiene un IR autohospedado en el entorno de desarrollo, el mismo IR también debe ser de tipo autohospedado en otros entornos, como los de prueba y producción. Del mismo modo, si va a compartir los entornos de ejecución de integración entre varias fases, tendrá que configurarlos como autohospedados vinculados en todos los entornos: desarrollo, prueba y producción.

## <a name="create-release-for-deployment"></a>Creación de una versión para la implementación 

Después de guardar todos los cambios, puede seleccionar **Create release** (Crear versión) para crear manualmente una versión. Para automatizar la creación de versiones, consulte [Desencadenadores de versión de Azure DevOps](/azure/devops/pipelines/release/triggers).

   ![Selección de Crear versión](media/release-creation-manually.png)

## <a name="use-custom-parameters-of-the-workspace-template"></a>Uso de parámetros personalizados de la plantilla de área de trabajo 

Usa integración continua y entrega continua automatizadas y desea cambiar algunas propiedades durante la implementación, pero las propiedades no están parametrizadas de forma predeterminada. En ese caso, puede invalidar la plantilla de parámetros predeterminada.

Para ello, debe crear una plantilla de parámetros personalizada, un archivo denominado **template-parameters-definition.json**, en la carpeta raíz de la rama de colaboración de Git. Debe usar ese nombre de archivo exacto. Al realizar la publicación desde la rama de colaboración, el área de trabajo de Synapse leerá este archivo y usará su configuración para generar los parámetros. Si no se encuentra ningún archivo, se usa la plantilla de parámetros predeterminada.

### <a name="custom-parameter-syntax"></a>Sintaxis de los parámetros personalizados

A continuación, se indican algunas directrices para crear el archivo de parámetros personalizados:

* Escriba la ruta de acceso de la propiedad en el tipo de entidad correspondiente.
* El establecimiento de un nombre de propiedad en `*` indica que quiere parametrizar todas las propiedades que incluye (solo hasta el primer nivel, no de forma recursiva). También puede proporcionar excepciones a esta configuración.
* El establecimiento del valor de una propiedad como una cadena indica que quiere parametrizar la propiedad. Use el formato `<action>:<name>:<stype>`.
   *  `<action>` puede ser uno de estos caracteres:
      * `=` significa que el valor actual se debe conservar como el predeterminado para el parámetro.
      * `-` significa que no se debe conservar el valor predeterminado para el parámetro.
      * `|` es un caso especial para los secretos de Azure Key Vault para cadenas de conexión o claves.
   * `<name>` es el nombre del parámetro. Si está en blanco, toma el nombre de la propiedad. Si el valor empieza por un carácter `-`, el nombre está abreviado. Por ejemplo, `AzureStorage1_properties_typeProperties_connectionString` se abreviará a `AzureStorage1_connectionString`.
   * `<stype>` es el tipo del parámetro. Si `<stype>` está en blanco, el tipo predeterminado es `string`. Valores admitidos: `string`, `securestring`, `int`, `bool`, `object`, `secureobject` y `array`.
* Si se ha especificado una matriz en el archivo, significa que la propiedad coincidente de la plantilla es una matriz. Synapse itera todos los objetos de la matriz y usa la definición que se ha especificado. El segundo objeto, una cadena, se convierte en el nombre de la propiedad, que se utiliza como el nombre del parámetro para cada iteración.
* Una definición no puede ser específica de una instancia de recurso. Cualquier definición se aplica a todos los recursos de ese tipo.
* De forma predeterminada, todas las cadenas seguras, como los secretos de Key Vault, las cadenas de conexión, las claves y los tokens, están parametrizadas.

### <a name="parameter-template-definition-samples"></a>Ejemplos de definición de una plantilla de parámetros 

Este es un ejemplo del aspecto de la definición de una plantilla de parámetros:

```json
{
"Microsoft.Synapse/workspaces/notebooks": {
        "properties":{
            "bigDataPool":{
                "referenceName": "="
            }
        }
    },
    "Microsoft.Synapse/workspaces/sqlscripts": {
     "properties": {
         "content":{
             "currentConnection":{
                    "*":"-"
                 }
            } 
        }
    },
    "Microsoft.Synapse/workspaces/pipelines": {
        "properties": {
            "activities": [{
                 "typeProperties": {
                    "waitTimeInSeconds": "-::int",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.Synapse/workspaces/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.Synapse/workspaces/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:int",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.Synapse/workspaces/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                     "*": "="
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.Synapse/workspaces/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```
Esta es una explicación de cómo se construye la plantilla anterior, desglosada por tipo de recurso.

#### <a name="notebooks"></a>Cuaderno 

* Cualquier propiedad de la ruta de acceso `properties/bigDataPool/referenceName` se parametriza con su valor predeterminado. Puede parametrizar el grupo de Spark asociado en todos los archivos de cuaderno. 

#### <a name="sql-scripts"></a>Scripts de SQL 

* Las propiedades (poolName y databaseName) de la ruta de acceso `properties/content/currentConnection` se parametrizan como cadenas sin los valores predeterminados en la plantilla. 

#### <a name="pipelines"></a>Procesos

* Cualquier propiedad de la ruta de acceso `activities/typeProperties/waitTimeInSeconds` está parametrizada. Cualquier actividad en una canalización que tiene una propiedad de nivel de código denominada `waitTimeInSeconds` (por ejemplo, la actividad `Wait`) está parametrizada como un número, con un nombre predeterminado. Sin embargo, no tendrá un valor predeterminado en la plantilla de Resource Manager. Será una entrada obligatoria durante la implementación de Resource Manager.
* De forma similar, una propiedad denominada `headers` (por ejemplo, en una actividad `Web`) está parametrizada con el tipo `object` (Object). Tiene un valor predeterminado, que es el mismo que el de la factoría de origen.

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Todas las propiedades bajo la ruta de acceso `typeProperties` están parametrizadas con sus valores predeterminados correspondientes. Por ejemplo, hay dos propiedades en las propiedades de tipo `IntegrationRuntimes`: `computeProperties` y `ssisProperties`. Ambos tipos de propiedades se crean con sus valores predeterminados y tipos (objeto) respectivos.

#### <a name="triggers"></a>Desencadenadores

* En `typeProperties`, hay dos propiedades parametrizadas. La primera de ellas es `maxConcurrency`, que tiene especificado un valor predeterminado y es de tipo `string`. Tiene el nombre de parámetro predeterminado `<entityName>_properties_typeProperties_maxConcurrency`.
* La propiedad `recurrence` también está parametrizada. En ella, se especifica que todas las propiedades de ese nivel están parametrizadas como cadenas, con valores predeterminados y los nombres de parámetro. Una excepción es la propiedad `interval`, que está parametrizada como el tipo `int`. El sufijo del nombre del parámetro es `<entityName>_properties_typeProperties_recurrence_triggerSuffix`. De forma similar, la propiedad `freq` es una cadena y está parametrizada como una cadena. Sin embargo, la propiedad `freq` está parametrizada sin un valor predeterminado. El nombre está abreviado y con un sufijo. Por ejemplo, `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Los servicios vinculados son únicos. Dado que los servicios vinculados y los conjuntos de datos tienen una gran variedad de tipos, puede proporcionar una personalización específica de tipo. En este ejemplo, para todos los servicios vinculados de tipo `AzureDataLakeStore`, se aplicará una plantilla específica. Para todos los demás (a través de `*`), se aplicará otra plantilla.
* La propiedad `connectionString` se parametrizará como un valor `securestring`. No tendrá un valor predeterminado. Tendrá un nombre de parámetro abreviado con el sufijo `connectionString`.
* La propiedad `secretAccessKey` resulta ser `AzureKeyVaultSecret` (por ejemplo, en un servicio vinculado de Amazon S3). Se parametriza automáticamente como un secreto de Azure Key Vault y se captura desde el almacén de claves configurado. También puede parametrizar el propio almacén de claves.

#### <a name="datasets"></a>Conjuntos de datos

* Aunque la personalización específica de tipos está disponible para conjuntos de datos, puede proporcionar configuración sin necesidad de una configuración explícita de nivel \*. En el ejemplo anterior, todas las propiedades del conjunto de datos de `typeProperties` están parametrizadas.


## <a name="best-practices-for-cicd"></a>Procedimientos recomendados para CI/CD

Si usa la integración de Git con el área de trabajo de Synapse y tiene una canalización de CI/CD que mueve los cambios del entorno de desarrollo al de prueba y, luego, al de producción, los procedimientos recomendados son los siguientes:

-   **Integración de Git**. Configure solo el área de trabajo de desarrollo de Synapse con la integración de Git. Los cambios en las áreas de trabajo de prueba y producción se implementan a través de CI/CD y no se necesita la integración de Git.
-   **Prepare los grupos antes de la migración de los artefactos**. Si tiene un script de SQL o un cuaderno asociados a grupos del área de trabajo de desarrollo, se espera el mismo nombre de los grupos en distintos entornos. 
-   **Infraestructura como código (IaC)** . Administración de la infraestructura (redes, máquinas virtuales, equilibradores de carga y topología de conexión) en un modelo descriptivo, donde se usa el mismo control de versiones que utiliza el equipo de DevOps para el código fuente. 
-   **Otros**. Consulte [Procedimientos recomendados para artefactos de ADF](../../data-factory/continuous-integration-deployment.md#best-practices-for-cicd).

## <a name="troubleshooting-artifacts-deployment"></a>Solución de problemas en la implementación de artefactos 

### <a name="use-the-synapse-workspace-deployment-task"></a>Uso de la tarea de implementación del área de trabajo de Synapse

En Synapse, hay varios artefactos que no son recursos de ARM. Esto difiere con respecto a Azure Data Factory. La tarea de implementación de plantilla de ARM no funcionará correctamente para implementar artefactos de Synapse.
 
### <a name="unexpected-token-error-in-release"></a>Error de token inesperado en la versión

Cuando el archivo de parámetros tiene valores de parámetro que no son de escape, la canalización de versión no podrá analizar el archivo y generará el error "token inesperado". Se recomienda invalidar los parámetros o usar Azure KeyVault para recuperar los valores de parámetro. Como alternativa, también puede usar caracteres de escape dobles.
