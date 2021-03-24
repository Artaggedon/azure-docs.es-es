---
title: Configuración de la protección contra bots para el firewall de aplicaciones web con Azure Front Door (versión preliminar)
description: Obtenga información sobre cómo configurar una regla de protección contra bots en Azure Web Application Firewall (WAF) para Front Door mediante Azure Portal.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 2357c51f47bcb9bd8bbc6c408cb6d8edbab4d10e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91267013"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Configuración de la protección contra bots para el firewall de aplicaciones web (versión preliminar)
En este artículo se muestra cómo configurar una regla de protección contra bots en el firewall de aplicaciones web (WAF) para Front Door mediante Azure Portal. La regla de protección contra bots también puede configurarse mediante la CLI, Azure PowerShell o la plantilla de Azure Resource Manager.

> [!IMPORTANT]
> Actualmente, el conjunto de reglas de protección contra bots se encuentra en versión preliminar pública y se proporciona con un Acuerdo de nivel de servicio de versión preliminar. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.  Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerequisites

Crear una directiva básica de WAF para Front Door siguiendo las instrucciones descritas en [Creación de una directiva de WAF para Azure Front Door mediante Azure Portal](waf-front-door-create-portal.md).

## <a name="enable-bot-protection-rule-set"></a>Habilitación del conjunto de reglas de protección contra bots

En la página **Reglas administradas** al crear una directiva de firewall de aplicaciones web, busque primero **Conjunto de reglas administradas**, active la casilla situada delante de la regla **Microsoft_BotManager_1.0** en el menú desplegable y, a continuación, seleccione **Revisar y crear**.

   ![Regla de protección contra bots](.././media/waf-front-door-configure-bot-protection/botmanager112019.png)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [supervisar WAF](waf-front-door-monitor.md).
