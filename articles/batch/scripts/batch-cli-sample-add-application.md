---
title: 'Ejemplo de script de la CLI de Azure: incorporación de una aplicación a Batch'
description: Este ejemplo de script demuestra cómo agregar una aplicación para usarla con un grupo o tarea de Azure Batch.
ms.topic: sample
ms.date: 01/29/2018
ms.custom: devx-track-azurecli
ms.openlocfilehash: 414a401168b1b378ed33f4904607243de0267d22
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "94565813"
---
# <a name="cli-example-add-an-application-to-an-azure-batch-account"></a>Ejemplo la CLI: incorporación de una aplicación en una cuenta de Azure Batch

Este script demuestra cómo agregar una aplicación para usarla con un grupo o tarea de Azure Batch. Para configurar una aplicación que se agregará a su cuenta de Batch, cree un archivo .zip que incluya el ejecutable y cualquier dependencia. 

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Este tutorial requiere la versión 2.0.20 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente. 

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Ejecute el siguiente comando para quitar el grupo de recursos y todos los recursos asociados a él.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos.
Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Crea una cuenta de almacenamiento. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Crea la cuenta de Batch. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Realiza la autenticación respecto de la cuenta de Batch especificada para una mayor interacción de la CLI.  |
| [az batch application create](/cli/azure/batch/application#az-batch-application-create) | Crea una aplicación.  |
| [az batch application package create](/cli/azure/batch/application/package#az-batch-application-package-create) | Agrega un paquete de aplicación a la aplicación especificada.  |
| [az batch application set](/cli/azure/batch/application#az-batch-application-set) | Actualiza las propiedades de una aplicación.  |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).
