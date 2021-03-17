---
title: Supervisión de la disponibilidad y la capacidad de respuesta de cualquier sitio web | Microsoft Docs
description: Configure pruebas web en Application Insights. Obtenga alertas si un sitio web deja de estar disponible o responde con lentitud.
ms.topic: conceptual
ms.date: 03/10/2021
ms.reviewer: sdash
ms.openlocfilehash: d7c610e374dcb7b97850d815ba8bb927cdebacfc
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "103012571"
---
# <a name="monitor-the-availability-of-any-website"></a>Supervisión de la disponibilidad de un sitio web

Después de haber implementado la aplicación o sitio web, puede configurar pruebas periódicas para supervisar la disponibilidad y capacidad de respuesta. [Azure Application Insights](./app-insights-overview.md) envía solicitudes web a su aplicación a intervalos regulares desde puntos de todo el mundo. Puede enviar una alerta si la aplicación no responde o si responde de manera demasiada lenta.

Puede configurar pruebas de disponibilidad para cualquier punto de conexión HTTP o HTTPS que sea accesible desde la red pública de Internet. No hace falta realizar cambios en el sitio web que está probando. De hecho, incluso no hace falta que sea un sitio de su propiedad. Puede probar la disponibilidad de una API de REST de la que depende su servicio.

### <a name="types-of-availability-tests"></a>Tipos de pruebas de disponibilidad:

Hay tres tipos de pruebas de disponibilidad:

