---
title: P+F sobre los problemas de implementación con Microsoft Azure Cloud Services | Microsoft Docs
description: En este artículo se enumeran las preguntas frecuentes sobre la implementación con Microsoft Azure Cloud Services.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: dd7b19a2c9e872b811c1aab6e504accb7de383b2
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98896484"
---
# <a name="deployment-issues-for-azure-cloud-services-classic-frequently-asked-questions-faqs"></a>Problemas de implementación de Azure Cloud Services (clásico): Preguntas más frecuentes (P+F)

> [!IMPORTANT]
> [Azure Cloud Services (soporte extendido)](../cloud-services-extended-support/overview.md) es un nuevo modelo de implementación basado en Azure Resource Manager para el producto Azure Cloud Services. Con este cambio, se ha modificado el nombre del modelo de implementación basado en Azure Cloud Services para Azure Service Manager a Cloud Services (clásico), y todas las implementaciones nuevas deben usar [Cloud Services (soporte extendido)](../cloud-services-extended-support/overview.md).
En este artículo se incluyen preguntas frecuentes sobre problemas de implementación con [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). También puede consultar la [página Tamaños de Cloud Services](cloud-services-sizes-specs.md) para obtener información de tamaño.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-does-deploying-a-cloud-service-to-the-staging-slot-sometimes-fail-with-a-resource-allocation-error-if-there-is-already-an-existing-deployment-in-the-production-slot"></a>¿Por qué a veces se produce un error de asignación de recursos al implementar un servicio en la nube en la ranura de ensayo si ya hay una implementación existente en la ranura de producción?
Si un servicio en la nube tiene una implementación en cualquier ranura, todo el servicio en la nube se ancla a un clúster concreto. Esto significa que si una implementación ya existe en la ranura de producción, una nueva implementación de ensayo solo se puede asignar en el mismo clúster que la ranura de producción.

La asignación produce un error cuando el clúster del servicio en la nube no tiene los recursos de proceso físicos suficientes para la solicitud de implementación.

