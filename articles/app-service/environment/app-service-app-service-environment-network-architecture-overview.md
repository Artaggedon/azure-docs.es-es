---
title: Arquitectura de red en v1
description: Introducción a la arquitectura de la topología de red de instancias de App Service Environment. Este documento solo se proporciona para los clientes que usan App Service Environment v1 heredado.
author: stefsch
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: b1b866f3be789c59eea38c5c22b5557d557440be
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "84701820"
---
# <a name="network-architecture-overview-of-app-service-environments"></a>Información general sobre la arquitectura de red de los entornos de App Service
Las instancias de App Service Environment siempre se crean dentro de una subred de una [red virtual][virtualnetwork]; las aplicaciones que se ejecutan en App Service Environment pueden comunicarse con puntos de conexión privados ubicados dentro de la misma topología de red virtual.  Puesto que los clientes pueden bloquear partes de su infraestructura de red virtual, es importante conocer los tipos de flujos de comunicación de red que se producen con un entorno de App Service.

## <a name="general-network-flow"></a>Flujo de red general
Cuando un entorno de App Service (ASE) utiliza una dirección IP virtual pública (VIP) para las aplicaciones, todo el tráfico entrante llega a esa VIP pública,  incluido el tráfico HTTP y HTTPS para las aplicaciones, así como otro tráfico de FTP, la funcionalidad de depuración remota y las operaciones de administración de Azure.  Para ver una lista completa de los puertos específicos (necesarios y opcionales) que están disponibles en la IP virtual pública, consulte el artículo sobre el [control del tráfico entrante][controllinginboundtraffic] a App Service Environment. 

Los entornos de App Service también admiten aplicaciones en ejecución enlazadas únicamente a una dirección de red virtual interna, que también se denomina "dirección del ILB" (equilibrador de carga interno).  En un ASE con un ILB, el tráfico HTTP y HTTPS de las aplicaciones, así como las llamadas de depuración remota, proceden de la dirección del ILB.  En las configuraciones de ASE con un ILB más habituales, el tráfico FTP y FTPS también llegan desde esa dirección.  Sin embargo, las operaciones de administración de Azure seguirán fluyendo a los puertos 454 y 455 en la VIP pública de un ASE con un ILB.

En el diagrama siguiente se muestra información general de los distintos flujos de red entrantes y salientes de un entorno de App Service, donde las aplicaciones se enlazan a una dirección IP virtual pública:

![Flujos de red generales][GeneralNetworkFlows]

Un entorno de App Service puede comunicarse con una variedad de extremos privados del cliente.  Por ejemplo, las aplicaciones que se ejecutan en el entorno de App Service pueden conectarse a servidores de base de datos que se ejecutan en máquinas virtuales de IaaS en la misma topología de red virtual.

> [!IMPORTANT]
> Si examinamos el diagrama de red, la implementación correspondiente a "Otros recursos de proceso" se efectúa en una subred diferente a la del entorno de App Service Environment. Al implementar recursos en la misma subred que el entorno del Servicio de aplicaciones (ASE), se bloqueará la conectividad del ASE a esos recursos (excepto para el enrutamiento específico dentro del entorno del Servicio de aplicaciones). Implemente en su lugar en una subred diferente (en la misma red virtual). A continuación, el entorno de App Service podrá conectarse. No se necesita ninguna configuración adicional.
> 
> 

Los entornos de App Service Environment también se comunican con SQL Database y los recursos de Azure Storage necesarios para administrar y operar un entorno de App Service Environment.  Algunos recursos de Storage y SQL con los que se comunica un entorno de App Service Environment se encuentran en la misma región que el entorno de App Service Environment, mientras que otros se encuentran en regiones de Azure remotas.  Como resultado, la conectividad saliente a Internet siempre es necesaria para que un entorno de App Service funcione correctamente. 

Puesto que un entorno de App Service se implementa en una subred, los grupos de seguridad de red pueden usarse para controlar el tráfico entrante a la subred.  Para más información sobre cómo controlar el tráfico entrante a App Service Environment, consulte el siguiente [artículo][controllinginboundtraffic].

Para más información sobre cómo permitir la conectividad saliente de Internet desde App Service Environment, consulte el artículo siguiente sobre cómo trabajar con [ExpressRoute][ExpressRoute].  El mismo enfoque que se describe en este artículo se aplica al trabajar con conectividad de sitio a sitio y al usar la tunelización forzada.

## <a name="outbound-network-addresses"></a>Direcciones de red de salida
Cuando un entorno de App Service realiza las llamadas salientes, una dirección IP siempre se asocia con las llamadas salientes.  La dirección IP específica que se usa depende de si el extremo al que se llama se encuentra dentro de la topología de red virtual o fuera de ella.