* [Prueba de ping de la dirección URL](#create-a-url-ping-test): una prueba sencilla que se puede crear en el portal de Azure.
* [Prueba web de varios pasos](availability-multistep.md): una grabación de una secuencia de solicitudes web que se pueden reproducir para probar los escenarios más complejos. Las pruebas web de varios pasos se crean en Visual Studio Enterprise y se carga en el portal para su ejecución.
* [Pruebas de disponibilidad de seguimiento personalizado](/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability): Si decide crear una aplicación personalizada para ejecutar pruebas de disponibilidad, puede usar el método `TrackAvailability()` para enviar los resultados a Application Insights.

**Puede crear hasta 100 pruebas de disponibilidad por recurso de Application Insights.**

> [!IMPORTANT]
> Tanto la [prueba de ping de URL](#create-a-url-ping-test) como la [prueba web de varios pasos](availability-multistep.md) se basan en la infraestructura DNS de la Internet pública para resolver los nombres de dominio de los puntos de conexión de prueba. Esto significa que, si usa un DNS privado, debe asegurarse de que todos los nombres de dominio de la prueba también pueden resolverse mediante los servidores de nombres de dominio públicos o, cuando no sea posible, puede usar las [pruebas de disponibilidad de seguimiento personalizadas](/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) en su lugar.

## <a name="create-an-application-insights-resource"></a>Creación de recursos en Application Insights

Para crear una prueba de disponibilidad, primero deberá crear un recurso de Application Insights. Si ya creó un recurso, siga con la siguiente sección para [crear una prueba de ping de dirección URL](#create-a-url-ping-test).

En Azure Portal, seleccione **Crear un recurso** > **Herramientas para desarrolladores** > **Application Insights** y [cree un recurso de Application Insights](create-new-resource.md).

## <a name="create-a-url-ping-test"></a>Creación de una prueba de ping de la dirección URL

El nombre "prueba de ping de dirección URL" es un poco inapropiado. Para que quede claro, esta prueba no usa ICMP (Protocolo de mensajes de control de Internet) para comprobar la disponibilidad del sitio. En cambio, usa la funcionalidad más avanzada de solicitud HTTP para validar si un punto de conexión responde. También mide el rendimiento asociado con esa respuesta y agrega la capacidad de establecer criterios de éxito personalizados junto con características más avanzadas, como analizar solicitudes dependientes y permitir reintentos.

Para crear la primera solicitud de disponibilidad, abra el panel Disponibilidad y seleccione **Crear prueba**.

![Fill at least the URL of your website](./media/monitor-web-app-availability/availability-create-test-001.png)

### <a name="create-a-test"></a>Creación de una prueba

|Configuración| Explicación
|----|----|----|
|**URL** |  La dirección URL puede ser cualquier página web que desee probar, pero debe ser visible desde la red pública de Internet. La dirección URL puede incluir una cadena de consulta. Así, por ejemplo, se puede ejercitar un poco la base de datos. Si la dirección URL se resuelve en una redirección, la seguimos, hasta 10 redirecciones.|
|**Analizar solicitudes dependientes**| La prueba solicitará imágenes, scripts, archivos de estilo y otros archivos que forman parte de la página web en pruebas. El tiempo de respuesta registrado incluye el tiempo dedicado a obtener estos archivos. La prueba da error si cualquiera de estos recursos no se puede descargar correctamente dentro del tiempo de espera de la prueba entera. Si la opción no está activada, la prueba solo solicita el archivo en la dirección URL que especificó. Si se habilita esta opción, se realiza una comprobación más estricta. Podría producirse un error en la prueba para los casos que puede que no sean evidentes al examinar el sitio de forma manual.
|**Habilitar reintentos**|cuando la prueba da error, se reintenta tras un corto intervalo. Se notifica un error únicamente si los tres intentos sucesivos producen un error. Las sucesivas pruebas se realizan según la frecuencia habitual de la prueba. El reintento se suspende temporalmente hasta que uno se complete correctamente. Esta regla se aplica independientemente en cada ubicación de la prueba. **Se recomienda esta opción**. Como media, cerca del 80 % de los errores desaparecen al reintentar.|
|**Frecuencia de prueba**| establece la frecuencia con que se ejecuta la prueba desde cada ubicación de prueba. Con una frecuencia predeterminada de cinco minutos y cinco ubicaciones de prueba, el sitio se prueba, de media, cada minuto.|
|**Ubicaciones de prueba**| Son los lugares desde donde nuestros servidores envían solicitudes web a la dirección URL. **El número mínimo de ubicaciones de prueba recomendadas es cinco** con el fin de asegurarse de que puede distinguir los problemas del sitio web de los problemas de la red. Puede seleccionar hasta 16 ubicaciones.

**Si la dirección URL no es visible desde la red Internet pública, tiene la opción de abrir selectivamente el firewall para permitir solo las transacciones de prueba**. Para más información sobre las excepciones de firewall para nuestros agentes de prueba de disponibilidad, consulte la [guía de direcciones IP](./ip-addresses.md#availability-tests).

> [!NOTE]
> Se recomienda probar desde varias ubicaciones con un **mínimo de cinco ubicaciones**. Esto es para evitar falsas alarmas que pueden deberse a problemas transitorios con una ubicación específica. Además, hemos descubierto que la configuración óptima es que el **número de ubicaciones de prueba sea igual que el umbral de ubicación de la alerta + 2**.

### <a name="success-criteria"></a>Criterios de éxito

|Configuración| Explicación
|----|----|----|
| **Tiempo de espera de prueba** |reduzca este valor para recibir una alerta sobre las respuestas lentas. La prueba se considera un error si no se han recibido respuestas de su sitio dentro de este período. Si seleccionó **Analizar solicitudes dependientes**, todas las imágenes, archivos de estilo, scripts y otros recursos dependientes se deben haber recibido durante este período.|
| **Respuesta HTTP** | el código de estado devuelto que se considera correcto. 200 es el código que indica que se ha devuelto una página web normal.|
| **Coincidencia de contenido** | Una cadena, como "Bienvenido". Probamos que se produce una coincidencia exacta entre mayúsculas y minúsculas en todas las respuestas. Debe ser una cadena sin formato, sin caracteres comodín. No se olvide de que si el contenido cambia, es posible que tenga que actualizarla. **En la coincidencia de contenido solo se admiten caracteres en inglés** |

### <a name="alerts"></a>Alertas

|Configuración| Explicación
|----|----|----|
|**Casi en tiempo real (versión preliminar)** | Se recomienda usar alertas casi en tiempo real. La configuración de este tipo de alertas se realiza después de crear la prueba de disponibilidad.  |
|**Umbral de la ubicación de la alerta**|se recomienda un mínimo de 3/5 ubicaciones. La relación óptima entre el umbral de ubicación de la alerta y el número de ubicaciones de prueba es **umbral de ubicación de la alerta** = **número de ubicaciones de prueba - 2, con un mínimo de cinco ubicaciones de prueba.**|

### <a name="location-population-tags"></a>Etiquetas para rellenar la ubicación

Se pueden usar las siguientes etiquetas para rellenar el atributo de ubicación geográfica al implementar una prueba de ping de la dirección URL para averiguar la disponibilidad mediante Azure Resource Manager.

#### <a name="azure-gov"></a>Azure Gov

| Display Name (Nombre para mostrar)   | Nombre de rellenado     |
|----------------|---------------------|
| USGov Virginia | usgov-va-azr        |
| USGov: Arizona  | usgov-phx-azr       |
| USGov Texas    | usgov-tx-azr        |
| Departamento de Defensa del este de EE. UU     | usgov-ddeast-azr    |
| Departamento de Defensa de centro de EE. UU.  | usgov-ddcentral-azr |

#### <a name="azure"></a>Azure

| Display Name (Nombre para mostrar)                           | Nombre de rellenado   |
|----------------------------------------|-------------------|
| Este de Australia                         | emea-au-syd-edge  |
| Sur de Brasil                           | latam-br-gru-edge |
| Centro de EE. UU.                             | us-fl-mia-edge    |
| Este de Asia                              | apac-hk-hkn-azr   |
| Este de EE. UU.                                | us-va-ash-azr     |
| Sur de Francia (anteriormente Centro de Francia) | emea-ch-zrh-edge  |
| Centro de Francia                         | emea-fr-pra-edge  |
| Japón Oriental                             | apac-jp-kaw-edge  |
| Norte de Europa                           | emea-gb-db3-azr   |
| Centro-Norte de EE. UU                       | us-il-ch1-azr     |
| Centro-sur de EE. UU.                       | us-tx-sn1-azr     |
| Sudeste de Asia                         | apac-sg-sin-azr   |
| Oeste de Reino Unido                                | emea-se-sto-edge  |
| Oeste de Europa                            | emea-nl-ams-azr   |
| Oeste de EE. UU.                                | us-ca-sjc-azr     |
| Sur de Reino Unido                               | emea-ru-msa-edge  |

## <a name="see-your-availability-test-results"></a>Visualización de los resultados de las pruebas de disponibilidad

Los resultados de la prueba de disponibilidad se pueden visualizar con vistas de línea o vistas de trazado de dispersión.

Después de unos minutos, haga clic en **Actualizar** para ver los resultados.

![Captura de pantalla que muestra la página Disponibilidad con el botón Actualizar resaltado.](./media/monitor-web-app-availability/availability-refresh-002.png)

En la vista de dispersión se muestran ejemplos de los resultados de las pruebas, que incluyen detalles sobre los pasos de las pruebas de diagnóstico. El motor de pruebas almacena los detalles de diagnóstico de las pruebas con errores. En el caso de las pruebas correctas, los detalles de diagnóstico se almacenan para un subconjunto de las ejecuciones. Mantenga el mouse sobre cualquiera de los puntos rojos o verdes para ver la prueba, el nombre de esta y su ubicación.

![Vista de línea](./media/monitor-web-app-availability/availability-scatter-plot-003.png)

Seleccione una prueba o una ubicación determinadas, o bien reduzca el período de tiempo para ver más resultados del período de tiempo que le interese. Use el Explorador de búsqueda para ver los resultados de todas las ejecuciones, o use consultas de Analytics para ejecutar informes personalizados en estos datos.

## <a name="inspect-and-edit-tests"></a>Inspección y edición de pruebas

Para editar, eliminar o deshabilitar temporalmente una prueba, haga clic en el botón de puntos suspensivos junto al nombre de la prueba. Pueden tardar hasta 20 minutos para que los cambios se propaguen a todos los agentes de prueba después de realizar un cambio.

![Visualice los detalles de la prueba. Edición y deshabilitación de una prueba web](./media/monitor-web-app-availability/edit.png)

Tal vez le interese deshabilitar las pruebas de disponibilidad o las reglas de alerta asociadas a ellas mientras esté realizando el mantenimiento del servicio.

## <a name="if-you-see-failures"></a>Si ve errores

Haga clic en un punto rojo.

![Click a red dot](./media/monitor-web-app-availability/open-instance-3.png)

Puede ver los detalles de transacción en todos los componentes desde el resultado de la prueba de disponibilidad. Aquí puede:

* Inspeccionar la respuesta recibida desde el servidor.
* Diagnosticar errores con la telemetría de lado servidor correlacionada que se recopiló durante el procesamiento de la prueba de disponibilidad con error.
* Registrar un problema o elemento de trabajo en GIT o Azure Boards para realizar un seguimiento del problema. El error contiene un vínculo a este evento.
* Abra el resultado de la prueba web en Visual Studio.

Obtenga más información acerca de la experiencia de diagnósticos de transacción extremo a extremo [aquí](./transaction-diagnostics.md).

Haga clic en la fila de excepciones para ver los detalles de la excepción del lado servidor que ha provocado un error en la prueba de disponibilidad sintética. También puede obtener la [instantánea de depuración](./snapshot-debugger.md) para realizar diagnósticos de nivel de código más completos.

![Diagnósticos del servidor](./media/monitor-web-app-availability/open-instance-4.png)

Además de los resultados sin formato, también puede ver dos métricas de disponibilidad clave en el [Explorador de métricas](../essentials/metrics-getting-started.md):

1. Disponibilidad: Porcentaje de las pruebas que obtuvieron resultados satisfactorios en todas las ejecuciones de prueba.
2. Duración de la prueba: Duración media de las pruebas en todas las ejecuciones de prueba.

## <a name="automation"></a>Automation

* [Use scripts de PowerShell para configurar una prueba de disponibilidad](./powershell.md#add-an-availability-test) automáticamente.
* Configure un [webhook](../alerts/alerts-webhooks.md) que se llama cuando se genera una alerta.

## <a name="troubleshooting"></a>Solución de problemas

[Artículo de solución de problemas](troubleshoot-availability.md) dedicado.

## <a name="next-steps"></a>Pasos siguientes

* [Alertas de disponibilidad](availability-alerts.md)
* [Pruebas web de varios pasos](availability-multistep.md)

