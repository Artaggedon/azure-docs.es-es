---
title: 'Tutorial: Limpieza de recursos de Azure Service Fabric Mesh'
description: Aprenda a quitar recursos de Azure Service Fabric Mesh para que no se le cobre por los recursos que ya no usa.
author: georgewallace
ms.topic: tutorial
ms.date: 09/18/2018
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: 4d78e1e27d70b147bb52dff13675e63b79335d62
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99625471"
---
# <a name="tutorial-remove-azure-resources"></a>Tutorial: Eliminación de recursos de Azure

> [!IMPORTANT]
> Se ha retirado la versión preliminar de Azure Service Fabric Mesh. Ya no se permitirán nuevas implementaciones a través de la API de Service Fabric Mesh. La compatibilidad con las implementaciones existentes continuará hasta el 28 de abril de 2021.
> 
> Para más información, consulte este artículo sobre la [retirada de la versión preliminar de Azure Service Fabric Mesh](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/).

Este tutorial es la quinta parte de una serie y muestra cómo eliminar la aplicación y sus recursos para que no se le cobre por ellos.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Limpiar los recursos utilizados por la aplicación para que no se le cobre por ellos.

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Crear una aplicación de Service Fabric Mesh en Visual Studio](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Depurar una aplicación de Service Fabric Mesh que se ejecuta en el clúster de desarrollo local](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Implementar una aplicación de Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * [Actualizar una aplicación de Service Fabric Mesh](service-fabric-mesh-tutorial-upgrade.md)
> * Limpiar los recursos de Service Fabric Mesh

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar este tutorial:

* Si no ha implementado la aplicación de tareas pendientes, siga las instrucciones de [Publicación de una aplicación web de Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Éste es el final del tutorial. Cuando haya terminado con los recursos que ha creado, elimínelos para que no se le cobre por recursos que ya no usa. Esto es especialmente importante ya que Mesh es un servicio sin servidor que se factura por segundo. Para más información sobre los precios de Mesh, visite https://aka.ms/sfmeshpricing.

Una de las ventajas que proporciona Azure es que al crear recursos que están asociados a un grupo de recursos concreto, al eliminar el grupo de recursos se eliminan todos los recursos asociados. De este modo no tiene que eliminarlos uno por uno.

Como ha creado un nuevo grupo de recursos para crear la aplicación de tareas pendientes, puede eliminar este grupo de recursos con seguridad, lo que eliminará todos los recursos asociados.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

También puede eliminar el grupo de recursos **sfmeshTutorial1RG**[desde el portal](../azure-resource-manager/management/manage-resource-groups-portal.md#delete-resource-groups). 

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha completado la publicación de una aplicación de Service Fabric Mesh en Azure, pruebe lo siguiente:

* Explore el [ejemplo de aplicación de votación](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) para ver otro ejemplo de comunicación entre servicios.
* Para obtener más información sobre el modelo de recursos de Service Fabric, consulte el [modelo de recursos de Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
* Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).
* Lea sobre [Cloud Shell](../cloud-shell/overview.md).
