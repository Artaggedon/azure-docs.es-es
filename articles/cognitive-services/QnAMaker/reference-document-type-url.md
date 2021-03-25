---
title: 'Tipos de direcciones URL admitidos para la importación: QnA Maker'
description: Comprenda cómo se usan los tipos de direcciones URL para importar y crear pares de QnA.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 01/02/2020
ms.openlocfilehash: 8bf50c1ea81cdf5246c47646d1a55926fe7d58d6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91776704"
---
# <a name="urls-supported-for-importing-documents"></a>Direcciones URL admitidas para la importación de documentos

Comprenda cómo se usan los tipos de direcciones URL para importar y crear pares de QnA.

## <a name="faq-urls"></a>Preguntas más frecuentes sobre las direcciones URL

QnA Maker puede admitir páginas web de preguntas más frecuentes de 3 formas diferentes:

* Páginas de preguntas más frecuentes sin formato
* Páginas de preguntas más frecuentes con vínculos
* Páginas de preguntas más frecuentes con una página principal de temas

### <a name="plain-faq-pages"></a>Páginas de preguntas más frecuentes sin formato

Este es el tipo más común de página de preguntas más frecuentes, en la que las respuestas siguen inmediatamente las preguntas de la misma página.

Aquí tiene un ejemplo de una página de preguntas más frecuentes sin formato:

![Ejemplo de página de preguntas más frecuentes sin formato para una base de conocimiento](./media/qnamaker-concepts-datasources/plain-faq.png)


### <a name="faq-pages-with-links"></a>Páginas de preguntas más frecuentes con vínculos

En este tipo de página de preguntas más frecuentes, las preguntas se agregan juntas y se vinculan a respuestas que están en secciones diferentes de la misma página o en páginas distintas.

A continuación se muestra un ejemplo de una página de preguntas más frecuentes con vínculos en las secciones que se encuentran en la misma página:

 ![Ejemplo de página de preguntas más frecuentes con sección de vínculos para una base de conocimiento](./media/qnamaker-concepts-datasources/sectionlink-faq.png)


### <a name="parent-topics-page-links-to-child-answers-pages"></a>Vínculos de páginas de temas primarios a páginas de respuestas secundarias

Este tipo de preguntas más frecuentes tiene una página de temas en la que cada tema se vincula a un conjunto de preguntas y respuestas correspondiente en una página diferente. QnA Maker rastrea todas las páginas vinculadas para extraer las preguntas y respuestas correspondientes.

A continuación, se muestra un ejemplo de una página de temas con vínculos a secciones de preguntas más frecuentes en páginas distintas.

 ![Ejemplo de página de preguntas más frecuentes con vínculos profundos para una base de conocimiento](./media/qnamaker-concepts-datasources/topics-faq.png)

## <a name="support-urls"></a>Direcciones URL de soporte técnico

QnA Maker puede procesar páginas web de soporte semiestructuradas, como artículos web que describan cómo realizar una tarea determinada, cómo diagnosticar y resolver un problema determinado y cuáles son las mejores prácticas para un determinado proceso. La extracción funciona mejor en contenido con una estructura clara y con encabezados jerárquicos.

> [!NOTE]
> La extracción para artículos de soporte técnico es una característica nueva que se encuentra en sus primeras etapas. Funciona mejor con páginas simples, que están bien estructuradas y no contienen encabezados ni pies de página complejos.

![QnA Maker admite la extracción en páginas web semiestructuradas con una estructura clara y con encabezados jerárquicos.](./media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)