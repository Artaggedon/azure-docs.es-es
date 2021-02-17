---
title: Azure Monitor Application Insights para Java
description: Supervisión del rendimiento de aplicaciones para aplicaciones Java que se ejecutan en cualquier entorno sin necesidad de modificar el código. Seguimiento distribuido y mapa de aplicación.
ms.topic: conceptual
ms.date: 03/29/2020
author: MS-jgol
ms.custom: devx-track-java
ms.author: jgol
ms.openlocfilehash: 811827c1053349d4fa80a25e5cf362331e5d87bc
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100383184"
---
# <a name="java-codeless-application-monitoring-azure-monitor-application-insights"></a>Supervisión de aplicaciones sin código de Java con Azure Monitor Application Insights

La supervisión de aplicaciones Java sin código destaca por su simplicidad. No hace falta que realice ningún cambio en el código, ya que el agente de Java se puede habilitar mediante un par de cambios de configuración.

 El agente de Java funciona en cualquier entorno y le permite supervisar todas sus aplicaciones Java. En otras palabras, tanto si ejecuta las aplicaciones Java en máquinas virtuales, en el entorno local, en AKS, en Windows o en Linux (usted decide), el agente de Java 3.0 las supervisará.

Ya no es necesario que agregue el SDK de Application Insights para Java a la aplicación, ya que el agente 3.0 recopila solicitudes, dependencias y registros por su cuenta automáticamente.

Así mismo, puede enviar telemetría personalizada desde la aplicación. El agente 3.0 realizará un seguimiento de esta junto con la telemetría recopilada automáticamente.

El agente 3.0 admite Java 8 y versiones posteriores.

## <a name="quickstart"></a>Guía de inicio rápido

**1. Descargue el agente.**

> [!WARNING]
> **Si va actualizar desde la versión preliminar 3.0**
>
> Revise todas las [opciones de configuración](./java-standalone-config.md) con cuidado, ya que la estructura JSON ha cambiado por completo, además del nombre de archivo, que es en minúsculas.

