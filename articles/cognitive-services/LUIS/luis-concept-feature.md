---
title: Características del aprendizaje automático con LUIS
description: Agregue características a un modelo de lenguaje para proporcionar sugerencias sobre cómo reconocer la entrada que quiera etiquetar o clasificar.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: da85abdff3d1022659f2d4e83fd14c5ae6003fc9
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100546064"
---
# <a name="machine-learning-features"></a>Características del aprendizaje automático

En el aprendizaje automático, una  *característica*  es un rasgo distintivo o un atributo de datos que el sistema observa y del que aprende.

Las características del aprendizaje automático proporcionan a LUIS indicaciones importantes sobre dónde buscar los elementos que distinguen un concepto. Son sugerencias que LUIS puede usar, pero no reglas rígidas. LUIS utiliza estas sugerencias en combinación con las etiquetas para buscar los datos.

Una característica se puede describir como una función, como f(x) = y. En la expresión de ejemplo, la característica indica dónde buscar el rasgo distintivo. Use esta información como ayuda para crear su esquema.

## <a name="types-of-features"></a>Tipos de características

Las características son una parte necesaria del diseño del esquema. LUIS admite listas de frases y modelos como características:

* Característica de lista de frases
* Modelo (intención o entidad) como una característica

## <a name="find-features-in-your-example-utterances"></a>Búsqueda de características en las expresiones de ejemplo

Dado que LUIS es una aplicación basada en lenguaje, las características se basan en texto. Seleccione el texto que indica el rasgo que quiere distinguir. En LUIS, la unidad más pequeña es el *token*. En el idioma inglés, un token es una extensión contigua, sin espacios ni puntuación, de letras y números.

Dado que los espacios y la puntuación no son tokens, céntrese en las pistas de texto que puede usar como características. Recuerde incluir variaciones de palabras, como:

* formas plurales
* tiempos verbales
* abreviaturas
* ortografías y errores ortográficos

Determine si el texto, ya que distingue un rasgo, tiene que:

* Coincidir con una palabra o frase exacta: considere la posibilidad de agregar una entidad de expresión regular o una entidad de lista como característica a la entidad o la intención.
* Coincidir con un concepto conocido como fechas, horas o nombres de personas: use una entidad precompilada como característica para la entidad o la intención.
* Aprender nuevos ejemplos con el tiempo: use una lista de frases de algunos ejemplos del concepto como característica para la entidad o la intención.

## <a name="create-a-phrase-list-for-a-concept"></a>Creación de una lista de frases para un concepto

Una lista de frases es una lista de palabras o frases que describe un concepto. Una lista de frases se aplica como coincidencia sin distinción entre mayúsculas y minúsculas en el nivel de token.

