---
title: Ejemplo de implementación de script de la CLI de Azure Service Fabric (sfctl)
description: Implementación de una aplicación en un clúster de Azure Service Fabric mediante la CLI de Azure Service Fabric
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 04/16/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: ff40dc62b4dcd622156a78518bbdcb6b9b430644
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "75526607"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster-using-the-service-fabric-cli"></a>Implementación de una aplicación en un clúster de Service Fabric mediante la CLI de Service Fabric

Este script de ejemplo copia un paquete de aplicación en un almacén de imágenes de clúster, registra el tipo de aplicación en el clúster y crea una instancia de la aplicación a partir del tipo de aplicación. En este momento también se crea cualquier servicio predeterminado.

Instale la [CLI de Service Fabric](../service-fabric-cli.md) si es necesario.

## <a name="sample-script"></a>Script de ejemplo

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Cuando termine, se puede usar el script [remove](cli-remove-application.md) para quitar la aplicación. El script remove elimina la instancia de aplicación, anula el registro del tipo de aplicación y elimina el paquete de aplicación del almacén de imágenes.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Service Fabric](../service-fabric-cli.md).

Puede encontrar ejemplos adicionales de la CLI de Service Fabric para Azure Service Fabric en los [ejemplos de la CLI de Service Fabric](../samples-cli.md).
