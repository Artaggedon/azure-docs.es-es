---
title: 'Firewall de aplicaciones web en Application Gateway: preguntas más frecuentes'
description: Este artículo proporciona respuestas a las preguntas más frecuentes sobre el Firewall de aplicaciones web en Application Gateway.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: article
ms.date: 05/05/2020
ms.author: victorh
ms.openlocfilehash: 890688dba70a7fa654e97652b3e474b919f9a077
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104585390"
---
# <a name="frequently-asked-questions-for-azure-web-application-firewall-on-application-gateway"></a>Preguntas más frecuentes sobre el Firewall de aplicaciones web de Azure en Application Gateway

En este artículo se responden preguntas comunes sobre la funcionalidad y las características del Firewall de aplicaciones web (WAF) en Application Gateway. 

## <a name="what-is-azure-waf"></a>¿Qué es WAF de Azure?

Azure WAF es un Firewall de aplicaciones web que ayuda a proteger sus aplicaciones web frente a amenazas comunes como inyección de código SQL, el scripting entre sitios y otras vulnerabilidades de seguridad de la web. Puede definir una directiva de WAF, que consta de una combinación de reglas personalizadas y administradas para controlar el acceso a sus aplicaciones web.

Para las aplicaciones web hospedadas en el servicio Application Gateway o Azure Front Door, se puede aplicar una directiva de WAF de Azure.

## <a name="what-features-does-the-waf-sku-support"></a>¿Qué características admite la SKU de WAF?

La SKU de WAF admite todas las características disponibles en la SKU Estándar.

## <a name="how-do-i-monitor-waf"></a>¿Cómo se supervisa el WAF?

Supervise WAF a través del registro de diagnóstico. Para más información, consulte [Diagnostic logging and metrics for Application Gateway](../../application-gateway/application-gateway-diagnostics.md) (Registro de diagnóstico y métricas de Application Gateway).

## <a name="does-detection-mode-block-traffic"></a>¿Bloquea el modo de detección el tráfico?

No. El modo de detección solo registra el tráfico que desencadena una regla de WAF.

## <a name="can-i-customize-waf-rules"></a>¿Se pueden personalizan las reglas de WAF?

Sí. Para más información, consulte [Personalización de reglas de firewall de aplicaciones web mediante Azure Portal](application-gateway-customize-waf-rules-portal.md).

## <a name="what-rules-are-currently-available-for-waf"></a>¿Qué reglas están disponibles actualmente para WAF?

Actualmente, WAF admite CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229), [3.0](application-gateway-crs-rulegroups-rules.md#owasp30) y [3.1](application-gateway-crs-rulegroups-rules.md#owasp31). Estas reglas proporcionan seguridad de línea base frente a la mayoría de las 10 vulnerabilidades principales que Open Web Application Security Project (OWASP) identifica: 

* Protección contra la inyección de código SQL
* Protección contra ataques de scripts de sitios
* Protección contra ataques web comunes, como la inyección de comandos, el contrabando de solicitudes HTTP, la división de respuestas HTTP y el ataque remoto de inclusión de archivos.
* Protección contra infracciones del protocolo HTTP
* Protección contra anomalías del protocolo HTTP, como la falta de agentes de usuario de host y encabezados de aceptación
* Prevención contra bots, rastreadores y escáneres
* Detección de errores de configuración comunes en aplicaciones (es decir, Apache, IIS, etc.).

Para obtener más información, consulte las [10 vulnerabilidades principales de OWASP](https://owasp.org/www-project-top-ten/).

## <a name="what-content-types-does-waf-support"></a>¿Qué tipos de contenido admite WAF?

Application Gateway WAF admiten los siguientes tipos de contenido para las reglas administradas:

* application/json
* aplicación/xml
* application/x-www-form-urlencoded
* multipart/form-data

Y para las reglas personalizadas:

* application/x-www-form-urlencoded
* multipart/form-data

## <a name="does-waf-support-ddos-protection"></a>¿Admite WAF la protección contra DDoS?

Sí. Puede habilitar la protección contra DDos en la red virtual donde se implementa Application Gateway. Esta configuración garantiza que el servicio Azure DDoS Protection también protege la IP virtual (VIP) de Application Gateway.

### <a name="does-waf-store-customer-data"></a>¿WAF almacena datos de los clientes?

No, WAF no almacena datos de clientes.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Firewall de aplicaciones web de Azure](../overview.md).
- Obtenga más información acerca de [Azure Front Door](../../frontdoor/front-door-overview.md).