Como ayuda para mitigar estos errores de asignación, consulte [Error de asignación de servicio en la nube: Soluciones](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-scaling-up-or-scaling-out-a-cloud-service-deployment-sometimes-result-in-allocation-failure"></a>¿Por qué produce a veces un error de asignación el escalado vertical u horizontal de la implementación de un servicio en la nube?
Cuando se implementa un servicio en la nube, suele anclarse a un clúster concreto. Esto significa que el escalado vertical u horizontal de un servicio en la nube existente debe asignar instancias nuevas en el mismo clúster. Si el clúster está casi lleno o el tamaño o tipo de máquina virtual deseado no está disponible, se puede producir un error en la solicitud.

Como ayuda para mitigar estos errores de asignación, consulte [Error de asignación de servicio en la nube: Soluciones](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-deploying-a-cloud-service-into-an-affinity-group-sometimes-result-in-allocation-failure"></a>¿Por qué implementar un servicio en la nube en un grupo de afinidad a veces produce un error de asignación?
Una nueva implementación en un servicio en la nube vacío se puede asignar mediante el tejido en cualquier clúster de esa región, a menos que el servicio en la nube esté anclado a un grupo de afinidad. Las implementaciones en el mismo grupo de afinidad se intentarán en el mismo clúster. Si la capacidad del clúster se está agotando, se puede producir un error en la solicitud.

Como ayuda para mitigar estos errores de asignación, consulte [Error de asignación de servicio en la nube: Soluciones](cloud-services-allocation-failures.md#solutions).

## <a name="why-does-changing-vm-size-or-adding-a-new-vm-to-an-existing-cloud-service-sometimes-result-in-allocation-failure"></a>¿Por qué cambiar el tamaño de máquina virtual o agregar una nueva a un servicio en la nube existente produce a veces un error de asignación?
Los clústeres de un centro de datos pueden tener configuraciones de tipos de máquina diferentes (por ejemplo, serie A, serie Av2, serie D, serie Dv2, serie G, serie H, etc.). Pero no todos los clústeres tienen necesariamente todos los tipos de máquina virtual. Por ejemplo, si intenta agregar una máquina virtual serie D a un servicio en la nube que ya se ha implementado en un clúster de serie A únicamente, se producirá un error de asignación. Esto también sucede si se intenta cambiar los tamaños de SKU de máquina virtual (por ejemplo, de una serie A a una serie D).

Como ayuda para mitigar estos errores de asignación, consulte [Error de asignación de servicio en la nube: Soluciones](cloud-services-allocation-failures.md#solutions).

Para comprobar los tamaños disponibles en su región, consulte [Microsoft Azure: Productos disponibles por región](https://azure.microsoft.com/regions/services).

## <a name="why-does-deploying-a-cloud-service-sometime-fail-due-to-limitsquotasconstraints-on-my-subscription-or-service"></a>¿Por qué implementar un servicio en la nube en ocasiones produce errores por límites, cuotas o restricciones en la suscripción o el servicio?
La implementación de un servicio en la nube puede producir un error si los recursos que deben asignarse superan la cuota máxima o permitida o predeterminada para su servicio en el nivel de región/centro de datos. Para más información, consulte [Límites de Cloud Services](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-cloud-services-limits).

También puede realizar el seguimiento del uso o la cuota actual de la suscripción en el portal: Azure Portal = > Suscripciones = > \<appropriate subscription> = > "Uso + cuota".

También se puede recuperar la información de consumo y del uso de recursos a través de las API de facturación de Azure. Vea [Información general sobre las API de consumo de Azure](../cost-management-billing/manage/consumption-api-overview.md).

## <a name="how-can-i-change-the-size-of-a-deployed-cloud-service-vm-without-redeploying-it"></a>¿Cómo se cambia el tamaño de una máquina virtual de servicio en la nube sin volver a implementarlo?
No se puede cambiar el tamaño de una máquina virtual de servicio en la nube implementado sin volver a implementarlo. El tamaño de la máquina virtual está integrado en el CSDEF, que solo se actualiza con una nueva implementación.

Para más información, consulte [Actualización de un servicio en la nube](cloud-services-update-azure-service.md).

## <a name="why-am-i-not-able-to-deploy-cloud-services-through-service-management-apis-or-powershell-when-using-azure-resource-manager-storage-account"></a>¿Por qué no puedo implementar Cloud Services mediante las API de Service Management o PowerShell cuando se usa la cuenta de Storage de Azure Resource Manager? 

Dado que el servicio en la nube es un recurso clásico que no es directamente compatible con el modelo de Azure Resource Manager, no es posible asociarlo con las cuentas de Storage de Azure Resource Manager. Estas son algunas opciones: 

- Implementación a través de la API de REST.

    Al realizar la implementación a través de la API de REST de Service Management, podría sortear la limitación mediante la especificación de una dirección URL de SAS a Blob Storage, que funcionará con la cuenta de Storage del modelo clásico y de Azure Resource Manager. Lea más acerca de la propiedad "PackageUrl" [aquí](/previous-versions/azure/reference/ee460813(v=azure.100)).

- Implementación a través de [Azure Portal](https://portal.azure.com).

    Funcionará desde [Azure Portal](https://portal.azure.com) dado que la llamada pasa por un servidor proxy o shim que permite la comunicación entre recursos del modelo clásico y de Azure Resource Manager. 

## <a name="why-does-azure-portal-require-me-to-provide-a-storage-account-for-deployment"></a>¿Por qué Azure Portal me pide que proporcione una cuenta de almacenamiento para la implementación?

En el portal clásico, el paquete se cargó directamente al nivel de API de administración y, a continuación, el nivel de API guardó temporalmente el paquete en una cuenta de almacenamiento interno.  Debido a este proceso, se crean problemas de rendimiento y escalabilidad porque el nivel de API no se diseñó para ser un servicio de carga de archivos.  En Azure Portal (modelo de implementación de administrador de recursos), hemos omitido el paso intermedio que supone cargar contenido primero en el nivel de API, lo que da lugar a implementaciones más rápidas y fiables.

En cuanto al costo, este es ínfimo, por lo que puede volver a usar la misma cuenta de almacenamiento a través de todas las implementaciones. Puede usar la [calculadora de costo de almacenamiento](https://azure.microsoft.com/pricing/calculator/#storage1) para determinar el costo que supone cargar, descargar y eliminar el paquete de servicio (CSPKG).
