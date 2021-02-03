---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 7cf3d86eeea9d1b0f5fcbb757d3597e21cbcc369
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98901204"
---
Form Recognizer trabaja con documentos de entrada que cumplen estos requisitos:

* El formato debe ser JPG, PNG o PDF (texto o digitalizado) o TIFF. Los PDF insertados de texto son mejores porque no hay posibilidad de error en la extracción de caracteres y en la ubicación.
* El tamaño del archivo debe ser inferior a 50 MB.
* Las imágenes deben tener unas dimensiones entre 50 x 50 píxeles y 10 000 x 10 000 píxeles.
* Los archivos PDF deben tener unas dimensiones de 17 x 17 pulgadas como máximo, lo que se corresponde con los tamaños de papel Legal o A3 y más pequeños.
* En el caso de PDF y TIFF, solo se procesan las primeras 200 páginas (con una suscripción de nivel gratuitos, solo se procesan las dos primeras páginas).
* El tamaño total del conjunto de datos de aprendizaje puede ser de 500 páginas o menos.
* Si los archivos PDF están bloqueados con contraseña, debe quitar el bloqueo antes de enviarlos.
* Si se digitalizan desde documentos en papel, los formularios deben ser digitalizaciones de alta calidad.
* El texto debe usar el alfabeto latino (caracteres ingleses).
* En el caso del aprendizaje sin supervisión (sin datos etiquetados), los datos deben contener claves y valores.
* En el caso del aprendizaje sin supervisión (sin datos etiquetados), las claves deben aparecer por encima o a la izquierda de los valores; no pueden aparecer por abajo ni a la derecha.
