---
title: 'Inicio rápido: Exploración de un escenario de ejemplo'
titleSuffix: Azure Digital Twins
description: 'Inicio rápido: Uso del ejemplo de ADT Explorer para visualizar y explorar un escenario precompilado.'
author: baanders
ms.author: baanders
ms.date: 9/24/2020
ms.topic: quickstart
ms.service: digital-twins
ms.openlocfilehash: 9d09bca246938f972a212f7ee71f03a618e16ac4
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99575697"
---
# <a name="quickstart---explore-a-sample-azure-digital-twins-scenario-using-adt-explorer"></a>Inicio rápido: Exploración de un escenario de Azure Digital Twins de ejemplo con ADT Explorer

Con Azure Digital Twins, puede crear modelos en directo de los entornos del mundo real e interactuar con ellos. En primer lugar, los elementos individuales se modelan como *gemelos digitales*. A continuación, se conectan en un *grafo* de conocimiento que puede dar respuesta a eventos en directo y en el que se puede consultar información.

En este inicio rápido, explorará un grafo de Azure Digital Twins precompilado con la ayuda de una aplicación de ejemplo llamada [Azure Digital Twins (ADT) Explorer](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/). ADT Explorer se usa para:

- Cargar una representación digital de un entorno.
- Ver las imágenes visuales de los gemelos y del grafo que se crean para representar el entorno en Azure Digital Twins.
- Realizar otras actividades de administración mediante una experiencia visual basada en explorador.

La guía de inicio rápido contiene los siguientes pasos principales:

1. Configuración de una instancia de Azure Digital Twins y ADT Explorer.
1. Carga de los modelos precompilados y los datos del grafo para construir el escenario de ejemplo.
1. Exploración del grafo del escenario que se crea.
1. Realización de cambios en el grafo.

El grafo de ejemplo con el que va a trabajar representa un edificio de dos plantas y dos salas. El grafo será como el de esta imagen:

:::image type="content" source="media/quickstart-adt-explorer/graph-view-full.png" alt-text="Vista de un grafo compuesto por cuatro nodos circulares conectados por flechas. Un círculo &quot;Floor1&quot; está conectado mediante una flecha &quot;contains&quot; a un círculo &quot;Room1&quot;. Un círculo &quot;Floor0&quot; está conectado mediante una flecha &quot;contains&quot; a un círculo &quot;Room0&quot;. &quot;Floor1&quot; y &quot;Floor0&quot; no están conectados.":::

## <a name="prerequisites"></a>Requisitos previos

