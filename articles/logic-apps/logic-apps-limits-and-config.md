---
title: Límites y configuración
description: Límites de servicio como la duración, el rendimiento y la capacidad, además de valores de configuración como, por ejemplo, las direcciones IP que se van a permitir, para Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 02/05/2021
ms.openlocfilehash: 19c7d37d62ec54e57127f5993e8bae4d4e9a2908
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100388539"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Información de límites y configuración para Azure Logic Apps

En este artículo, se describen los límites y los detalles de configuración para crear y ejecutar flujos de trabajo automatizados con Azure Logic Apps. Para Power Automate, consulte [Límites y configuración en Power Automate](/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="logic-app-definition-limits"></a>Límites de definición de aplicación lógica

Estos son los límites de una definición de aplicación lógica:

| Nombre | Límite | Notas |
| ---- | ----- | ----- |
| Acciones por flujo de trabajo | 500 | Para ampliar este límite, puede agregar flujos de trabajo anidados según sea necesario. |
| Niveles de anidamiento permitidos para acciones | 8 | Para ampliar este límite, puede agregar flujos de trabajo anidados según sea necesario. |
| Flujos de trabajo por región y suscripción | 1,000 | |
| Desencadenadores por flujo de trabajo | 10 | Al trabajar en la vista Código, no en el diseñador |
| Límite de cambio de ámbito de casos | 25 | |
| Variables por flujo de trabajo | 250 | |
| Nombre de `action` o `trigger` | 80 caracteres | |
| Caracteres por expresión | 8192 | |
| Longitud de `description` | 256 caracteres | |
| Número máximo de `parameters` | 50 | |
| Número máximo de `outputs` | 10 | |
| Tamaño máximo de `trackedProperties` | 16 000 caracteres |
| Acción de código en línea: número máximo de caracteres de código | 1024 caracteres | Para ampliar este límite a 100 000 caracteres, cree las aplicaciones lógicas con el tipo de recurso **Logic Apps (versión preliminar)** , [mediante Azure Portal](create-stateful-stateless-workflows-azure-portal.md) o [mediante Visual Studio Code y la extensión **Azure Logic Apps (versión preliminar)**](create-stateful-stateless-workflows-visual-studio-code.md). |
| Acción de código en línea: duración máxima para la ejecución de código | 5 segundos | Para ampliar este límite a 15 segundos, cree las aplicaciones lógicas con el tipo de recurso **Logic Apps (versión preliminar)** , [mediante Azure Portal](create-stateful-stateless-workflows-azure-portal.md) o [mediante Visual Studio Code y la extensión **Azure Logic Apps (versión preliminar)**](create-stateful-stateless-workflows-visual-studio-code.md). |
||||

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-history-limits"></a>Límites del historial de retención y duración de ejecución

Estos son los límites de ejecución de una única aplicación lógica:

| Nombre | Límite de multiinquilino | Límite del entorno del servicio de integración | Notas |
|------|--------------------|---------------------------------------|-------|
| Duración de la ejecución | 90 días | 366 días | Para calcular la duración de la ejecución se usa la hora de inicio de una ejecución y el límite especificado en la configuración del flujo de trabajo, [**Retención del historial de ejecución en días**](#change-duration) en esa hora de inicio. <p><p>Para cambiar el límite predeterminado,vea [Cambio de la retención y el historial de ejecución en el almacenamiento](#change-duration). |
| Retención del historial de ejecución en el almacenamiento | 90 días | 366 días | Cuando la duración de una ejecución supera el límite de retención del historial de ejecución actual, la ejecución se quita del historial de ejecución en el almacenamiento. Tanto si se completa la ejecución como si se agota el tiempo de espera, la retención del historial de ejecución siempre se calcula mediante la hora de inicio de la ejecución y el límite actual especificado en la configuración del flujo de trabajo [**Retención del historial de ejecución en días**](#change-retention). Con independencia del límite anterior, el actual siempre se usa para calcular la retención. <p><p>Para cambiar el límite predeterminado y obtener más información,vea [Cambio de la retención de la duración y el historial de ejecución en el almacenamiento](#change-retention). En cambio, para aumentar el límite máximo, [póngase en contacto con el equipo de Logic Apps](mailto://logicappsemail@microsoft.com) para obtener ayuda con sus requisitos. |
| Intervalo de periodicidad mínima | 1 segundo | 1 segundo ||
| Intervalo de periodicidad máxima | 500 días | 500 días ||
|||||

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-history-retention-in-storage"></a>Cambio de la duración de ejecución y de la retención del historial en el almacenamiento

El mismo valor controla el número máximo de días que un flujo de trabajo puede ejecutarse y mantener el historial de ejecución en el almacenamiento. Para cambiar el valor predeterminado o el límite actual de estas propiedades, siga estos pasos.

* En el caso de las aplicaciones lógicas en Azure multiinquilino, el límite predeterminado de 90 días es el mismo que el límite máximo. Solo puede disminuir este valor.

* En el caso de las aplicaciones lógicas en un entorno de servicio de integración, puede disminuir o aumentar el límite predeterminado de 90 días.

Por ejemplo, imagine que reduce el límite de retención de 90 días a 30. Se quita una ejecución de 60 días de antigüedad del historial de ejecución. Si aumenta el período de retención de 30 a 60 días, una ejecución de 20 días de antigüedad permanece en el historial de ejecución 40 días más.

> [!IMPORTANT]
> Cuando la duración de una ejecución supera el límite de retención del historial de ejecución actual, la ejecución se quita del historial de ejecución en el almacenamiento. Para evitar la pérdida del historial de ejecución, asegúrese de que el límite de retención sea *siempre* mayor que la duración más larga posible de la ejecución.

1. En el cuadro de búsqueda de [Azure Portal](https://portal.azure.com), busque y seleccione **Logic Apps**.

1. Busque y seleccione la aplicación lógica. Abra la aplicación lógica en el Diseñador de aplicaciones lógicas.

1. En el menú de la aplicación lógica, seleccione **Configuración del flujo de trabajo**.

1. En **Opciones del entorno de ejecución**, en la lista **Retención del historial de ejecución en días**, seleccione **Personalizado**.

1. Arrastre el control deslizante para cambiar el número de días que desea.

1. Cuando haya terminado, en la barra de herramientas **Configuración del flujo de trabajo**, seleccione **Guardar**.

Si genera una plantilla de Azure Resource Manager para la aplicación lógica, esta configuración aparece como una propiedad en la definición de recursos del flujo de trabajo, que se describe en la [referencia de plantillas de flujo de trabajo Microsoft.Logic](/azure/templates/microsoft.logic/workflows):

```json
{
   "name": "{logic-app-name}",
   "type": "Microsoft.Logic/workflows",
   "location": "{Azure-region}",
   "apiVersion": "2019-05-01",
   "properties": {
      "definition": {},
      "parameters": {},
      "runtimeConfiguration": {
         "lifetime": {
            "unit": "day",
            "count": {number-of-days}
         }
      }
   }
}
```

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>Límites de simultaneidad, bucle y anulación de procesamiento por lotes

Estos son los límites de ejecución de una única aplicación lógica:

### <a name="loops"></a>Bucles

| Nombre | Límite | Notas |
| ---- | ----- | ----- |
| Elementos de matriz de foreach | 100 000 | Este límite describe el número máximo de elementos de matriz que puede procesar un bucle "for each". <p><p>Para filtrar matrices más grandes, puede usar la [acción de consulta](logic-apps-perform-data-operations.md#filter-array-action). |
| Simultaneidad de foreach | Con la simultaneidad desactivada: 20 <p><p>Con la simultaneidad activada: <p><p>- Predeterminadas: 20 <br>- Mín.: 1 <br>- Máx.: 50 | Este límite es el número máximo de iteraciones de bucles "for each" que se pueden ejecutar al mismo tiempo o en paralelo. <p><p>Para cambiar este límite, consulte [Cambio del límite de simultaneidad de "for each"](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency) o [Ejecución secuencial de bucles "for each"](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each). |
| Iteraciones Until | - Predeterminadas: 60 <br>- Mín.: 1 <br>- Máx.: 5.000 | Número máximo de ciclos que un bucle "Until" puede tener durante la ejecución de una aplicación lógica. <p><p>Para cambiar este límite, en la forma de bucle "Until", seleccione **Cambiar límites** y especifique el valor de la propiedad **Recuento**. |
| Tiempo de espera de Until | - Predeterminadas: PT1H (1 hora) | La cantidad máxima de tiempo que se puede ejecutar el bucle "Until" antes de que se cierre, y se especifica en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). El valor de tiempo de espera se evalúa para cada ciclo del bucle. Si cualquier acción en el bucle tarda más que el límite de tiempo de expiración, el ciclo actual no se detiene. Pero el siguiente ciclo no se inicia porque no se cumple la condición del límite. <p><p>Para cambiar este límite, en la forma de bucle "Until", seleccione **Cambiar límites** y especifique el valor de la propiedad **Tiempo de espera**. |
||||

### <a name="concurrency-and-debatching"></a>Simultaneidad y separación de lotes

| Nombre | Límite | Notas |
| ---- | ----- | ----- |
| Simultaneidad de desencadenadores | Con la simultaneidad desactivada: Sin límite <p><p>Con la simultaneidad activada, que no se puede deshacer después de habilitar: <p><p>- Predeterminadas: 25 <br>- Mín.: 1 <br>- Máx.: 50 | Este límite es el número máximo de instancias de aplicación lógica que se pueden ejecutar al mismo tiempo o en paralelo. <p><p>**Nota**: Cuando se activa la simultaneidad, el límite SplitOn se reduce a 100 elementos para [desagrupación de matrices](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). <p><p>Para cambiar este límite, consulte [Cambio en el límite de simultaneidad del desencadenador](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency) o [Desencadenamiento secuencial de instancias](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger). |
| Número máximo de ejecuciones en espera | Con la simultaneidad desactivada: <p><p>- Mín.: 1 <br>- Máx.: 50 <p><p>Con la simultaneidad activada: <p><p>- Mín.: 10 más el número de ejecuciones simultáneas (simultaneidad del desencadenador) <br>- Máx.: 100 | Este límite es el número máximo de instancias de aplicación lógica que pueden ponerse en espera de ejecución cuando la aplicación lógica ya esté ejecutando el número máximo de instancias simultáneas. <p><p>Para cambiar este límite, consulte [Cambio del límite de ejecuciones en espera](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs). |
| Elementos SplitOn | Con la simultaneidad desactivada: 100 000 <p><p>Con la simultaneidad activada: 100 | Para los desencadenadores que devuelven una matriz, puede especificar una expresión que use la propiedad "SplitOn", la cual [divide o desagrupa los elementos de matriz en varias instancias de flujo de trabajo](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) para su procesamiento, en lugar de usar un bucle "foreach". Esta expresión hace referencia a la matriz que se usará para crear y ejecutar una instancia de flujo de trabajo para cada elemento de la matriz. <p><p>**Nota**: Cuando se activa la simultaneidad, el límite SplitOn se reduce a 100 elementos. |
||||

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>Límites de rendimiento

Estos son los límites de una definición de aplicación lógica:

### <a name="multi-tenant-logic-apps-service"></a>Servicio Logic Apps multiinquilino

| Nombre | Límite | Notas |
| ---- | ----- | ----- |
| Acción: Ejecuciones por cada 5 minutos | 100 000 es el límite predeterminado, pero 300 000 es el límite máximo. | Para elevar el límite predeterminado al máximo en la aplicación lógica, consulte [Ejecución en modo de alto rendimiento](#run-high-throughput-mode), que se encuentra en versión preliminar. También, puede [distribuir la carga de trabajo entre varias aplicaciones lógicas](../logic-apps/handle-throttling-problems-429-errors.md#logic-app-throttling). |
| Acción: Llamadas salientes simultáneas | Aproximadamente 2500 | Puede reducir el número de solicitudes simultáneas o la duración según sea necesario. |
| Punto de conexión en tiempo de ejecución: Llamadas entrantes simultáneas | ~1000 | Puede reducir el número de solicitudes simultáneas o la duración según sea necesario. |
| Punto de conexión en tiempo de ejecución: llamadas de lectura por 5 minutos  | 60 000 | Este límite se aplica a las llamadas que obtienen las entradas y salidas sin procesar del historial de ejecución de una aplicación lógica. Puede distribuir la carga de trabajo entre varias aplicaciones, según sea necesario. |
| Punto de conexión en tiempo de ejecución: llamadas de invocación por 5 minutos | 45 000 | Puede distribuir una carga de trabajo entre varias aplicaciones, según sea necesario. |
| Rendimiento de contenido cada 5 minutos | 600 MB | Puede distribuir una carga de trabajo entre varias aplicaciones, según sea necesario. |
||||

<a name="run-high-throughput-mode"></a>

#### <a name="run-in-high-throughput-mode"></a>Ejecutar en modo de alto rendimiento

Para una única definición de aplicación lógica, el número de acciones que se ejecutan cada cinco minutos tiene un [límite predeterminado](../logic-apps/logic-apps-limits-and-config.md#throughput-limits). Para elevar el límite predeterminado al máximo en la aplicación lógica, puede habilitar el modo de alto rendimiento, que se encuentra en versión preliminar. También, puede [distribuir la carga de trabajo entre varias aplicaciones lógicas](../logic-apps/handle-throttling-problems-429-errors.md#logic-app-throttling).

1. En Azure Portal, en el menú de la aplicación lógica, seleccione **Configuración** y, luego, **Configuración del flujo de trabajo**.

1. En **Opciones del entorno de ejecución** > **Alto rendimiento**, cambie la configuración a **Activado**.

   ![Captura de pantalla que muestra el menú de la aplicación lógica en Azure Portal con "Configuración del flujo de trabajo" y "Alto rendimiento" establecido en "Activado".](./media/logic-apps-limits-and-config/run-high-throughput-mode.png)

Para habilitar este valor en una plantilla de Resource Manager a fin de implementar la aplicación lógica, en el objeto `properties` de la definición de recurso de la aplicación lógica, agregue el objeto `runtimeConfiguration` con la propiedad `operationOptions` establecida en `OptimizedForHighThroughput`:

```json
{
   <template-properties>
   "resources": [
      // Start logic app resource definition
      {
         "properties": {
            <logic-app-resource-definition-properties>,
            <logic-app-workflow-definition>,
            <more-logic-app-resource-definition-properties>,
            "runtimeConfiguration": {
               "operationOptions": "OptimizedForHighThroughput"
            }
         },
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {},
         "apiVersion": "2016-06-01",
         "dependsOn": [
         ]
      }
      // End logic app resource definition
   ],
   "outputs": {}
}
```

Para más información sobre la definición de recurso de la aplicación lógica, consulte [Información general: Automatización de la implementación en Azure Logic Apps mediante plantillas de Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#logic-app-resource-definition).

### <a name="integration-service-environment-ise"></a>Entorno del servicio de integración (ISE)

* [SKU de ISE de desarollador](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level): Proporciona hasta 500 ejecuciones por minuto. Sin embargo, tenga en cuenta estas consideraciones:

  * Asegúrese de usar esta SKU solo para exploración, experimentos, desarrollo o pruebas, no para producción o pruebas de rendimiento. Esta SKU no tiene Acuerdo de Nivel de Servicio, funcionalidad de escalado vertical ni redundancia durante el reciclaje, lo que significa que puede experimentar retrasos o tiempo de inactividad.

  * Las actualizaciones de back-end pueden interrumpir el servicio de forma intermitente.

* [SKU de ISE prémium](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level): En la tabla siguiente se describen los límites de rendimiento de esta SKU; sin embargo, para superar estos límites en el procesamiento normal o ejecutar pruebas de carga que podrían superar estos límites, [póngase en contacto con el equipo de Logic Apps](mailto://logicappsemail@microsoft.com) para que le ayude con sus requisitos.

  | Nombre | Límite | Notas |
  |------|-------|-------|
  | Límite de ejecución de la unidad base | Sistema limitado cuando la capacidad de la infraestructura alcanza el 80 % | Proporciona unas 4000 ejecuciones de acciones por minuto, que equivale a 160 millones de ejecuciones de acciones al mes aproximadamente | |
  | Límite de ejecución de unidad de escalado | Sistema limitado cuando la capacidad de la infraestructura alcanza el 80 % | Cada unidad de escalado puede proporcionar aproximadamente 2000 ejecuciones de acciones adicionales por minuto, que equivale aproximadamente a 80 millones de ejecuciones de acciones más al mes | |
  | Unidades de escalado máximas que puede agregar | 10 | |
  ||||

<a name="gateway-limits"></a>

## <a name="gateway-limits"></a>Límites de puerta de enlace

Azure Logic Apps admite operaciones de escritura, inserciones y actualizaciones incluidas, mediante la puerta de enlace. Sin embargo, estas operaciones tienen [límites en su tamaño de carga](/data-integration/gateway/service-gateway-onprem#considerations).

<a name="http-limits"></a>

## <a name="http-limits"></a>Límites de HTTP

Estos son los límites para una única llamada entrante o saliente:

<a name="http-timeout-limits"></a>

#### <a name="timeout-duration"></a>Duración del tiempo de espera

Algunas operaciones de conector realizan llamadas asincrónicas o escuchan las solicitudes de webhook, por lo que el tiempo de expiración de estas operaciones puede ser superior a estos límites. Para obtener más información, vea los detalles técnicos del conector concreto y también [Acciones y desencadenadores de flujos de trabajo](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| Nombre | Logic Apps (multiinquilino) | Logic Apps (versión preliminar) | entorno de servicio de integración | Notas |
|------|---------------------------|----------------------|---------------------------------|-------|
| Solicitud saliente | 120 segundos <br>(2 minutos) | 230 Segundos <br>(3,9 minutes) | 240 segundos <br>(4 minutos) | Entre los ejemplos de solicitudes salientes se incluyen las llamadas realizadas por desencadenadores o acciones HTTP. Para más información acerca de la versión preliminar, consulte [Versión preliminar de Azure Logic Apps](logic-apps-overview-preview.md). <p><p>**Sugerencia**: Para las operaciones de ejecución más largas, use un [patrón de sondeo asincrónico](../logic-apps/logic-apps-create-api-app.md#async-pattern) o un [bucle Until](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). Para solucionar de forma alternativa los límites de tiempo de espera cuando se llama a otra aplicación lógica que tiene un [punto de conexión al que se puede llamar](logic-apps-http-endpoint.md), puede usar la acción integrada Azure Logic Apps en su lugar, que puede encontrar en el selector de conectores en **Integrado**. |
| Solicitud entrante | 120 segundos <br>(2 minutos) | 230 Segundos <br>(3,9 minutes) | 240 segundos <br>(4 minutos) | Entre los ejemplos de solicitudes entrantes se incluyen las llamadas recibidas por los desencadenadores de solicitud, desencadenadores de webhook HTTP y acciones de webhook HTTP. Para más información acerca de la versión preliminar, consulte [Versión preliminar de Azure Logic Apps](logic-apps-overview-preview.md). <p><p>**Nota**: Para que el autor de llamada original obtenga la respuesta, todos los pasos de la respuesta deben terminar dentro del límite, a menos que llame a otra aplicación lógica como un flujo de trabajo anidado. Para obtener más información, consulte [Llamada, desencadenamiento o anidación de aplicaciones lógicas](../logic-apps/logic-apps-http-endpoint.md). |
||||||

<a name="message-size-limits"></a>

#### <a name="message-size"></a>Tamaño del mensaje

| Nombre | Límite de multiinquilino | Límite del entorno del servicio de integración | Notas |
|------|--------------------|---------------------------------------|-------|
| Tamaño del mensaje | 100 MB | 200 MB | Para evitar este límite, consulte [Handle large messages with chunking](../logic-apps/logic-apps-handle-large-messages.md) (Controlar mensajes grandes con fragmentación). En cambio, puede que algunos conectores y API no admitan la fragmentación ni el límite predeterminado. <p><p>- Los conectores como AS2, X12 y EDIFACT tienen sus propios [límites de mensajes B2B](#b2b-protocol-limits). <br>- Los conectores ISE usan el límite de ISE, no los límites de aquellos que no son ISE. |
| Tamaño del mensaje con fragmentación | 1 GB | 5 GB | Este límite se aplica a las acciones que admiten la fragmentación de forma nativa o que le permiten habilitar la opción de fragmentación en la configuración del entorno de ejecución. <p><p>Si utiliza ISE, el motor de Logic Apps admite este límite, pero los conectores tienen sus propios límites de fragmentación hasta el límite del motor; por ejemplo, consulte la [referencia de API del conector de Azure Blob Storage](/connectors/azureblob/). Para más información sobre la fragmentación, consulte [Control de mensajes grandes con la fragmentación](../logic-apps/logic-apps-handle-large-messages.md). |
|||||

#### <a name="character-limits"></a>Límites de caracteres

| Nombre | Notas |
|------|-------|
| Límite de evaluación de expresiones | 131 072 caracteres. | Las expresiones `@concat()`, `@base64()` y `@string()` no pueden superar este límite. |
| Límite de caracteres de dirección URL de solicitud | 16 384 caracteres |
|||

<a name="retry-policy-limits"></a>

#### <a name="retry-policy"></a>Directiva de reintentos

| Nombre | Límite | Notas |
| ---- | ----- | ----- |
| Número de reintentos | 90 | El valor predeterminado es 4. Para cambiar el valor predeterminado, use el [parámetro de directiva de reintentos](../logic-apps/logic-apps-workflow-actions-triggers.md). |
| Retraso máximo entre reintentos | 1 día | Para cambiar el valor predeterminado, use el [parámetro de directiva de reintentos](../logic-apps/logic-apps-workflow-actions-triggers.md). |
| Retraso mínimo entre reintentos | 5 segundos | Para cambiar el valor predeterminado, use el [parámetro de directiva de reintentos](../logic-apps/logic-apps-workflow-actions-triggers.md). |
||||

<a name="authentication-limits"></a>

### <a name="authentication-limits"></a>Límites de autenticación

Estos son los límites para una aplicación lógica que se inicia con un desencadenador de solicitud y habilita [Azure Active Directory Open Authentication](../active-directory/develop/index.yml) (Azure AD OAuth) para autorizar las llamadas entrantes al desencadenador de solicitud:

| Nombre | Límite | Notas |
| ---- | ----- | ----- |
| Directivas de autorización de Azure AD | 5 | |
| Notificaciones por directiva de autorización | 10 | |
| Valor de notificación: número máximo de caracteres | 150 |
||||

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Límites de conector personalizado

Estos son los límites para los conectores personalizados que puede crear a partir de las API web.

| Nombre | Límite de multiinquilino | Límite del entorno del servicio de integración | Notas |
|------|--------------------|---------------------------------------|-------|
| Número de conectores personalizados | 1000 por cada suscripción de Azure | 1000 por cada suscripción de Azure ||
| Número de solicitudes por minuto para un conector personalizado | 500 solicitudes por minuto por conexión | 2000 solicitudes por minuto por *conector personalizado* ||
|||

<a name="managed-identity"></a>

## <a name="managed-identities"></a>Identidades administradas

| Nombre | Límite |
|------|-------|
| Identidades administradas por aplicación lógica | La identidad asignada por el sistema o una identidad asignada por el usuario |
| Número de aplicaciones lógicas que tienen una identidad administrada en una suscripción de Azure por región | 1,000 |
|||

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Límites de cuenta de integración

Cada suscripción de Azure tiene estos límites de cuenta de integración:

* una cuenta de integración de [nivel Gratis](../logic-apps/logic-apps-pricing.md#integration-accounts) por cada región de Azure. Este nivel solo está disponible para las regiones públicas en Azure, por ejemplo, Oeste de EE. UU. o Sudeste Asiático, pero no para [Azure China 21Vianet](/azure/china/overview-operations) o [Azure Government](../azure-government/documentation-government-welcome.md).

* 1000 cuentas de integración en total, incluidas las cuentas de integración de todos los [entornos de servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) en las SKU de [desarrollador y Premium](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level).

* Cada entorno de servicio de integración (ISE), [Developer o Premium](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level), puede usar una sola cuenta de integración sin costo adicional, aunque el tipo de cuenta incluido varíe por la SKU de ISE. Por un [costo adicional](logic-apps-pricing.md#fixed-pricing), puede crear más cuentas de integración para el ISE hasta el límite total:

  | SKU de ISE | Límites de cuenta de integración |
  |---------|----------------------------|
  | **Premium** | 20 cuentas en total, incluida una cuenta Estándar sin costo adicional. Con esta SKU, solo puede tener cuentas [Estándar](../logic-apps/logic-apps-pricing.md#integration-accounts). No se permiten cuentas Gratis ni Básicas. |
  | **Developer** | 20 cuentas en total, incluida una cuenta [Gratuita](../logic-apps/logic-apps-pricing.md#integration-accounts) (limitada a 1). Con esta SKU, puede tener cualquier combinación: <p>- Una cuenta gratuita y hasta 19 cuentas [Estándar](../logic-apps/logic-apps-pricing.md#integration-accounts). <br>- Ninguna cuenta gratuita y hasta 20 cuentas Estándar. <p>No se permiten cuentas básicas ni gratuitas adicionales. <p><p>**Importante**: Use la [SKU de Desarrollador](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) para experimentar, desarrollar y probar, no para pruebas de rendimiento ni en producción. |
  |||

Para saber cómo funcionan los precios y la facturación para los ISE, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing). Para ver las tarifas de precios, consulte los [precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Límites de artefacto por cuenta de integración

Estos son los límites en cuanto al número de artefactos de cada nivel de cuenta de integración. Para ver las tarifas de precios, consulte los [precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). Para saber cómo funcionan los precios y la facturación de las cuentas de integración, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#integration-accounts).

> [!NOTE]
> Use el nivel Gratis solo en escenarios de exploración, no en escenarios de producción. Este nivel restringe el uso y el rendimiento, y no tiene ningún acuerdo de nivel de servicio (SLA).

| Artefacto | Gratuito | Básica | Estándar |
|----------|------|-------|----------|
| Acuerdos comerciales de EDI | 10 | 1 | 1,000 |
| Socios comerciales de EDI | 25 | 2 | 1,000 |
| Mapas | 25 | 500 | 1,000 |
| Esquemas | 25 | 500 | 1,000 |
| Ensamblados | 10 | 25 | 1,000 |
| Certificados | 25 | 2 | 1,000 |
| Configuraciones por lotes | 5 | 1 | 50 |
||||

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Límites de capacidad de artefactos

| Artefacto | Límite | Notas |
| -------- | ----- | ----- |
| Assembly | 8 MB | Para cargar archivos de más de 2 MB, use una [cuenta de almacenamiento y un contenedor de blobs de Azure](../logic-apps/logic-apps-enterprise-integration-schemas.md). |
| Asignación (archivo XSLT) | 8 MB | Para cargar archivos de más de 2 MB, use las [asignaciones de la API REST de Azure Logic Apps](/rest/api/logic/maps/createorupdate). <p><p>**Nota**: La cantidad de datos o registros que una asignación puede procesar correctamente se basa en el tamaño del mensaje y en los límites de tiempo de espera de la acción en Azure Logic Apps. Por ejemplo, si usa una acción HTTP, en función de los [límites de tamaño y tiempo de espera del mensaje HTTP](#http-limits), una asignación puede procesar los datos hasta el límite de tamaño del mensaje HTTP si la operación se completa dentro del límite de tiempo de espera de HTTP. |
| Schema | 8 MB | Para cargar archivos de más de 2 MB, use una [cuenta de almacenamiento y un contenedor de blobs de Azure](../logic-apps/logic-apps-enterprise-integration-schemas.md). |
||||

<a name="integration-account-throughput-limits"></a>

### <a name="throughput-limits"></a>Límites de rendimiento

| Punto de conexión en tiempo de ejecución | Gratuito | Básica | Estándar | Notas |
|------------------|------|-------|----------|-------|
| Llamadas de lectura cada 5 minutos | 3,000 | 30,000 | 60 000 | Este límite se aplica a las llamadas que obtienen las entradas y salidas sin procesar del historial de ejecución de una aplicación lógica. Puede distribuir la carga de trabajo entre varias cuentas según sea necesario. |
| Invocación de llamadas cada 5 minutos | 3,000 | 30,000 | 45 000 | Puede distribuir la carga de trabajo entre varias cuentas según sea necesario. |
| Seguimiento de llamadas cada 5 minutos | 3,000 | 30,000 | 45 000 | Puede distribuir la carga de trabajo entre varias cuentas según sea necesario. |
| Bloqueo de llamadas simultáneas | ~1000 | ~1000 | ~1000 | Lo mismo para todas las SKU. Puede reducir el número de solicitudes simultáneas o la duración según sea necesario. |
||||

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>Tamaño de mensaje de protocolo B2B (AS2, X12 y EDIFACT)

Estos son los límites de tamaño de mensaje que se aplican a los protocolos B2B:

| Nombre | Límite de multiinquilino | Límite del entorno del servicio de integración | Notas |
|------|--------------------|---------------------------------------|-------|
| AS2 | v2: 100 MB<br>v1: 25 MB | v2: 200 MB <br>v1: 25 MB | Se aplica a la decodificación y la codificación |
| X12 | 50 MB | 50 MB | Se aplica a la decodificación y la codificación |
| EDIFACT | 50 MB | 50 MB | Se aplica a la decodificación y la codificación |
||||

<a name="disable-delete"></a>

## <a name="disabling-or-deleting-logic-apps"></a>Deshabilitación o eliminación de aplicaciones lógicas

Cuando se deshabilita una aplicación lógica, no se crean instancias de nuevas ejecuciones. Todas las ejecuciones en curso y pendientes continuarán hasta que finalicen, lo que puede tardar un tiempo.

Cuando se elimina una aplicación lógica, no se crean instancias de nuevas ejecuciones. Todas las ejecuciones nuevas y pendientes se cancelan. Si tiene miles de ejecuciones, la cancelación puede tardar bastante tiempo en completarse.

<a name="configuration"></a>
<a name="firewall-ip-configuration"></a>

## <a name="firewall-configuration-ip-addresses-and-service-tags"></a>Configuración del firewall: Direcciones IP y etiquetas de servicio

Cuando la aplicación lógica necesita comunicarse a través de un firewall que limite el tráfico a direcciones IP específicas, ese firewall deberá permitir el acceso para *ambas* direcciones IP, la [de entrada](#inbound) y la [de salida](#outbound), que usa el servicio Logic Apps o el entorno de ejecución en la región de Azure donde se encuentra la aplicación lógica. *Todas* las aplicaciones lógicas de una misma región usan los mismos intervalos de direcciones IP.

Por ejemplo, para admitir llamadas que las aplicaciones lógicas de la región Oeste de EE. UU. envían o reciben a través de desencadenadores y acciones integrados, como el [desencadenador o acción HTTP](../connectors/connectors-native-http.md), el firewall debe permitir el acceso para *todas*  las direcciones IP de entrada del servicio Logic Apps *y* las direcciones IP de salida que existen en la región Oeste de EE. UU.

Si la aplicación lógica también usa [conectores administrados](../connectors/apis-list.md#managed-api-connectors), como el conector de Outlook de Office 365 o el conector de SQL, o emplea [conectores personalizados](/connectors/custom-connectors/), el firewall también debe permitir el acceso a *todas* las [direcciones IP de salida del conector administrado](#outbound) en la región de Azure de la aplicación lógica. Además, si usa conectores personalizados que acceden a recursos locales a través del [recurso de puerta de enlace de datos local en Azure](logic-apps-gateway-connection.md), debe configurar la instalación de la puerta de enlace para permitir el acceso a las *[direcciones IP de salida](#outbound) de conectores administrados* correspondientes.

Para obtener más información sobre cómo configurar las opciones de comunicación en la puerta de enlace, consulte los siguientes temas:

* [Ajuste de la configuración de comunicación para la puerta de enlace de datos local](/data-integration/gateway/service-gateway-communication)
* [Configuración de los valores del proxy para la puerta de enlace de datos local](/data-integration/gateway/service-gateway-proxy)

<a name="ip-setup-considerations"></a>

### <a name="firewall-ip-configuration-considerations"></a>Consideraciones sobre la configuración de la IP de firewall

Antes de configurar el firewall con direcciones IP, revise estas consideraciones:

* Si está usando [Power Automate](/power-automate/getting-started), algunas acciones como **HTTP** y **HTTP + OpenAPI** pasan directamente por el servicio Azure Logic Apps y proceden de las direcciones IP que se indican aquí. Para más información sobre las direcciones IP que usa Power Automate, consulte [Límites y configuración para Power Automate](/flow/limits-and-config#ip-address-configuration).

* En el caso de [Azure China 21Vianet](/azure/china/), las direcciones IP fijas o reservadas no están disponibles para [conectores personalizados](../logic-apps/custom-connector-overview.md) y [conectores administrados](../connectors/apis-list.md#managed-api-connectors), como Azure Storage, SQL Server, Office 365 Outlook, etc.

* Si sus aplicaciones lógicas se ejecutan en un [entorno de servicio de integración (ISE)](connect-virtual-network-vnet-isolated-environment-overview.md), asegúrese de [abrir estos puertos también](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#network-ports-for-ise).

* Para ayudarle a simplificar las reglas de seguridad que quiere crear, también puede usar [etiquetas de servicio](../virtual-network/service-tags-overview.md), en lugar de especificar prefijos de dirección IP para cada región. Estas etiquetas funcionan en las regiones en las que está disponible el servicio Logic Apps:

  * **LogicAppsManagement**: representa los prefijos de la dirección IP entrante para el servicio Logic Apps.

  * **LogicApps**: representa los prefijos de la dirección IP saliente para el servicio Logic Apps.

  * **AzureConnectors**: representa los prefijos de dirección IP que se usan para los conectores administrados que realizan devoluciones de llamada de webhook entrantes al servicio de Logic Apps y a las llamadas salientes a sus servicios respectivos, como Azure Storage o Azure Event Hubs.

* Si sus aplicaciones lógicas tienen problemas de acceso a cuentas de almacenamiento de Azure que usan [firewalls y reglas de firewall](../storage/common/storage-network-security.md), dispone de [otras tantas opciones para habilitar el acceso](../connectors/connectors-create-api-azureblobstorage.md#access-storage-accounts-behind-firewalls).

  Por ejemplo, las aplicaciones lógicas no pueden acceder directamente a cuentas de almacenamiento que usen reglas de firewall y que se encuentren en la misma región. Sin embargo, si permite las [direcciones IP de salida para los conectores administrados de la región](../logic-apps/logic-apps-limits-and-config.md#outbound), las aplicaciones lógicas pueden acceder a las cuentas de almacenamiento de una región diferente excepto cuando se usan los conectores de Azure Table Storage o de Azure Queue Storage. Para acceder a Table Storage o Queue Storage, puede usar el desencadenador HTTP y las acciones en su lugar. Para otras opciones, consulte [Access storage accounts behind firewalls](../connectors/connectors-create-api-azureblobstorage.md#access-storage-accounts-behind-firewalls) (Acceso a cuentas de almacenamiento detrás de firewalls).

<a name="inbound"></a>

### <a name="inbound-ip-addresses"></a>Direcciones IP entrantes

En esta sección se enumeran las direcciones IP entrantes solo para el servicio Azure Logic Apps. Si tiene Azure Government, consulte [Azure Government: direcciones IP entrantes](#azure-government-inbound).

> [!TIP]
> Para ayudar a reducir la complejidad al crear reglas de seguridad, tiene la opción de usar la [etiqueta de servicio](../virtual-network/service-tags-overview.md) **LogicAppsManagement**, en lugar de especificar prefijos de dirección IP entrante de Logic Apps para cada región. Opcionalmente, también puede usar la etiqueta de servicio **AzureConnectors** para los conectores administrados que realizan devoluciones de llamada de webhook de entrada al servicio de Logic Apps, en lugar de especificar prefijos de direcciones IP de conectores administrados de entrada para cada región. Estas etiquetas funcionan en las regiones en las que está disponible el servicio Logic Apps.
>
> Los siguientes conectores realizan devoluciones de llamada de webhook entrantes al servicio Logic Apps:
>
> Adobe Creative Cloud, Adobe Sign, Adobe Sign Demo, Adobe Sign Preview, Adobe Sign Stage, Azure Sentinel, Business Central, Calendly, Common Data Service, DocuSign, DocuSign Demo, Dynamics 365 for Financials y Operations, LiveChat, Office 365 Outlook, Outlook.com, Parserr, SAP*, turnos para Microsoft Teams, Teamwork Projects, Typeform
>
> \* **SAP**: el autor de llamada de devolución depende de si el entorno de implementación es Azure o ISE de varios inquilinos. En el entorno multiinquilino, la puerta de enlace de datos local realiza la devolución de llamada al servicio Logic Apps. En un ISE, el conector SAP realiza la devolución de llamada al servicio Logic Apps.

<a name="multi-tenant-inbound"></a>

#### <a name="multi-tenant-azure---inbound-ip-addresses"></a>Azure de varios inquilinos: direcciones IP entrantes

| Región de varios inquilinos | IP |
|---------------------|----|
| Este de Australia | 13.75.153.66, 104.210.89.222, 104.210.89.244, 52.187.231.161 |
| Sudeste de Australia | 13.73.115.153, 40.115.78.70, 40.115.78.237, 52.189.216.28 |
| Sur de Brasil | 191.235.86.199, 191.235.95.229, 191.235.94.220, 191.234.166.198 |
| Centro de Canadá | 13.88.249.209, 52.233.30.218, 52.233.29.79, 40.85.241.105 |
| Este de Canadá | 52.232.129.143, 52.229.125.57, 52.232.133.109, 40.86.202.42 |
| Centro de la India | 52.172.157.194, 52.172.184.192, 52.172.191.194, 104.211.73.195 |
| Centro de EE. UU. | 13.67.236.76, 40.77.111.254, 40.77.31.87, 104.43.243.39 |
| Este de Asia | 168.63.200.173, 13.75.89.159, 23.97.68.172, 40.83.98.194 |
| Este de EE. UU. | 137.135.106.54, 40.117.99.79, 40.117.100.228, 137.116.126.165 |
| Este de EE. UU. 2 | 40.84.25.234, 40.79.44.7, 40.84.59.136, 40.70.27.253 |
| Centro de Francia | 52.143.162.83, 20.188.33.169, 52.143.156.55, 52.143.158.203 |
| Sur de Francia | 52.136.131.145, 52.136.129.121, 52.136.130.89, 52.136.131.4 |
| Norte de Alemania | 51.116.211.29, 51.116.208.132, 51.116.208.37, 51.116.208.64 |
| Centro-oeste de Alemania | 51.116.168.222, 51.116.171.209, 51.116.233.40, 51.116.175.0 |
| Japón Oriental | 13.71.146.140, 13.78.84.187, 13.78.62.130, 13.78.43.164 |
| Japón Occidental | 40.74.140.173, 40.74.81.13, 40.74.85.215, 40.74.68.85 |
| Centro de Corea del Sur | 52.231.14.182, 52.231.103.142, 52.231.39.29, 52.231.14.42 |
| Corea del Sur | 52.231.166.168, 52.231.163.55, 52.231.163.150, 52.231.192.64 |
| Centro-Norte de EE. UU | 168.62.249.81, 157.56.12.202, 65.52.211.164, 65.52.9.64 |
| Norte de Europa | 13.79.173.49, 52.169.218.253, 52.169.220.174, 40.112.90.39 |
| Este de Noruega | 51.120.88.93, 51.13.66.86, 51.120.89.182, 51.120.88.77 |
| Norte de Sudáfrica | 102.133.228.4, 102.133.224.125, 102.133.226.199, 102.133.228.9 |
| Oeste de Sudáfrica | 102.133.72.190, 102.133.72.145, 102.133.72.184, 102.133.72.173 |
| Centro-sur de EE. UU. | 13.65.98.39, 13.84.41.46, 13.84.43.45, 40.84.138.132 |
| Sur de la India | 52.172.9.47, 52.172.49.43, 52.172.51.140, 104.211.225.152 |
| Sudeste de Asia | 52.163.93.214, 52.187.65.81, 52.187.65.155, 104.215.181.6 |
| Norte de Suiza | 51.103.128.52, 51.103.132.236, 51.103.134.138, 51.103.136.209 |
| Oeste de Suiza | 51.107.225.180, 51.107.225.167, 51.107.225.163, 51.107.239.66 |
| Centro de Emiratos Árabes Unidos | 20.45.75.193, 20.45.64.29, 20.45.64.87, 20.45.71.213 |
| Norte de Emiratos Árabes Unidos | 20.46.42.220, 40.123.224.227, 40.123.224.143, 20.46.46.173 |
| Sur de Reino Unido | 51.140.79.109, 51.140.78.71, 51.140.84.39, 51.140.155.81 |
| Oeste de Reino Unido | 51.141.48.98, 51.141.51.145, 51.141.53.164, 51.141.119.150 |
| Centro-Oeste de EE. UU. | 52.161.26.172, 52.161.8.128, 52.161.19.82, 13.78.137.247 |
| Oeste de Europa | 13.95.155.53, 52.174.54.218, 52.174.49.6 |
| Oeste de la India | 104.211.164.112, 104.211.165.81, 104.211.164.25, 104.211.157.237 |
| Oeste de EE. UU. | 52.160.90.237, 138.91.188.137, 13.91.252.184, 157.56.160.212 |
| Oeste de EE. UU. 2 | 13.66.224.169, 52.183.30.10, 52.183.39.67, 13.66.128.68 |
|||

<a name="azure-government-inbound"></a>

#### <a name="azure-government---inbound-ip-addresses"></a>Azure Government: direcciones IP entrantes

| Región de Azure Government | IP |
|-------------------------|----|
| US Gov: Arizona | 52.244.67.164, 52.244.67.64, 52.244.66.82 |
| US Gov Texas | 52.238.119.104, 52.238.112.96, 52.238.119.145 |
| US Gov - Virginia | 52.227.159.157, 52.227.152.90, 23.97.4.36 |
| US DoD (centro) | 52.182.49.204, 52.182.52.106 |
|||

<a name="outbound"></a>

### <a name="outbound-ip-addresses"></a>Direcciones IP salientes

En esta sección se enumeran las direcciones IP salientes para el servicio Azure Logic Apps y los conectores administrados. Si tiene Azure Government, consulte [Azure Government: direcciones IP salientes](#azure-government-outbound).

> [!TIP]
> Para ayudar a reducir la complejidad al crear reglas de seguridad, tiene la opción de usar la [etiqueta de servicio](../virtual-network/service-tags-overview.md) **LogicApps**, en lugar de especificar prefijos de dirección IP salientes de Logic Apps para cada región. Opcionalmente, también puede usar la etiqueta de servicio **AzureConnectors** para los conectores administrados que realizan llamadas de salida a los servicios correspondientes, como Azure Storage o Azure Event Hubs, en lugar de especificar prefijos de direcciones IP de conectores administrados de salida para cada región. Estas etiquetas funcionan en las regiones en las que está disponible el servicio Logic Apps.

<a name="multi-tenant-outbound"></a>

#### <a name="multi-tenant-azure---outbound-ip-addresses"></a>Azure de varios inquilinos: direcciones IP salientes

| Región de varios inquilinos | IP de Logic Apps | IP de conectores administrados |
|---------------------|---------------|-----------------------|
| Este de Australia | 13.75.149.4, 104.210.91.55, 104.210.90.241, 52.187.227.245, 52.187.226.96, 52.187.231.184, 52.187.229.130, 52.187.226.139 | 52.237.214.72, 13.72.243.10, 13.70.72.192 - 13.70.72.207, 13.70.78.224 - 13.70.78.255 |
| Sudeste de Australia | 13.73.114.207, 13.77.3.139, 13.70.159.205, 52.189.222.77, 13.77.56.167, 13.77.58.136, 52.189.214.42, 52.189.220.75 | 52.255.48.202, 13.70.136.174, 13.77.50.240 - 13.77.50.255, 13.77.55.160 - 13.77.55.191 |
| Sur de Brasil | 191.235.82.221, 191.235.91.7, 191.234.182.26, 191.237.255.116, 191.234.161.168, 191.234.162.178, 191.234.161.28, 191.234.162.131 | 191.232.191.157, 104.41.59.51, 191.233.203.192 - 191.233.203.207, 191.233.207.160 - 191.233.207.191 |
| Centro de Canadá | 52.233.29.92, 52.228.39.244, 40.85.250.135, 40.85.250.212, 13.71.186.1, 40.85.252.47, 13.71.184.150 | 52.237.32.212, 52.237.24.126, 13.71.170.208 - 13.71.170.223, 13.71.175.160 - 13.71.175.191 |
| Este de Canadá | 52.232.128.155, 52.229.120.45, 52.229.126.25, 40.86.203.228, 40.86.228.93, 40.86.216.241, 40.86.226.149, 40.86.217.241 | 52.242.30.112, 52.242.35.152, 40.69.106.240 - 40.69.106.255, 40.69.111.0 - 40.69.111.31 |
| Centro de la India | 52.172.154.168, 52.172.186.159, 52.172.185.79, 104.211.101.108, 104.211.102.62, 104.211.90.169, 104.211.90.162, 104.211.74.145 | 52.172.212.129, 52.172.211.12, 20.43.123.0 - 20.43.123.31, 104.211.81.192 - 104.211.81.207 |
| Centro de EE. UU. | 13.67.236.125, 104.208.25.27, 40.122.170.198, 40.113.218.230, 23.100.86.139, 23.100.87.24, 23.100.87.56, 23.100.82.16 | 52.173.241.27, 52.173.245.164, 13.89.171.80 - 13.89.171.95, 13.89.178.64 - 13.89.178.95 |
| Este de Asia | 13.75.94.173, 40.83.127.19, 52.175.33.254, 40.83.73.39, 65.52.175.34, 40.83.77.208, 40.83.100.69, 40.83.75.165 | 13.75.110.131, 52.175.23.169, 13.75.36.64 - 13.75.36.79, 104.214.164.0 - 104.214.164.31 |
| Este de EE. UU. | 13.92.98.111, 40.121.91.41, 40.114.82.191, 23.101.139.153, 23.100.29.190, 23.101.136.201, 104.45.153.81, 23.101.132.208 | 40.71.249.139, 40.71.249.205, 40.114.40.132, 40.71.11.80 - 40.71.11.95, 40.71.15.160 - 40.71.15.191 |
| Este de EE. UU. 2 | 40.84.30.147, 104.208.155.200, 104.208.158.174, 104.208.140.40, 40.70.131.151, 40.70.29.214, 40.70.26.154, 40.70.27.236 | 52.225.129.144, 52.232.188.154, 104.209.247.23, 40.70.146.208 - 40.70.146.223, 40.70.151.96 - 40.70.151.127 |
| Centro de Francia | 52.143.164.80, 52.143.164.15, 40.89.186.30, 20.188.39.105, 40.89.191.161, 40.89.188.169, 40.89.186.28, 40.89.190.104 | 40.89.186.239, 40.89.135.2, 40.79.130.208 - 40.79.130.223, 40.79.148.96 - 40.79.148.127 |
| Sur de Francia | 52.136.132.40, 52.136.129.89, 52.136.131.155, 52.136.133.62, 52.136.139.225, 52.136.130.144, 52.136.140.226, 52.136.129.51 | 52.136.142.154, 52.136.133.184, 40.79.178.240 - 40.79.178.255, 40.79.180.224 - 40.79.180.255 |
| Norte de Alemania | 51.116.211.168, 51.116.208.165, 51.116.208.175, 51.116.208.192, 51.116.208.200, 51.116.208.222, 51.116.208.217, 51.116.208.51 | 51.116.60.192, 51.116.211.212, 51.116.59.16 - 51.116.59.31, 51.116.60.192 - 51.116.60.223 |
| Centro-oeste de Alemania | 51.116.233.35, 51.116.171.49, 51.116.233.33, 51.116.233.22, 51.116.168.104, 51.116.175.17, 51.116.233.87, 51.116.175.51 | 51.116.158.97, 51.116.236.78, 51.116.155.80 - 51.116.155.95, 51.116.158.96 - 51.116.158.127 |
| Japón Oriental | 13.71.158.3, 13.73.4.207, 13.71.158.120, 13.78.18.168, 13.78.35.229, 13.78.42.223, 13.78.21.155, 13.78.20.232 | 13.73.21.230, 13.71.153.19, 13.78.108.0 - 13.78.108.15, 40.79.189.64 - 40.79.189.95 |
| Japón Occidental | 40.74.140.4, 104.214.137.243, 138.91.26.45, 40.74.64.207, 40.74.76.213, 40.74.77.205, 40.74.74.21, 40.74.68.85 | 104.215.27.24, 104.215.61.248, 40.74.100.224 - 40.74.100.239, 40.80.180.64 - 40.80.180.95 |
| Centro de Corea del Sur | 52.231.14.11, 52.231.14.219, 52.231.15.6, 52.231.10.111, 52.231.14.223, 52.231.77.107, 52.231.8.175, 52.231.9.39 | 52.141.1.104, 52.141.36.214, 20.44.29.64 - 20.44.29.95, 52.231.18.208 - 52.231.18.223 |
| Corea del Sur | 52.231.204.74, 52.231.188.115, 52.231.189.221, 52.231.203.118, 52.231.166.28, 52.231.153.89, 52.231.155.206, 52.231.164.23 | 52.231.201.173, 52.231.163.10, 52.231.147.0 - 52.231.147.15, 52.231.148.224 - 52.231.148.255 |
| Centro-Norte de EE. UU | 168.62.248.37, 157.55.210.61, 157.55.212.238, 52.162.208.216, 52.162.213.231, 65.52.10.183, 65.52.9.96, 65.52.8.225 | 52.162.126.4, 52.162.242.161, 52.162.107.160 - 52.162.107.175, 52.162.111.192 - 52.162.111.223 |
| Norte de Europa | 40.113.12.95, 52.178.165.215, 52.178.166.21, 40.112.92.104, 40.112.95.216, 40.113.4.18, 40.113.3.202, 40.113.1.181 | 52.169.28.181, 52.178.150.68, 94.245.91.93, 13.69.227.208 - 13.69.227.223, 13.69.231.192 - 13.69.231.223 |
| Este de Noruega | 51.120.88.52, 51.120.88.51, 51.13.65.206, 51.13.66.248, 51.13.65.90, 51.13.65.63, 51.13.68.140, 51.120.91.248 | 51.120.100.192, 51.120.92.27, 51.120.98.224 - 51.120.98.239, 51.120.100.192 - 51.120.100.223 |
| Norte de Sudáfrica | 102.133.231.188, 102.133.231.117, 102.133.230.4, 102.133.227.103, 102.133.228.6, 102.133.230.82, 102.133.231.9, 102.133.231.51 | 102.133.168.167, 40.127.2.94, 102.133.155.0 - 102.133.155.15, 102.133.253.0 - 102.133.253.31 |
| Oeste de Sudáfrica | 102.133.72.98, 102.133.72.113, 102.133.75.169, 102.133.72.179, 102.133.72.37, 102.133.72.183, 102.133.72.132, 102.133.75.191 | 102.133.72.85, 102.133.75.194, 102.37.64.0 - 102.37.64.31, 102.133.27.0 - 102.133.27.15 |
| Centro-sur de EE. UU. | 104.210.144.48, 13.65.82.17, 13.66.52.232, 23.100.124.84, 70.37.54.122, 70.37.50.6, 23.100.127.172, 23.101.183.225 | 52.171.130.92, 13.65.86.57, 13.73.244.224 - 13.73.244.255, 104.214.19.48 - 104.214.19.63 |
| Sur de la India | 52.172.50.24, 52.172.55.231, 52.172.52.0, 104.211.229.115, 104.211.230.129, 104.211.230.126, 104.211.231.39, 104.211.227.229 | 13.71.127.26, 13.71.125.22, 20.192.184.32 - 20.192.184.63, 40.78.194.240 - 40.78.194.255 |
| Sudeste de Asia | 13.76.133.155, 52.163.228.93, 52.163.230.166, 13.76.4.194, 13.67.110.109, 13.67.91.135, 13.76.5.96, 13.67.107.128 | 52.187.115.69, 52.187.68.19, 13.67.8.240 - 13.67.8.255, 13.67.15.32 - 13.67.15.63 |
| Norte de Suiza | 51.103.137.79, 51.103.135.51, 51.103.139.122, 51.103.134.69, 51.103.138.96, 51.103.138.28, 51.103.136.37, 51.103.136.210 | 51.103.142.22, 51.107.86.217, 51.107.59.16 - 51.107.59.31, 51.107.60.224 - 51.107.60.255 |
| Oeste de Suiza | 51.107.239.66, 51.107.231.86, 51.107.239.112, 51.107.239.123, 51.107.225.190, 51.107.225.179, 51.107.225.186, 51.107.225.151, 51.107.239.83 | 51.107.156.224, 51.107.231.190, 51.107.155.16 - 51.107.155.31, 51.107.156.224 - 51.107.156.255 |
| Centro de Emiratos Árabes Unidos | 20.45.75.200, 20.45.72.72, 20.45.75.236, 20.45.79.239, 20.45.67.170, 20.45.72.54, 20.45.67.134, 20.45.67.135 | 20.45.67.45, 20.45.67.28, 20.37.74.192 - 20.37.74.207, 40.120.8.0 - 40.120.8.31 |
| Norte de Emiratos Árabes Unidos | 40.123.230.45, 40.123.231.179, 40.123.231.186, 40.119.166.152, 40.123.228.182, 40.123.217.165, 40.123.216.73, 40.123.212.104 | 65.52.250.208, 40.123.224.120, 40.120.64.64 - 40.120.64.95, 65.52.250.208 - 65.52.250.223 |
| Sur de Reino Unido | 51.140.74.14, 51.140.73.85, 51.140.78.44, 51.140.137.190, 51.140.153.135, 51.140.28.225, 51.140.142.28, 51.140.158.24 | 51.140.74.150, 51.140.80.51, 51.140.61.124, 51.105.77.96 - 51.105.77.127, 51.140.148.0 - 51.140.148.15 |
| Oeste de Reino Unido | 51.141.54.185, 51.141.45.238, 51.141.47.136, 51.141.114.77, 51.141.112.112, 51.141.113.36, 51.141.118.119, 51.141.119.63 | 51.141.52.185, 51.141.47.105, 51.141.124.13, 51.140.211.0 - 51.140.211.15, 51.140.212.224 - 51.140.212.255 |
| Centro-Oeste de EE. UU. | 52.161.27.190, 52.161.18.218, 52.161.9.108, 13.78.151.161, 13.78.137.179, 13.78.148.140, 13.78.129.20, 13.78.141.75 | 52.161.101.204, 52.161.102.22, 13.78.132.82, 13.71.195.32 - 13.71.195.47, 13.71.199.192 - 13.71.199.223 |
| Oeste de Europa | 40.68.222.65, 40.68.209.23, 13.95.147.65, 23.97.218.130, 51.144.182.201, 23.97.211.179, 104.45.9.52, 23.97.210.126 | 52.166.78.89, 52.174.88.118, 40.91.208.65, 13.69.64.208 - 13.69.64.223, 13.69.71.192 - 13.69.71.223 |
| Oeste de la India | 104.211.164.80, 104.211.162.205, 104.211.164.136, 104.211.158.127, 104.211.156.153, 104.211.158.123, 104.211.154.59, 104.211.154.7 | 104.211.189.124, 104.211.189.218, 20.38.128.224 - 20.38.128.255, 104.211.146.224 - 104.211.146.239 |
| Oeste de EE. UU. | 52.160.92.112, 40.118.244.241, 40.118.241.243, 157.56.162.53, 157.56.167.147, 104.42.49.145, 40.83.164.80, 104.42.38.32 | 13.93.148.62, 104.42.122.49, 40.112.195.87, 13.86.223.32 - 13.86.223.63, 40.112.243.160 - 40.112.243.175 |
| Oeste de EE. UU. 2 | 13.66.210.167, 52.183.30.169, 52.183.29.132, 13.66.210.167, 13.66.201.169, 13.77.149.159, 52.175.198.132, 13.66.246.219 | 52.191.164.250, 52.183.78.157, 13.66.140.128 - 13.66.140.143, 13.66.145.96 - 13.66.145.127 |
||||

<a name="azure-government-outbound"></a>

#### <a name="azure-government---outbound-ip-addresses"></a>Azure Government: direcciones IP salientes

| Region | IP de Logic Apps | IP de conectores administrados |
|--------|---------------|-----------------------|
| US DoD (centro) | 52.182.48.215, 52.182.92.143 | 52.127.58.160 - 52.127.58.175, 52.182.54.8, 52.182.48.136, 52.127.61.192 - 52.127.61.223 |
| US Gov: Arizona | 52.244.67.143, 52.244.65.66, 52.244.65.190 | 52.127.2.160 - 52.127.2.175, 52.244.69.0, 52.244.64.91, 52.127.5.224 - 52.127.5.255 |
| US Gov Texas | 52.238.114.217, 52.238.115.245, 52.238.117.119 | 52.127.34.160 - 52.127.34.175, 40.112.40.25, 52.238.161.225, 20.140.137.128 - 20.140.137.159 |
| US Gov - Virginia | 13.72.54.205, 52.227.138.30, 52.227.152.44 | 52.127.42.128 - 52.127.42.143, 52.227.143.61, 52.227.162.91 |
||||

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [crear su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* Obtenga información acerca de [ejemplos y escenarios comunes](../logic-apps/logic-apps-examples-and-scenarios.md)
