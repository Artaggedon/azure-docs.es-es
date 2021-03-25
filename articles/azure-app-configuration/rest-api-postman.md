---
title: 'API REST de Azure Active Directory: prueba con Postman'
description: Uso de Postman para probar la API REST de Azure App Configuration
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 0b4444b049d181c2f66f8b02a5202dd17a3f4b6b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96932496"
---
# <a name="test-the-azure-app-configuration-rest-api-using-postman"></a>Prueba de la API REST de Azure App Configuration con Postman

Para probar la API REST con [Postman](https://www.getpostman.com/), deberá incluir los encabezados HTTP necesarios para la [autenticación](./rest-api-authentication-hmac.md) en las solicitudes. Aquí se muestra cómo configurar Postman para probar la API REST mediante la generación automática de los encabezados de autenticación:

1. Cree una nueva [solicitud](https://learning.getpostman.com/docs/postman/sending_api_requests/requests/).
1. Agregue la función `signRequest` del [ejemplo de autenticación de JavaScript](./rest-api-authentication-hmac.md#javascript) al [script previo a la solicitud](https://learning.getpostman.com/docs/postman/scripts/pre_request_scripts/) de la solicitud.
1. Agregue el código siguiente al final del script previo a la solicitud. Actualice la clave de acceso como indica el comentario TODO.

    ```js
    // TODO: Replace the following placeholders with your access key
    var credential = "<Credential>"; // Id
    var secret = "<Secret>"; // Value

    var isBodyEmpty = pm.request.body === null || pm.request.body === undefined || pm.request.body.isEmpty();

    var headers = signRequest(
        pm.request.url.getHost(),
        pm.request.method,
        pm.request.url.getPathWithQuery(),
        isBodyEmpty ? undefined : pm.request.body.toString(),
        credential,
        secret);

    // Add headers to the request
    headers.forEach(header => {
        pm.request.headers.upsert({key: header.name, value: header.value});
    })
    ```

1. Envío de la solicitud
