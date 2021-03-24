---
title: Plantillas de Azure Notification Hubs
description: Aprenda a usar las plantillas de Azure Notification Hubs.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 02/16/2021
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: ee42512a468f4ff86ad7ba273d3971fd124779e2
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100635649"
---
# <a name="notification-hubs-templates"></a>Plantillas de Notification Hubs

Las plantillas permiten que una aplicación cliente especifique el formato exacto de la notificación que desea recibir. Con las plantillas, una aplicación puede obtener muchos beneficios distintos, incluidos los siguientes:

- Un back-end independiente de la plataforma
- Notificaciones personalizadas
- Independencia de la versión del cliente
- Localización sencilla

Esta sección proporciona dos ejemplos detallados sobre cómo usar las plantillas para enviar notificaciones independientes de la plataforma orientadas a todos los dispositivos en todas las plataformas y para personalizar las notificaciones de difusión a cada dispositivo.

## <a name="using-templates-cross-platform"></a>Uso de plantillas multiplataforma

La forma estándar de enviar notificaciones push es enviar una carga específica a los servicios de notificación de plataforma (WNS, APNS) para cada notificación que se debe enviar. Por ejemplo, para enviar una alerta a APNS, la carga es un objeto JSON con el formato siguiente:

```json
{"aps": {"alert" : "Hello!" }}
```

Para enviar un mensaje de notificación del sistema similar en una aplicación de la Tienda Windows, la carga XML es la siguiente:

```xml
<toast>
  <visual>
    <binding template=\"ToastText01\">
      <text id=\"1\">Hello!</text>
    </binding>
  </visual>
</toast>
```

Puede crear cargas similares para plataformas MPNS (Windows Phone) y FCM (Android).

Este requisito obliga al back-end de la aplicación a generar cargas distintas para cada plataforma y, de manera efectiva, hace que el back-end sea responsable de parte del nivel de presentación de la aplicación. Algunos problemas incluye diseños gráficos y de localización (especialmente para las aplicaciones de la Tienda Windows que incluyen notificaciones para varios tipos de iconos).

La característica de plantilla de Notification Hubs permite que una aplicación cliente cree registros especiales, llamados registros de plantilla, que, además del conjunto de etiquetas, incluye una plantilla. La característica de plantilla de Notification Hubs permite que una aplicación cliente asocie los dispositivos con plantillas, tanto si trabaja con instalaciones (la opción de preferencia) como con registros. Dados los ejemplos de carga anteriores, la única información independiente de la plataforma es el propio mensaje de alerta (**Hello!** ). Una plantilla es un conjunto de instrucciones para el centro de notificaciones sobre cómo dar formato a un mensaje independiente de la plataforma para el registro de esa aplicación cliente específica. En el ejemplo anterior, el mensaje independiente de la plataforma es una propiedad única: `message = Hello!`.

La figura siguiente ilustra el proceso:

![Diagrama que muestra el proceso de uso de las plantillas multiplataforma](./media/notification-hubs-templates/notification-hubs-hello.png)

La plantilla para el registro de una aplicación cliente de iOS es la siguiente:

```json
{"aps": {"alert": "$(message)"}}
```

La plantilla correspondiente a una aplicación cliente de la Tienda Windows es:

```xml
<toast>
    <visual>
        <binding template=\"ToastText01\">
            <text id=\"1\">$(message)</text>
        </binding>
    </visual>
</toast>
```

Observe que la expresión `$(message)` sustituye al mensaje real. Cada vez que se envía un mensaje a este registro en particular, esta expresión indica al centro de notificaciones que cree un mensaje según las indicaciones e inserta el valor común.

Si trabaja con el modelo de instalación, la clave "templates" (plantillas) de la instalación contiene el código JSON de varias plantillas. Si trabaja con el modelo de registro, la aplicación cliente puede crear varios registros para usar varias plantillas; por ejemplo, una plantilla para los mensajes de alerta y una plantilla para las actualizaciones de iconos. Las aplicaciones cliente también pueden combinar los registros nativos (registros sin plantilla) y los registros de plantilla.

El centro de notificaciones envía una notificación para cada plantilla sin tener en cuenta si pertenecen a la misma aplicación cliente. Este comportamiento se puede usar para traducir las notificaciones independientes de la plataforma en más notificaciones. Por ejemplo, el mismo mensaje independiente de la plataforma al centro de notificaciones se puede traducir sin problemas en una alerta de notificación del sistema y una actualización de iconos, sin que sea necesario que el back-end lo sepa. Algunas plataformas (por ejemplo, iOS) pueden contraer varias notificaciones al mismo dispositivo si se envían en un período breve.

## <a name="using-templates-for-personalization"></a>Uso de plantillas para personalización

