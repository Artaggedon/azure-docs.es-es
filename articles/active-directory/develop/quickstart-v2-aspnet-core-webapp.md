---
title: 'Inicio rápido: Incorporación del inicio de sesión con Microsoft a una aplicación web de ASP.NET Core | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, aprenderá cómo una aplicación implementa el inicio de sesión de Microsoft en una aplicación web de ASP.NET mediante OpenID Connect.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/11/2020
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: b2e42dcaeb72a053f20afa47439d1825445b0ad8
ms.sourcegitcommit: 2dd0932ba9925b6d8e3be34822cc389cade21b0d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99225840"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Inicio rápido: Adición del inicio de sesión con Microsoft a una aplicación web de ASP.NET Core

En este inicio rápido, descargará y ejecutará un código de ejemplo que muestra cómo una aplicación web de ASP.NET Core puede realizar el inicio de sesión de los usuarios desde cualquier organización de Azure Active Directory.  

Para ilustrar este tema, consulte el apartado en el que se explica el [funcionamiento del ejemplo](#how-the-sample-works).

> [!div renderon="docs"]
> ## <a name="prerequisites"></a>Prerrequisitos
>
> * [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) o [Visual Studio Code](https://code.visualstudio.com/)
> * [SDK de .NET Core 3.1+](https://dotnet.microsoft.com/download)
>
> ## <a name="register-and-download-your-quickstart-app"></a>Registro y descarga de la aplicación de inicio rápido
> Tiene dos opciones para comenzar con la aplicación de inicio rápido:
> * [Rápido] [Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Manual] [Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo
>
> 1. Vaya a la experiencia de inicio rápido <a href="https://aka.ms/aspnetcore2-1-aad-quickstart-v2/" target="_blank">Azure Portal: Registros de aplicaciones<span class="docon docon-navigate-external x-hidden-focus"></span></a>.
> 1. Escriba un nombre para la aplicación y seleccione **Registrar**.
> 1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación en un solo clic.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo
>
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
> Para registrar la aplicación y agregar la información de registro de la aplicación a la solución de forma manual, siga estos pasos:
>
> 1. Inicie sesión en <a href="https://portal.azure.com/" target="_blank">Azure Portal<span class="docon docon-navigate-external x-hidden-focus"></span></a>.
> 1. Si tiene acceso a varios inquilinos, use el filtro **Directorio + suscripción** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: del menú superior para seleccionar el inquilino en el que desea registrar una aplicación.
> 1. Busque y seleccione **Azure Active Directory**.
> 1. En **Administrar**, seleccione **Registros de aplicaciones** >  y, luego, **Nuevo registro**.
> 1. Escriba el **Nombre** de la aplicación, por ejemplo `AspNetCore-Quickstart`. Los usuarios de la aplicación pueden ver este nombre, el cual se puede cambiar más tarde.
> 1. Escriba un **URI de redirección** de `https://localhost:44321/`.
> 1. Seleccione **Registrar**.
> 1. En **Administrar**, seleccione **Autenticación**.
> 1. En **URI de redirección**, seleccione **Agregar URI** y escriba `https://localhost:44321/signin-oidc`.
> 1. Especifique un valor para **Front-channel logout URL** (Dirección URL de cierre de sesión del canal frontal) de `https://localhost:44321/signout-oidc`.
> 1. En **Implicit grant and hybrid flows** (Flujos de concesión implícita e híbridos), seleccione **Tokens de id.**
> 1. Seleccione **Guardar**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> El código de ejemplo de este inicio rápido requiere un **URI de redirección**  de `https://localhost:44321/` y `https://localhost:44321/signin-oidc`, y un valor para **Front-channel logout URL** (Dirección URL de cierre de sesión del canal frontal) de `https://localhost:44321/signout-oidc`. El punto de conexión de autorización emitirá los tokens de identificador de solicitud.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-aspnet-webapp/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-your-aspnet-core-project"></a>Paso 2: Descarga del proyecto de ASP.NET Core

> [!div renderon="docs"]
> [Descargar la solución de ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)

> [!div renderon="portal" class="sxs-lookup"]
> Ejecute el proyecto.

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse
> Hemos configurado el proyecto con los valores de las propiedades de su aplicación y está preparado para ejecutarse.
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> [!div renderon="docs"]
> #### <a name="step-3-configure-your-aspnet-core-project"></a>Paso 3: Configuración del proyecto de ASP.NET Core
> 1. Extraiga el archivo. zip en una carpeta local cerca de la raíz de la unidad. Por ejemplo, en *C:\Azure-Samples*.
> 1. Abra la solución en Visual Studio 2019.
> 1. Abra el archivo *appsettings.json* y modifique lo siguiente:
>
>    ```json
>    "ClientId": "Enter_the_Application_Id_here",
>    "TenantId": "common",
>    ```
>
>    - Reemplace `Enter_the_Application_Id_here` por el **Id. de aplicación (cliente)** de la aplicación que ha registrado en Azure Portal. Puede encontrar el **Identificador de aplicación (cliente)** en la página **Información general** de la aplicación.
>    - Reemplace `common` por una de las opciones siguientes:
>       - Si la aplicación admite **Solo las cuentas de este directorio organizativo**, reemplace este valor por el **Id. de directorio (inquilino)** (un GUID) o por el **Nombre del inquilino** (por ejemplo, `contoso.onmicrosoft.com`). Puede encontrar el **Id. de directorio (inquilino)** en la página **Introducción** de la aplicación.
>       - Si la aplicación admite **Cuentas en cualquier directorio organizativo**, reemplace este valor por `organizations`
>       - Si la aplicación admite **Todos los usuarios de cuentas Microsoft**, deje este valor establecido en `common`.
>
> Para esta guía de inicio rápido, no cambie ningún otro valor del archivo *appsettings.json*.
>
> #### <a name="step-4-build-and-run-the-application"></a>Paso 4: Compilación y ejecución de la aplicación
>
> Para compilar y ejecutar la aplicación en Visual Studio, seleccione el menú **Depurar** > **Iniciar depuración** o presione la tecla `F5`.
>
> Se le pedirán las credenciales y, a continuación, se le pedirá que dé su consentimiento a los permisos que requiere la aplicación. Seleccione **Aceptar** en la solicitud de consentimiento.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp/webapp-01-consent.png" alt-text="Cuadro de diálogo de consentimiento que muestra los permisos que solicita la aplicación al usuario >":::
>
> Después de haber dado su consentimiento a los permisos solicitados, la aplicación muestra que ha iniciado sesión correctamente con sus credenciales de Azure Active Directory.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp/webapp-02-signed-in.png" alt-text="Explorador web que muestra la aplicación web en ejecución y el usuario con la sesión iniciada":::

## <a name="more-information"></a>Más información

En esta sección, se proporciona una introducción al código necesario para el inicio de sesión de usuarios. Esto puede ser útil para comprender cómo funciona el código, los argumentos principales y también si desea agregar el inicio de sesión a una aplicación ASP.NET Core existente.

### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo
![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

### <a name="startup-class"></a>Clase Startup

El middleware *Microsoft.AspNetCore.Authentication* usa una clase `Startup` que se ejecuta cuando se inicializa el proceso de hospedaje:

```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
          .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"));

      services.AddControllersWithViews(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
          options.Filters.Add(new AuthorizeFilter(policy));
      });
      services.AddRazorPages()
          .AddMicrosoftIdentityUI();
  }
```

El método `AddAuthentication()` configura el servicio para agregar la autenticación basada en cookies, que se usa en escenarios con explorador, y para establecer el desafío en OpenId Connect.

La línea que contiene `.AddMicrosoftIdentityWebApp` agrega la autenticación de la Plataforma de identidad de Microsoft a la aplicación. Después se configura para iniciar sesión mediante la plataforma de identidad de Microsoft según la información de la sección `AzureAD` del archivo de configuración *appsettings.json*:

| Clave de *appsettings.json* | Descripción                                                                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ClientId`             | El **Id. de aplicación (cliente)** de la aplicación registrada en Azure Portal.                                                                                       |
| `Instance`             | El punto de conexión del servicio de token de seguridad (STS) para que el usuario se autentique. Normalmente, este valor es `https://login.microsoftonline.com/`, que indica la nube pública de Azure. |
| `TenantId`             | Nombre o id. (un GUID) del inquilino, o bien *common*, para el inicio de sesión de usuarios con cuentas profesionales o educativas o cuentas personales de Microsoft.                             |

El método `Configure()` contiene dos métodos importantes, `app.UseAuthentication()` y `app.UseAuthorization()`, que habilitan su funcionalidad con nombre. También en el método `Configure()`, debe registrar las rutas de Microsoft Identity Web con al menos una llamada a `endpoints.MapControllerRoute()` o a `endpoints.MapControllers()`.

```csharp
app.UseAuthentication();
app.UseAuthorization();

app.UseEndpoints(endpoints =>
{

    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});

// endpoints.MapControllers(); // REQUIRED if MapControllerRoute() isn't called.
```

### <a name="protect-a-controller-or-a-controllers-method"></a>Protección de un controlador o del método de un controlador

Puede proteger un controlador o los métodos de un controlador mediante el atributo `[Authorize]`. Este atributo restringe el acceso al controlador o a los métodos, ya que solo permite usuarios autenticados, lo que significa que se puede iniciar el desafío de autenticación para acceder al controlador si el usuario no está autenticado.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

El repositorio de GitHub que contiene este tutorial de ASP.NET Core incluye instrucciones y más ejemplos de código que muestran cómo:

- Agregar autenticación a una nueva aplicación web de ASP.NET Core
- Llamar a Microsoft Graph u otras API de Microsoft en sus propias API web
- Agregar autorización
- Iniciar la sesión de usuarios en nubes nacionales o con identidades de redes sociales

> [!div class="nextstepaction"]
> [Tutoriales de aplicaciones web de ASP.NET Core en GitHub](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
