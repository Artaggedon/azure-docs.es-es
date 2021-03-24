---
title: Eliminación del controlador de datos de Azure Arc
description: Eliminación del controlador de datos de Azure Arc
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: a040200c5746defcaee84a951521d5919c0c4d28
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91660683"
---
# <a name="delete-azure-arc-data-controller"></a>Eliminación del controlador de datos de Azure Arc

En el siguiente artículo se describe cómo eliminar un controlador de datos de Azure Arc.

Antes de continuar, asegúrese de que todos los servicios de datos que se han creado en el controlador de datos se quitan de la siguiente manera:

## <a name="log-in-to-the-data-controller"></a>Inicio de sesión en el controlador de datos

Inicie sesión en el controlador de datos que quiere eliminar:

```
azdata login
```

## <a name="list--delete-existing-data-services"></a>Enumeración y eliminación de los servicios de datos existentes

Ejecute el comando siguiente para comprobar si hay alguna instancia administrada de SQL creada:

```
azdata arc sql mi list
```

Para cada instancia administrada de SQL de la lista anterior, ejecute el comando delete de la siguiente manera:

```
azdata arc sql mi delete -n <name>
# for example: azdata arc sql mi delete -n sqlinstance1
```

Del mismo modo, para comprobar las instancias de Hiperescala de PostgreSQL, ejecute lo siguiente:

```
azdata arc postgres server list
```

Y para cada instancia de Hiperescala de PostgreSQL, ejecute el comando delete de la siguiente manera:
```
azdata arc postgres server delete -n <name>
# for example: azdata arc postgres server delete -n pg1
```

## <a name="delete-controller"></a>Eliminación del controlador

Una vez eliminadas todas las instancias administradas de SQL y las de Hiperescala de PostgreSQL, el controlador de datos se puede eliminar de la siguiente manera:

```
azdata arc dc delete -n <name> -ns <namespace>
# for example: azdata arc dc delete -ns arc -n arcdc
```

### <a name="remove-sccs-red-hat-openshift-only"></a>Quite los SCC (solo Red Hat OpenShift).

```console
oc adm policy remove-scc-from-user privileged -z default -n arc
oc adm policy remove-scc-from-user anyuid     -z default -n arc
```

### <a name="delete-cluster-level-objects"></a>Eliminación de objetos de nivel de clúster

Además de los objetos con ámbito de espacio de nombres, si también desea eliminar objetos de nivel de clúster como los CRD (`clusterroles` y `clusterrolebindings`), ejecute los comandos siguientes:

```console
# Cleanup azure arc data service artifacts
#Delete CRDs
kubectl delete crd datacontrollers.arcdata.microsoft.com 
kubectl delete crd sqlmanagedinstances.sql.arcdata.microsoft.com 
kubectl delete crd postgresql-11s.arcdata.microsoft.com 
kubectl delete crd postgresql-12s.arcdata.microsoft.com
#Delete clusterrole
kubectl delete clusterrole <namespace>:cr-arc-metricsdc-reader
#For example: kubectl delete clusterrole arc:cr-arc-metricsdc-reader
#Delete rolebinding
kubectl delete clusterrolebinding <namespace>:crb-arc-metricsdc-reader
#For example: kubectl delete clusterrolebinding arc:crb-arc-metricsdc-reader
```

### <a name="optionally-delete-the-azure-arc-data-controller-namespace"></a>Opcionalmente, elimine el espacio de nombres del controlador de datos de Azure Arc.


```console
kubectl delete ns <nameSpecifiedDuringCreation>
# for example: kubectl delete ns arc
```

## <a name="next-steps"></a>Pasos siguientes

[¿Qué son los servicios de datos habilitados para Azure Arc?](overview.md)
