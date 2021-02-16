---
title: Volumen de disco confiable de Service Fabric con Service Fabric Mesh
description: Aprenda a almacenar el estado en una aplicación de Azure Service Fabric Mesh montando el volumen de disco confiable de Service Fabric en el contenedor mediante la CLI de Azure.
author: ashishnegi
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: asnegi
ms.custom: mvc, devcenter, devx-track-azurecli
ms.openlocfilehash: ac65693f2513338695e07cd8a19acb13333e7281
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2021
ms.locfileid: "99625793"
---
# <a name="mount-highly-available-service-fabric-reliable-disk-based-volume-in-a-service-fabric-mesh-application"></a>Montaje de un volumen de disco confiable de Service Fabric en una aplicación de Service Fabric Mesh 

> [!IMPORTANT]
> Se ha retirado la versión preliminar de Azure Service Fabric Mesh. Ya no se permitirán nuevas implementaciones a través de la API de Service Fabric Mesh. La compatibilidad con las implementaciones existentes continuará hasta el 28 de abril de 2021.
> 
> Para obtener más información, consulte [Retirada de la versión preliminar de Azure Service Fabric Mesh](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/).

El método habitual para conservar el estado con aplicaciones de contenedor es usar un almacenamiento remoto como Azure File Storage o una base de datos como Azure Cosmos DB. Esto supone una importante latencia de red de lectura y escritura en el almacenamiento remoto.

En este artículo se muestra cómo almacenar el estado en discos confiables de Service Fabric de alta disponibilidad mediante el montaje de un volumen dentro del contenedor de una aplicación de Service Fabric Mesh.
El disco confiable de Service Fabric proporciona volúmenes para lecturas locales con escrituras replicadas en el clúster de Service Fabric para alta disponibilidad. Esto elimina las llamadas de red para las lecturas y reduce la latencia de red para operaciones de escritura. Si el contenedor se reinicia o se mueve a otro nodo, la nueva instancia de contenedor verá el mismo volumen como uno más antiguo. Por tanto, es eficaz y ofrece una alta disponibilidad.

En este ejemplo, la aplicación Counter tiene un servicio de ASP.NET Core con una página web que muestra el valor del contador en un explorador.

`counterService` lee regularmente un valor de contador de un archivo, lo incrementa y lo vuelve a escribir en el archivo. El archivo se almacena en una carpeta que está montada en el volumen respaldado por el disco confiable de Service Fabric.

## <a name="prerequisites"></a>Prerequisites

Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar esta tarea. Para usar la CLI de Azure con este artículo, asegúrese de que `az --version` devuelve al menos `azure-cli (2.0.43)`.  Instale (o actualice) el módulo de extensión de la CLI de Azure Service Fabric Mesh siguiendo estas [instrucciones](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure y configure la suscripción.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos en el que implementar la aplicación. El siguiente comando crea un grupo de recursos denominado `myResourceGroup` en una ubicación en la parte oriental de Estados Unidos. Si cambia el nombre del grupo de recursos en el siguiente comando, recuerde cambiarlo en todos los comandos que siguen.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="deploy-the-template"></a>Implementación de la plantilla

>[!NOTE]
> Desde el 2 de noviembre de 2020, [se aplican límites de frecuencia de descarga](https://docs.docker.com/docker-hub/download-rate-limit/) a las solicitudes anónimas y autenticadas a Docker Hub desde las cuentas del plan gratuito de Docker. Estos se aplican por la dirección IP. 
> 
> Esta plantilla utiliza imágenes públicas de Docker Hub. Tenga en cuenta que puede tener una limitación de frecuencia. Para obtener más información, consulte [Autenticación con Docker Hub](../container-registry/buffer-gate-public-content.md#authenticate-with-docker-hub).

El siguiente comando implementa una aplicación Linux mediante la [plantilla counter.sfreliablevolume.linux.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.linux.json). Para implementar una aplicación Windows, use la [plantilla counter.sfreliablevolume.windows.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/counter/counter.sfreliablevolume.windows.json). Tenga en cuenta que las imágenes de contenedor más grandes pueden tardar más en implementarse.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/counter/counter.sfreliablevolume.linux.json
```

También puede ver el estado de la implementación con el comando:

```azurecli-interactive
az deployment group show --name counter.sfreliablevolume.linux --resource-group myResourceGroup
```

Tenga en cuenta el nombre del recurso de puerta de enlace que tiene un tipo de recurso como `Microsoft.ServiceFabricMesh/gateways`. Esto se utilizará al obtener la dirección IP pública de la aplicación.

## <a name="open-the-application"></a>Abrir la aplicación

Una vez que la aplicación se implementa correctamente, puede obtener la dirección IP del recurso de puerta de enlace para la aplicación. Use el nombre de la puerta de enlace que haya observado en sección anterior.
```azurecli-interactive
az mesh gateway show --resource-group myResourceGroup --name counterGateway
```

La salida debe tener una propiedad `ipAddress` que es la dirección IP pública del punto de conexión de servicio. Ábrala desde un explorador. Mostrará una página web con el valor del contador que se actualiza cada segundo.

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Comprobar que la aplicación es capaz de usar el volumen

La aplicación crea un archivo denominado `counter.txt` en el volumen dentro de la carpeta `counter/counterService`. El contenido de este archivo es el valor del contador que se muestra en la página web.

## <a name="delete-the-resources"></a>Eliminación de los recursos

Elimine periódicamente los recursos que no use en Azure. Para eliminar los recursos relacionados con este ejemplo, elimine el grupo de recursos en el que se implementaron (con lo que se elimina todo el contenido asociado con él) con el siguiente comando:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte la aplicación de ejemplo de disco de volumen confiable de Service Fabric en [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Para obtener más información sobre el modelo de recursos de Service Fabric, consulte el [modelo de recursos de Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
- Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).