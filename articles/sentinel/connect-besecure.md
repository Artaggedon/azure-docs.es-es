---
title: Conexión de datos de Beyond Security beSECURE a Azure Sentinel | Microsoft Docs
description: Obtenga información acerca de cómo usar el conector de datos de Beyond Security beSECURE para extraer los registros de beSECURE en Azure Sentinel. Visualice los datos de beSECURE en libros, cree alertas y mejore la investigación.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2021
ms.author: yelevin
ms.openlocfilehash: 313f201aeabd470850b27d979dc5253f80e82a55
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98541176"
---
# <a name="connect-your-beyond-security-besecure-to-azure-sentinel"></a>Conexión de Beyond Security beSECURE a Azure Sentinel

> [!IMPORTANT]
> El conector de Beyond Security beSECURE se encuentra actualmente en **versión preliminar**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

El conector de Beyond Security beSECURE permite conectar fácilmente todos los registros de la solución de seguridad de beSECURE con Azure Sentinel para ver los paneles, crear alertas personalizadas y mejorar la investigación. La integración entre beSECURE y Azure Sentinel usa la API de REST.

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-besecure"></a>Configuración y conexión de beSECURE

beSECURE se puede integrar con Azure Sentinel y exportar los registros directamente a esta plataforma.

1. En el menú de navegación de Azure Sentinel, seleccione **Conectores de datos**.

1. En la galería **Conectores de datos**, seleccione **Beyond Security beSECURE (versión preliminar)** y, a continuación, seleccione la página **Abrir conector**.

1. Siga los pasos que se indican a continuación para configurar la solución beSECURE para enviar los resultados del examen, el estado del examen y los registros de auditoría a Azure Sentinel.

    **Acceda al menú Integración:**
    1. Haga clic en la opción de menú "Más".

    1. Seleccionar servidor

    1. Seleccione Integración.

    1. Habilitación de Azure Sentinel

    **Proporcione a beSECURE la configuración de Azure Sentinel:**

      Copie los valores de *Id. de área de trabajo* y *Clave principal* de la página del conector de Azure Sentinel, péguelos en la configuración de beSECURE y haga clic en **Modificar**.
      
      :::image type="content" source="media/connectors/workspace-id-primary-key.png" alt-text="{Identificador de área de trabajo y clave principal}":::

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez establecida una conexión correcta, los datos aparecen en **Registros**, debajo de la sección **CustomLogs**, en una o varias de las tablas siguientes:
  - `beSECURE_ScanResults_CL`
  - `beSECURE_ScanEvents_CL`
  - `beSECURE_Audit_CL`

Para consultar los registros de beSECURE en reglas de análisis, consultas de búsqueda, investigaciones o en cualquier otro lugar de Azure Sentinel, escriba uno de los nombres de tabla anteriores en la parte superior de la ventana de consulta.

## <a name="validate-connectivity"></a>Validar conectividad
Los registros pueden tardar hasta 20 minutos en empezar a aparecer en Log Analytics.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar beSECURE a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar los datos.