Necesitará una suscripción de Azure para completar esta guía de inicio rápido. Si aún no tiene una, [cree una gratis](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .

También necesitará **Node.js** en su equipo. Para obtener la versión más reciente, consulte [Node.js](https://nodejs.org/).

Por último, también deberá descargar el ejemplo que se usará durante el inicio rápido. La aplicación de ejemplo es **ADT Explorer**. Este ejemplo contiene la aplicación que se usa en el inicio rápido para cargar y explorar un escenario de Azure Digital Twins. También contiene los archivos del escenario de ejemplo. Para obtener el ejemplo, vaya a [Azure Digital Twins (ADT) Explorer](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/). Seleccione el botón **Descargar archivo ZIP** para descargar el archivo .zip de este código de ejemplo en la máquina. Descomprima la carpeta **Azure_Digital_Twins__ADT__explorer.zip** y extraiga los archivos.

## <a name="set-up-azure-digital-twins-and-adt-explorer"></a>Configuración de Azure Digital Twins y Explorador de ADT

El primer paso para trabajar con Azure Digital Twins es configurar una instancia de Azure Digital Twins. Después de crear una instancia del servicio y configurar las credenciales para autenticarse con ADT Explorer, podrá conectarse a la instancia de ADT Explorer y rellenarla con los datos de ejemplo más adelante en el inicio rápido.

El resto de esta sección le guía a través de estos pasos.

### <a name="set-up-an-azure-digital-twins-instance"></a>Configurará una instancia de Azure Digital Twins.

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

### <a name="set-up-local-azure-credentials"></a>Configuración de credenciales locales de Azure

La aplicación ADT Explorer usa [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) (parte de la biblioteca `Azure.Identity`) para autenticar a los usuarios en la instancia de Azure Digital Twins cuando la ejecuta en la máquina local. Para más información sobre las distintas formas en que una aplicación cliente puede autenticarse con Azure Digital Twins, consulte [Escritura de código de autenticación de aplicación](how-to-authenticate-client.md).

Con este tipo de autenticación, ADT Explorer buscará las credenciales en el entorno local, por ejemplo, un inicio de sesión de Azure en una [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) local o en Visual Studio o Visual Studio Code. Por este motivo, debe *iniciar sesión en Azure localmente* mediante uno de estos mecanismos para configurar las credenciales de la aplicación ADT Explorer.

Si ya ha iniciado sesión en Azure mediante una de estas formas, puede pasar directamente a la [siguiente sección](#run-and-configure-adt-explorer).

De lo contrario, puede instalar la CLI de Azure local con estos pasos:

1. Siga el proceso que se describe en [este vínculo de instalación](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) para completar la instalación correspondiente a su sistema operativo.
1. Abra una ventana de la consola en la máquina.
1. Ejecute `az login` y siga los avisos de autenticación para iniciar sesión en su cuenta de Azure.
1. Posible último paso: si usa varias suscripciones de Azure en esta cuenta, establezca el contexto de autenticación en la suscripción de Azure que contiene la instancia de Azure Digital Twins. Para eso, ejecute `az account set --subscription "<your-subscription-name-or-ID>"` (el nombre o el valor del identificador de la suscripción servirán).

Después iniciar sesión, ADT Explorer seleccionará las credenciales de Azure automáticamente al ejecutarlo en la sección siguiente.

Si lo desea, puede cerrar la ventana de la consola de autenticación. O bien, puede mantenerla abierta para usarla en el paso siguiente.

### <a name="run-and-configure-adt-explorer"></a>Ejecución y configuración de Explorador de ADT

A continuación, ejecute la aplicación Explorador de ADT y configúrela para la instancia de Azure Digital Twins.

1. Vaya a la carpeta **Azure_Digital_Twins__ADT__explorer** descargada y descomprimida.
Abra una ventana de la consola en la ubicación de la carpeta **Azure_Digital_Twins__ADT__explorer/client/src**.

1. Ejecute `npm install` para descargar todas las dependencias necesarias.

1. Ejecute `npm run start` para iniciar la aplicación.

   Al cabo de unos segundos, se abre una ventana del explorador y aparece ahí la aplicación.

   :::image type="content" source="media/quickstart-adt-explorer/explorer-blank.png" alt-text="Ventana del explorador que muestra una aplicación que se ejecuta en localhost:3000. La aplicación se llama ADT Explorer y contiene los cuadros Query Explorer (Explorador de consultas), Model View (Vista de modelo), Graph View (Vista de grafo) y Property Explorer (Explorador de propiedades). Todavía no hay datos en pantalla." lightbox="media/quickstart-adt-explorer/explorer-blank.png":::

1. Seleccione el botón **Sign In** (Iniciar sesión) en la esquina superior derecha de la ventana, como se muestra en la siguiente imagen, para configurar ADT Explorer para que funcione con la instancia que ha configurado.

   :::image type="content" source="media/quickstart-adt-explorer/sign-in.png" alt-text="Explorador de ADT con el icono de inicio de sesión resaltado cerca de la parte superior de la ventana. El icono muestra una silueta simple de una persona superpuesta con la silueta de una llave." lightbox="media/quickstart-adt-explorer/sign-in.png":::

1. Escriba la dirección URL de la instancia de Azure Digital Twins que recopiló anteriormente en la sección [Configuración de una instancia de Azure Digital Twins](#set-up-an-azure-digital-twins-instance), con el formato *https://{nombre de host de instancia}* .

>[!NOTE]
> Puede revisar o editar esta información en cualquier momento; solo tiene que seleccionar el mismo icono para volver a abrir el cuadro **Sign In** (Iniciar sesión). Mantendrá los valores que ha especificado.

> [!TIP]
> Si aparece un mensaje de error de `SignalRService.subscribe` al conectarse, asegúrese de que la dirección URL de Azure Digital Twins comienza por *https://* .

Si ve la ventana emergente **Permisos solicitados** de Microsoft, conceda el consentimiento para esta aplicación y acepte para continuar.

## <a name="add-the-sample-data"></a>Adición de los datos de ejemplo

A continuación, importará el escenario de ejemplo y el grafo en ADT Explorer. El escenario de ejemplo también se encuentra en la carpeta **Azure_Digital_Twins__ADT__explorer** que descargó anteriormente.

### <a name="models"></a>Modelos

El primer paso en una solución de Azure Digital Twins es definir el vocabulario del entorno. Creará [modelos](concepts-models.md) personalizados que describan los tipos de entidad que existen en el entorno.

Cada modelo se escribe en un lenguaje de tipo JSON-LD, conocido como lenguaje de definición de gemelos digitales (DTDL). Cada modelo describe un único tipo de entidad en lo referente a sus *propiedades*, *telemetría*, *relaciones* y *componentes*. Más adelante, usará estos modelos como base para los gemelos digitales que representan instancias específicas de estos tipos.

Normalmente, cuando se crea un modelo, se realizan tres pasos:

1. Escribir la definición del modelo. En el inicio rápido, este paso ya se ha hecho como parte de la solución de ejemplo.
1. Validarlo para asegurarse de que la sintaxis sea precisa. En el inicio rápido, este paso ya se ha hecho como parte de la solución de ejemplo.
1. Cargarlo en la instancia de Azure Digital Twins.
 
En este inicio rápido, los archivos del modelo ya se han escrito y validado. Están incluidos en la solución que descargó. En esta sección, se cargarán los dos modelos escritos previamente en la instancia para definir estos componentes de un entorno de edificio:

* Floor
* Sala

#### <a name="upload-models"></a>Carga de modelos

Siga estos pasos para cargar los modelos.

1. En el cuadro **MODEL VIEW** (VISTA DE MODELO), seleccione el icono **Upload a Model** (Cargar un modelo).

   :::image type="content" source="media/quickstart-adt-explorer/upload-model.png" alt-text="Cuadro Vista de modelo con el icono central resaltado. Muestra una flecha que apunta a una nube." lightbox="media/quickstart-adt-explorer/upload-model.png":::
 
1. En el cuadro del selector de archivos que aparece, vaya a la carpeta **Azure_Digital_Twins__ADT__explorer/client/examples** en el repositorio descargado.
1. Seleccione **Room.json** y **Floor.json** y, luego, **OK** (ACEPTAR). Aunque puede cargar otros modelos si así lo desea, no se usarán en este inicio rápido.
1. Siga el cuadro de diálogo emergente que le pide que inicie sesión en su cuenta de Azure.

>[!NOTE]
>Si aparece el siguiente mensaje de error: :::image type="content" source="media/quickstart-adt-explorer/error-models-popup.png" alt-text="Un cuadro emergente que dice &quot;Error: Error al recuperar modelos: ClientAuthError: Error al abrir la ventana emergente. Este error puede ocurrir si usa Internet Explorer o si los elementos emergentes están bloqueados en el explorador con un botón Cerrar en la parte inferior." border="false"::: 
> Pruebe a deshabilitar el bloqueador de elementos emergentes o a usar otro explorador.

ADT Explorer carga ahora estos archivos del modelo en la instancia de Azure Digital Twins. Deberían aparecer en el cuadro **MODEL VIEW** (VISTA DE MODELO), que muestra los nombres descriptivos y los identificadores del modelo completos. Puede seleccionar los iconos de información **View Model** (Ver modelo) para ver el código DTDL subyacente.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/model-info.png" alt-text="Cuadro Model View (Vista de modelo) con dos definiciones de modelo en su interior, Floor (dtmi:example:Floor;1) y Room (dtmi:example:Room;1). El icono de información &quot;View Model&quot; (Ver modelo) muestra una letra &quot;i&quot; en un círculo resaltada para cada modelo." lightbox="media/quickstart-adt-explorer/model-info.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="twins-and-the-twin-graph"></a>Gemelos y grafo de gemelos

Ahora que se han cargado algunos modelos en la instancia de Azure Digital Twins, puede crear [gemelos digitales](concepts-twins-graph.md) basados en las definiciones de modelo.

Los gemelos digitales representan las entidades reales dentro del entorno empresarial. Pueden ser cosas como sensores de una granja, luces de un coche o, en este inicio rápido, salas de la planta de un edificio. Puede crear muchos gemelos de un tipo de modelo determinado, como varias salas que usen todas el modelo *Room*. Estos gemelos se conectan mediante relaciones en un *grafo de gemelos* que representa todo el entorno.

En esta sección, cargará los gemelos creados previamente que están conectados en un grafo ya creado. El grafo contiene dos plantas y dos salas, conectadas en el siguiente diseño:

* Floor0
    - Contains Room0
* Floor1
    - Contains Room1

#### <a name="import-the-graph"></a>Importación de grafo

Siga estos pasos para importar el grafo.

1. En el cuadro **GRAPH VIEW** (VISTA DE GRAFO), seleccione el icono **Import Graph** (Importar grafo).

   :::image type="content" source="media/quickstart-adt-explorer/import-graph.png" alt-text="Hay un icono resaltado en el cuadro de texto Vista de grafo. Muestra una flecha que apunta a una nube." lightbox="media/quickstart-adt-explorer/import-graph.png":::

2. En el cuadro del selector de archivos, vaya a la carpeta **Azure_Digital_Twins__ADT__explorer/client/examples** y elija el archivo de hoja de cálculo **buildingScenario.xlsx**. Este archivo contiene una descripción del grafo de ejemplo. Seleccione **Aceptar**.

   Al cabo de unos segundos, ADT Explorer abre una vista **Import** (Importar) que muestra una vista previa del grafo que se va a cargar.

3. Para confirmar la carga del grafo, seleccione el icono **Save** (Guardar) situado en la esquina superior derecha del cuadro **GRAPH VIEW** (VISTA DE GRAFO).

   :::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/graph-preview-save.png" alt-text="Resaltado del icono Save (Guardar) en el panel de vista previa del grafo." lightbox="media/quickstart-adt-explorer/graph-preview-save.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
   :::row-end:::

4. ADT Explorer usará ahora el archivo cargado para crear los gemelos solicitados y las relaciones entre ellos. Al terminar, aparecerá un cuadro de diálogo. Seleccione **Close** (Cerrar).

   :::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/import-success.png" alt-text="Cuadro de diálogo que indica que el grafo se ha importado correctamente. Indica &quot;Importación correcta. 4 gemelos importados. 2 relaciones importadas.&quot;" lightbox="media/quickstart-adt-explorer/import-success.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
   :::row-end:::

5. El grafo ya se ha cargado en Explorador de ADT. Para ver el grafo, seleccione el botón **Run Query** (Ejecutar consulta) en el cuadro **GRAPH EXPLORER** (EXPLORADOR DE GRAFOS), cerca de la parte superior de la ventana de ADT Explorer.

   :::image type="content" source="media/quickstart-adt-explorer/run-query.png" alt-text="Se resalta el botón Run Query (Ejecutar consulta) situado en la esquina superior derecha de la ventana." lightbox="media/quickstart-adt-explorer/run-query.png":::

Esta acción ejecuta la consulta predeterminada para seleccionar y mostrar todos los gemelos digitales. ADT Explorer recupera todos los gemelos y relaciones del servicio, y dibuja el grafo definido por ellos en el cuadro **GRAPH VIEW** (VISTA DE GRAFO).

## <a name="explore-the-graph"></a>Exploración del grafo

Ahora puede ver el grafo cargado del escenario de ejemplo.

:::image type="content" source="media/quickstart-adt-explorer/graph-view-full.png" alt-text="Vista del cuadro Graph View (Vista de grafo) con un grafo de gemelos dentro. Un círculo &quot;floor1&quot; está conectado mediante una flecha &quot;contains&quot; a un círculo &quot;room1&quot;. Un círculo &quot;floor0&quot; está conectado mediante una flecha &quot;contains&quot; a un círculo &quot;room0&quot;.":::

Los círculos ("nodos" del grafo) representan los gemelos digitales. Las líneas representan las relaciones. El gemelo **Floor0** contiene **Room0**, y el gemelo **Floor1** contiene **Room1**.

Si usa un mouse, puede arrastrar trozos del grafo para moverlos.

### <a name="view-twin-properties"></a>Visualización de las propiedades de un gemelo

Puede seleccionar un gemelo para ver una lista de sus propiedades y sus valores en el cuadro **PROPERTY EXPLORER** (Explorador de propiedades).

Estas son las propiedades de Room0:

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/properties-room0.png" alt-text="Resaltado del cuadro Property Explorer (Explorador de propiedades) que muestra las propiedades de Room0, incluidos (entre otros) el campo $dtId con &quot;Room0&quot;, el campo Temperature con el valor 70 y el campo Humidity con el valor 30." lightbox="media/quickstart-adt-explorer/properties-room0.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Room0 tiene una temperatura de 70.

Estas son las propiedades de Room1:

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/properties-room1.png" alt-text="Resaltado del cuadro Property Explorer (Explorador de propiedades) que muestra las propiedades de Room1, incluidos (entre otros) el campo $dtId con &quot;Room1&quot;, el campo Temperature con el valor 80 y el campo Humidity con el valor 60." lightbox="media/quickstart-adt-explorer/properties-room1.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Room1 tiene una temperatura de 80.

### <a name="query-the-graph"></a>Consulta del grafo

Una de las principales características de Azure Digital Twins es la posibilidad de [consultar](concepts-query-language.md) el gráfico de gemelos de forma fácil y eficaz para responder a las preguntas sobre el entorno.

Una forma de consultar los gemelos en el grafo es mediante sus *propiedades*. La consulta basada en propiedades puede ayudar a responder a diversas preguntas. Por ejemplo, puede encontrar valores atípicos en el entorno que necesiten atención.

En esta sección, ejecutará una consulta que responda a la pregunta de cuántos gemelos de su entorno tienen una temperatura superior a 75.

Para ver la respuesta, ejecute la siguiente consulta en el cuadro **QUERY EXPLORER** (EXPLORADOR DE CONSULTAS).

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="TemperatureQuery":::

Recuerde que, al ver las propiedades de los gemelos anteriores, Room0 tiene una temperatura de 70 y Room1 tiene una temperatura de 80. Por lo tanto, aquí solo aparece Room1 en los resultados.
    
:::image type="content" source="media/quickstart-adt-explorer/result-query-property-before.png" alt-text="Resultados de la consulta de propiedades, con solo Room1." lightbox="media/quickstart-adt-explorer/result-query-property-before.png":::

>[!TIP]
> También se admiten otros operadores de comparación (<,>, = o !=) en la consulta anterior. Puede intentar conectar estos operadores, otros valores u otras propiedades de los gemelos a la consulta para tratar de responder a sus propias preguntas.

## <a name="edit-data-in-the-graph"></a>Edición de los datos del grafo

Puede usar Explorador de ADT para editar las propiedades de los gemelos representados en el grafo. En esta sección, se va a elevar la temperatura de Room0 a 76.

Para comenzar, seleccione **Room0** para mostrar su lista de propiedades en el cuadro **PROPERTY EXPLORER** (EXPLORADOR DE PROPIEDADES).

Las propiedades de esta lista son editables. Seleccione el valor de temperatura **70** para habilitar la entrada de un nuevo valor. Escriba **76** y seleccione el icono **Save** (Guardar) para actualizar la temperatura a **76**.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/new-properties-room0.png" alt-text="Cuadro &quot;Property Explorer&quot; (Explorador de propiedades) que muestra las propiedades de Room0. El valor de temperatura es un cuadro editable que muestra 76 y hay un resaltado alrededor del icono de guardar." lightbox="media/quickstart-adt-explorer/new-properties-room0.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Ahora verá la ventana **Patch Information** (Información de revisión) que muestra el código de revisión que se usó en segundo plano con las [API](how-to-use-apis-sdks.md) de Azure Digital Twins para realizar la actualización. Seleccione **Close** (Cerrar).

### <a name="query-to-see-the-result"></a>Ejecución de la consulta para ver el resultado

Para comprobar que el grafo ha registrado correctamente la actualización de la temperatura de Room0, vuelva a ejecutar la consulta anterior para obtener todos los gemelos del entorno con una temperatura superior a 75.

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="TemperatureQuery":::

Ahora que se ha cambiado la temperatura de Room0 de 70 a 76, ambos gemelos deberían aparecer en el resultado.

:::image type="content" source="media/quickstart-adt-explorer/result-query-property-after.png" alt-text="Resultados de la consulta de propiedades, con Room0 y Room1." lightbox="media/quickstart-adt-explorer/result-query-property-after.png":::

## <a name="review-and-contextualize-learnings"></a>Revisión y contextualización del aprendizaje

En esta guía de inicio rápido, ha creado una instancia de Azure Digital Twins, la ha conectado a Explorador de ADT y la ha rellenado con un escenario de ejemplo.

Después, ha explorado el grafo mediante:

* El uso de una consulta para responder a una pregunta sobre el escenario.
* La edición de una propiedad en un gemelo digital.
* La ejecución de la consulta de nuevo para ver cómo ha cambiado la respuesta como resultado de la actualización.

La intención de este ejercicio es demostrar cómo puede usar el grafo de Azure Digital Twins para responder a preguntas sobre el entorno, incluso a medida que el entorno cambia.

En este inicio rápido, realizó la actualización de la temperatura manualmente. Es habitual en Azure Digital Twins conectar gemelos digitales a dispositivos IoT reales para que reciban actualizaciones automáticamente, en función de los datos de telemetría. De este modo, puede crear un grafo dinámico que siempre refleje el estado real del entorno. Puede usar consultas para obtener información sobre lo que ocurre en su entorno en tiempo real.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para finalizar el trabajo de esta guía de inicio rápido, primero finalice la aplicación de consola en ejecución. Esta acción cierra la conexión a la aplicación ADT Explorer en el explorador. Ya no podrá ver allí los datos en directo. Puede cerrar la pestaña del explorador.

Después, puede elegir los recursos que desea quitar en función de lo que desee hacer a continuación.

* **Si tiene previsto seguir con los tutoriales de Azure Digital Twins**, puede reutilizar la instancia de este inicio rápido en esos artículos, no es necesario eliminarla.

[!INCLUDE [digital-twins-cleanup-clear-instance.md](../../includes/digital-twins-cleanup-clear-instance.md)]
 
[!INCLUDE [digital-twins-cleanup-basic.md](../../includes/digital-twins-cleanup-basic.md)]

También puede que desee eliminar la carpeta del proyecto de la máquina local.

## <a name="next-steps"></a>Pasos siguientes

A continuación, siga con los tutoriales de Azure Digital Twins para crear su propio escenario de Azure Digital Twins y las herramientas de interacción.

> [!div class="nextstepaction"]
> [Tutorial: Programación de una aplicación cliente](tutorial-code.md)