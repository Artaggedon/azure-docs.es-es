---
title: 'Solución VMware de Azure: Búsqueda directa de DNS de la nube privada a un entorno local'
description: Describe cómo habilitar el servidor DNS de la nube privada de CloudSimple para la búsqueda directa de recursos locales.
author: Ajayan1008
ms.author: v-hborys
ms.date: 02/29/2020
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3481e21a93fbf5f658c5ca00513ea179c8f95400
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97899208"
---
# <a name="enable-cloudsimple-private-cloud-dns-servers-to-forward-dns-lookup-of-on-premises-resources-to-your-dns-servers"></a>Habilitación de los servidores DNS de la nube privada de CloudSimple para la búsqueda directa de DNS de recursos locales a servidores DNS

Los servidores DNS de la nube privada pueden realizar una búsqueda directa de DNS de cualquier recurso local en los servidores DNS.  La habilitación de la búsqueda permite que los componentes de vSphere de la nube privada busquen cualquier servicio que se ejecute en el entorno local y se comuniquen con ellos mediante nombres de dominio completos (FQDN).

## <a name="scenarios"></a>Escenarios 

La búsqueda directa de DNS para el servidor DNS local permite usar la nube privada para los siguientes escenarios:

* Uso de la nube privada como configuración de la recuperación ante desastres para la solución VMware en el entorno local
* Uso de la instancia local de Active Directory como origen de identidades para vSphere de la nube privada.
* Uso de HCX para la migración de máquinas virtuales del entorno local a la nube privada

## <a name="before-you-begin"></a>Antes de empezar

Debe haber una conexión de red desde la red de nube privada a la red local para que la búsqueda directa de DNS funcione.  Puede configurar la conexión de red mediante:

* [Conexión desde una red local a CloudSimple mediante ExpressRoute](on-premises-connection.md)
* [Configuración de una puerta de enlace de VPN de sitio a sitio](./vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)

Los puertos de firewall deben estar abiertos en esta conexión para que la búsqueda de DNS funcione.  Los puertos que se usan son el puerto TCP 53 o UDP 53.

> [!NOTE]
> Si usa una VPN de sitio a sitio, la subred del servidor DNS local debe agregarse como parte de los prefijos locales.

## <a name="request-dns-forwarding-from-private-cloud-to-on-premises"></a>Solicitud de la entrega directa de DNS de la nube privada a un entorno local

Para habilitar la entrega directa de DNS de la nube privada a un entorno local, envíe una [solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) y proporcione la siguiente información.

* Tipo de problema: **Técnico**
* Suscripción: **Suscripción en la que se implementa el servicio CloudSimple**
* Servicio: **VMware Solution by CloudSimple**
* Tipo de problema: **Aviso o procedimientos**
* Subtipo de problema: **Necesito ayuda con NW**
* Proporcione el nombre de dominio del dominio local en el panel de detalles.
* Proporcione la lista de los servidores DNS locales a los que se reenviará la búsqueda desde la nube privada en el panel de detalles.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre la configuración del firewall local](on-premises-firewall-configuration.md)
* [Configuración del servidor DNS local](on-premises-dns-setup.md)
