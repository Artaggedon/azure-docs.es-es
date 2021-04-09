---
title: Pruebas de penetración | Microsoft Docs
description: En el artículo se proporciona información general sobre el proceso de pruebas de penetración y cómo realizar dichas pruebas en las aplicaciones que se ejecutan en la infraestructura de Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2021
ms.author: terrylan
ms.openlocfilehash: 9f65133f622c71189f35a1533e278ca09ab02085
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99550208"
---
# <a name="penetration-testing"></a>Pruebas de penetración

Una de las ventajas del uso de Azure para las pruebas y la implementación de aplicaciones es que puede crear entornos rápidamente. No tiene que preocuparse del pedido, la adquisición, la "instalación en bastidor y apilamiento" de su propio hardware local.

Crear entornos rápidamente está muy bien, pero debe asegurarse de realizar sus diligencias de seguridad normales. Una de las cosas que quizá desee hacer es realizar pruebas de penetración de las aplicaciones que implemente en Azure.

No realizamos pruebas de penetración de su aplicación, pero sabemos que quiere y necesita realizar dichas pruebas en sus propias aplicaciones. Eso es bueno, ya que al mejorar la seguridad de sus aplicaciones, ayuda a hacer que todo el ecosistema de Azure sea más seguro.

Desde el 15 de junio de 2017, Microsoft ya no requiere la aprobación previa para llevar a cabo pruebas de penetración con recursos de Azure. Este proceso solo está relacionado con Microsoft Azure y no es aplicable ningún otro servicio de Microsoft Cloud.

>[!IMPORTANT]
>Aunque notificar a Microsoft de las actividades de pruebas de penetración ya no es obligatorio, los clientes deben cumplir las [reglas de compromiso de las pruebas de penetración unificadas de Microsoft Cloud](https://technet.microsoft.com/mt784683) de todos modos.

Entre las pruebas estándar que puede realizar se incluyen:

* Pruebas en los puntos de conexión para descubrir las [10 principales vulnerabilidades del Proyecto de seguridad de aplicación web abierta (OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Pruebas de vulnerabilidad ante datos aleatorios o inesperados](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) de los puntos de conexión
* [Exploración de puertos](https://en.wikipedia.org/wiki/Port_scanner) de los puntos de conexión

Un tipo de prueba que no puede realizar es cualquier tipo de ataque de [denegación de servicio (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack). Esta prueba incluye iniciar un ataque de denegación de servicio o realizar pruebas relacionadas que puedan determinar, demostrar o simular cualquier tipo de ataque de denegación de servicio.

>[!Note]
>Microsoft se ha asociado con BreakingPoint Cloud para crear una interfaz en la que pueda generar tráfico destinado a las direcciones IP públicas que tengan habilitado el servicio DDoS Protection con fines de simulación. Para obtener más información sobre la simulación de BreakingPoint Cloud, consulte [Pruebas a través de simulaciones](../../ddos-protection/test-through-simulations.md).

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [reglas de compromiso de pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2).
