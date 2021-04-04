---
title: Presentación de información sobre una coordenada en un mapa | Microsoft Azure Maps
description: Aprenda a mostrar información sobre una dirección del mapa cuando un usuario selecciona una coordenada.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-js
ms.openlocfilehash: 31e4004379340912051204786da592fe33a5bd63
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92890758"
---
# <a name="get-information-from-a-coordinate"></a>Obtención de información de una coordenada

En este artículo se muestra cómo realizar una búsqueda de dirección inversa que muestre la dirección de una ubicación emergente donde ha hecho clic.

Hay dos maneras de realizar una búsqueda de dirección inversa. Una es consultar la [Reverse Address Search API de Azure Maps](/rest/api/maps/search/getsearchaddressreverse) a través de un módulo de servicio. La otra forma es utilizar la [API Fetch](https://fetch.spec.whatwg.org/) para realizar una solicitud a la [Azure Maps Reverse Address Search API](/rest/api/maps/search/getsearchaddressreverse) para buscar una dirección. Ambas formas se describen a detalle a continuación.

## <a name="make-a-reverse-search-request-via-service-module"></a>Realización de una solicitud de búsqueda inversa a través del módulo de servicio

<iframe height='500' scrolling='no' title='Obtención de información de una coordenada (módulo de servicio)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>Get information from a coordinate (Service Module)</a> (Obtención de información de una coordenada) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque construye un objeto de mapa y establece el mecanismo de autenticación para usar el token de acceso. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea un elemento `TokenCredential` para autenticar las solicitudes HTTP en Azure Maps con el token de acceso. A continuación, pasa `TokenCredential` a `atlas.service.MapsURL.newPipeline()` y crea una instancia de [canalización](/javascript/api/azure-maps-rest/atlas.service.pipeline). `searchURL` representa una dirección URL para las operaciones [Search](/rest/api/maps/search) de Azure Maps.

El tercer bloque de código actualiza el estilo del cursor del mouse a un puntero y crea un objeto [emergente](/javascript/api/azure-maps-control/atlas.popup#open). Puede consultar [Adición de un elemento emergente en el mapa](./map-add-popup.md) para obtener instrucciones.

El cuarto bloque de código agrega un [agente de escucha de eventos](/javascript/api/azure-maps-control/atlas.map#events) para los clics del mouse. Una vez desencadenado, crea una consulta de búsqueda con las coordenadas del punto donde hizo clic. A continuación, usa el método [getSearchAddressReverse](/javascript/api/azure-maps-rest/atlas.service.searchurl#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-) para consultar la [Get Search Address Reverse API](/rest/api/maps/search/getsearchaddressreverse) para la dirección de las coordenadas. Se extrae una colección de características de GeoJSON de la respuesta con el método `geojson.getFeatures()`.

El quinto bloque de código configura el contenido emergente HTML para mostrar la dirección de respuesta para la posición de la coordenada donde se ha hecho clic.

El cambio de cursor, el objeto emergente y el evento de hacer clic se crean en el [agente de escucha de eventos de carga](/javascript/api/azure-maps-control/atlas.map#events) del mapa. Esta estructura de código garantiza que el mapa se cargue por completo antes de recuperar la información de coordenadas.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>Realización de una solicitud de búsqueda inversa a través de la API de captura

Haga clic en el mapa para realizar una solicitud de código geográfico inversa para esa ubicación mediante la captura.

<iframe height='500' scrolling='no' title='Obtención de información de una coordenada' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>Obtención de información de una coordenada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa y establece el mecanismo de autenticación para usar el token de acceso. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código actualiza el estilo del cursor del mouse a un puntero. Crea una instancia de un objeto [emergente](/javascript/api/azure-maps-control/atlas.popup#open). Puede consultar [Adición de un elemento emergente en el mapa](./map-add-popup.md) para obtener instrucciones.

El tercer bloque de código agrega un agente de escucha de eventos para los clics del mouse. Una vez que se hace clic en el mouse, utiliza la [API de captura](https://fetch.spec.whatwg.org/) para consultar la [Azure Maps Reverse Address Search API](/rest/api/maps/search/getsearchaddressreverse) para la dirección de coordenadas en la que se ha hecho clic. Para obtener una respuesta correcta, recopila la dirección de la ubicación en la que se hizo clic. Define el contenido y la posición del elemento emergente mediante la función [setOptions](/javascript/api/azure-maps-control/atlas.popup#setoptions-popupoptions-) de la clase emergente.

El cambio de cursor, el objeto emergente y el evento de hacer clic se crean en el [agente de escucha de eventos de carga](/javascript/api/azure-maps-control/atlas.map#events) del mapa. Esta estructura de código garantiza que el mapa se cargue por completo antes de recuperar la información de coordenadas.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados de uso del servicio de búsqueda](how-to-use-best-practices-for-search.md)

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [Popup](/javascript/api/azure-maps-control/atlas.popup)

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Presentación de indicaciones de ruta de A a B](./map-route.md)

> [!div class="nextstepaction"]
> [Visualización del tráfico](./map-show-traffic.md)