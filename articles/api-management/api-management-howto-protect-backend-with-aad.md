---
title: Protección del back-end de API en API Management mediante OAuth 2.0 y Azure AD
titleSuffix: Azure API Management
description: Aprenda a proteger el acceso a un back-end de API web en Azure API Management con la autorización de usuario de OAuth 2.0 y Azure Active Directory
services: api-management
author: miaojiang
ms.service: api-management
ms.topic: article
ms.date: 09/23/2020
ms.author: apimpm
ms.custom: contperf-fy21q1
ms.openlocfilehash: face4beab450e92be76b2bb90e45625e025de6ee
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97027924"
---
# <a name="protect-a-web-api-backend-in-azure-api-management-by-using-oauth-20-authorization-with-azure-ad"></a>Protección de un back-end de API web en Azure API Management mediante la autorización de OAuth 2.0 con Azure AD 

En esta guía se muestra cómo configurar la instancia de [Azure API Management](api-management-key-concepts.md) para proteger una API mediante el [protocolo OAuth 2.0 con Azure Active Directory (Azure AD)](../active-directory/develop/active-directory-v2-protocols.md). 

> [!NOTE]
> Esta característica está disponible en los niveles **Desarrollador**, **Básico**, **Estándar** y **Premium** de API Management.

## <a name="prerequisites"></a>Requisitos previos

Para seguir los pasos de este artículo, debe tener:

- Una instancia de API Management
- Una API que se vaya a publicar y que use la instancia de API Management
- Un inquilino de Azure AD

## <a name="overview"></a>Información general

A continuación se muestra una introducción rápida de los pasos:

1. Registre una aplicación (aplicación back-end) en Azure AD que represente la API.
1. Registre otra aplicación (aplicación cliente) en Azure AD que represente una aplicación de cliente que deba llamar a la API.
1. En Azure AD, conceda permisos para que la aplicación cliente llame a la aplicación back-end.
1. Configure la consola del desarrollador para llamar a la API con la autorización de usuario OAuth 2.0.
1. Agregue la directiva **validate-jwt** para validar el token de OAuth para todas las solicitudes entrantes.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>Registro de una aplicación en Azure AD que represente la API

Para proteger una API con Azure AD, primero registre una aplicación en Azure AD que la represente. En los pasos siguientes se usa Azure Portal para registrar la aplicación. Para obtener detalles sobre el registro de aplicaciones, vea [Inicio rápido: Configuración de una aplicación para exponer las API web](../active-directory/develop/quickstart-configure-app-expose-web-apis.md).

