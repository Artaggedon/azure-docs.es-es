---
title: Panel de Azure Defender y sus características
description: Obtenga información sobre las características del panel de Azure Defender.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 16379380fc35bb2355c496dc857e9de3b41347f9
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096911"
---
# <a name="the-azure-defender-dashboard"></a>Panel de Azure Defender

El panel de Azure Defender proporciona lo siguiente:

- Visibilidad de la cobertura de Azure Defender en los diferentes tipos de recursos
- Vínculos para configurar las funcionalidades avanzadas de protección contra amenazas
- Estado de incorporación e instalación del agente
- Alertas de detección de amenazas de Azure Defender 

Para acceder al panel de Azure Defender, seleccione  **Azure Defender** en la sección Seguridad en la nube del menú de Security Center.

## <a name="whats-shown-on-the-dashboard"></a>¿Qué se muestra en el panel?

:::image type="content" source="./media/azure-defender-dashboard/sample-defender-dashboard-numbered.png" alt-text="Ejemplo del panel de Azure Defender" lightbox="./media/azure-defender-dashboard/sample-defender-dashboard-numbered.png":::

El panel incluye las secciones siguientes:

1. **Cobertura de Azure Defender**: aquí puede ver los tipos de recursos que se encuentran en su suscripción y que pueden protegerse mediante Azure Defender. Siempre que proceda, tendrá también la opción de actualizar. Si desea actualizar todos los recursos válidos posibles, seleccione **Actualizar todo**.

2. **Alertas de seguridad**: cuando Azure Defender detecta una amenaza en cualquiera de las áreas del entorno, genera una alerta. Estas alertas describen los detalles de los recursos afectados, los pasos de corrección sugeridos y, en algunos casos, una opción para desencadenar una aplicación lógica como respuesta. Al seleccionar cualquier parte de este gráfico, se abre la **página de alertas de seguridad**.

3. **Protección avanzada**: Azure Defender incluye muchas capacidades avanzadas de protección contra amenazas para máquinas virtuales, bases de datos SQL, contenedores, aplicaciones web, la red y mucho más. En esta sección de protección avanzada, puede ver el estado de los recursos de las suscripciones seleccionadas para cada una de estas protecciones. Seleccione cualquiera de ellas para ir directamente al área de configuración de ese tipo de protección.

4. **Información detallada** : este panel continuo de noticias, lecturas sugeridas y alertas de alta prioridad aporta a Security Center información sobre aspectos de seguridad apremiantes que son pertinentes para usted y su suscripción. Ya sea que se trate de una lista de vulnerabilidades y exposiciones comunes de alta gravedad descubiertas en sus máquinas virtuales por una herramienta de análisis de vulnerabilidades, o una nueva publicación de blog de un miembro del equipo de Security Center, lo encontrará en el panel de información detallada de su **panel de Azure Defender**.




## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió las características del panel de Azure Defender. 

Para más información sobre Azure Defender, consulte [Introducción a Azure Defender](azure-defender.md).

> [!div class="nextstepaction"]
> [Habilitación de Azure Defender](enable-azure-defender.md)