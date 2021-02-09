---
title: Incorporación de almacenamiento a una instancia de Azure HPC Cache
description: Definición de los destinos de almacenamiento para que Azure HPC Cache pueda usar el sistema NFS local o los contenedores de Azure Blob Storage para el almacenamiento de archivos a largo plazo
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 01/28/2021
ms.author: v-erkel
ms.openlocfilehash: b4df5863cc746490f13685a8d412232217af3bc8
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99054372"
---
# <a name="add-storage-targets"></a>Incorporación de destinos de almacenamiento

Los *destinos de almacenamiento* son espacios de almacenamiento en servidores de back-end para archivos a los que se accede mediante una instancia de Azure HPC Cache. Puede agregar almacenamiento NFS (como un sistema de hardware local) o almacenar datos en Azure Blob Storage.

Puede definir hasta diez destinos de almacenamiento diferentes para una caché. La caché presenta todos los destinos de almacenamiento en un espacio de nombres agregado.

Las rutas de acceso del espacio de nombres se configuran por separado después de agregar los destinos de almacenamiento. En general, un destino de almacenamiento de NFS puede tener hasta diez rutas de acceso de espacio de nombres, o más, para algunas configuraciones grandes. Lea [Rutas de acceso de espacio de nombres de NFS](add-namespace-paths.md#nfs-namespace-paths) para obtener más información.

Recuerde que las exportaciones de almacenamiento deben ser accesibles desde la red virtual de la caché. En el caso del almacenamiento en hardware local, es posible que tenga que configurar un servidor DNS que pueda resolver nombres de host para el acceso al almacenamiento de NFS. Obtenga más información en [Acceso DNS](hpc-cache-prerequisites.md#dns-access).

Agregue destinos de almacenamiento después de crear su caché. Siga este proceso:

1. [Creación de la caché](hpc-cache-create.md)
1. Definición de un destino de almacenamiento (información en este artículo)
1. [Cree las rutas de acceso orientadas al cliente](add-namespace-paths.md) (para el [espacio de nombres agregado](hpc-cache-namespace.md))

El procedimiento para agregar un destino de almacenamiento es ligeramente diferente en función de si agrega almacenamiento de Azure Blob Storage o una exportación de NFS. A continuación se muestran los detalles de cada uno.

Haga clic en la imagen siguiente para ver una [demostración en vídeo](https://azure.microsoft.com/resources/videos/set-up-hpc-cache/) sobre cómo crear una caché y agregar un destino de almacenamiento de Azure Portal.

[![Miniatura de vídeo: Azure HPC Cache: Instalación (haga clic para visitar la página del vídeo)](media/video-4-setup.png)](https://azure.microsoft.com/resources/videos/set-up-hpc-cache/)

## <a name="add-a-new-azure-blob-storage-target"></a>Incorporación de un nuevo destino de almacenamiento de Azure Blob Storage

Un nuevo destino de almacenamiento de Azure Blob Storage necesita un contenedor de blobs vacío o un contenedor rellenado con datos con el formato de sistema de archivos en la nube de Azure HPC Cache. Obtenga más información sobre la carga previa de un contenedor de blobs en [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md).

En la página **Agregar destino de almacenamiento** de Azure Portal se incluye la opción de crear un nuevo contenedor de blobs justo antes de agregar el destino.

### <a name="portal"></a>[Portal](#tab/azure-portal)

En Azure Portal, abra la instancia de caché y haga clic en **Destinos de almacenamiento** en la barra lateral izquierda.

![captura de pantalla de la página Configuración > Destino de almacenamiento, con dos destinos de almacenamiento en una tabla y el botón + Agregar destino de almacenamiento seleccionado encima de la tabla](media/add-storage-target-button.png)

En la página **Destinos de almacenamiento** se enumeran todos los destinos existentes y se proporciona un vínculo para agregar uno nuevo.

Haga clic en el botón **Agregar destino de almacenamiento**.

![captura de pantalla de la página de incorporación de destino de almacenamiento, rellenada con información para un nuevo destino de almacenamiento de Azure Blob Storage](media/hpc-cache-add-blob.png)

Para definir un contenedor de blobs de Azure, escriba esta información.

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.
* **Target type** (Tipo de destino): elija **Blob**.
* **Cuenta de almacenamiento**: seleccione la cuenta que quiere usar.

  Tendrá que autorizar a la instancia de caché para acceder a la cuenta de almacenamiento, tal como se describe en [Incorporación de los roles de acceso](#add-the-access-control-roles-to-your-account).

  Para más información sobre el tipo de cuenta de almacenamiento que puede usar, consulte [Requisitos de Blob Storage](hpc-cache-prerequisites.md#blob-storage-requirements).

* **Contenedor de almacenamiento**: seleccione el contenedor de blobs para este destino o haga clic en **Crear**.

  ![Captura de pantalla del cuadro de diálogo para especificar el nombre y el nivel de acceso (privado) del nuevo contenedor](media/add-blob-new-container.png)

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

> [!NOTE]
> Si el firewall de la cuenta de almacenamiento está configurado para restringir el acceso solo a "redes seleccionadas", use la solución alternativa documentada en [Solución alternativa para la configuración del firewall de la cuenta de Blob Storage](hpc-cache-blob-firewall-fix.md).

### <a name="add-the-access-control-roles-to-your-account"></a>Incorporación de los roles de control de acceso a la cuenta

Azure HPC Cache usa el [control de acceso basado en rol de Azure (Azure RBAC)](../role-based-access-control/index.yml) para autorizar el acceso del servicio de caché a la cuenta de almacenamiento de los destinos de Azure Blob Storage.

El propietario de la cuenta de almacenamiento debe agregar explícitamente los roles [Colaborador de la cuenta de almacenamiento](../role-based-access-control/built-in-roles.md#storage-account-contributor) y [Colaborador de datos de Storage Blob](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) para el usuario "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache).

Puede realizar esta tarea con anterioridad, o bien hacer clic en un vínculo de la página donde se agrega un destino de almacenamiento de Azure Blob Storage. Tenga en cuenta que la configuración de roles puede tardar hasta cinco minutos en propagarse a través del entorno de Azure, por lo que debe esperar unos minutos después de agregar los roles antes de crear un destino de almacenamiento.

Pasos para agregar los roles de Azure:

1. Abra la página **Control de acceso (IAM)** correspondiente a la cuenta de almacenamiento. El vínculo de la página **Add storage target** (Agregar destino de almacenamiento) abre automáticamente esta página para la cuenta seleccionada.

1. Haga clic en **+** en la parte superior de la página y elija **Agregar una asignación de roles**.

1. Seleccione el rol "Colaborador de la cuenta de almacenamiento" en la lista.

1. En el campo **Asignar acceso a**, deje el valor predeterminado seleccionado ("Usuario, grupo o entidad de servicio de Azure AD").  

1. En el campo **Seleccionar**, busque "hpc".  Esta cadena debe coincidir con una entidad de servicio, denominada "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache). Haga clic en ella para seleccionarla.

   > [!NOTE]
   > Si una búsqueda de "hpc" no funciona, pruebe a usar la cadena "storagecache" en su lugar. Es posible que los usuarios que han participado en las versiones preliminares (antes de la disponibilidad general) necesiten usar el nombre anterior de la entidad de servicio.

1. Haga clic en el botón **Guardar** en la parte inferior.

1. Repita este proceso para asignar el rol "Colaborador de datos de Storage Blob".  

![captura de pantalla de la GUI de Agregar asignación de roles](media/hpc-cache-add-role.png)

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

### <a name="prerequisite-storage-account-access"></a>Requisito previo: Acceso a las cuentas de almacenamiento

[Configuración de la CLI de Azure para Azure HPC Cache](./az-cli-prerequisites.md).

Antes de agregar un destino de almacenamiento de blobs, compruebe que la memoria caché tiene los roles correctos para acceder a la cuenta de almacenamiento y que la configuración del firewall permite la creación del destino de almacenamiento.

Azure HPC Cache usa el [control de acceso basado en rol de Azure (Azure RBAC)](../role-based-access-control/index.yml) para autorizar el acceso del servicio de caché a la cuenta de almacenamiento de los destinos de Azure Blob Storage.

El propietario de la cuenta de almacenamiento debe agregar explícitamente los roles [Colaborador de la cuenta de almacenamiento](../role-based-access-control/built-in-roles.md#storage-account-contributor) y [Colaborador de datos de Storage Blob](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) para el usuario "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache).

Se producirá un error en la creación del destino de almacenamiento si la memoria caché no tiene estos roles.

La configuración de roles puede tardar hasta cinco minutos en propagarse por el entorno de Azure, por lo que, después de agregar los roles, debe esperar para crear un destino de almacenamiento.

Lea [Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure](../role-based-access-control/role-assignments-cli.md) para instrucciones detalladas.

Compruebe también la configuración del firewall de la cuenta de almacenamiento. Si restringe el acceso solo a las "redes seleccionadas", puede producirse un error al crearse el destino de almacenamiento. Use la solución alternativa que se indica en el documento [Solución alternativa para la configuración del firewall de la cuenta de Blob Storage](hpc-cache-blob-firewall-fix.md).

### <a name="add-a-blob-storage-target-with-azure-cli"></a>Incorporación de un destino de almacenamiento de blobs con la CLI de Azure

Use la interfaz [az hpc-cache blob-storage-target add](/cli/azure/ext/hpc-cache/hpc-cache/blob-storage-target#ext-hpc-cache-az-hpc-cache-blob-storage-target-add) para definir un destino de almacenamiento en Azure Blob Storage.

> [!NOTE]
> Actualmente, los comandos de la CLI de Azure requieren la creación de una ruta de acceso de espacio de nombres al agregar un destino de almacenamiento. Esto es diferente del proceso que se usa con la interfaz de Azure Portal.

Además de los parámetros estándar del grupo de recursos y del nombre de la memoria caché, para el destino de almacenamiento debe proporcionar estas opciones:

* ``--name`` (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.

* ``--storage-account`` - Identificador de la cuenta, en este formato: /subscriptions/ *<id_de_suscripción>* /resourceGroups/ *<grupo_de_recursos_de_almacenamiento>* /providers/Microsoft.Storage/storageAccounts/ *<nombre_de_la_cuenta>*

  Para más información sobre el tipo de cuenta de almacenamiento que puede usar, consulte [Requisitos de Blob Storage](hpc-cache-prerequisites.md#blob-storage-requirements).

* ``--container-name`` -Especifique el nombre del contenedor que se va a usar para este destino de almacenamiento.

* ``--virtual-namespace-path`` (Ruta de acceso del espacio de nombres virtual): establezca la ruta de archivo orientada al cliente para este destino de almacenamiento. Escriba las rutas de acceso entre comillas. Lea [Planeamiento del espacio de nombres agregado](hpc-cache-namespace.md) para más información acerca de la característica de espacio de nombres virtual.

Comando de ejemplo:

```azurecli
az hpc-cache blob-storage-target add --resource-group "hpc-cache-group" \
    --cache-name "cache1" --name "blob-target1" \
    --storage-account "/subscriptions/<subscriptionID>/resourceGroups/myrgname/providers/Microsoft.Storage/storageAccounts/myaccountname" \
    --container-name "container1" --virtual-namespace-path "/blob1"
```

---

## <a name="add-a-new-nfs-storage-target"></a>Incorporación de un nuevo destino de almacenamiento NFS

Un destino de almacenamiento de NFS tiene una configuración diferente de un destino de almacenamiento de blobs. La configuración del modelo de uso ayuda a la caché a almacenar en caché eficazmente los datos de este sistema de almacenamiento.

![Captura de pantalla de la página para agregar destino de almacenamiento con destino NFS definido](media/add-nfs-target.png)

> [!NOTE]
> Antes de crear un destino de almacenamiento NFS, asegúrese de que el sistema de almacenamiento sea accesible desde la instancia de Azure HPC Cache y cumple los requisitos de permisos. Se producirá un error en la creación del destino de almacenamiento si la memoria caché no tiene acceso suficiente al sistema de almacenamiento. Consulte [Requisitos de almacenamiento NFS](hpc-cache-prerequisites.md#nfs-storage-requirements) y [Solución de problemas de configuración de NAS y problemas del destino de almacenamiento de NFS](troubleshoot-nas.md) para más información.

### <a name="choose-a-usage-model"></a>Selección de un modelo de uso
<!-- referenced from GUI - update aka.ms link if you change this heading -->

Cuando cree un destino de almacenamiento que apunte a un sistema de almacenamiento NFS, deberá elegir el modelo de uso de ese destino. Este modelo determina cómo se almacenan los datos en caché.

Los modelos de uso integrados permiten elegir cómo equilibrar la respuesta rápida con el riesgo de obtener datos obsoletos. Si desea optimizar la velocidad de lectura de los archivos, es posible que no le interese si los archivos de la memoria caché se comparan con los archivos de back-end. Por otro lado, si desea asegurarse de que los archivos estén siempre actualizados con el almacenamiento remoto, elija un modelo que realice comparaciones frecuentes.

Hay tres opciones:

* **Lectura de textos densos y poco frecuentes**: utilice esta opción si desea acelerar el acceso de lectura a archivos que son estáticos o que no se suelen modificar.

  Esta opción almacena en caché los archivos que los clientes leen, pero pasa las escrituras por el almacenamiento de back-end inmediatamente. Los archivos almacenados en la memoria caché no se comparan automáticamente con los archivos del volumen de almacenamiento NFS. (Para más información, lea la nota siguiente sobre la comprobación de back-end).

  No use esta opción si existe el riesgo de que un archivo se pueda modificar directamente en el sistema de almacenamiento sin escribirlo primero en la memoria caché. Si esto sucede, la versión en caché del archivo no estará sincronizada con el archivo de back-end.

* **Mayor que el 15 % de textos**: esta opción acelera el rendimiento de lectura y escritura. Al usar esta opción, todos los clientes deben acceder a los archivos mediante Azure HPC Cache en lugar de montar el almacenamiento de back-end directamente. Los archivos almacenados en caché tendrán cambios recientes que no se almacenan en el back-end.

  En este modelo de uso, los archivos de la caché solo se comparan con los archivos del almacenamiento de back-end cada ocho horas. Se supone que la versión en caché del archivo es más actual. Un archivo modificado en la memoria caché se escribe en el sistema de almacenamiento de back-end después de que esté en la memoria caché durante una hora sin cambios adicionales.

* **Los clientes escriben en el destino NFS omitiendo la caché**: elija esta opción si algún cliente del flujo de trabajo escribe datos directamente en el sistema de almacenamiento sin escribir primero en la caché o si quiere optimizar la coherencia de los datos. Los archivos que solicitan los clientes se almacenan en caché, pero los cambios que se realicen en esos archivos desde el cliente se devuelven al sistema de almacenamiento de back-end inmediatamente.

  Con este modelo de uso, los archivos de la memoria caché se comparan con frecuencia con las versiones de back-end para determinar si hay actualizaciones. Esta comprobación permite cambiar los archivos fuera de la memoria caché al tiempo que se preserva la coherencia de los datos.

En esta tabla se resumen las diferencias de los modelos de uso:

| Modelo de uso                   | Modo de almacenamiento en caché | Comprobación de back-end | Retraso máximo de reescritura |
|-------------------------------|--------------|-----------------------|--------------------------|
| Lectura de textos densos y poco frecuentes | Lectura         | Nunca                 | None                     |
| Mayor que el 15 % de textos       | Lectura/escritura   | 8 horas               | 1 hora                   |
| Los clientes omiten la memoria caché      | Lectura         | 30 segundos            | None                     |

> [!NOTE]
> El valor de la **Comprobación de back-end** muestra cuándo la caché compara automáticamente sus archivos con los archivos de origen que están en el almacenamiento remoto. Sin embargo, puede forzar que Azure HPC Cache compare los archivos mediante una operación de directorio que incluye una solicitud readdirplus. Readdirplus es una API NFS estándar (también llamada lectura extendida) que devuelve metadatos de directorio, lo que hace que la caché compare y actualice los archivos.

### <a name="create-an-nfs-storage-target"></a>Creación de un destino de almacenamiento de NFS

### <a name="portal"></a>[Portal](#tab/azure-portal)

En Azure Portal, abra la instancia de caché y haga clic en **Destinos de almacenamiento** en la barra lateral izquierda.

![captura de pantalla de la página Configuración > Destino de almacenamiento, con dos destinos de almacenamiento en una tabla y el botón + Agregar destino de almacenamiento seleccionado encima de la tabla](media/add-storage-target-button.png)

En la página **Destinos de almacenamiento** se enumeran todos los destinos existentes y se proporciona un vínculo para agregar uno nuevo.

Haga clic en el botón **Agregar destino de almacenamiento**.

![Captura de pantalla de la página para agregar destino de almacenamiento con destino NFS definido](media/add-nfs-target.png)

Proporcione esta información para un destino de almacenamiento respaldado por NFS:

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.

* **Target type** (Tipo de destino): elija **NFS**.

* **Hostname** (Nombre de host): escriba la dirección IP o el nombre de dominio completo del sistema de almacenamiento NFS. Use un nombre de dominio solo si la caché tiene acceso a un servidor DNS que puede resolverlo.

* **Modelo de uso**: elija uno de los perfiles de almacenamiento en caché de datos en función del flujo de trabajo, tal como se describe en la sección [Selección de un modelo de uso](#choose-a-usage-model) anterior.

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

[Configuración de la CLI de Azure para Azure HPC Cache](./az-cli-prerequisites.md).

Use el comando [az hpc-cache nfs-storage-target add](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target#ext-hpc-cache-az-hpc-cache-nfs-storage-target-add) de la CLI de Azure para crear el destino de almacenamiento.

> [!NOTE]
> Actualmente, los comandos de la CLI de Azure requieren la creación de una ruta de acceso de espacio de nombres al agregar un destino de almacenamiento. Esto es diferente del proceso que se usa con la interfaz de Azure Portal.

Proporcione estos valores además del nombre de la caché y el grupo de recursos de caché:

* ``--name`` (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.
* ``--nfs3-target``: dirección IP del sistema de almacenamiento NFS. (Puede usar un nombre de dominio completo aquí si la caché tiene acceso a un servidor DNS que lo resuelva).
* ``--nfs3-usage-model``: uno de los perfiles de almacenamiento en caché de datos, que se describe en la sección [Selección de un modelo de uso](#choose-a-usage-model) anterior.

  Compruebe los nombres de los modelos de uso con el comando [az hpc-cache usage-model list](/cli/azure/ext/hpc-cache/hpc-cache/usage-model#ext-hpc-cache-az-hpc-cache-usage-model-list).

* ``--junction``: parámetro de unión que vincula la ruta de acceso del archivo virtual orientado al cliente con una ruta de acceso de exportación del sistema de almacenamiento.

  Un destino de almacenamiento NFS puede tener varias rutas de acceso virtuales, siempre y cuando cada ruta represente una exportación o un subdirectorio diferente sen el mismo sistema de almacenamiento. Creación de todas las rutas de acceso de un sistema de almacenamiento en un destino de almacenamiento.

  Puede [agregar y editar rutas de acceso de espacio de nombres](add-namespace-paths.md) en un destino de almacenamiento en cualquier momento.

  El parámetro ``--junction`` usa estos valores:

  * ``namespace-path``: Ruta de acceso del archivo virtual orientado al cliente
  * ``nfs-export``: Exportación del sistema de almacenamiento que se va a asociar a la ruta de acceso orientada al cliente
  * ``target-path`` (opcional): Subdirectorio de la exportación, si es necesario

  Ejemplo: ``--junction namespace-path="/nas-1" nfs-export="/datadisk1" target-path="/test"``

  Lea [Configuración del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más información acerca de la característica de espacio de nombres virtual.

Comando de ejemplo:

```azurecli

az hpc-cache nfs-storage-target add --resource-group "hpc-cache-group" --cache-name "doc-cache0629" \
    --name nfsd1 --nfs3-target 10.0.0.4 --nfs3-usage-model WRITE_WORKLOAD_15 \
    --junction namespace-path="/nfs1/data1" nfs-export="/datadisk1" target-path=""
```

Salida:
```azurecli

{- Finished ..
  "clfs": null,
  "id": "/subscriptions/<subscriptionID>/resourceGroups/hpc-cache-group/providers/Microsoft.StorageCache/caches/doc-cache0629/storageTargets/nfsd1",
  "junctions": [
    {
      "namespacePath": "/nfs1/data1",
      "nfsExport": "/datadisk1",
      "targetPath": ""
    }
  ],
  "location": "eastus",
  "name": "nfsd1",
  "nfs3": {
    "target": "10.0.0.4",
    "usageModel": "WRITE_WORKLOAD_15"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "hpc-cache-group",
  "targetType": "nfs3",
  "type": "Microsoft.StorageCache/caches/storageTargets",
  "unknown": null
}

```

---

## <a name="view-storage-targets"></a>Visualización de los destinos de almacenamiento

Puede usar Azure Portal o la CLI de Azure para mostrar los destinos de almacenamiento ya definidos para la memoria caché.

### <a name="portal"></a>[Portal](#tab/azure-portal)

En Azure Portal, abra la instancia de caché y haga clic en **Destinos de almacenamiento**, que se encuentra debajo del título Configuración en la barra lateral izquierda. En la página Destinos de almacenamiento se enumeran todos los destinos y se proporciona un vínculo para agregar uno nuevo.

Haga clic en el nombre de un destino de almacenamiento para abrir su página de detalles.

Lea [Editar destinos de almacenamiento](hpc-cache-edit-storage.md) para obtener más información.

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

[Configuración de la CLI de Azure para Azure HPC Cache](./az-cli-prerequisites.md).

Use la opción [az hpc-cache storage-target list](/cli/azure/ext/hpc-cache/hpc-cache/storage-target#ext-hpc-cache-az-hpc-cache-storage-target-list) para mostrar los destinos de almacenamiento de la caché existentes. Proporcione el nombre de la caché y el grupo de recursos (a menos que lo haya establecido como global).

```azurecli
az hpc-cache storage-target list --resource-group "scgroup" --cache-name "sc1"
```

Use [az hpc-cache storage-target show](/cli/azure/ext/hpc-cache/hpc-cache/storage-target#ext-hpc-cache-az-hpc-cache-storage-target-list) para ver detalles sobre un destino de almacenamiento concreto (especifíquelo por su nombre).

Ejemplo:

```azurecli
$ az hpc-cache storage-target show --cache-name doc-cache0629 --name nfsd1

{
  "clfs": null,
  "id": "/subscriptions/<subscription_ID>/resourceGroups/scgroup/providers/Microsoft.StorageCache/caches/doc-cache0629/storageTargets/nfsd1",
  "junctions": [
    {
      "namespacePath": "/nfs1/data1",
      "nfsExport": "/datadisk1",
      "targetPath": ""
    }
  ],
  "location": "eastus",
  "name": "nfsd1",
  "nfs3": {
    "target": "10.0.0.4",
    "usageModel": "WRITE_WORKLOAD_15"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "scgroup",
  "targetType": "nfs3",
  "type": "Microsoft.StorageCache/caches/storageTargets",
  "unknown": null
}
$
```

---

## <a name="next-steps"></a>Pasos siguientes

Después de crear destinos de almacenamiento, continúe con estas tareas para preparar la caché para su uso:

* [Configuración del espacio de nombres agregado](add-namespace-paths.md)
* [Montaje de la instancia de Azure HPC Cache](hpc-cache-mount.md)
* [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md)

Si necesita actualizar cualquier configuración, puede [editar un destino de almacenamiento](hpc-cache-edit-storage.md).