---
title: Autenticación entre servicios de Azure AD mediante OAuth2.0 | Microsoft Docs
description: Este artículo describe cómo utilizar los mensajes HTTP para implementar la autenticación entre servicios mediante el flujo de concesión de credenciales de cliente de OAuth2.0.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: ryanwi
ms.reviewer: nacanuma
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 977dfea28c5c0dc3f34ada0c138556d70c979e04
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "85551702"
---
# <a name="service-to-service-calls-using-client-credentials-shared-secret-or-certificate"></a>Llamadas entre servicios mediante las credenciales del cliente (secreto compartido o certificado)

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

El flujo de concesión de credenciales de cliente de OAuth 2.0 permite a un servicio web (*cliente confidencial*) usar sus propias credenciales para autenticarse al llamar a otro servicio web, en lugar de suplantar a un usuario. En este escenario, el cliente es normalmente un servicio web de nivel intermedio, un servicio demonio o un sitio web. Para conseguir un mayor nivel de control, Azure AD también permite al servicio que realiza la llamada usar un certificado (en lugar de un secreto compartido) como credencial.

## <a name="client-credentials-grant-flow-diagram"></a>Diagrama de flujo de concesión de credenciales de cliente
El diagrama siguiente explica cómo funciona el flujo de concesión de credenciales de cliente en Azure Active Directory (Azure AD).