1. Vaya a [Azure Portal](https://portal.azure.com) para registrar la aplicación. Busque y seleccione **Registros de aplicaciones**.

1. Seleccione **Nuevo registro**. 

1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:

   - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, *backend-app*. 
   - En la sección **Tipos de cuenta admitidos**, seleccione una opción que se ajuste a su escenario. 

1. Deje vacía la sección **URI de redireccionamiento**.

1. Seleccione **Registrar** para crear la aplicación. 

1. En la página **Información general** de la aplicación, busque el valor de **Id. de aplicación (cliente)** y regístrelo para usarlo más tarde.

1. Seleccione **Exponer una API** y establezca la opción **URI de id. de aplicación** con el valor predeterminado. Anote este valor para más adelante.

1. Seleccione el botón **Agregar un ámbito** para mostrar la página **Agregar un ámbito**. Después, cree un ámbito compatible con la API (por ejemplo, `Files.Read`).

1. Seleccione el botón **Agregar ámbito** para crear el ámbito. Repita este paso para agregar todos los ámbitos compatibles con la API.

1. Cuando se creen los ámbitos, anótelos para usarlos en un paso posterior. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Registro de otra aplicación en Azure AD que represente una aplicación cliente

Todas las aplicaciones cliente que llamen a la API deben registrarse como aplicación en Azure AD. En este ejemplo, la aplicación cliente es la **consola del desarrollador** en el portal para desarrolladores de API Management. 

Para registrar otra aplicación en Azure AD que represente la consola del desarrollador:

1. Vaya a [Azure Portal](https://portal.azure.com) para registrar la aplicación.

1. Busque y seleccione **Registros de aplicaciones**.

1. Seleccione **Nuevo registro**.

1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:

   - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, *client-app*. 
   - En la sección **Tipos de cuenta admitidos**, seleccione **Cuentas en cualquier directorio organizativo (cualquier directorio de Azure AD: multiinquilino)** . 

1. En la sección **URI de redirección**, seleccione `Web` y deje el campo de dirección URL vacío por ahora.

1. Seleccione **Registrar** para crear la aplicación. 

1. En la página **Información general** de la aplicación, busque el valor de **Id. de aplicación (cliente)** y regístrelo para usarlo más tarde.

1. Cree un secreto de cliente para esta aplicación que se usará en un paso posterior.

   1. En la lista de páginas de la aplicación cliente, seleccione **Certificados y secretos** y **Nuevo secreto de cliente**.

   1. En **Agregar un secreto de cliente**, proporcione una **descripción**. Elija cuando debe expirar la clave y seleccione **Agregar**.

Cuando se cree el secreto, anote el valor de clave para usarlo en un paso posterior. 

## <a name="grant-permissions-in-azure-ad"></a>Concesión de permisos en Azure AD

Ahora que hemos registrado dos aplicaciones que representan la API y la consola del desarrollador, conceda permisos para que la aplicación cliente pueda llamar a la aplicación back-end.  

1. Vaya a [Azure Portal](https://portal.azure.com) para conceder permisos a la aplicación cliente. Busque y seleccione **Registros de aplicaciones**.

1. Seleccione la aplicación cliente. Después, en la lista de páginas de la aplicación, seleccione **Permisos de API**.

1. Seleccione **Agregar un permiso**.

1. En **Seleccionar una API**, seleccione **Mis API** y busque y seleccione la aplicación de back-end.

1. En **Permisos delegados**, seleccione los permisos adecuados para la aplicación de back-end y, luego, seleccione **Agregar permisos**.

1. De manera opcional, en la página **Permisos de API**, seleccione **Conceder consentimiento del administrador para \<your-tenant-name>** para conceder el consentimiento en nombre de todos los usuarios de este directorio. 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>Habilitación de la autorización de usuario OAuth 2.0 en la consola del desarrollador

En este punto ya se han creado nuestras aplicaciones en Azure AD y se han concedido los permisos pertinentes para que la aplicación cliente pueda llamar a la aplicación back-end. 

En este ejemplo, la consola del desarrollador es la aplicación cliente. En los pasos siguientes se describe la habilitación de la autorización de usuario OAuth 2.0 en la consola del desarrollador. 

1. En Azure Portal, vaya a la instancia de API Management.

1. Seleccione **OAuth 2.0** > **Agregar**.

1. Proporcione un **Nombre para mostrar** y una **Descripción**.

1. Como **URL de la página de registro de cliente**, escriba un valor de marcador de posición como `http://localhost`. La **dirección URL de la página de registro de cliente** señala a una página que los usuarios pueden utilizar para crear y configurar sus propias cuentas para proveedores de OAuth 2.0 que admiten esto. En este ejemplo, los usuarios no crean ni configuran sus propias cuentas, por lo que se usa un marcador de posición en su lugar.

1. En **Tipos de concesión de autorización**, seleccione **Código de autorización**.

1. Especifique la **URL del punto de conexión de autorización** y la **URL del punto de conexión de token**. Recupere estos valores desde la página **Puntos de conexión** del inquilino de Azure AD. Vaya a la página **Registros de aplicaciones** de nuevo y seleccione **Puntos de conexión**.


1. Copie el **punto de conexión de autorización de OAuth 2.0** y péguelo en el cuadro de texto **URL de punto de conexión de autorización**. Seleccione **POST** en Método de solicitud de autorización.

1. Copie el **punto de conexión de token de OAuth 2.0** y péguelo en el cuadro de texto **URL de punto de conexión del token**. 

   >[!IMPORTANT]
   > Use los puntos de conexión **v1** o **v2**. Sin embargo, el paso siguiente será distinto según la versión que elija. Se recomienda usar puntos de conexión v2. 

1. Si usa puntos de conexión **v1**, agregue un parámetro de cuerpo denominado **resource**. Como valor de este parámetro, use el **id. de aplicación** de la aplicación de back-end. 

1. Si usa puntos de conexión **v2**, use el ámbito que creó para la aplicación de back-end en el campo **Ámbito predeterminado**. Además, asegúrese de establecer el valor de la propiedad [`accessTokenAcceptedVersion`](../active-directory/develop/reference-app-manifest.md#accesstokenacceptedversion-attribute) en `2` en el [manifiesto de aplicación](../active-directory/develop/reference-app-manifest.md).

1. A continuación, especifique las credenciales del cliente. Estas son las credenciales para la aplicación cliente.

1. Como **Id. de cliente**, use el **id. de aplicación** de la aplicación cliente.

1. Como **secreto de cliente**, use la clave que creó para la aplicación cliente. 

1. Inmediatamente después del secreto del cliente se encuentra **redirect_url** como tipo de concesión de código de autorización. Anote esta dirección URL.

1. Seleccione **Crear**.

1. Vuelva al registro de la aplicación de cliente en Azure Active Directory y seleccione **Autenticación**.

1. En **Configuraciones de plataforma** haga clic en **Agregar una plataforma** y seleccione el tipo **Web**, pegue el valor de **redirect_url** en **URI de redirección** y, luego, haga clic en el botón **Configurar** para guardar los cambios.

Ahora que ha configurado un servidor de autorización de OAuth 2.0, la consola del desarrollador puede obtener tokens de acceso de Azure AD. 

El paso siguiente consiste en habilitar la autorización de usuario de OAuth 2.0 para la API. Esto permite que la consola del desarrollador sepa que debe obtener un token de acceso en nombre del usuario antes de realizar llamadas a la API.

1. Vaya a la instancia de API Management y a **API**.

1. Seleccione la API que desea proteger. Por ejemplo, `Echo API`.

1. Vaya a **Configuración**.

1. En **Seguridad**, elija **OAuth 2.0** y seleccione el servidor OAuth 2.0 que se configuró anteriormente. 

1. Seleccione **Guardar**.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Llamada correcta a la API desde el portal para desarrolladores

> [!NOTE]
> Esta sección no es aplicable al nivel de **consumo**, que no es compatible con el portal para desarrolladores.

Ahora que la autorización de usuario OAuth 2.0 está habilitada en la API, la consola del desarrollador obtendrá un token de acceso en nombre del usuario antes de llamar a la API.

1. Vaya a cualquier operación de la API en el portal para desarrolladores y, a continuación, seleccione **Probar**. Aparecerá la consola del desarrollador.

1. Tenga en cuenta el nuevo elemento de la sección **Autorización** correspondiente al servidor de autorización que acaba de agregar.

1. Seleccione **Código de autorización** de la lista desplegable de autorizaciones y se le pedirá que inicie sesión en el inquilino de Azure AD. Si ya ha iniciado sesión con la cuenta, es posible que no se le indique nada.

1. Al iniciar sesión correctamente, un encabezado `Authorization` se agregará a la solicitud con un token de acceso de Azure AD. El siguiente es un token de ejemplo (con codificación Base64):

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

1. Seleccione **Enviar** para llamar a la API correctamente.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configurar una directiva de validación de JWT para autorizar previamente las solicitudes

En este momento, si un usuario intenta realizar una llamada desde la consola del desarrollador, se le pide al usuario que inicie sesión. La consola del desarrollador obtiene un token de acceso en nombre del usuario e incluye el token en la solicitud realizada a la API.

Sin embargo, ¿qué ocurre si alguien llama a la API sin un token o con uno no válido? Por ejemplo, si intenta llamar a la API sin el encabezado `Authorization`, la llamada continuará de todos modos. La razón es que API Management no valida el token de acceso en este momento. Simplemente pasa el encabezado `Authorization` a la API de back-end.

Use la directiva de [validación de JWT](./api-management-access-restriction-policies.md#ValidateJWT) para autorizar previamente las solicitudes en API Management validando los tokens de acceso de cada solicitud entrante. Si una solicitud no tiene un token válido, API Management la bloquea. Por ejemplo, agregue la directiva siguiente a la sección de la directiva `<inbound>` de `Echo API`. Comprueba la notificación de audiencia en un token de acceso y devuelve un mensaje de error si el token no es válido. Para información sobre cómo configurar directivas, consulte el artículo sobre [edición o establecimiento de directivas](./set-edit-policies.md).


```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```

> [!NOTE]
> Esta dirección URL de `openid-config` corresponde al punto de conexión v1. Para el punto de conexión v2`openid-config`, use esta URL:
>
> `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`.

## <a name="build-an-application-to-call-the-api"></a>Compilación de una aplicación para llamar a la API

En esta guía, se utiliza la consola del desarrollador en API Management como la aplicación de cliente de ejemplo para llamar a `Echo API` protegido por OAuth 2.0. Para más información sobre cómo compilar una aplicación e implementar OAuth 2.0, consulte [Ejemplos de código de Azure Active Directory](../active-directory/develop/sample-v2-code.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [Escenarios de autenticación para Azure AD](../active-directory/develop/authentication-vs-authorization.md).
- Consulte más [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre la administración de API.
- Para conocer otras formas de proteger el servicio back-end, consulte [Autenticación de certificado mutua](./api-management-howto-mutual-certificates.md).
- [Creación de una instancia del servicio de API Management](./get-started-create-service-instance.md).
- [Administración de su primera API en Administración de API de Azure](./import-and-publish.md).
