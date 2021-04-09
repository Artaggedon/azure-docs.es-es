---
title: Preguntas más frecuentes acerca de las redes en Azure Functions
description: Respuestas a algunas de las preguntas y escenarios de redes con Azure Functions más comunes.
ms.topic: troubleshooting
ms.date: 4/11/2019
ms.reviewer: glenga
ms.openlocfilehash: 24afeeee3207127bb9404156dc390433671dd5da
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104592309"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Preguntas más frecuentes acerca de las redes en Azure Functions

En este artículo se incluyen las preguntas más frecuentes acerca de las redes en Azure Functions. Para obtener información general más completa, consulte el tema sobre [opciones de redes de Functions](functions-networking-options.md).

## <a name="how-do-i-set-a-static-ip-in-functions"></a>¿Cómo se puede establecer una dirección IP estática en Functions?

La implementación de una función en App Service Environment es la principal forma de tener direcciones IP de entrada y de salida estáticas para las funciones. Para obtener más información sobre el uso de una instancia de App Service Environment, empiece con el artículo [Creación y uso de un equilibrador de carga interno con una instancia de App Service Environment](../app-service/environment/create-ilb-ase.md).

También puede usar una instancia de NAT Gateway de red virtual para enrutar el tráfico de salida a través de una dirección IP pública controlada por el usuario. Para más información, consulte [Tutorial: Control de la IP de salida de Azure Functions mediante un servicio NAT Gateway de Azure Virtual Network](functions-how-to-use-nat-gateway.md). 

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>¿Cómo puedo restringir el acceso a Internet a mi función?

Dispone de un par de métodos para restringir el acceso a Internet:

* [Restricciones de IP](../app-service/app-service-ip-restrictions.md): restrinja el tráfico entrante a su aplicación de funciones por intervalo de IP.
    * En Restricciones de IP, también puede configurar [Puntos de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md), que restringen la función para que solo acepte tráfico entrante procedente de una red virtual concreta.
* Eliminación de todos los desencadenadores de HTTP. Para algunas aplicaciones, basta simplemente con evitar los desencadenadores de HTTP y usar cualquier otro origen de evento para desencadenar la función.

Tenga en cuenta que el editor de Azure Portal requiere acceso directo a la función en ejecución. Cualquier cambio de código mediante Azure Portal requerirá que el dispositivo que está usando examine el portal para incluir su IP en la lista aprobada. No obstante, todavía puede usar todas las características de la pestaña de características de la plataforma con las restricciones de red implementadas.

## <a name="how-do-i-restrict-my-function-app-to-a-virtual-network"></a>¿Cómo puedo restringir mi aplicación de funciones a una red virtual?

Puede restringir el tráfico **entrante** de una aplicación de funciones a una red virtual mediante [Puntos de conexión de servicio](./functions-networking-options.md#use-service-endpoints). Esta configuración todavía permite que la aplicación de funciones realice llamadas salientes a Internet.

Para restringir completamente una función de forma que todo el tráfico fluya a través de una red virtual, puede usar un [punto de conexión privado](./functions-networking-options.md#private-endpoint-connections) con la integración de red virtual de salida o una instancia de App Service Environment. Para más información, consulte [Tutorial: Integración de Azure Functions con una red virtual de Azure mediante puntos de conexión privados](functions-create-vnet.md).

## <a name="how-can-i-access-resources-in-a-virtual-network-from-a-function-app"></a>¿Cómo puedo acceder a los recursos de una red virtual desde una aplicación de funciones?

Puede acceder a los recursos de una red virtual desde una función en ejecución mediante la integración de la red virtual. Para obtener más información, consulte [Integración de la red virtual](functions-networking-options.md#virtual-network-integration).

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>¿Cómo puedo acceder a los recursos protegidos mediante puntos de conexión de servicio?

Mediante la integración de red virtual puede acceder a los recursos protegidos mediante puntos de conexión de servicio desde una función en ejecución. Para obtener más información, consulte [Integración de la red virtual](functions-networking-options.md#virtual-network-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-virtual-network"></a>¿Cómo puedo desencadenar una función desde un recurso de una red virtual?

Puede permitir la llamada de los desencadenadores de HTTP desde una red virtual con [Puntos de conexión de servicio](./functions-networking-options.md#use-service-endpoints) o [Conexiones de punto de conexión privado](./functions-networking-options.md#private-endpoint-connections). 

También puede desencadenar una función a partir de todos los demás recursos de una red virtual mediante la implementación de la aplicación de función en un plan Premium, un plan de App Service o una instancia de App Service Environment. Consulte [Desencadenadores de red virtual (no HTTP)](./functions-networking-options.md#virtual-network-triggers-non-http) para más información.

## <a name="how-can-i-deploy-my-function-app-in-a-virtual-network"></a>¿Cómo puedo implementar mi aplicación de funciones en una red virtual?

La implementación en una instancia de App Service Environment es la única forma de crear una aplicación de funciones que esté totalmente dentro de una red virtual. Para obtener más información sobre el uso de un equilibrador de carga interno con una instancia de App Service Environment, empiece con el artículo [Creación y uso de un equilibrador de carga interno con una instancia de App Service Environment](../app-service/environment/create-ilb-ase.md).

Para escenarios donde necesita solo acceso unidireccional a recursos de red virtual o un aislamiento de red menos exhaustivo, consulte el tema de [información general de redes de Functions](functions-networking-options.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las redes y las funciones: 

* [Siga el tutorial sobre cómo empezar con la integración de red virtual](./functions-create-vnet.md)
* [Obtenga más información sobre las opciones de redes en Azure Functions](./functions-networking-options.md)
* [Obtenga más información sobre la integración de red virtual con App Service y Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Obtenga más información sobre las redes virtuales en Azure](../virtual-network/virtual-networks-overview.md)
* [Habilite más características de redes y control con App Service Environment](../app-service/environment/intro.md)
