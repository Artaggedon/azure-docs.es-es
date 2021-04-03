---
title: Autenticación OpenID Connect con Azure Active Directory
description: Guía de arquitectura para lograr la autenticación OpenID Connect con Azure Active Directory.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e5bf7e51de38d42e64f6737e687c5946a464160
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96168668"
---
# <a name="openid-connect-authentication-with-azure-active-directory"></a>Autenticación OpenID Connect con Azure Active Directory

OpenID Connect (OIDC) es un protocolo de autenticación basado en el protocolo OAuth2 (que se usa para la autorización). OIDC usa los flujos de mensajes estandarizados desde OAuth2 para proporcionar servicios de identidad. 

El objetivo de diseño de OIDC es "hacer que las cosas simples sean sencillas y que las complicadas sean posibles". OIDC permite a los desarrolladores autenticar a sus usuarios en sitios web y aplicaciones sin tener que poseer y administrar archivos de contraseñas. Esto brinda al creador de aplicaciones una manera segura de comprobar la identidad de la persona que usa actualmente el explorador o la aplicación nativa que está conectada a la aplicación.

La autenticación del usuario debe realizarse en un proveedor de identidades en el que se comprueben las credenciales o la sesión del usuario. Para ello, se necesita un agente de confianza. En general, las aplicaciones nativas inician el explorador del sistema para ese propósito. Se considera que las vistas incrustadas no son de confianza, ya que no hay nada que impida que la aplicación inspeccione la contraseña del usuario. 

Además de la autenticación, se puede solicitar consentimiento al usuario. El consentimiento es el permiso explícito del usuario para permitir que una aplicación acceda a recursos protegidos. El consentimiento es diferente de la autenticación porque el consentimiento solo debe darse una vez para un recurso. El consentimiento sigue siendo válido hasta que el usuario o administrador revoca manualmente la concesión. 

## <a name="use-when"></a>Cuándo se utiliza

Se necesita el consentimiento del usuario y para el inicio de sesión web.

![Diagrama de arquitectura](./media/authentication-patterns/oidc-auth.png)

## <a name="components-of-system"></a>Componentes del sistema

* **Usuario**: Solicita un servicio a la aplicación.

* **Agente de confianza**: Componente con el que interactúa el usuario. Este agente de confianza suele ser un explorador web.

* **Aplicación**: La aplicación, o el servidor de recursos, es donde residen los datos o el recurso. Confía en el proveedor de identidades para autenticar y autorizar de forma segura al agente de confianza. 

* **Azure AD**: El proveedor de OIDC, también se le conoce como proveedor de identidades, administra de forma segura todo lo que tenga que ver con la información del usuario, su acceso y las relaciones de confianza entre las partes de un flujo. Autentica la identidad del usuario, concede y revoca el acceso a los recursos y emite tokens. 

## <a name="implement-oidc-with-azure-ad"></a>Implementación de OIDC con Azure AD

* [Integración de aplicaciones con Azure AD](../saas-apps/tutorial-list.md) 

* [Protocolos OAuth 2.0 y OpenID Connect en la Plataforma de identidad de Microsoft](../develop/active-directory-v2-protocols.md) 

* [Plataforma de identidad de Microsoft y protocolo OpenID Connect](../develop/v2-protocols-oidc.md) 

* [Inicio de sesión web con OpenID Connect en Azure Active Directory B2C](../../active-directory-b2c/openid-connect.md) 

* [Protección de la aplicación con OpenID Connect y Azure AD](/learn/modules/secure-app-with-oidc-and-azure-ad/) 

