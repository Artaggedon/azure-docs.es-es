---
title: 'Actualización a Azure Defender: Azure Security Center'
description: En este inicio rápido se muestra cómo realizar la actualización a Azure Defender de Security Center para incrementar la seguridad.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2020
ms.author: memildin
ms.openlocfilehash: d3ed064547b1202aa562fffdfde85a28cbe8514e
ms.sourcegitcommit: 31d242b611a2887e0af1fc501a7d808c933a6bf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97809554"
---
# <a name="quickstart-set-up-azure-security-center"></a>Inicio rápido: Configuración de Azure Security Center

Azure Security Center proporciona administración unificada de la seguridad y protección contra amenazas para cargas de trabajo en la nube híbrida. Aunque las características gratuitas solo ofrecen una seguridad limitada para los recursos de Azure, al habilitar Azure Defender, se amplían estas funcionalidades tanto en un entorno local como en otras nubes. Azure Defender le ayuda a encontrar y corregir los puntos vulnerables de seguridad, aplicar controles de acceso y de aplicación para bloquear actividades malintencionadas, detectar amenazas mediante análisis e inteligencia, y responder rápidamente en caso de ataque. Puede probar Azure Defender sin costo alguno. Para más información, consulte la [página de precios](https://azure.microsoft.com/pricing/details/security-center/).

En este inicio rápido, se le guiará por los procesos de habilitación de Azure Defender para lograr una mayor seguridad y de instalación del agente de Log Analytics en las máquinas para supervisar las amenazas y los puntos vulnerables de la seguridad.

Siga estos pasos:

> [!div class="checklist"]
> * Habilitación de Security Center en una suscripción de Azure
> * Habilitación de Azure Defender en las suscripciones de Azure
> * Habilitación de la recopilación automática de datos

## <a name="prerequisites"></a>Prerrequisitos
Para empezar a trabajar con el Centro de seguridad, debe disponer de una suscripción a Microsoft Azure. Si no tiene una suscripción, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).

Para habilitar Azure Defender en una suscripción, debe tener asignados los roles Propietario de la suscripción, Colaborador de la suscripción o Administrador de seguridad.


## <a name="enable-security-center-on-your-azure-subscription"></a>Habilitación de Security Center en una suscripción de Azure

