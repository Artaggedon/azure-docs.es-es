---
title: Solución de problemas comunes de Kubernetes habilitado para Azure Arc (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Solución de problemas comunes con los clústeres de Kubernetes habilitado para Arc
keywords: Kubernetes, Arc, Azure, containers
ms.openlocfilehash: 0827386eb6ec089cf7951e8fa513a77fc78aef22
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98684096"
---
# <a name="azure-arc-enabled-kubernetes-troubleshooting-preview"></a>Solución de problemas de Kubernetes habilitado para Azure Arc (versión preliminar)

En este documento se proporcionan algunos escenarios de solución de problemas comunes de conectividad, permisos y agentes.

## <a name="general-troubleshooting"></a>Solución general de problemas

### <a name="azure-cli-set-up"></a>Configuración de la CLI de Azure
Antes de usar los comandos az connectedk8s o az k8sconfiguration de la CLI, asegúrese de que az está configurado para funcionar con la suscripción de Azure correcta.

```azurecli
az account set --subscription 'subscriptionId'
az account show
```

### <a name="azure-arc-agents"></a>Agentes de Azure Arc
Todos los agentes de Kubernetes habilitado para Azure Arc se implementan como pods en el espacio de nombres `azure-arc`. En operaciones normales, todos los pods deben ejecutarse y pasar sus comprobaciones de estado.

En primer lugar, compruebe la versión de Helm para Azure Arc:

```console
$ helm --namespace default status azure-arc
NAME: azure-arc
LAST DEPLOYED: Fri Apr  3 11:13:10 2020
NAMESPACE: default
STATUS: deployed
REVISION: 5
TEST SUITE: None
```

Si no se encuentra la versión de Helm o no existe, intente volver a incorporar el clúster.

Si la versión de Helm existe y `STATUS: deployed` determina el estado de los agentes mediante `kubectl`:

```console
$ kubectl -n azure-arc get deployments,pods
NAME                                       READY  UP-TO-DATE  AVAILABLE  AGE
deployment.apps/clusteridentityoperator     1/1       1          1       16h
deployment.apps/config-agent                1/1       1          1       16h
deployment.apps/cluster-metadata-operator   1/1       1          1       16h
deployment.apps/controller-manager          1/1       1          1       16h
deployment.apps/flux-logs-agent             1/1       1          1       16h
deployment.apps/metrics-agent               1/1       1          1       16h
deployment.apps/resource-sync-agent         1/1       1          1       16h

NAME                                            READY   STATUS  RESTART  AGE
pod/cluster-metadata-operator-7fb54d9986-g785b  2/2     Running  0       16h
pod/clusteridentityoperator-6d6678ffd4-tx8hr    3/3     Running  0       16h
pod/config-agent-544c4669f9-4th92               3/3     Running  0       16h
pod/controller-manager-fddf5c766-ftd96          3/3     Running  0       16h
pod/flux-logs-agent-7c489f57f4-mwqqv            2/2     Running  0       16h
pod/metrics-agent-58b765c8db-n5l7k              2/2     Running  0       16h
pod/resource-sync-agent-5cf85976c7-522p5        3/3     Running  0       16h
```

Todos los pods deben mostrar `STATUS` como `Running`, y `READY` debe ser `3/3` o `2/2`. Capture los registros y describa los pods que devuelven `Error` o `CrashLoopBackOff`. Si alguno de estos pods está atascado en el estado `Pending`, podría deberse a que no hay recursos suficientes en los nodos del clúster. [Escalar verticalmente el clúster](https://kubernetes.io/docs/tasks/administer-cluster/) hará que estos pods pasen al estado `Running`.

## <a name="connecting-kubernetes-clusters-to-azure-arc"></a>Conexión de clústeres de Kubernetes con Azure Arc

La conexión de clústeres a Azure requiere acceso a una suscripción de Azure y acceso `cluster-admin` a un clúster de destino. Si no se puede acceder al clúster o los permisos son insuficientes, se producirá un error en la incorporación.

### <a name="insufficient-cluster-permissions"></a>Permisos de clúster insuficientes

Si el archivo kubeconfig proporcionado no tiene permisos suficientes para instalar los agentes de Azure Arc, el comando de la CLI de Azure devolverá un error al intentar llamar a la API de Kubernetes.

```azurecli
$ az connectedk8s connect --resource-group AzureArc --name AzureArcCluster
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Ensure that you have the latest helm version installed before proceeding to avoid unexpected errors.
This operation might take a while...

Error: list: failed to list: secrets is forbidden: User "myuser" cannot list resource "secrets" in API group "" at the cluster scope
```

El propietario del clúster debe usar un usuario de Kubernetes con permisos de administrador de clústeres.

### <a name="installation-timeouts"></a>Tiempos de espera de instalación

La instalación del agente de Azure Arc requiere la ejecución de un conjunto de contenedores en el clúster de destino. Si el clúster se ejecuta a través de una conexión lenta a Internet, la extracción de la imagen de contenedor puede tardar más que los tiempos de espera de la CLI de Azure.

```azurecli
$ az connectedk8s connect --resource-group AzureArc --name AzureArcCluster
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Ensure that you have the latest helm version installed before proceeding to avoid unexpected errors.
This operation might take a while...
```

### <a name="helm-issue"></a>Problema de Helm

La versión de Helm `v3.3.0-rc.1` tiene un [problema](https://github.com/helm/helm/pull/8527) cuando la instalación/actualización de Helm (utilizada en segundo plano por la extensión de la CLI conectada) provoca la ejecución de todos los enlaces que conducen al siguiente error:

```console
$ az connectedk8s connect -n shasbakstest -g shasbakstest
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Ensure that you have the latest helm version installed before proceeding.
This operation might take a while...

Please check if the azure-arc namespace was deployed and run 'kubectl get pods -n azure-arc' to check if all the pods are in running state. A possible cause for pods stuck in pending state could be insufficientresources on the kubernetes cluster to onboard to arc.
ValidationError: Unable to install helm release: Error: customresourcedefinitions.apiextensions.k8s.io "connectedclusters.arc.azure.com" not found
```

Para resolver el problema, siga estos pasos:

1. Elimine el recurso de Kubernetes habilitado por Azure Arc que preocupa en Azure Portal.
2. Ejecute los comandos siguientes en la máquina:
    
    ```console
    kubectl delete ns azure-arc
    kubectl delete clusterrolebinding azure-arc-operator
    kubectl delete secret sh.helm.release.v1.azure-arc.v1
    ```

3. [Instale una versión estable](https://helm.sh/docs/intro/install/) de Helm 3 en el equipo en lugar de la versión candidata para lanzamiento.
4. Ejecute el comando `az connectedk8s connect` con los valores adecuados para conectar el clúster a Azure Arc.

## <a name="configuration-management"></a>Administración de configuración

### <a name="general"></a>General
Para ayudar a solucionar problemas con la configuración del control de código fuente, ejecute comandos az con el modificador --debug.

```console
az provider show -n Microsoft.KubernetesConfiguration --debug
az k8sconfiguration create <parameters> --debug
```

### <a name="create-source-control-configuration"></a>Creación de la configuración del control de código fuente
El rol de colaborador del recurso Microsoft.Kubernetes/connectedCluster es necesario y suficiente para crear el recurso Microsoft.KubernetesConfiguration/sourceControlConfiguration.

### <a name="configuration-remains-pending"></a>La configuración se mantiene como `Pending`

```console
kubectl -n azure-arc logs -l app.kubernetes.io/component=config-agent -c config-agent
$ k -n pending get gitconfigs.clusterconfig.azure.com  -o yaml
apiVersion: v1
items:
- apiVersion: clusterconfig.azure.com/v1beta1
  kind: GitConfig
  metadata:
    creationTimestamp: "2020-04-13T20:37:25Z"
    generation: 1
    name: pending
    namespace: pending
    resourceVersion: "10088301"
    selfLink: /apis/clusterconfig.azure.com/v1beta1/namespaces/pending/gitconfigs/pending
    uid: d9452407-ff53-4c02-9b5a-51d55e62f704
  spec:
    correlationId: ""
    deleteOperator: false
    enableHelmOperator: false
    giturl: git@github.com:slack/cluster-config.git
    helmOperatorProperties: null
    operatorClientLocation: azurearcfork8s.azurecr.io/arc-preview/fluxctl:0.1.3
    operatorInstanceName: pending
    operatorParams: '"--disable-registry-scanning"'
    operatorScope: cluster
    operatorType: flux
  status:
    configAppliedTime: "2020-04-13T20:38:43.081Z"
    isSyncedWithAzure: true
    lastPolledStatusTime: ""
    message: 'Error: {exit status 1} occurred while doing the operation : {Installing
      the operator} on the config'
    operatorPropertiesHashed: ""
    publicKey: ""
    retryCountPublicKey: 0
    status: Installing the operator
kind: List
metadata:
  resourceVersion: ""
  selfLink: ""
```
## <a name="monitoring"></a>Supervisión

Azure Monitor para contenedores requiere que su DaemonSet se ejecute en modo con privilegiado. Para configurar correctamente un clúster de Canonical Charmed Kubernetes para la supervisión, ejecute el siguiente comando:

```console
juju config kubernetes-worker allow-privileged=true
```