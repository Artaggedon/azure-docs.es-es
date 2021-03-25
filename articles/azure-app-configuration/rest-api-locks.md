---
title: 'API REST de Azure App Configuration: bloqueos'
description: Páginas de referencia para trabajar con bloqueos de clave y valor mediante la API REST de Azure App Configuration
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: e99ce5595bae8ed64285317d9249da60e0fc1b83
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96932530"
---
# <a name="locks"></a>Bloqueos

Esta API (versión 1.0) proporciona semántica de bloqueo y desbloqueo para el recurso de clave y valor. Admite las siguientes operaciones:

- Establecer bloqueo
- Eliminar bloqueo

Si está presente, `label` debe ser un valor de etiqueta explícito (no un carácter comodín). Es un parámetro opcional para todas las operaciones. Si se omite, significa que no hay etiqueta.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="lock-key-value"></a>Bloqueo de clave y valor

- Obligatorio: ``{key}``, ``{api-version}``  
- Opcional: ``label``

```http
PUT /locks/{key}?label={label}&api-version={api-version} HTTP/1.1
```

**Respuestas:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8"
```

```json
{
  "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
  "key": "{key}",
  "label": "{label}",
  "content_type": null,
  "value": "example value",
  "created": "2017-12-05T02:41:26.4874615+00:00",
  "locked": true,
  "tags": []
}
```

Si el elemento de clave y valor no existe, se devuelve la respuesta siguiente:

```http
HTTP/1.1 404 Not Found
```

## <a name="unlock-key-value"></a>Desbloqueo de clave y valor

- Obligatorio: ``{key}``, ``{api-version}``  
- Opcional: ``label``

```http
DELETE /locks/{key}?label={label}?api-version={api-version} HTTP/1.1
```

**Respuestas:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8"
```

```json
{
  "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
  "key": "{key}",
  "label": "{label}",
  "content_type": null,
  "value": "example value",
  "created": "2017-12-05T02:41:26.4874615+00:00",
  "locked": true,
  "tags": []
}
```

Si el elemento de clave y valor no existe, se devuelve la respuesta siguiente:

```http
HTTP/1.1 404 Not Found
```

## <a name="conditional-lock-and-unlock"></a>Bloqueo y desbloqueo condicionales

Para evitar condiciones de carrera, use los encabezados de solicitud `If-Match` o `If-None-Match`. El argumento `etag` forma parte de la representación de la clave. Si se omiten `If-Match` o `If-None-Match`, la operación es incondicional.

La siguiente solicitud aplica la operación solo si la representación de clave y valor actual coincide con el elemento `etag`especificado:

```http
PUT|DELETE /locks/{key}?label={label}&api-version={api-version} HTTP/1.1
If-Match: "4f6dd610dd5e4deebc7fbaef685fb903"
```

La siguiente solicitud aplica la operación solo si la representación de clave y valor actual existe pero no coincide con el elemento `etag`especificado:

```http
PUT|DELETE /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
If-None-Match: "4f6dd610dd5e4deebc7fbaef685fb903"
```