1. Inicie sesión en el [Portal de Azure](https://azure.microsoft.com/features/azure-portal/).

1. En el menú del portal, seleccione **Security Center**. 

    Se abre la página de información general de Security Center.

    :::image type="content" source="./media/security-center-get-started/overview.png" alt-text="Panel de información general de Security Center" lightbox="./media/security-center-get-started/overview.png":::

**Security Center - Información general** proporciona una vista unificada de la situación de seguridad de las cargas de trabajo de la nube híbrida, lo que le permite detectar y evaluar la seguridad de las cargas de trabajo e identificar y mitigar riesgos. Security Center habilita de forma automática y sin costo alguno cualquiera de las suscripciones de Azure que no fueran incorporadas previamente por usted o por otro usuario de la suscripción.

Puede ver la lista de suscripciones y filtrarla, si hace clic en el elemento de menú **Suscripciones**. Security Center ajustará la pantalla para reflejar la postura de seguridad de las suscripciones seleccionadas. 

A los pocos minutos de iniciar Security Center la primera vez, puede ver:

- **Recomendaciones** de formas de mejorar la seguridad de los recursos de Azure.
- Un inventario de los recursos que Security Center está evaluando, junto con la postura de seguridad de cada uno.

Si desea sacar el máximo provecho de Security Center, debe completar los pasos siguientes para habilitar Azure Defender e instalar el agente de Log Analytics.

> [!TIP]
> Para habilitar Security Center en todas las suscripciones de un grupo de administración, consulte el artículo sobre la [habilitación de Security Center en varias suscripciones de Azure](onboard-management-group.md).

## <a name="enable-azure-defender"></a>Habilitación de Azure Defender

Para seguir los inicios rápidos y tutoriales de Security Center, debe habilitar Azure Defender. Hay disponible una versión de evaluación gratuita de 30 días. Para más información, consulte la [página de precios](https://azure.microsoft.com/pricing/details/security-center/). 

1. En la barra lateral de Security Center, seleccione **Introducción**.

    :::image type="content" source="./media/security-center-get-started/get-started-upgrade-tab.png" alt-text="Pestaña Actualizar de la página introducción"::: 

    La pestaña **Actualizar** muestra las suscripciones y áreas de trabajo aptas para la incorporación.

1. En la lista **Selección de las áreas de trabajo en las que quiera habilitar Azure Defender**, seleccione las áreas de trabajo que desee actualizar.
   - Si selecciona suscripciones y áreas de trabajo que no sean válidas para la evaluación, el paso siguiente las actualizará y se iniciarán los cargos.
   - Si selecciona un área de trabajo apta para una evaluación gratuita, el siguiente paso comenzará una evaluación gratuita.
1. Seleccione **Actualizar** para habilitar Azure Defender.

## <a name="enable-automatic-data-collection"></a>Habilitación de la recopilación automática de datos
Security Center recopila datos de las máquinas virtuales para supervisar las amenazas y los puntos vulnerables de la seguridad. Los datos se recopilan con el agente de Log Analytics, que lee distintas configuraciones relacionadas con la seguridad y distintos registros de eventos de la máquina y copia los datos en el área de trabajo para analizarlos. De forma predeterminada, Azure Security Center creará automáticamente una nueva área de trabajo.

Si el aprovisionamiento automático está habilitado, Security Center instala el agente de Log Analytics en todas las máquinas compatibles y en las nuevas que se creen. Se recomienda encarecidamente el aprovisionamiento automático.

Para habilitar el aprovisionamiento automático del agente de Log Analytics:

1. En el menú de Security Center, seleccione **Precios y configuración**.
1. Seleccione la suscripción correspondiente.
1. En la página **Recopilación de datos**, establezca **Aprovisionamiento automático** en **Activado**.
1. Seleccione **Guardar**.

    :::image type="content" source="./media/security-center-enable-data-collection/enable-automatic-provisioning.png" alt-text="Habilitación del aprovisionamiento automático del agente de Log Analytics":::

>[!TIP]
> Si es necesario aprovisionar un área de trabajo, la instalación del agente puede tardar hasta 25 minutos.

Con el agente implementado en las máquinas, Security Center puede proporcionar recomendaciones adicionales relacionadas con el estado de actualización del sistema, las configuraciones de seguridad del sistema operativo y la protección de los puntos de conexión, así como generar alertas de seguridad adicionales.

>[!NOTE]
> Al establecer el aprovisionamiento automático en **Desactivado**, el agente de Log Analytics no se quita de las máquinas virtuales de Azure en las que ya se haya aprovisionado. La deshabilitación del aprovisionamiento automático limita la supervisión de seguridad de los recursos.



## <a name="next-steps"></a>Pasos siguientes
En este inicio rápido, habilitó Azure Defender y aprovisionó el agente de Log Analytics para lograr una administración de la seguridad y una protección contra amenazas unificadas en sus cargas de trabajo en la nube híbrida. Para más información sobre cómo usar Security Center, siga la guía de inicio rápido para la incorporación de equipos Windows que se encuentran en local y en otras nubes.

> [!div class="nextstepaction"]
> [Inicio rápido: Incorporación de máquinas que no son de Azure](quickstart-onboard-machines.md)

¿Quiere optimizar y ahorrar en el gasto en la nube?

> [!div class="nextstepaction"]
> [Comience a analizar los costos con Cost Management](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/get-started.png
[5]: ./media/security-center-get-started/pricing.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
[9]: ./media/security-center-get-started/select-subscription.png