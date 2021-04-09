---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/28/2020
ms.author: cherylmc
ms.openlocfilehash: 34986ac80a309bcfd495e5782496ba560f84c5f7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "93041514"
---
Cada equipo cliente que se conecta a una red virtual con una conexión de punto a sitio debe tener instalado un certificado de cliente. Se genera desde el certificado raíz y se instala en cada equipo cliente. Si no instala ningún certificado de cliente válido, la autenticación no podrá realizarse cuando el cliente trate de conectarse a la red virtual.

Puede generar un certificado único para cada cliente o puede usar el mismo para varios clientes. La ventaja de generar certificados de cliente únicos es la capacidad de revocar un solo certificado. De lo contrario, si varios clientes usan el mismo certificado de cliente para autenticarse y este se revoca, deberá generar e instalar nuevos certificados para cada cliente que use dicho certificado.

Para generar certificados de cliente, use los métodos siguientes:

* **Certificado de empresa:**

  * Si usa una solución de certificación de empresa, genere un certificado de cliente con el formato de valor de nombre común *nombre\@dominio.com*. Use este formato en lugar de *domain name\username*.

  * Asegúrese de que el certificado de cliente se base en la plantilla de certificado de usuario que tenga *Autenticación de cliente* como primer elemento de la lista de usuarios. Para comprobar el certificado, haga doble clic en él y vea **Uso mejorado de clave** en la pestaña **Detalles**.

* **Certificado raíz autofirmado**: siga los pasos de alguno de los siguientes artículos de certificados de P2S para que los certificados de cliente que cree sean compatibles con las conexiones de P2S.

  Si genera un certificado de cliente desde un certificado raíz autofirmado, este se instala automáticamente en el equipo que utilizó para generarlo. Si desea instalar un certificado de cliente en otro equipo cliente, expórtelo como un archivo .pfx junto con toda la cadena de certificados. De esta forma, creará un archivo .pfx que contiene la información del certificado raíz necesaria para que el cliente se autentique.

  Los pasos de estos artículos generan un certificado de cliente compatible que puede, posteriormente, exportar y distribuir.

  * [Instrucciones para Windows 10 y PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert): estas instrucciones requieren Windows 10 y PowerShell para generar certificados. Los certificados generados pueden instalarse en cualquier cliente P2S compatible.

  * [Instrucciones para MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): use MakeCert si no tiene acceso a un equipo Windows 10 para generar certificados. Aunque MakeCert está en desuso, todavía puede usarlo para generar certificados. Puede instalar los certificados generados en cualquier cliente P2S compatible.

  * [Instrucciones para Linux](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md).