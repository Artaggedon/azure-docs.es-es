---
title: 'Diseño de identidades híbridas: requisitos de control de acceso en Azure | Microsoft Docs'
description: Cubre los pilares de identidad y la identificación de los requisitos de acceso de los recursos para los usuarios de un entorno híbrido.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84b786a1701892823554a83fa2015ac88d6eff4d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "60295150"
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Determinación de los requisitos de control de acceso para la solución de identidad híbrida
Cuando una organización va a diseñar su solución de identidad híbrida también pueden usar esta oportunidad para revisar los requisitos de acceso de los recursos que tienen pensado poner a disposición de los usuarios. El acceso a los datos atraviesa los cuatro pilares de identidad, que son:

* Administración
* Authentication
* Authorization
* Auditoría

En las secciones que siguen ahora se analizan con más detalle la autenticación y la autorización. La administración y la auditoría forman parte del ciclo de vida de identidad híbrida. Lea [Determinación de las tareas de administración de identidad híbrida](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) para más información acerca de estas funcionalidades.

> [!NOTE]
> Lea [Los cuatro pilares de identidad: la administración de identidades en la era de la TI híbrida](https://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) para obtener más información sobre cada uno de esos pilares.
> 
> 

## <a name="authentication-and-authorization"></a>Autenticación y autorización
Existen diferentes escenarios para la autenticación y la autorización. Estos escenarios tendrán requisitos específicos que debe cumplir la solución de identidad híbrida que la empresa va a adoptar. Los escenarios en los que interviene la comunicación de negocio a negocio (B2B) pueden suponer una dificultad añadida para los administradores de TI, quienes deberán asegurarse de que el método de autenticación y autorización empleado por la organización puede establecer comunicación con sus socios comerciales. Durante el proceso de diseño de los requisitos de autenticación y autorización, asegúrese de que puede responder a las siguientes cuestiones:

* ¿Su organización va a autenticar y autorizar solamente a los usuarios ubicados en su sistema de administración de identidades?
  * ¿Existen planes para escenarios B2B?
  * En caso afirmativo, ¿ya sabe qué protocolos (SAML, OAuth, Kerberos, tokens o certificados) se usarán para conectar ambas empresas?
* ¿Admite la solución de identidad híbrida estos protocolos?

Otro punto importante a tener en cuenta es dónde estará el repositorio de autenticación que usarán los usuarios y los socios, y el modelo administrativo que se empleará. Considere las dos opciones básicas siguientes:

* Centralizado: en este modelo, las credenciales, las directivas y la administración del usuario pueden estar centralizadas en el entorno local o en la nube.
* Híbrido: en este modelo, las credenciales, las directivas y la administración del usuario pueden estar centralizadas en el entorno local y tener una réplica en la nube.

¿Qué modelo adoptará su organización¿ ¿Variará según sus requisitos empresariales? Querrá responder a las siguientes preguntas con el fin de identificar donde residirá el sistema de administración de identidades y el modo administrativo que usará:

* ¿Su organización cuenta actualmente con administración de identidades local?
  * En caso afirmativo, ¿tienen pensado mantenerla?
  * ¿Existe algún requisito o normativa que su organización deba seguir que dictamine dónde debe residir el sistema de administración de identidades?
* ¿Usa su organización inicio de sesión único para las aplicaciones ubicadas en el entorno local o en la nube?
  * En caso afirmativo, ¿la adopción de un modelo de identidad híbrida afectará a este proceso?

## <a name="access-control"></a>Control de acceso
Si bien la autenticación y la autorización son elementos esenciales para permitir el acceso a los datos corporativos mediante la validación del usuario, también es importante controlar el nivel de acceso que estos usuarios tendrán y el que tendrán los administradores sobre los recursos que administran. La solución de identidad híbrida debe ser capaz de proporcionar acceso granular a los recursos, delegación y control de acceso basado en roles. Asegúrese de que la siguiente pregunta respecto al control de acceso queda respondida:

* ¿Su empresa cuenta con más de un usuario con privilegios elevados para administrar el sistema de identidad?
  * En caso afirmativo, ¿todos los usuarios necesitan el mismo nivel de acceso?
* ¿Deberá delegar su empresa el acceso a los usuarios para administrar recursos específicos?
  * En caso afirmativo, ¿con qué frecuencia sucede esto?
* ¿Necesitará su empresa integrar las funcionalidades de control de acceso entre los recursos locales y de nube?
* ¿Necesitará su empresa limitar el acceso a los recursos de acuerdo con algunas condiciones?
* ¿Tiene su empresa alguna aplicación que necesite control de acceso personalizado a algunos recursos?
  * En caso afirmativo, ¿donde se encuentran esas aplicaciones (en el entorno local o en la nube)?
  * En caso afirmativo, ¿donde están ubicados esos recursos de destino (en el entorno local o en la nube)?

> [!NOTE]
> Asegúrese de anotar cada respuesta y de que comprende las razones que se esconden detrás. [Definición de la estrategia de protección de datos](plan-hybrid-identity-design-considerations-data-protection-strategy.md) se recorren las opciones disponibles y las ventajas y desventajas de cada una.  Las respuestas que obtenga  partir de estas preguntas le servirán para seleccionar la opción que mejor se adapte a sus necesidades empresariales.
> 
> 

## <a name="next-steps"></a>Pasos siguientes
[Determinación de los requisitos de respuesta a incidentes](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Consulte también
[Información general sobre las consideraciones de diseño](plan-hybrid-identity-design-considerations-overview.md)

