---
title: Introducción a Azure DDoS Protection Standard
description: Obtenga información sobre cómo Azure DDoS Protection, junto con los procedimientos recomendados de diseño de aplicaciones, constituyen una defensa frente a los ataques DDoS.
services: virtual-network
documentationcenter: na
author: yitoh
ms.service: virtual-network
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/9/2020
ms.author: yitoh
ms.openlocfilehash: 5e633b5e4b2ff2ce884bfaf99e3730968150cbed
ms.sourcegitcommit: 6e2d37afd50ec5ee148f98f2325943bafb2f4993
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97746378"
---
# <a name="azure-ddos-protection-standard-overview"></a>Introducción a Protección contra DDoS de Azure estándar

Los ataques por denegación de servicio distribuido (DDoS) son uno de los problemas de seguridad y disponibilidad más extendidos a los que se enfrentan los clientes que mueven sus aplicaciones a la nube. Un ataque DDoS intenta agotar los recursos de una aplicación haciendo que esta no esté disponible para los usuarios legítimos. Los ataques DDoS pueden ir dirigidos a cualquier punto de conexión que sea públicamente accesible a través de Internet.

Todas las propiedades de Azure están protegidas por DDoS Protection (básico) de la infraestructura de Azure sin costo adicional. El tamaño y la capacidad de la red de implementación global de Azure proporciona una defensa contra los ataques al nivel de red más comunes mediante la supervisión constante del tráfico y la mitigación en tiempo real. DDoS Protection Basic no requiere ningún cambio en la configuración de usuarios o aplicaciones. DDoS Protection Basic ayuda a proteger todos los servicios de Azure, incluidos servicios de PaaS como Azure DNS.

Azure DDoS Protection, junto con los procedimientos recomendados de diseño de aplicaciones, proporciona características mejoradas de mitigación DDoS para protegerse de los ataques DDoS. Se ajusta de forma automática para proteger los recursos específicos de Azure de una red virtual. La protección se puede habilitar fácilmente en cualquier red virtual nueva o existente y no requiere cambios en las aplicaciones ni los recursos. Tiene varias ventajas sobre el servicio básico, incluidos el registro, las alertas y la telemetría. 

![Comparación de servicios Azure DDoS Protection](./media/ddos-protection-overview/ddos-comparison.png)

Azure DDoS Protection no almacena datos de los clientes.

## <a name="features"></a>Características

- **Integración de plataforma nativa:** integrado de forma nativa en Azure. Incluye la configuración a través de Azure Portal. Protección contra DDoS estándar comprende sus recursos y la configuración de recursos.
- **Protección llave en mano:** la configuración simplificada protege de inmediato todos los recursos de una red virtual desde el momento en que se habilita DDoS Protection Estándar. No se requiere intervención ni definición del usuario. 
- **Supervisión continua del tráfico:** los patrones de tráfico de la aplicación se supervisan de forma ininterrumpida en busca de indicadores de ataques DDoS. Protección contra DDoS estándar mitiga el ataque de forma instantánea y automática una vez detectado.
- **Ajuste adaptable:** la generación de perfiles de tráfico inteligente va conociendo con el tiempo el tráfico de la aplicación y selecciona y actualiza el perfil que resulta más adecuado para el servicio. El perfil se ajusta a medida que el tráfico cambia con el tiempo.
- **Protección multicapa:** Proporciona protección contra DDoS de pila completa, cuando se usa con un firewall de aplicaciones web, para obtener protección en el nivel de red (nivel 3 y 4, ofrecido por Azure DDoS Protection Standard) y en el nivel de aplicación (nivel 7, ofrecido por un WAF). Entre las ofertas de WAF se incluyen la [SKU de WAF de Application Gateway](../web-application-firewall/ag/ag-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de Azure, así como ofertas de firewall de aplicaciones web de terceros disponibles en [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall).
- **Escala de mitigación amplia:** se pueden mitigar más de 60 tipos de ataque diferentes con capacidad global para protegerse contra los ataques DDoS más conocidos.
- **Análisis de ataques:** ofrece informes detallados en incrementos de cinco minutos durante un ataque y un resumen completo después de que el ataque termine. Transmita los registros del flujo de mitigación a [Azure Sentinel](../sentinel/connect-azure-ddos-protection.md) o a un sistema de administración de eventos e información de seguridad (SIEM) sin conexión para supervisar el sistema casi en tiempo real durante un ataque.
- **Métricas de ataques:** con Azure Monitor se puede acceder a un resumen de métricas de cada ataque.
- **Alertas de ataques:** las alertas se pueden configurar en el inicio y la detención de un ataque y a lo largo de la duración del ataque mediante métricas de ataque integradas. Las alertas se integran en el software operativo, como los registros de Microsoft Azure Monitor, Splunk, Azure Storage, el correo electrónico y Azure Portal.
- **Respuesta rápida de DDoS**: póngase en contacto con el equipo de respuesta rápida de DDoS Protection (DRR) para obtener ayuda con la investigación y el análisis de los ataques. Para obtener más información, vea [Respuesta rápida de DDoS](ddos-rapid-response.md).
- **Garantía de costo:** reciba crédito de servicio de escalabilidad horizontal de aplicaciones y transferencia de datos para los costos de recursos en los que se incurre como resultado de ataques DDoS documentados.

## <a name="pricing"></a>Precios

Para obtener información sobre los precios de Azure DDoS Protection Estándar, vea [Precios de Azure DDoS Protection Estándar](https://azure.microsoft.com/pricing/details/ddos-protection/).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de un plan de DDoS Protection](manage-ddos-protection.md)
