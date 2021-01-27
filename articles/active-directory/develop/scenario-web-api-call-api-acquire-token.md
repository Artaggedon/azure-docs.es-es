---
title: Obtención de un token para una API web que llama a otras API web | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a compilar una API web que llama a otras API web que necesitan adquirir un token para la aplicación.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9f9758ec765ad34e5ef5d8b4d4e0a420a6701b6e
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98756389"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>API web que llama a API web: Adquisición de un token para la aplicación

Una vez que ha creado un objeto de aplicación cliente, lo puede usar para adquirir un token y usarlo para llamar a una API web.

## <a name="code-in-the-controller"></a>Código del controlador

### <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

*Microsoft.Identity.Web* agrega métodos de extensión que proporcionan servicios útiles para llamar a Microsoft Graph o a una API web de nivel inferior. Estos métodos se explican en detalle en [Una API web que llama a las API web: llamada a una API](scenario-web-api-call-api-call-api.md). Con estos métodos auxiliares, no es necesario adquirir un token manualmente.

Sin embargo, si desea adquirir manualmente un token, el código siguiente muestra un ejemplo de uso de *Microsoft.Identity.Web* para hacerlo en un controlador de API. Llama a una API de bajada denominada *todolist*.
Para obtener un token para llamar a la API de nivel inferior, se inserta el servicio `ITokenAcquisition` mediante la inserción de dependencias en el constructor del controlador (o el constructor de la página si utiliza Blazor) y se usa en las acciones del controlador, de modo que se obtiene un token para el usuario (`GetAccessTokenForUserAsync`) o para la propia aplicación (`GetAccessTokenForAppAsync`) en caso de un escenario de demonio.

```csharp
[Authorize]
public class MyApiController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    static readonly string[] scopeRequiredByApi = new string[] { "access_as_user" };

     static readonly string[] scopesToAccessDownstreamApi = new string[] { "api://MyTodolistService/access_as_user" };

    private readonly ITokenAcquisition _tokenAcquisition;

    public MyApiController(ITokenAcquisition tokenAcquisition)
    {
        _tokenAcquisition = tokenAcquisition;
    }

    public IActionResult Index()
    {
        HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi);

        string accessToken = _tokenAcquisition.GetAccessTokenForUserAsync(scopesToAccessDownstreamApi);
        return await callTodoListService(accessToken);
    }
}
```

Para más información sobre el método `callTodoListService`, consulte [Una API web que llama a las API web: llamada a una API](scenario-web-api-call-api-call-api.md).

### <a name="java"></a>[Java](#tab/java)

A continuación, se muestra un ejemplo de código que se llama en las acciones de los controladores de API. Llama a la API de nivel inferior de Microsoft Graph.

```java
@RestController
public class ApiController {

    @Autowired
    MsalAuthHelper msalAuthHelper;

    @RequestMapping("/graphMeApi")
    public String graphMeApi() throws MalformedURLException {

        String oboAccessToken = msalAuthHelper.getOboToken("https://graph.microsoft.com/.default");

        return callMicrosoftGraphMeEndpoint(oboAccessToken);
    }

}
```

### <a name="python"></a>[Python](#tab/python)
 
Una API web de Python requiere el uso de middleware para validar el token de portador recibido del cliente. A continuación, la API web podrá obtener el token de acceso para una API de nivel inferior mediante la biblioteca de MSAL para Python al llamar al método [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of).
 
A continuación se muestra un ejemplo de código que adquiere un token de acceso mediante el método `acquire_token_on_behalf_of` y el marco de Flask. Llama a la API de nivel inferior: el punto de conexión de suscripciones de Azure Management.
 
```python
def get(self):
 
        _scopes = ["https://management.azure.com/user_impersonation"]
        _azure_management_subscriptions_uri = "https://management.azure.com/subscriptions?api-version=2020-01-01"
 
        current_access_token = request.headers.get("Authorization", None)
        
        #This example only uses the default memory token cache and should not be used for production
        msal_client = msal.ConfidentialClientApplication(
                client_id=os.environ.get("CLIENT_ID"),
                authority=os.environ.get("AUTHORITY"),
                client_credential=os.environ.get("CLIENT_SECRET"))
 
        #acquire token on behalf of the user that called this API
        arm_resource_access_token = msal_client.acquire_token_on_behalf_of(
            user_assertion=current_access_token.split(' ')[1],
            scopes=_scopes
        )
 
        headers = {'Authorization': arm_resource_access_token['token_type'] + ' ' + arm_resource_access_token['access_token']}
 
        subscriptions_list = req.get(_azure_management_subscriptions_uri), headers=headers).json()
 
        return jsonify(subscriptions_list)
```

## <a name="advanced-accessing-the-signed-in-users-token-cache-from-background-apps-apis-and-services"></a>(Avanzado) Acceso a la memoria caché de tokens del usuario que ha iniciado sesión desde aplicaciones en segundo plano, API y servicios

[!INCLUDE [advanced-token-caching](../../../includes/advanced-token-cache.md)]

---

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo de este escenario, [Llamada a una API](scenario-web-api-call-api-call-api.md).
