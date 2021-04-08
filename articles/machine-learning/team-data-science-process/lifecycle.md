---
title: El ciclo de vida del proceso de ciencia de datos en equipo
description: El Proceso de ciencia de datos en equipo (TDSP) ofrece un ciclo de vida recomendado que se puede usar para estructurar los proyectos de ciencia de datos.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: ed54fda744978ac43bbffc6a70d6b331e85dc5e7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "93305603"
---
# <a name="the-team-data-science-process-lifecycle"></a>El ciclo de vida del proceso de ciencia de datos en equipo

El Proceso de ciencia de datos en equipo (TDSP) ofrece un ciclo de vida recomendado que se puede usar para estructurar los proyectos de ciencia de datos. En el ciclo de vida se describen los pasos completos que siguen los proyectos correctos. Si usa otro ciclo de vida de la ciencia de datos, como Cross Industry Standard Process for Data Mining [(CRISP-DM)](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), Knowledge Discovery in Databases [(KDD)](https://wikipedia.org/wiki/Data_mining#Process) o propio proceso personalizado de la organización, puede seguir usando el TDSP basado en tareas. 

Este ciclo de vida está diseñado para los proyectos de ciencia de datos que se enviarán como parte de aplicaciones inteligentes. Estas aplicaciones implementan modelos de aprendizaje o inteligencia artificial de máquina para realizar un análisis predictivo. Los proyectos de ciencia de datos exploratorios y los proyectos de análisis improvisados también se pueden beneficiar del uso de este proceso. Pero para esos proyectos, algunos de los pasos descritos a continuación pueden no ser necesarios. 

## <a name="five-lifecycle-stages"></a>Cinco fases del ciclo de vida

El ciclo de vida de TDSP se compone de cinco fases principales que se ejecutan de forma iterativa. Estas fases incluyen:

   1. [Conocimiento del negocio](lifecycle-business-understanding.md)
   2. [Adquisición y comprensión de los datos](lifecycle-data.md)
   3. [Modelado](lifecycle-modeling.md)
   4. [Implementación](lifecycle-deployment.md)
   5. [Aceptación del cliente](lifecycle-acceptance.md)

Esta es una representación visual del ciclo de vida de TDSP: 

![Ciclo de vida del TDSP](./media/lifecycle/tdsp-lifecycle2.png) 


El ciclo de vida del TDSP se modela como una secuencia de pasos repetidos que le orientan respecto a las tareas necesarias para usar modelos predictivos. El usuario implementa modelos de predicción en el entorno de producción que tiene previsto utilizar para compilar las aplicaciones inteligentes. El objetivo del ciclo de vida del proceso consiste en hacer avanzar un proyecto de ciencia de datos hacia un punto final de interacción claro. La ciencia de datos es un ejercicio de investigación y detección. La posibilidad de comunicar tareas a su equipo y sus clientes mediante un conjunto bien definido de artefactos que utilizan plantillas estandarizadas ayuda a evitar malos entendidos. El uso de estas plantillas también incrementa la posibilidad de finalizar correctamente un proyecto de ciencia de datos complejo.

Para cada fase, damos la siguiente información:

   * **Objetivos**: objetivos específicos.
   * **Cómo hacerlo**: un esquema de las tareas específicas y orientación sobre cómo realizarlas.
   * **Artefactos**: las entregas y la asistencia para producirlas.

## <a name="next-steps"></a>Pasos siguientes

Proporcionamos tutoriales completos que muestran todos los pasos del proceso en escenarios concretos. El artículo con [tutoriales de ejemplo](walkthroughs.md) proporciona una lista de los escenarios con vínculos y descripciones de miniatura. En los tutoriales se muestra cómo combinar servicios y herramientas en la nube y locales en un flujo de trabajo o una canalización con el fin de crear una aplicación inteligente. 

Para obtener ejemplos de cómo ejecutar pasos en TDSP que usan Microsoft Azure Machine Learning Studio, consulte [Uso del Proceso de ciencia de los datos en equipos con Azure Machine Learning](./index.yml).