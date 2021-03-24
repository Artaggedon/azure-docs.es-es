---
title: 'Idiomas admitidos: Bing News Search API'
titleSuffix: Azure Cognitive Services
description: Una lista de los idiomas naturales, países y regiones admitidos por Bing News Search API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: 9c7fd03c2239cea05dc79ad4dd1965fe253a2ce9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96341604"
---
# <a name="language-and-region-support-for-the-bing-news-search-api"></a>Compatibilidad de idiomas y regiones para Bing News Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020**, las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing News Search API admite numerosos países o regiones, muchos de ellos con más de un idioma. La especificación de un país o región en una consulta sirve principalmente para refinar los resultados de la búsqueda en función de los intereses en ese país o región. Además, los resultados pueden contener vínculos a Bing, y estos vínculos pueden localizar la experiencia del usuario de Bing según el país o región especificado o el idioma.

Puede especificar un país o región utilizando el parámetro de consulta `cc`. Si especifica un país o región, también debe especificar uno o más códigos de idioma con el encabezado HTTP `Accept-Language`. Los idiomas admitidos varían según el país o región; se facilitan para cada país en la tabla Mercados.

Como alternativa, puede especificar el mercado con el parámetro de consulta `mkt` y un código de la tabla **Mercados**. La especificación de un mercado especifica al mismo tiempo un país o región y un idioma preferido. El parámetro de consulta `setLang` puede establecerse en un código de idioma en este caso; normalmente se trata del mismo idioma especificado por `mkt` a menos que el usuario prefiera ver Bing en otro idioma.

## <a name="supported-markets-for-news-search-endpoint"></a>Mercados compatibles para el punto de conexión de búsqueda de noticias

En la tabla siguiente se listan los valores de código de mercado para el punto de conexión `/news/search` que puede usar para especificar el parámetro de consulta `mkt`. Bing devuelve contenido para estos mercados solamente. La lista está sujeta a cambios.  

Para obtener una lista de códigos de país que puede especificar en el parámetro de consulta `cc`, consulte [Códigos de país](#countrycodes).  

|País/región|Idioma|Código de mercado|  
|---------------------|--------------|-----------------|
|Dinamarca|Danés|da-DK|
|Austria|Alemán|de-AT|
|Suiza|Alemán|de-CH|
|Alemania|Alemán|de-DE|
|Australia|Inglés|en-AU|
|Canadá|Inglés|en-CA|
|Reino Unido|Inglés|en-GB|
|Indonesia|Inglés|en-ID|
|Irlanda|Inglés|en-IE|
|India|Inglés|en-IN|
|Malasia|Inglés|en-MY|
|Nueva Zelanda|Inglés|en-NZ|
|República de Filipinas|Inglés|en-PH|
|Singapur|Inglés|en-SG|
|Estados Unidos|Inglés|es-ES|
|Inglés|general|en-WW|
|Inglés|general|en-XA|
|Sudáfrica|Inglés|en-ZA|
|Argentina|Español|es-AR|
|Chile|Español|es-CL|
|España|Español|es-ES|
|México|Español|es-MX|
|Estados Unidos|Español|es-US|
|Español|general|es-XL|
|Finlandia|Finés|fi-FI|  
|Francia|Francés|fr-BE|
|Canadá|Francés|fr-CA|
|Bélgica|Neerlandés|nl-BE|
|Suiza|Francés|fr-CH|
|Francia|Francés|fr-FR|  
|Italia|Italiano|it-IT|
|RAE de Hong Kong|Chino tradicional|zh-HK|  
|Taiwán|Chino tradicional|zh-TW|
|Japón|Japonés|ja-JP|  
|Corea|Coreano|ko-KR|  
|Países Bajos|Neerlandés|nl-NL|  
|República Popular China|Chino|zh-CN|  
|Brasil|Portugués|pt-BR|
|Rusia|Ruso|ru-RU|  
|Suecia|Sueco|sv-SE|  
|Turquía|Turco|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Mercados compatibles para el punto de conexión de noticias
En la tabla siguiente se listan los valores de código de mercado para el punto de conexión `/news` que puede usar para especificar el parámetro de consulta `mkt`. Bing devuelve contenido para estos mercados solamente. La lista está sujeta a cambios.  

Para obtener una lista de códigos de país que puede especificar en el parámetro de consulta `cc`, consulte [Códigos de país](#countrycodes).  

|País/región|Idioma|Código de mercado|  
|---------------------|--------------|-----------------|
|Dinamarca|Danés|da-DK|
|Alemania|Alemán|de-DE|
|Australia|Inglés|en-AU|
|Reino Unido|Inglés|en-GB|
|Estados Unidos|Inglés|es-ES|
|Inglés|general|en-WW|
|Chile|Español|es-CL|
|México|Español|es-MX|
|Estados Unidos|Español|es-US|
|Finlandia|Finés|fi-FI|  
|Canadá|Francés|fr-CA|
|Francia|Francés|fr-FR|  
|Italia|Italiano|it-IT|
|Brasil|Portugués|pt-BR|
|República Popular China|Chino|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Mercados compatibles para el punto de conexión de noticias populares
En la tabla siguiente se listan los valores de código de mercado para el punto de conexión `/news/trendingtopics` que puede usar para especificar el parámetro de consulta `mkt`. Bing devuelve contenido para estos mercados solamente. La lista está sujeta a cambios.  

Para obtener una lista de códigos de país que puede especificar en el parámetro de consulta `cc`, consulte [Códigos de país](#countrycodes).  

|País/región|Idioma|Código de mercado|  
|---------------------|--------------|-----------------|
|Alemania|Alemán|de-DE|
|Australia|Inglés|en-AU|
|Reino Unido|Inglés|en-GB|
|Estados Unidos|Inglés|es-ES|
|Canadá|Inglés|en-CA|
|India|Inglés|en-IN|
|Francia|Francés|fr-FR|
|Canadá|Francés|fr-CA|
|Brasil|Portugués|pt-BR|
|República Popular China|Chino|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Códigos de país  

Los códigos de país o región siguientes se pueden especificar en el parámetro de consulta `cc`. La lista está sujeta a cambios.  

|País/región|Código de país|  
|---------------------|------------------|  
|Argentina|AR|  
|Australia|AU|  
|Austria|AT|  
|Bélgica|BE|  
|Brasil|BR|  
|Canadá|CA|  
|Chile|CL|  
|Dinamarca|DK|  
|Finlandia|FI|  
|Francia|VF|  
|Alemania|DE|  
|RAE de Hong Kong|HK|  
|India|IN|  
|Indonesia|ID|  
|Italia|IT|  
|Japón|JP|  
|Corea|KR|  
|Malasia|MY|  
|México|MX|  
|Países Bajos|NL|  
|Nueva Zelanda|NZ|  
|Noruega|No|  
|República Popular China|CN|  
|Polonia|PL|  
|Portugal|PT|  
|República de Filipinas|PH|  
|Rusia|RU|  
|Arabia Saudí|SA|  
|Sudáfrica|ZA|  
|España|ES|  
|Suecia|SE|  
|Suiza|CH|  
|Taiwán|TW|  
|Turquía|TR|  
|Reino Unido|GB|  
|Estados Unidos|US|

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre los puntos de conexión de Bing News Search, consulte [News Search API v7 reference](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) (Referencia de News Search API v7).