Si el extremo al que se llama está **fuera** de la topología de red virtual, entonces la dirección saliente (también conocida como la dirección NAT saliente) que se utiliza es la VIP pública del entorno de App Service.  Esta dirección se encuentra en la interfaz de usuario del portal para el entorno de App Service en la hoja Propiedades.

![Dirección IP saliente][OutboundIPAddress]

Esta dirección también se puede determinar para los ASE que solo tienen una VIP pública creando una aplicación en el entorno de App Service y, después, realizando una operación *nslookup* en la dirección de la aplicación. La dirección IP resultante es tanto la VIP pública como la dirección NAT saliente del entorno de App Service.

Si el extremo al que se llama está **dentro** de la topología de red virtual, la dirección saliente de la aplicación de llamada será la dirección IP interna del recurso de proceso individual que ejecuta la aplicación.  Sin embargo, no hay una asignación persistente de direcciones IP internas de red virtual a las aplicaciones.  Las aplicaciones pueden moverse a través de recursos informáticos diferentes y el grupo de recursos informáticos disponibles en un entorno de App Service puede cambiar debido a las operaciones de ajuste de escala.

Sin embargo, puesto que un entorno de App Service siempre se encuentra dentro de una subred, se garantiza que la dirección IP interna de un recurso informático que ejecuta una aplicación siempre se queda dentro del intervalo CIDR de la subred.  Como resultado, cuando las ACL específicas o los grupos de seguridad de red se utilizan para proteger el acceso a otros extremos en la red virtual, el intervalo de subred que contiene el entorno de App Service necesita tener acceso.

En el diagrama siguiente se explican estos conceptos de manera detallada:

![Direcciones de red de salida][OutboundNetworkAddresses]

En el diagrama anterior:

* Dado que la VIP pública del entorno de App Service es 192.23.1.2, que es la dirección IP saliente que se utiliza cuando se realizan llamadas a los extremos de "Internet".
* El intervalo CIDR de la subred que contiene para el entorno de App Service es 10.0.1.0/26.  Otros extremos dentro de la misma infraestructura de red virtual verán las llamadas de aplicaciones como originadas en algún lugar dentro de este intervalo de direcciones.

## <a name="calls-between-app-service-environments"></a>Llamadas entre entornos de App Service
El escenario puede resultar más complejo si implementa varios entornos de App Service en la misma red virtual y realiza llamadas salientes desde un entorno de App Service a otro.  Estos tipos de llamadas cruzadas entre entornos de App Service también se tratarán como llamadas de "Internet".

En el siguiente diagrama, se muestra un ejemplo de arquitectura en capas con aplicaciones en un entorno de App Service (p. ej., aplicaciones web de "entrada principal") que llaman a aplicaciones en un segundo entorno de App Service (por ejemplo, aplicaciones de API de back-end internas que no se crearon para que sean accesibles desde Internet). 

![Llamadas entre entornos de App Service][CallsBetweenAppServiceEnvironments] 

En el ejemplo anterior, el entorno de App Service "ASE One" tiene la dirección IP saliente 192.23.1.2.  Si una aplicación que se ejecuta en este entorno de App Service hace una llamada saliente a una aplicación que se ejecuta en un segundo entorno de App Service ("ASE Two") ubicado en la misma red virtual, la llamada saliente se tratará como una llamada de "Internet".  Como resultado, el tráfico de red que llega al segundo entorno de App Service se mostrará como originado en 192.23.1.2 (es decir, no el intervalo de direcciones de subred del primer entorno de App Service).

Aunque las llamadas entre diferentes entornos de App Service se tratan como llamadas de "Internet", cuando ambos entornos de App Service se encuentran en la misma región de Azure, el tráfico de red permanecerá en la red regional de Azure y no pasará físicamente a través de la Internet pública.  Como resultado, puede usar un grupo de seguridad de red en la subred del segundo entorno de App Service para permitir solo las llamadas entrantes desde el primer entorno de App Service (cuya dirección IP saliente es 192.23.1.2), lo que garantiza una comunicación segura entre los entornos de App Service.

## <a name="additional-links-and-information"></a>Información y vínculos adicionales
Se puede encontrar más información sobre los puertos de entrada usados por App Service Environment y sobre el uso de grupos de seguridad de red para controlar el tráfico entrante [aquí][controllinginboundtraffic].

Los detalles sobre el uso de rutas definidas por el usuario para conceder acceso saliente a Internet a App Service Environment están disponibles en este [artículo][ExpressRoute]. 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[ExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

