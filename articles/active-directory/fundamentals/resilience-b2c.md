---
title: Aumento de la resistencia en la administración de identidades y acceso de los clientes mediante Azure AD B2C | Microsoft Docs
description: Métodos para aumentar la resistencia en la administración de identidades y acceso de los clientes con Azure AD B2C
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: gargi-sinha
ms.author: gasinh
manager: martinco
ms.reviewer: ''
ms.date: 11/30/2020
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b40918db03c260f899c36d306c892b787cc6371c
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98724898"
---
# <a name="build-resilience-in-your-customer-identity-and-access-management-with-azure-active-directory-b2c"></a>Compilación de resistencia en la administración de identidades y acceso de clientes con Azure Active Directory B2C

[Azure Active Directory (AD) B2C](../../active-directory-b2c/overview.md) es una plataforma de administración de identidades y acceso de clientes (CIAM) que está diseñada para ayudarle a iniciar correctamente las aplicaciones críticas orientadas al cliente. Contamos con muchas características integradas enfocadas a la [resistencia](https://azure.microsoft.com/blog/advancing-azure-active-directory-availability/) diseñadas para ayudar a que nuestro servicio se escale según sus necesidades y a mejorarla ante posibles situaciones de interrupción. Además, al iniciar una aplicación crítica, es importante tener en cuenta diversos elementos de configuración y diseño de la aplicación, así como el modo en que está configurada en Azure AD B2C para conseguir un comportamiento resistente en respuesta a escenarios de interrupción o error. En este artículo, trataremos algunos de los procedimientos recomendados para mejorar la resistencia.

Un servicio resistente es el que sigue funcionando a pesar de las interrupciones. Para ayudar a mejorar la resistencia en el servicio:

- Conozca todos los componentes.

- Elimine los únicos puntos de error.

- Aísle los componentes con errores para limitar su efecto.

- Proporcione redundancia con mecanismos de conmutación por error rápida y rutas de recuperación.

Al desarrollar la aplicación, se recomienda considerar cómo [aumentar la resistencia de la autenticación y la autorización en las aplicaciones](resilience-app-development-overview.md) con los componentes de identidad de la solución. En este artículo se tratan las mejoras en la resistencia específica de las aplicaciones de Azure AD B2C. Nuestras recomendaciones están agrupadas en funciones de CIAM.

![En la imagen se muestran los componentes de CIAM](media/resilience-b2c/high-level-components.png). En las secciones siguientes, le guiaremos por los pasos para aumentar la resistencia en las siguientes áreas:

- [Experiencia del usuario final](resilient-end-user-experience.md): habilite un plan de reserva para el flujo de autenticación y mitigue el posible efecto de una interrupción del servicio de autenticación de Azure AD B2C.

- [Interfaces con procesos externos](resilient-external-processes.md): recupérese de los errores para aumentar la resistencia de sus aplicaciones e interfaces.  

- [Procedimientos recomendados para desarrolladores](resilience-b2c-developer-best-practices.md): evite la fragilidad derivada de los problemas comunes con las directivas personalizadas y mejore el control de errores en áreas como las interacciones con los comprobadores de notificaciones, las aplicaciones de terceros y las API REST.

- [Supervisión y análisis](resilience-with-monitoring-alerting.md): use alertas para evaluar el estado del servicio mediante la supervisión de los indicadores clave y la detección de errores e interrupciones del rendimiento.

- [Aumento de la resistencia en la infraestructura de autenticación](resilience-in-infrastructure.md)

- [Aumento de la resistencia de la autenticación y autorización en las aplicaciones](resilience-app-development-overview.md)

Vea este vídeo para aprender a crear flujos resistentes y escalables mediante Azure AD B2C.
>[!Video https://www.youtube.com/embed/8f_Ozpw9yTs]
