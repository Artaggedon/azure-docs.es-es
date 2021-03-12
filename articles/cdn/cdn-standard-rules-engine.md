---
title: Uso de un motor de reglas para exigir HTTPS en Azure CDN estándar | Microsoft Docs
description: Use el motor de reglas de Microsoft Standard Azure Content Delivery Network (Azure CDN) para personalizar el modo en que Azure CDN controla las solicitudes HTTP, incluido el bloqueo de la entrega de ciertos tipos de contenido, la definición de una directiva de almacenamiento en caché y la modificación de encabezados HTTP. En este artículo, aprenderá a crear una regla para redirigir a los usuarios a HTTPS.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: how-to
ms.date: 11/01/2019
ms.author: allensu
ms.openlocfilehash: b94798580b2d4ba746c152486c0de753cf64d2c9
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102564907"
---
# <a name="set-up-the-standard-rules-engine-for-azure-cdn"></a>Configuración del motor de reglas estándar de Azure CDN

En este artículo se describe cómo configurar y usar el motor de reglas estándar para Azure Content Delivery Network (Azure CDN).

## <a name="standard-rules-engine"></a>Motor de reglas estándar

Puede usar el motor de reglas estándar de Azure CDN para personalizar cómo se controlan las solicitudes HTTP. Por ejemplo, puede usar el motor de reglas para exigir que la entrega de contenido use protocolos específicos, para definir una directiva de almacenamiento en caché o para modificar un encabezado HTTP. En este artículo se muestra cómo crear una regla que redirigirá automáticamente a los usuarios a HTTPS. 

> [!NOTE]
> El motor de reglas que se describe en este artículo solo está disponible para Azure CDN estándar de Microsoft. 

## <a name="redirect-users-to-https"></a>Redireccionamiento de usuarios a HTTPS

1. En sus perfiles de Microsoft, vaya a Azure Content Delivery Network.

1. En la página **Perfil de CDN**, seleccione el punto de conexión para el que quiere crear reglas.
  
1. Seleccione la pestaña **Motor de reglas**.
   
    Se abre el panel **Motor de reglas** y se muestra la lista de reglas globales disponibles. 
   
    [![Página de nuevas reglas de Azure CDN](./media/cdn-standard-rules-engine/cdn-new-rule.png)](./media/cdn-standard-rules-engine/cdn-new-rule.png#lightbox)
   
   > [!IMPORTANT]
   > El orden en que se muestran varias reglas afecta a la manera en que se controlan. Las acciones que se especifican en una regla se pueden sobrescribir con una regla subsiguiente.
   >

1. Seleccione **Agregar regla** y escriba un nombre de regla. Los nombres de las reglas deben empezar con una letra y pueden contener solamente números y letras.

1. Para identificar el tipo de solicitudes al que se aplica la regla, cree una condición de coincidencia:
    1. Seleccione **Agregar condición** y, a continuación, seleccione la condición de coincidencia del **Protocolo de solicitud**.
    1. En **Operador**, seleccione **Es igual a**.
    1. En **Valor**, seleccione **HTTP**.
   
   [![Condición de coincidencia de regla de Azure CDN](./media/cdn-standard-rules-engine/cdn-match-condition.png)](./media/cdn-standard-rules-engine/cdn-match-condition.png#lightbox)
   
   > [!NOTE]
   > Puede seleccionar entre varias condiciones de coincidencia en la lista desplegable **Agregar condición**. Para ver una lista detallada de las condiciones de coincidencia, consulte [Condiciones de coincidencia del motor de reglas estándar](cdn-standard-rules-engine-match-conditions.md).
   
1. Seleccione la acción que se va a aplicar a las solicitudes que cumplen la condición de coincidencia:
   1. Seleccione **Agregar acción** y, a continuación, seleccione **Redireccionamiento de la URL**.
   1. En **Tipo**, seleccione **Encontrado (302)** .
   1. En **Protocol** (Protocolo), seleccione **HTTPS**.
   1. Deje el resto de campos en blanco para usar los valores entrantes.
   
   [![Acción de regla de Azure CDN](./media/cdn-standard-rules-engine/cdn-action.png)](./media/cdn-standard-rules-engine/cdn-action.png#lightbox)
   
   > [!NOTE]
   > Puede seleccionar entre varias acciones en la lista desplegable **Agregar acción**. Para obtener una lista detallada de acciones, consulte [Acciones del motor de reglas estándar](cdn-standard-rules-engine-actions.md).

6. Seleccione **Guardar** para guardar la regla nueva. La regla ahora está disponible para usar.
   
   > [!IMPORTANT]
   > Los cambios en las reglas pueden tardar hasta 15 minutos en propagarse a través de Azure CDN.
   >
   

## <a name="next-steps"></a>Pasos siguientes

- [Información general de Azure CDN](cdn-overview.md)
- [Referencia del motor de reglas estándar](cdn-standard-rules-engine-reference.md)
- [Condiciones de coincidencia en el motor de reglas estándar](cdn-standard-rules-engine-match-conditions.md)
- [Acciones en el motor de reglas estándar](cdn-standard-rules-engine-actions.md)