![Flujo de concesión de credenciales de cliente de OAuth2.0](./media/v1-oauth2-client-creds-grant-flow/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. La aplicación cliente se autentica en el punto de conexión de emisión de tokens de Azure AD y solicita un token de acceso.
2. El punto de conexión de emisión de tokens de Azure AD emite el token de acceso.
3. El token de acceso se utiliza para autenticar los recursos protegidos.
4. Los datos de los recursos protegidos se devuelven a la aplicación cliente.

## <a name="register-the-services-in-azure-ad"></a>Registro de los servicios en Azure AD
Registre el servicio de llamada y el servicio de recepción en Azure Active Directory (Azure AD). Para obtener más información, consulte [Integración de aplicaciones con Azure Active Directory](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

## <a name="request-an-access-token"></a>Solicitar un token de acceso
Para solicitar un token de acceso, utilice una solicitud HTTP POST al punto de conexión específico del inquilino de Azure AD.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Solicitud de token de acceso entre servicios
Se pueden dar dos casos en función de si la aplicación cliente elige un secreto compartido o un certificado para su protección.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primer caso: solicitud de token de acceso con un secreto compartido
Cuando se utiliza un secreto compartido, una solicitud de token de acceso entre servicios contiene los parámetros siguientes:

| Parámetro | Tipo | Descripción |
| --- | --- | --- |
| grant_type |requerido |Especifica el tipo de concesión solicitado. En un flujo de concesión de credenciales de cliente, el valor debe ser **client_credentials**. |
| client_id |requerido |Especifica el identificador de cliente de Azure AD del servicio web que realiza la llamada. Para buscar el identificador del cliente de la aplicación que realiza la llamada, en [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory** y **Registros de aplicaciones**, y haga clic en la aplicación. El identificador client_id es el *identificador de aplicación* |
| client_secret |requerido |Escriba una clave registrada para el servicio web de llamada o la aplicación de demonio de Azure AD. Para crear una clave, en Azure Portal, haga clic en **Azure Active Directory** y en **Registros de aplicaciones**, haga clic en la aplicación, en **Configuración** y en **Claves** y agregue una clave.  Cuando lo proporcione, codifique como dirección URL este secreto. |
| resource |requerido |Escriba el URI del identificador de la aplicación del servicio web de recepción. Para buscar el URI del identificador de la aplicación, en Azure Portal, haga clic en **Azure Active Directory** y en **Registros de aplicaciones**, haga clic en la aplicación de servicio y, a continuación, en **Configuración** y en **Propiedades**. |

#### <a name="example"></a>Ejemplo
El siguiente elemento HTTP POST solicita un [token de acceso](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) para el servicio web `https://service.contoso.com/`. El parámetro `client_id` permite identificar el servicio web que solicita el token de acceso.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Segundo caso: solicitud de token de acceso con un certificado
Una solicitud de token de acceso entre servicios con un certificado contiene los parámetros siguientes:

| Parámetro | Tipo | Descripción |
| --- | --- | --- |
| grant_type |requerido |Especifica el tipo de respuesta solicitado. En un flujo de concesión de credenciales de cliente, el valor debe ser **client_credentials**. |
| client_id |requerido |Especifica el identificador de cliente de Azure AD del servicio web que realiza la llamada. Para buscar el identificador del cliente de la aplicación que realiza la llamada, en [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory** y **Registros de aplicaciones**, y haga clic en la aplicación. El identificador client_id es el *identificador de aplicación* |
| client_assertion_type |requerido |El valor debe ser `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |requerido | Aserción (un JSON Web Token) que debe crear y firmar con el certificado que ha registrado como credenciales de la aplicación. Lea el artículo sobre las [credenciales de certificado](../develop/active-directory-certificate-credentials.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) para información sobre cómo registrar el certificado y el formato de la aserción.|
| resource | requerido |Escriba el URI del identificador de la aplicación del servicio web de recepción. Para buscar el URI del identificador de la aplicación, en Azure Portal, haga clic en **Azure Active Directory** y en **Registros de aplicaciones**, haga clic en la aplicación de servicio y, a continuación, en **Configuración** y en **Propiedades**. |

Tenga en cuenta que los parámetros son casi iguales que en el caso de solicitud con un secreto compartido, salvo que el parámetro client_secret se sustituye por dos parámetros: client_assertion_type y client_assertion.

#### <a name="example"></a>Ejemplo
El siguiente elemento HTTP POST solicita un token de acceso para el servicio web `https://service.contoso.com/` con un certificado. El parámetro `client_id` permite identificar el servicio web que solicita el token de acceso.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### <a name="service-to-service-access-token-response"></a>Respuesta de token de acceso entre servicios

Una respuesta correcta contiene una respuesta de OAuth 2.0 de JSON con los siguientes parámetros:

| Parámetro | Descripción |
| --- | --- |
| access_token |El token de acceso solicitado. El servicio web de llamada puede usar este token para autenticarse en el servicio web de recepción. |
| token_type |Indica el valor de tipo de token. El único tipo que admite Azure AD es el **portador**. Para más información sobre los tokens de portador, consulte [OAuth2.0 Authorization Framework: Bearer Token Usage (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt) (Marco de autorización de OAuth2.0: uso del token de portador [RFC 6750]). |
| expires_in |Durante cuánto tiempo es válido el token de acceso (en segundos). |
| expires_on |La hora a la que expira el token de acceso. La fecha se representa como el número de segundos desde 1970-01-01T0:0:0Z UTC hasta la fecha de expiración. Este valor se utiliza para determinar la duración de los tokens almacenados en caché. |
| not_before |La hora a partir de la cual el token de acceso pasa a ser utilizable. La fecha se representa como el número de segundos desde 1970-01-01T0:0:0Z UTC hasta la fecha de expiración del token.|
| resource |El URI del identificador de la aplicación del servicio web de recepción. |

#### <a name="example-of-response"></a>Ejemplo de respuesta
En el ejemplo siguiente se muestra una respuesta correcta a una solicitud de un token de acceso a un servicio web.

```
{
"access_token":"eyJ0eXAiO ... 0X2tnSQLEANnSPHY0gKcgw",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```
## <a name="use-the-access-token-to-access-the-secured-resource"></a>Usar el token de acceso para obtener acceso al recurso protegido

El servicio puede usar el token de acceso adquirido para realizar solicitudes autenticadas a la API web de bajada mediante el establecimiento del token en el encabezado `Authorization`.

### <a name="example"></a>Ejemplo

```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiO ... 0X2tnSQLEANnSPHY0gKcgw
```

## <a name="see-also"></a>Consulte también
* [OAuth 2.0 en Azure AD](v1-protocols-oauth-code.md)
* [Ejemplo en C# de la llamada entre servicios con un secreto compartido](https://github.com/Azure-Samples/active-directory-dotnet-daemon) y [Ejemplo en C# de la llamada entre servicios con un certificado](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
