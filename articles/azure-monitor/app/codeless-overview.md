---
title: 'Supervisión de las aplicaciones sin cambios de código: instrumentación automática para Azure Monitor Application Insights | Microsoft Docs'
description: 'Información general de la instrumentación automática para Azure Monitor Application Insights: administración del rendimiento de aplicaciones sin código'
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 05/31/2020
ms.reviewer: mbullwin
ms.openlocfilehash: efa951ce5a15460e3eacfd4c7abecfac17106b4e
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98880515"
---
# <a name="what-is-auto-instrumentation-or-codeless-attach---azure-monitor-application-insights"></a>¿Qué es la instrumentación automática o la conexión sin código de Azure Monitor Application Insights?

La instrumentación automática, o conexión sin código, permite habilitar la supervisión de las aplicaciones con Application Insights sin modificar el código.  

Application Insights se integra con varios proveedores de recursos y funciona en distintos entornos. En resumen, solo debe habilitar y, en algunos casos, configurar el agente, que estará listo para recopilar la telemetría. En unos instantes, podrá ver las métricas, los datos y las dependencias en el recurso de Application Insights, lo que le permitirá detectar el origen de los posibles problemas antes de que se produzcan y analizar la causa raíz a través de la vista de transacciones integral.

## <a name="supported-environments-languages-and-resource-providers"></a>Entornos, lenguajes y proveedores de recursos admitidos

A medida que agregamos más integraciones, la matriz de funcionalidades de instrumentación automática se vuelve compleja. En la tabla siguiente se muestra el estado actual de la situación en lo que se refiere a la compatibilidad con varios proveedores de recursos, lenguajes y entornos.

|Entorno/proveedor de recursos          | .NET            | .NET Core       | Java            | Node.js         | Python          |
|---------------------------------------|-----------------|-----------------|-----------------|-----------------|-----------------|
|Azure App Service en Windows           | Disponibilidad general, OnBD*       | Disponibilidad general, opcional      | Vista previa privada | Vista previa privada | No compatible   |
|Azure App Service en Linux             | N/D             | No compatible   | Vista previa privada | Vista previa pública  | No compatible   |
|Azure App Service en AKS               | N/D             | En diseño       | En diseño       | En diseño       | No compatible   |
|Azure Functions: básico                | Disponibilidad general, OnBD*       | Disponibilidad general, OnBD*       | Disponibilidad general, OnBD*       | Disponibilidad general, OnBD*       | Disponibilidad general, OnBD*       |
|Azure Functions en Windows: dependencias | No compatible   | No compatible   | Vista previa pública  | No compatible   | No compatible   |
|Azure Kubernetes Service               | N/D             | En diseño       | Mediante el agente   | En diseño       | No compatible   |
|VM de Azure con Windows                      | Vista previa pública  | No compatible   | No compatible   | No compatible   | No compatible   |
|VM locales con Windows                | Disponibilidad general, opcional      | No compatible   | Mediante el agente   | No compatible   | No compatible   |
|Agente independiente: cualquier ent.            | No compatible   | No compatible   | Disponibilidad general              | No compatible   | No compatible   |

*OnBD significa "Activado de forma predeterminada"; Application Insights se habilitará automáticamente una vez que implemente la aplicación en los entornos compatibles. 

## <a name="azure-app-service"></a>Azure App Service

### <a name="windows"></a>Windows

La [supervisión de aplicaciones en Azure App Service](./azure-web-apps.md?tabs=net) está disponible para la aplicación .NET y está habilitada de forma predeterminada, .NET Core se puede habilitar con un solo clic, y Java y Node.js están en versión preliminar privada.

### <a name="linux"></a>Linux 

La supervisión de las aplicaciones Java y Node.js en App Service se encuentra en versión preliminar pública y se puede habilitar en Azure Portal, disponible en todas las regiones.

## <a name="azure-functions"></a>Azure Functions

La supervisión básica de Azure Functions está habilitada de forma predeterminada para recopilar el registro, el rendimiento, los datos de error y las solicitudes HTTP. En el caso de las aplicaciones Java, puede habilitar una supervisión más completa con seguimiento distribuido y obtener los detalles de la transacción de un extremo a otro. Esta funcionalidad de Java está en versión preliminar pública y puede [habilitarla en Azure Portal](./monitor-functions.md).

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

La instrumentación sin código de Azure Kubernetes Service está disponible actualmente para aplicaciones Java a través del [agente independiente](./java-in-process-agent.md). 

## <a name="azure-windows-vms-and-virtual-machine-scale-set"></a>Conjunto de escalado de máquinas virtuales y máquinas virtuales de Azure con Windows

La [instrumentación automática para máquinas virtuales de Azure y el conjunto de escalado de máquinas virtuales](./azure-vm-vmss-apps.md) está disponible para las aplicaciones .NET 

## <a name="on-premises-servers"></a>Servidores locales
Puede habilitar fácilmente la supervisión de sus [servidores de Windows locales para las aplicaciones .NET](./status-monitor-v2-overview.md) y para las [aplicaciones Java](./java-in-process-agent.md).

## <a name="other-environments"></a>Otros entornos
El versátil agente independiente de Java funciona en cualquier entorno, por lo que no es necesario instrumentar el código. [Siga esta guía](./java-in-process-agent.md) para habilitar Application Insights y obtener información sobre las increíbles funcionalidades del agente de Java. El agente se encuentra en versión preliminar pública y está disponible en todas las regiones. 

## <a name="next-steps"></a>Pasos siguientes

* [Información general de Application Insights](./app-insights-overview.md)
* [Mapa de aplicación](./app-map.md)
* [Supervisión del rendimiento de un extremo a otro](../learn/tutorial-performance.md)