Descargue [applicationinsights-agent-3.0.2.jar](https://github.com/microsoft/ApplicationInsights-Java/releases/download/3.0.2/applicationinsights-agent-3.0.2.jar)

**2. Apunte JVM al agente.**

Agregue `-javaagent:path/to/applicationinsights-agent-3.0.2.jar` a los argumentos de JVM de la aplicación.

Los argumentos típicos de JVM son `-Xmx512m` y `-XX:+UseG1GC`. Por lo tanto, si sabe dónde debe agregarlos, lo mismo se aplica para este.

Para obtener ayuda adicional con la configuración de los argumentos de JVM de la aplicación, consulte [Sugerencias para actualizar los argumentos de JVM](./java-standalone-arguments.md).

**3. Apunte el agente al recurso de Application Insights.**

Si aún no tiene ningún recurso de Application Insights, puede crear uno nuevo siguiendo los pasos descritos en la [guía de creación de recursos](./create-new-resource.md).

Apunte el agente hacia el recurso de Application Insights, ya sea estableciendo una variable de entorno:

```
APPLICATIONINSIGHTS_CONNECTION_STRING=InstrumentationKey=...
```

O creando un archivo de configuración denominado `applicationinsights.json` y colocándolo en el mismo directorio que `applicationinsights-agent-3.0.2.jar`, con el siguiente contenido:

```json
{
  "connectionString": "InstrumentationKey=..."
}
```

Puede encontrar la cadena de conexión en el recurso de Application Insights:

:::image type="content" source="media/java-ipa/connection-string.png" alt-text="Cadena de conexión a Application Insights":::

**4. ¡Ya está!**

Ahora, inicie la aplicación y vaya a su recurso de Application Insights en Azure Portal para ver los datos de supervisión.

> [!NOTE]
> Los datos de supervisión pueden tardar un par de minutos en aparecer en el portal.


## <a name="configuration-options"></a>Opciones de configuración

En el archivo `applicationinsights.json`, también puede configurar lo siguiente:

* Nombre del rol en la nube
* Instancia de rol en la nube
* muestreo
* Métricas JMX
* Dimensiones personalizadas
* Procesadores de telemetría (versión preliminar)
* Registros recopilados automáticamente
* Métricas de Micrometer recopiladas automáticamente (incluidas las métricas del accionador de Spring Boot)
* Latido
* Proxy HTTP
* Diagnóstico automático

Consulte las [opciones de configuración](./java-standalone-config.md) para obtener todos los detalles.

## <a name="auto-collected-requests-dependencies-logs-and-metrics"></a>Solicitudes, dependencias, registros y métricas recopilados automáticamente

### <a name="requests"></a>Requests

* Consumidores de JMS
* Consumidores de Kafka
* Netty/WebFlux
* Servlets
* Programación de Spring

### <a name="dependencies-with-distributed-trace-propagation"></a>Dependencias con propagación de seguimiento distribuido

* Apache HttpClient y HttpAsyncClient
* gRPC
* java.net.HttpURLConnection
* JMS
* Kafka
* Cliente Netty
* OkHttp

### <a name="other-dependencies"></a>Otras dependencias

* Cassandra
* JDBC
* MongoDB (asincrónico y sincrónico)
* Redis (Lettuce y Jedis)

### <a name="logs"></a>Registros

* java.util.logging
* Log4j (incluidas las propiedades de MDC)
* SLF4J/Logback (incluidas las propiedades de MDC)

### <a name="metrics"></a>Métricas

* Micrometer (incluidas las métricas del actuador de Spring Boot)
* Métricas JMX

## <a name="send-custom-telemetry-from-your-application"></a>Envío de telemetría personalizada desde la aplicación

Nuestro objetivo con la versión 3.0 y las posteriores es permitirle enviar telemetría personalizada mediante API estándar.

Por ahora, puede usar Micrometer, plataformas de registro populares y el SDK de Java 2.x de Application Insights.
La versión de Java 3.0 de Application Insights capturará automáticamente la telemetría, la enviará a través de las API y la correlacionará con la telemetría recopilada automáticamente.

### <a name="supported-custom-telemetry"></a>Telemetría personalizada admitida

En la tabla siguiente se representan los tipos de telemetría personalizados admitidos actualmente que se pueden habilitar para complementar el agente Java 3.0. En resumen, las métricas personalizadas se admiten a través de Micrometer, las excepciones personalizadas y los seguimientos se pueden habilitar a través de las plataformas de registro, y se admite cualquier tipo de telemetría personalizada a través del [SDK de Application Insights Java 2.x](#send-custom-telemetry-using-the-2x-sdk).

|                     | Micrómetro | Log4j, logback, JUL | SDK 2.x |
|---------------------|------------|---------------------|---------|
| **Eventos personalizados**   |            |                     |  Sí    |
| **Métricas personalizadas**  |  Sí       |                     |  Sí    |
| **Dependencias**    |            |                     |  Sí    |
| **Excepciones**      |            |  Sí                |  Sí    |
| **Vistas de página**      |            |                     |  Sí    |
| **Solicitudes**        |            |                     |  Sí    |
| **Traces**          |            |  Sí                |  Sí    |

No tenemos previsto publicar ningún SDK con Application Insights 3.0 en este momento.

Java 3.0 de Application Insights ya escucha la telemetría que se envía al SDK de Java 2.x de Application Insights. Esta funcionalidad es una parte importante de la historia de actualización de los usuarios existentes de 2.x y llena una brecha importante en la compatibilidad con la telemetría personalizada hasta que OpenTelemetry API ofrezca disponibilidad general.

### <a name="send-custom-metrics-using-micrometer"></a>Envío de métricas personalizadas mediante Micrometer

Agregue Micrometer a la aplicación:

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-core</artifactId>
  <version>1.6.1</version>
</dependency>
```

Use el [registro global](https://micrometer.io/docs/concepts#_global_registry) de Micrometer para crear un medidor:

```java
static final Counter counter = Metrics.counter("test_counter");
```

y úselo para registrar las métricas:

```java
counter.increment();
```

### <a name="send-custom-traces-and-exceptions-using-your-favorite-logging-framework"></a>Envío de seguimientos y excepciones personalizados mediante su plataforma de registro favorita

Log4j, Logback y java.util.logging se instrumentan automáticamente y los registros creados mediante estas plataformas de registro se recopilan automáticamente como telemetría de seguimiento y excepciones.

De forma predeterminada, los registros solo se recopilan cuando se crean en el nivel INFO o superior.
Consulte las [opciones de configuración](./java-standalone-config.md#auto-collected-logging) para cambiar este nivel.

Si quiere adjuntar dimensiones personalizadas a los registros, puede usar [Log4j 1.2 MDC](https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/MDC.html), [Log4j 2 MDC](https://logging.apache.org/log4j/2.x/manual/thread-context.html) o [Logback MDC](http://logback.qos.ch/manual/mdc.html), y Java 3.0 de Application Insights se encargará de capturar automáticamente las propiedades de MDC como dimensiones personalizadas en la telemetría de seguimiento y excepciones.

### <a name="send-custom-telemetry-using-the-2x-sdk"></a>Envío de telemetría personalizada mediante el SDK 2.x

Agregue `applicationinsights-core-2.6.2.jar` a la aplicación (todas las versiones 2.x son compatibles con Application Insights Java 3.0, pero merece la pena usar la más reciente si es posible):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-core</artifactId>
  <version>2.6.2</version>
</dependency>
```

Cree un elemento TelemetryClient:

  ```java
static final TelemetryClient telemetryClient = new TelemetryClient();
```

y úselo para enviar telemetría personalizada:

##### <a name="events"></a>Eventos

```java
telemetryClient.trackEvent("WinGame");
```

##### <a name="metrics"></a>Métricas

```java
telemetryClient.trackMetric("queueLength", 42.0);
```

##### <a name="dependencies"></a>Dependencias

```java
boolean success = false;
long startTime = System.currentTimeMillis();
try {
    success = dependency.call();
} finally {
    long endTime = System.currentTimeMillis();
    RemoteDependencyTelemetry telemetry = new RemoteDependencyTelemetry();
    telemetry.setSuccess(success);
    telemetry.setTimestamp(new Date(startTime));
    telemetry.setDuration(new Duration(endTime - startTime));
    telemetryClient.trackDependency(telemetry);
}
```

##### <a name="logs"></a>Registros

```java
telemetryClient.trackTrace(message, SeverityLevel.Warning, properties);
```

##### <a name="exceptions"></a>Excepciones

```java
try {
    ...
} catch (Exception e) {
    telemetryClient.trackException(e);
}
```

### <a name="add-request-custom-dimensions-using-the-2x-sdk"></a>Adición de dimensiones personalizadas de solicitud mediante el SDK 2.x

> [!NOTE]
> Esta característica solo está disponible en la versión 3.0.2 y versiones posteriores.

Agregue `applicationinsights-web-2.6.2.jar` a la aplicación (todas las versiones 2.x son compatibles con Application Insights Java 3.0, pero merece la pena usar la más reciente si es posible):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

Agregue también dimensiones personalizadas al código:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.getProperties().put("mydimension", "myvalue");
```

### <a name="set-the-request-telemetry-user_id-using-the-2x-sdk"></a>Establecimiento de user_Id de telemetría de solicitud con el SDK 2.x

> [!NOTE]
> Esta característica solo está disponible en la versión 3.0.2 y versiones posteriores.

Agregue `applicationinsights-web-2.6.2.jar` a la aplicación (todas las versiones 2.x son compatibles con Application Insights Java 3.0, pero merece la pena usar la más reciente si es posible):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

Establezca `user_Id` en el código:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.getContext().getUser().setId("myuser");
```

### <a name="override-the-request-telemetry-name-using-the-2x-sdk"></a>Reemplazo del nombre de telemetría de solicitud con el SDK 2.x

> [!NOTE]
> Esta característica solo está disponible en la versión 3.0.2 y versiones posteriores.

Agregue `applicationinsights-web-2.6.2.jar` a la aplicación (todas las versiones 2.x son compatibles con Application Insights Java 3.0, pero merece la pena usar la más reciente si es posible):

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>applicationinsights-web</artifactId>
  <version>2.6.2</version>
</dependency>
```

Establezca el nombre en el código:

```java
import com.microsoft.applicationinsights.web.internal.ThreadContext;

RequestTelemetry requestTelemetry = ThreadContext.getRequestTelemetryContext().getHttpRequestTelemetry();
requestTelemetry.setName("myname");
```
