---
title: Preguntas frecuentes de Azure App Configuration
description: Lea las respuestas a las preguntas más frecuentes (P+F) sobre Azure App Configuration, por ejemplo, las diferencias con Azure Key Vault.
services: azure-app-configuration
author: AlexandraKemperMS
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: alkemper
ms.openlocfilehash: 39ad20bd57e3da6345c63d4601f34b19e640c1d6
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99256482"
---
# <a name="azure-app-configuration-faq"></a>Preguntas frecuentes de Azure App Configuration

En este artículo se responde a preguntas frecuentes sobre Azure App Configuration.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>¿En qué se diferencia App Configuration de Azure Key Vault?

App Configuration ayuda a los desarrolladores a administrar la configuración de las aplicaciones y a controlar la disponibilidad de características. Pretende simplificar muchas de las tareas del trabajo con datos de configuración complejos.

App Configuration admite:

- Espacios de nombres jerárquicos
- Etiquetado
- Consultas amplias
- Recuperación por lotes
- Operaciones de administración especializadas
- Una interfaz de usuario de administración de características

App Configuration complementa a Key Vault y los dos deben usarse en paralelo en la mayoría de las implementaciones de aplicaciones.

## <a name="should-i-store-secrets-in-app-configuration"></a>¿Debo almacenar secretos en App Configuration?

Aunque App Configuration ofrece mayor seguridad, Key Vault sigue siendo el mejor lugar para almacenar los secretos de la aplicación. Key Vault proporciona cifrado de nivel de hardware, directivas de acceso pormenorizadas y operaciones de administración como la rotación de certificados.

Puede crear valores de App Configuration que hagan referencia a secretos almacenados en Key Vault. Para obtener más información, consulte [Uso de referencias de Key Vault en una aplicación de ASP.NET Core](./use-key-vault-references-dotnet-core.md).

## <a name="does-app-configuration-encrypt-my-data"></a>¿Cifra App Configuration mis datos?

Sí. App Configuration cifra todos los valores de clave que contiene, así como la comunicación de red. Los nombres de clave y las etiquetas se usan como índices para recuperar los datos de configuración y no se cifran.

## <a name="where-does-data-stored-in-app-configuration-reside"></a>¿Dónde residen los datos almacenados en App Configuration? 

