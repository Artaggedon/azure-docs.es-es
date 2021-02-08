---
title: Solución de problemas de inicio de sesión en la suscripción a Azure
description: Ayuda a resolver problemas que no permiten iniciar sesión en Azure Portal o en el Centro de cuentas de Azure.
services: cost-management-billing
author: v-miegge
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 08/20/2020
ms.author: v-miegge
ms.openlocfilehash: 915b4fb289b6af6f4aac84d8bfa4d491a7f77efe
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99050477"
---
# <a name="troubleshoot-azure-subscription-sign-in-issues"></a>Solución de problemas de inicio de sesión en la suscripción a Azure

Esta guía ayuda a resolver problemas que no permiten iniciar sesión en Azure Portal o en el Centro de cuentas de Azure.

> [!NOTE]
> Si tiene problemas para registrar una nueva cuenta de Azure, consulte [Solución de problemas al registrar una suscripción en Azure](./troubleshoot-azure-sign-up.md).

## <a name="page-hangs-in-the-loading-status"></a>La página se bloquea en el estado de carga

Si la página del explorador de Internet se bloquea, pruebe cada uno de los siguientes procedimientos hasta que pueda acceder a Azure Portal.

- Actualice la página.
- Use un explorador de Internet diferente.
- Use el modo de exploración privado en el explorador:

   - **Edge:** abra **Configuración** (los tres puntos que están al lado de la imagen del perfil), seleccione **Ventana InPrivate nueva** y, después, busque e inicie sesión en [Azure Portal](https://portal.azure.com/). 
   - **Chrome:** elija el modo **Incógnito**.
   - **Safari:** Elija **File** (Archivo) y, después, **New Private Window** (Nueva ventana privada).

- Borre la memoria caché y elimine las cookies de Internet:

   - **Microsoft Edge:** abra **Configuración** y seleccione **Privacidad y servicios**. Siga los pasos descritos en **Borrar datos de exploración**. Compruebe que las casillas **Historial de exploración**, **Historial de descargas** e **Imágenes y archivos en caché** están seleccionadas y, después, seleccione **Eliminar**.
   - **Chrome:** Elija **Settings** (Configuración) y seleccione **Clear browsing data** (Borrar datos de exploración) en **Privacy and Security** (Privacidad y seguridad).

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Inicia sesión automáticamente con un usuario diferente

Este problema puede producirse si usa más de una cuenta de usuario en un explorador de Internet.

Para solucionar este problema, pruebe uno de los métodos siguientes:

- Borre la memoria caché y elimine las cookies de Internet.

   - **Microsoft Edge:** abra **Configuración** y seleccione **Privacidad y servicios**. Siga los pasos descritos en **Borrar datos de exploración**. Compruebe que las casillas **Historial de exploración**, **Historial de descargas**, **Cookies** e **Imágenes y archivos en caché** están seleccionadas y, después, seleccione **Eliminar**.
   - **Chrome:** elija **Settings** (Configuración) y seleccione **Clear browsing data** (Borrar datos de exploración) en **Privacy and Security** (Privacidad y seguridad).
- Restablezca los valores predeterminados de la configuración del explorador.
- Use el modo de exploración privado para el explorador. 
   - **Edge:** abra **Configuración** (los tres puntos que están al lado de la imagen del perfil), seleccione **Ventana InPrivate nueva** y, después, busque e inicie sesión en [Azure Portal](https://portal.azure.com/). 
   - **Chrome:** elija el modo **Incógnito**.
   - **Safari:** elija **File** (Archivo) y, después, **New Private Window** (Nueva ventana privada).

## <a name="i-can-sign-in-but-i-see-the-error-no-subscriptions-found"></a>Puedo iniciar sesión, pero veo el error No se encontraron suscripciones

Este problema se produce si ha seleccionado el directorio equivocado o si la cuenta no tiene permisos suficientes.

**Escenario 1:** recibe el error al iniciar sesión en [Azure Portal](https://portal.azure.com/)

Para corregir este problema:

- Compruebe que ha seleccionado el directorio correcto de Azure. Para ello, seleccione la cuenta pertinente en la esquina superior derecha.
- Si ha seleccionado el directorio correcto de Azure, pero el mensaje de error no desaparece, [agregue la cuenta como propietario](./add-change-subscription-administrator.md).

**Escenario 2:** recibe el error al iniciar sesión en el [Centro de cuentas de Azure](https://account.windowsazure.com/Subscriptions)

Compruebe si la cuenta que ha usado es el administrador de cuentas. Para comprobar quién es el administrador de cuentas, siga estos pasos:

1.  Inicie sesión en la [vista de suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1.  Seleccione la suscripción que quiera comprobar y, a continuación, seleccione **Configuración**.
1.  Seleccione **Propiedades**. El administrador de cuentas de la suscripción se muestra en el cuadro **Administrador de cuentas** .

## <a name="additional-help-resources"></a>Más recursos de ayuda

Otros artículos de solución de problemas relativos a la facturación y las suscripciones de Azure

- [Tarjeta rechazada](./troubleshoot-declined-card.md)
- [Problemas de registro de suscripción](./troubleshoot-azure-sign-up.md)
- [No se han encontrado suscripciones](./no-subscriptions-found.md)
- [Se ha deshabilitado la vista de costos empresariales](./enterprise-mgmt-grp-troubleshoot-cost-view.md)
- [Documentación de Facturación de Azure](../index.yml)

## <a name="contact-us-for-help"></a>Póngase en contacto con nosotros para obtener ayuda

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).