Otra ventaja de usar plantillas es la capacidad de usar Notification Hubs para ejecutar la personalización por registro de las notificaciones. Por ejemplo, considere una aplicación para el clima que muestra un icono con las condiciones climáticas de una ubicación específica. Un usuario puede elegir entre grados Celsius o Fahrenheit, además de un pronóstico de un día o de cinco días. Con las plantillas, cada instalación de aplicación cliente puede registrar el formato requerido (pronóstico de 1 día en grados Celsius, pronóstico de 1 día en grados Fahrenheit, pronóstico de 5 días en grados Celsius, pronóstico de 5 días en grados Fahrenheit) y hacer que el back-end envíe un mensaje único con toda la información necesaria para rellenar esas plantillas (por ejemplo, un pronóstico de 5 días con grados Celsius y Fahrenheit).

La plantilla para un pronóstico de 1 día con temperaturas expresadas en Celsius es la siguiente:

```xml
<tile>
  <visual>
    <binding template="TileWideSmallImageAndText04">
      <image id="1" src="$(day1_image)" alt="alt text"/>
      <text id="1">Seattle, WA</text>
      <text id="2">$(day1_tempC)</text>
    </binding>  
  </visual>
</tile>
```

El mensaje enviado al centro de notificaciones contiene todas las propiedades siguientes:

| day1_image | day2_image | day3_image | day4_image | day5_image |
|------------|------------|------------|------------|------------|
| day1_tempC | day2_tempC | day3_tempC | day4_tempC | day5_tempC |
| day1_tempF | day2_tempF | day3_tempF | day4_tempF | day5_tempF |

Con este patrón, el back-end solo envía un mensaje único sin tener que almacenar opciones de personalización específicas para los usuarios de la aplicación. En la siguiente ilustración se muestra este escenario:

![Diagrama que muestra cómo el back-end solo envía un único mensaje a cada plataforma.](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a>Registro de las plantillas

Para registrar las plantillas con el modelo de instalación (la opción de preferencia) o el modelo de registro, consulte [Administración de registros](notification-hubs-push-notification-registration-management.md).

## <a name="template-expression-language"></a>Lenguaje de expresión de plantilla

Las plantillas se limitan a los formatos de documento XML o JSON. Además, solo puede ubicar expresiones en lugares específicos; por ejemplo, valores o atributos de nodo para XML, valores de propiedad de cadena para JSON.

La tabla siguiente muestra el lenguaje que se permite en las plantillas:

| Expression       | Descripción |
| ---------------- | --- |
| $(prop)          | Referencia a una propiedad de evento con el nombre especificado. Los nombres de propiedad no distinguen mayúsculas de minúsculas. Esta expresión se resuelve en el valor de texto de la propiedad o en una cadena vacía, si la propiedad no está presente. |
|$(prop, n)       | Igual que el caso anterior, pero el texto se recorta explícitamente en n caracteres, por ejemplo, $(title, 20) recorta el contenido de la propiedad title en 20 caracteres. |
| .(prop, n)      | Igual que el caso anterior, pero se agregan tres puntos como sufijo al texto debido a que se recorta. El tamaño total de la cadena recortada y el sufijo no supera los n caracteres. (title, 20), con lo que la propiedad de entrada "This is the title line" daría como resultado **This is the title..** . |
| %(prop)          | Similar a $(name), salvo en que la salida está codificada en URI. |
| #(prop)          | Se usa en las plantillas JSON (por ejemplo, para plantillas de iOS y Android).<br><br>Esta función se comporta igual que la especificada anteriormente '$(prop)', salvo cuando se usa en plantillas JSON (por ejemplo, plantillas de Apple). En este caso, si esta función no está entre "{','}" (por ejemplo, 'myJsonProperty' : '#(name)') y se evalúa en un número en formato JavaScript, por ejemplo, regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*))(\.&#91;0-9&#93;+)?((e&#124;E)(+&#124;-)?&#91;0-9&#93;+)?, el código JSON de salida es un número.<br><br>Por ejemplo, 'badge: '#(name)' se convierte en 'badge' : 40 (y no '40'). |
| 'text' o "text" | Literal. Los literales contienen texto arbitrario encerrado entre comillas simples o dobles. |
| expr1 + expr2    | El operador de concatenación que une dos expresiones en una sola cadena. |

Las expresiones pueden estar en cualquiera de los formatos anteriores.

Cuando se usa la concatenación, toda la expresión debe estar entre `{}`. Por ejemplo, `{$(prop) + ‘ - ’ + $(prop2)}`.

El siguiente ejemplo no es una plantilla XML válida:

```xml
<tile>
  <visual>
    <binding $(property)>
      <text id="1">Seattle, WA</text>
    </binding>  
  </visual>
</tile>
```

Como se explicó anteriormente, cuando se usa la concatenación, las expresiones deben ir entre corchetes. Por ejemplo:

```xml
<tile>
  <visual>
    <binding template="ToastText01">
      <text id="1">{'Hi, ' + $(name)}</text>
    </binding>  
  </visual>
</tile>
```

## <a name="next-steps"></a>Pasos siguientes

[Más información acerca de Azure Notification Hubs](notification-hubs-push-notification-overview.md)