Al agregar una lista de frases, puede establecer la característica como **[global](#global-features)** . Una característica global se aplica a toda la aplicación.

### <a name="when-to-use-a-phrase-list"></a>Cuándo usar una lista de frases

Use una lista de frases si es necesario que la aplicación LUIS generalice e identifique los elementos nuevos del concepto. Las listas de frases son similares a un vocabulario específico de un dominio. Mejoran la calidad de la comprensión de las intenciones y las entidades.

### <a name="how-to-use-a-phrase-list"></a>Cómo utilizar una lista de frases

Con una lista de frases, LUIS considera el contexto y lo generaliza para identificar los elementos que son similares, pero que no son una coincidencia de texto exacta. Siga estos pasos para utilizar una lista de frases:

1. Empezar con una entidad de aprendizaje automático:
    1. Agregue expresiones de ejemplo.
    1. Etiquete con una entidad de aprendizaje automático.
1. Agregar una lista de frases:
    1. Agregue palabras con un significado similar. No agregue todas las palabras o frases posibles. En su lugar, agregue solo algunas cada vez. A continuación, vuelva a entrenar y a publicar la aplicación.
    1. Revise y agregue palabras sugeridas.

### <a name="a-typical-scenario-for-a-phrase-list"></a>Escenario típico para una lista de frases

Un escenario típico para una lista de frases es potenciar las palabras relacionadas con una idea concreta.

Un buen ejemplo de palabras que pueden necesitar una lista de frases para definir mejor su significado son los términos médicos. Estos términos pueden tener un significado específico físico, químico, terapéutico o abstracto. LUIS no sabrá que los términos son importantes para el dominio del tema sin una lista de frases.

Para extraer términos médicos:

1. Cree expresiones de ejemplo y etiquete los términos médicos dentro de esas expresiones.
2. Cree una lista de frases con ejemplos de los términos dentro del dominio del tema. En esta lista de frases se debe incluir el término real que etiquetó y otros términos que describen el mismo concepto.
3. Agregue la lista de frases a la entidad o subentidad que extrae el concepto utilizado en la lista de frases. El escenario más común es un componente (secundario) de una entidad de aprendizaje automático. Si la lista de frases debe aplicarse a todos las intenciones o entidades, marque la lista de frases como lista de frases global. La marca **enabledForAllModels** controla este ámbito del modelo en la API.

### <a name="token-matches-for-a-phrase-list"></a>Coincidencias de token para una lista de frases

Una lista de frases se aplica siempre en el nivel de token. En la tabla siguiente se muestra cómo se aplica una lista de frases que contiene la palabra **Ann** a variaciones de los mismos caracteres en ese orden.


| Variación de token de **Ann** | Coincidencia de lista de frases cuando se encuentra el token |
|--------------------------|---------------------------------------|
| **ANN**<br>**aNN**<br>           | Sí: el token es **Ann**                  |
| **Ann's**                    | Sí: el token es **Ann**                  |
| **Anne**                     | No: el token es **Anne**                  |

<a name="how-to-use-phrase-lists"></a>
<a name="how-to-use-a-phrase-lists"></a>
<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="a-model-as-a-feature-helps-another-model"></a>Modelo como una característica que ayuda a otro modelo

Puede agregar un modelo (intención o entidad) como una característica para otro modelo (intención o entidad). Al agregar una intención o entidad existente como una característica, agrega un concepto bien definido con ejemplos con etiquetas.

Al agregar un modelo como una característica, puede establecer la característica como:
* **[Obligatoria](#required-features)** . Se deben encontrar las características que son obligatorias para que se devuelva el modelo desde el punto de conexión de predicción.
* **[Global](#global-features)** . Una característica global se aplica a toda la aplicación.

### <a name="when-to-use-an-entity-as-a-feature-to-an-intent"></a>Cuándo usar una entidad como una característica para una intención

Agregue una entidad como una característica a una intención cuando la detección de esa entidad sea significativa para la intención.

Por ejemplo, si la intención es reservar un vuelo, como **BookFlight**, y la entidad es la información sobre el billete (como el número de asiento, el origen y el destino), la búsqueda de la entidad de información del billete debe agregar un peso significativo a la predicción de la intención **BookFlight**.

### <a name="when-to-use-an-entity-as-a-feature-to-another-entity"></a>Cuándo usar una entidad como una característica para otra entidad

Una entidad (A) debe agregarse como una característica a otra entidad (B), cuando la detección de esa entidad (A) es importante para la predicción de la entidad (B).

Por ejemplo, si una entidad de dirección de envío contiene una subentidad de dirección postal, la búsqueda de la subentidad de dirección postal agrega peso significativo a la predicción para la entidad de dirección de envío.

* Dirección de envío (entidad de aprendizaje automático):

    * Número de la calle (subentidad)
    * Dirección postal (subentidad)
    * Ciudad (subentidad)
    * Estado o provincia (subentidad)
    * País o región (subentidad)
    * Código postal (subentidad)

## <a name="nested-subentities-with-features"></a>Subentidades anidadas con características

Una subentidad de aprendizaje automático indica que hay un concepto en la entidad primaria. El elemento primario puede ser otra subentidad o la entidad superior. El valor de la subentidad actúa como una característica de su elemento primario.

Una subentidad puede tener tanto una lista de frases como un modelo (otra entidad) como característica.

Cuando la subentidad tiene una lista de frases, aumentará el vocabulario del concepto, pero no agregará información a la respuesta JSON de la predicción.

Cuando la subentidad tiene una característica de otra entidad, la respuesta JSON incluye los datos extraídos de esa otra entidad.


## <a name="required-features"></a>Características obligatorias

Se deben encontrar las características que son obligatorias para que se devuelva el modelo desde el punto de conexión de predicción. Use una característica obligatoria si sabe que los datos entrantes deben coincidir con la característica.

Si el texto de la expresión no coincide con la característica requerida, no se extraerá.

Una característica obligatoria utiliza una entidad sin aprendizaje automático:

* Entidad de la expresión regular
* Entidad de lista
* Entidad creada previamente

Si está seguro de que se encontrará el modelo en los datos, establezca la característica como obligatoria. Una característica obligatoria no devuelve nada si no se encuentra.

Continuando con el ejemplo de la dirección de envío:

Dirección de envío (entidad con aprendizaje automático)

 * Número de la calle (subentidad)
 * Dirección postal (subentidad)
 * Nombre de la calle (subentidad)
 * Ciudad (subentidad)
 * Estado o provincia (subentidad)
 * País o región (subentidad)
 * Código postal (subentidad)

### <a name="required-feature-using-prebuilt-entities"></a>Característica obligatoria con entidades precompiladas

Las entidades pregeneradas como la ciudad, el estado y el país o región suelen ser un conjunto cerrado de listas, lo que significa que no cambian mucho con el tiempo. Estas entidades podrían tener las características recomendadas pertinentes y estas características podrían marcarse como obligatorias. Sin embargo, la marca `isRequired` solo está relacionada con la entidad a la que se asigna y no afecta a la jerarquía. Si no se encuentra la característica de subentidad pregenerada, esto no afectará a la detección y devolución de la entidad primaria.

Como ejemplo de una característica necesaria, considere la posibilidad de detectar direcciones. Considere la posibilidad de hacer que el número de la calle sea un requisito. Esto permitiría que un usuario especificara "1 Microsoft Way" o "One Microsoft Way", y ambos se resolverían en el número "1" para la subentidad del número de la calle. Consulte el artículo sobre [entidades pregeneradas](luis-reference-prebuilt-entities.md) para más información.

### <a name="required-feature-using-list-entities"></a>Característica obligatoria con entidades de lista

Una [entidad de lista](reference-entity-list.md) se utiliza como una lista de nombres canónicos junto con sus sinónimos. Como característica obligatoria, si la expresión no incluye el nombre canónico o un sinónimo, la entidad no se devuelve como parte del punto de conexión de predicción.

Supongamos que su empresa solo realiza envíos a un conjunto limitado de países o regiones. Puede crear una entidad de lista que incluya varias maneras en las que el cliente puede hacer referencia al país o región. Si LUIS no encuentra una coincidencia exacta en el texto de la expresión, la entidad (que tiene la característica obligatoria de la entidad de lista) no se devuelve en la predicción.

|Nombre canónico|Sinónimos|
|--|--|
|Estados Unidos|EE. UU.<br>EE. UU.<br>US<br>EE. UU.<br>0|

Una aplicación cliente, como un bot de chat, puede hacer una pregunta de seguimiento para ayudar. Esto ayuda al cliente a entender que la selección del país o región es limitada y *obligatoria*.

### <a name="required-feature-using-regular-expression-entities"></a>Característica obligatoria con entidades de expresión regular

Una [entidad de expresión regular](reference-entity-regular-expression.md) utilizada como característica obligatoria proporciona funciones de coincidencia de texto enriquecidas.

En el ejemplo de la dirección de envío, puede crear una expresión regular que capture las reglas de sintaxis de los códigos postales de los países o regiones.

## <a name="global-features"></a>Características globales

Aunque el uso más común es aplicar una característica a un modelo específico, puede configurar la característica como **característica global** para aplicarla a toda la aplicación.

El uso más común de una característica global es agregar un vocabulario adicional a la aplicación. Por ejemplo, si los clientes usan un idioma principal, pero se espera poder usar otro idioma en la misma expresión, puede agregar una característica que incluya palabras del idioma secundario.

Dado que el usuario espera usar el idioma secundario en cualquier intención o entidad, agregue palabras del idioma secundario a la lista de frases. Configure la lista de frases como una característica global.

## <a name="combine-features-for-added-benefit"></a>Combinación de características para obtener una ventaja adicional

Puede usar más de una característica para describir un rasgo o concepto. El siguiente es un emparejamiento que se utiliza habitualmente:

* Característica de lista de frases: Puede usar listas de frases múltiples como características para el mismo modelo.
* Modelo como característica: [entidad precompilada](luis-reference-prebuilt-entities.md), [entidad de expresión regular](reference-entity-regular-expression.md), [entidad de lista](reference-entity-list.md). 

### <a name="example-ticket-booking-entity-features-for-a-travel-app"></a>Ejemplo: características de entidad de reserva de billetes para una aplicación de viajes  

Como ejemplo básico, imagine una aplicación para reservar un vuelo con una _intención_ de reserva de vuelo y una _entidad_ de reserva de billetes. La entidad de reserva de billetes captura la información para reservar un billete de avión en un sistema de reserva. 

La entidad de aprendizaje automático para la reserva del billete tiene dos subentidades para capturar el origen y el destino. Las características deben agregarse a cada subentidad, no a la entidad de nivel superior.

:::image type="content" source="media/luis-concept-features/ticket-booking-entity.png" alt-text="Esquema de la entidad de reserva de billetes":::

La entidad de reserva de billetes es una entidad de aprendizaje automático, con una subentidad de _origen_ y otra de _destino_. Estas dos subentidades indican una ubicación geográfica. Para ayudar a extraer las ubicaciones y distinguir entre el _origen_ y el _destino_, cada subentidad debe tener unas determinadas características.

|Tipo|Subentidad de origen |Subentidad de destino|
|--|--|--|
|Modelo como característica|Entidad [geographyV2](luis-reference-prebuilt-geographyv2.md?tabs=V3) precompilada|Entidad [geographyV2](luis-reference-prebuilt-geographyv2.md?tabs=V3) precompilada|
|Lista de frases|**Palabras de origen**: `start at`, `begin from`, `leave`|**Palabras de destino**: `to`, `arrive`, `land at`, `go`, `going`, `stay`, `heading`|
|Lista de frases|Códigos de aeropuerto: la misma lista para el origen y el destino|Códigos de aeropuerto: la misma lista para el origen y el destino|
|Lista de frases|Nombres de aeropuerto: la misma lista para el origen y el destino|Códigos de aeropuerto: la misma lista para el origen y el destino|

Si prevé que se van a usar códigos y nombres de aeropuerto, LUIS debe tener listas de frases que utilicen ambos tipos de frases. Los códigos de aeropuerto pueden ser más comunes como texto escrito en un bot de chat, mientras que los nombres de aeropuerto pueden ser más habituales como conversación hablada, como un bot de chat con voz.

Los detalles de coincidencia de las características solo se devuelven para los modelos, no para las listas de frases porque solo se devuelven modelos en el JSON de predicción.

#### <a name="ticket-booking-labeling-in-the-intent"></a>Etiquetado de reserva de billetes en la intención

Después de crear la entidad de aprendizaje automático, debe agregar expresiones de ejemplo a una intención y etiquetar la entidad primaria y todas las subentidades.

En el ejemplo de reserva de billetes, etiquete las expresiones de ejemplo en la intención con la entidad `TicketBooking` y las subentidades del texto.

:::image type="content" source="media/luis-concept-features/intent-example-utterances-machine-learning-entity.png" alt-text="Expresiones de ejemplo de etiqueta":::

### <a name="example-pizza-ordering-app"></a>Ejemplo: aplicación de pedidos de pizza

En un segundo ejemplo, imagine una aplicación para una pizzería, que recibe pedidos de pizzas que incluyen los detalles del tipo de pizza que pide un cliente. Cada detalle de la pizza debe extraerse, si es posible, para completar el procesamiento del pedido.

La entidad de aprendizaje automático de este ejemplo es más compleja, pues tiene subentidades anidadas, listas de frases, entidades precompiladas y entidades personalizadas.

:::image type="content" source="media/luis-concept-features/pizza-order-entity.png" alt-text="Esquema de la entidad de pedidos de pizza":::

En este ejemplo se usan características en el nivel de subentidad y en el elemento secundario del nivel de subentidad. Determinar qué nivel obtiene qué tipo de lista de frases o modelo como una característica es una parte importante del diseño de la entidad.

Aunque las subentidades pueden tener muchas listas de frases como características que ayudan a detectar la entidad, cada subentidad solo tiene un modelo como una característica. En este [aplicación de pizzería](https://github.com/Azure/pizza_luis_bot/blob/master/CognitiveModels/MicrosoftPizza.json), estos modelos son principalmente listas.

:::image type="content" source="media/luis-concept-features/intent-example-utterances-machine-learning-entity-pizza.png" alt-text="Intención de pedido de pizza con expresiones de ejemplo etiquetadas":::

La expresiones de ejemplo correctamente etiquetadas aparecen de modo que se muestre cómo se anidan las entidades. 


## <a name="best-practices"></a>Procedimientos recomendados

Obtenga información sobre los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="next-steps"></a>Pasos siguientes

* [Extienda](schema-change-prediction-runtime.md) los modelos de aplicación en tiempo de ejecución de predicción.
* Vea [Agregar características](luis-how-to-add-features.md) para obtener más información sobre cómo agregar características a la aplicación de LUIS.