Los datos de cliente almacenados en App Configuration residen en la región en la que se creó el almacén de App Configuration del cliente. App Configuration puede replicar datos en [regiones emparejadas](../best-practices-availability-paired-regions.md) para la resistencia de los datos, pero no replicará ni moverá los datos de cliente fuera de su geoárea según lo definido por la [residencia de datos en Azure](https://azure.microsoft.com/global-infrastructure/data-residency/). Los clientes y los usuarios finales pueden mover, copiar o acceder a los datos de cliente desde cualquier ubicación global.

## <a name="how-is-app-configuration-different-from-azure-app-service-settings"></a>¿En qué se diferencia App Configuration de la configuración de Azure App Service?

Azure App Service permite definir la configuración de la aplicación para cada instancia de App Service. Estas configuraciones se pasan como variables de entorno al código de la aplicación. Si lo desea, puede asociar un valor a una ranura de implementación específica. Para obtener más información, consulte [Configuración de aplicaciones](../app-service/configure-common.md#configure-app-settings).

En cambio, Azure App Configuration permite definir una configuración que se puede compartir entre varias aplicaciones. Esto incluye las aplicaciones que se ejecutan en App Service, así como otras plataformas. El código de la aplicación accede a esta configuración mediante los proveedores de configuración para .NET y Java, mediante el SDK de Azure o directamente mediante las API REST.

También puede importar y exportar configuraciones entre App Service y App Configuration. Esta funcionalidad permite configurar rápidamente un nuevo almacén de App Configuration en función de la configuración de App Service existente. También puede compartir la configuración con una aplicación existente que se base en la configuración de App Service.

## <a name="are-there-any-size-limitations-on-keys-and-values-stored-in-app-configuration"></a>¿Hay alguna limitación de tamaño en las claves y los valores almacenados en App Configuration?

Hay un límite de 10 KB para un único elemento de clave-valor.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>¿Cómo debo almacenar las configuraciones para varios entornos (pruebas, ensayo, producción etc.)?

Puede controlar quién accede a App Configuration en cada almacén. Use un almacén independiente para cada entorno que requiera permisos distintos. Este enfoque ofrece el mejor aislamiento de seguridad.

Si no necesita aislamiento de seguridad entre entornos, puede usar etiquetas para diferenciar los valores de configuración. [Uso de etiquetas para habilitar diferentes configuraciones para distintos entornos](./howto-labels-aspnet-core.md) proporciona un ejemplo completo.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>¿Cuáles son los métodos recomendados para usar App Configuration?

Consulte los [procedimientos recomendados](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>¿Cuánto cuesta App Configuration?

Hay dos planes de tarifa:

- Nivel Gratis
- Nivel Estándar.

Si ha creado un almacén antes de la introducción del nivel Estándar, este se mueve automáticamente al nivel Gratis cuando está disponible con carácter general. Puede optar por actualizar al nivel Estándar o continuar en el nivel Gratis.

No se puede degradar un almacén del nivel Estándar al nivel Gratis. Puede crear un nuevo almacén en el nivel Gratis y luego importar los datos de configuración en ese almacén.

## <a name="which-app-configuration-tier-should-i-use"></a>¿Qué nivel de App Configuration debo usar?

Ambos niveles de App Configuration ofrecen funcionalidad básica, como opciones de configuración, marcas de características, referencias de Key Vault, operaciones de administración básicas, métricas y registros.

A continuación se indican algunas consideraciones para elegir un nivel.

- **Recursos por suscripción**: Un recurso se compone de un único almacén de configuración. Cada suscripción está limitada a un almacén de configuración en el nivel Gratis. Las suscripciones pueden tener un número ilimitado de almacenes de configuración en el nivel Estándar.
- **Almacenamiento por recurso**: En el nivel Gratis, cada almacén de configuración está limitado a 10 MB de almacenamiento. En el nivel Estándar, cada almacén de configuración puede usar hasta 1 GB de almacenamiento.
- **Historial de revisiones**: App Configuration almacena un historial de todos los cambios realizados en las claves. En el nivel Gratis, el historial se almacena durante siete días. En el nivel Estándar, el historial se almacena durante 30 días.
- **Cuota de solicitudes**: los almacenes del nivel Gratis se limitan a 1 000 solicitudes al día. Cuando un almacén alcanza 1000 solicitudes, devuelve el código de estado HTTP 429 para todas las solicitudes hasta medianoche UTC.

    Los almacenes de nivel Estándar están limitados a 20 000 solicitudes por hora. Cuando se agota la cuota, se devuelve el código de estado HTTP 429 para todas las solicitudes hasta el final de la hora.

- **Contrato de nivel de servicio**: El nivel Estándar tiene un contrato de nivel de servicio del 99,9 % de disponibilidad. El nivel Gratis no tiene un contrato de nivel de servicio.
- **Características de seguridad**: ambos niveles proporcionan funcionalidad de seguridad básica, lo que incluye el cifrado con claves administradas por Microsoft, la autenticación mediante HMAC o Azure Active Directory, la compatibilidad con RBAC de Azure, la identidad administrada y las etiquetas de servicio. El nivel Estándar ofrece una funcionalidad de seguridad más avanzada, incluida la compatibilidad con Private Link y el cifrado con claves administradas por el cliente.
- **Costo**: Los almacenes del nivel Estándar tienen un cargo de uso diario. Las primeras 200 000 solicitudes de cada día se incluyen en el cargo diario. También hay un cargo por uso por encima del límite por las solicitudes más allá de la asignación diaria. El uso de un almacén del nivel Gratis no supone ningún costo.

## <a name="can-i-upgrade-a-store-from-the-free-tier-to-the-standard-tier-can-i-downgrade-a-store-from-the-standard-tier-to-the-free-tier"></a>¿Puedo actualizar un almacén desde el nivel Gratis al nivel Estándar? ¿Puedo degradar un almacén del nivel Estándar al nivel Gratis?

Puede actualizar desde el nivel Gratis al nivel Estándar en cualquier momento.

No se puede degradar un almacén del nivel Estándar al nivel Gratis. Puede crear un nuevo almacén en el nivel Gratis y, a continuación, [importar los datos de configuración en ese almacén](howto-import-export-data.md).

## <a name="are-there-any-limits-on-the-number-of-requests-made-to-app-configuration"></a>¿Hay algún límite para el número de solicitudes que se realiza a App Configuration?

En App Configuration, al leer los valores de clave, los datos se paginarán, y cada solicitud podrá leer hasta 100 valores de clave. Al escribir los valores de clave, cada solicitud puede crear o actualizar un valor de clave. Esto se admite a través de la API REST, los SDK de App Configuration y los proveedores de configuración. Los almacenes de configuración del nivel Gratis están limitados a 1000 solicitudes al día. Los almacenes de configuración del nivel Estándar pueden experimentar una limitación temporal cuando la tasa de solicitudes supera las 20 000 solicitudes por hora.

Cuando un almacén alcanza su límite, devolverá el código de estado HTTP 429 para todas las solicitudes realizadas hasta que expire el período de tiempo. El encabezado `retry-after-ms` de la respuesta proporciona un tiempo de espera sugerido (en milisegundos) antes de volver a intentar la solicitud.

Si la aplicación recibe periódicamente respuestas de código de estado HTTP 429, considere la posibilidad de rediseñarla para reducir el número de solicitudes realizadas. Para obtener más información, consulte [Reducción de las solicitudes realizadas a App Configuration](./howto-best-practices.md#reduce-requests-made-to-app-configuration).

## <a name="my-application-receives-http-status-code-429-responses-why"></a>Mi aplicación recibe respuestas con el código de estado HTTP 429. ¿Por qué?

Recibirá una respuesta con el código de estado HTTP 429 en estas circunstancias:

* Superación del límite de solicitudes diarias de un almacén en el nivel Gratis.
* Limitación temporal debido a una tasa de solicitudes elevada para un almacén en el nivel Estándar.
* Uso de ancho de banda excesivo.
* Intento de crear o modificar una clave cuando se ha superado la cuota de almacenamiento.

Compruebe el cuerpo de la respuesta 429 para conocer el motivo específico por el que se produjo un error en la solicitud.

## <a name="how-can-i-receive-announcements-on-new-releases-and-other-information-related-to-app-configuration"></a>¿Cómo puedo recibir anuncios sobre nuevas versiones y otra información relacionada con App Configuration?

Suscríbase a nuestro [repositorio de anuncios de GitHub](https://github.com/Azure/AppConfiguration-Announcements).

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>¿Cómo puedo notificar un problema o proporcionar una sugerencia?

Puede comunicarse con nosotros directamente en [GitHub](https://github.com/Azure/AppConfiguration/issues).

## <a name="next-steps"></a>Pasos siguientes

* [Acerca de Azure App Configuration](./overview.md)
