---
title: Configuración de Azure AD B2C como IdP de SAML para las aplicaciones
title-suffix: Azure AD B2C
description: Procedimiento para configurar Azure AD B2C para proporcionar aserciones de protocolo SAML a las aplicaciones (proveedores de servicios). Azure AD B2C actuará como proveedor de identidades (IdP) único en la aplicación SAML.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/17/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 96a72dbc0e45ebd50a49000ae66e3713cb28aa9a
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98916939"
---
# <a name="register-a-saml-application-in-azure-ad-b2c"></a>Registrar una aplicación SAML en Azure AD B2C

En este artículo, aprenderá a configurar Azure Active Directory B2C (Azure AD B2C) para que actúe como proveedor de identidades (IdP) de SAML (Lenguaje de marcado de aserción de seguridad) para sus aplicaciones.

## <a name="scenario-overview"></a>Información general de escenario

Las organizaciones que usan Azure AD B2C como solución de administración de acceso e identidad de clientes pueden requerir la interacción con aplicaciones o proveedores de identidades que estén configurados para autenticarse mediante el protocolo SAML.

Azure AD B2C consigue la interoperabilidad de SAML de una de estas dos maneras:

* Actuando como *proveedor de identidades* (IdP) y logrando el inicio de sesión único (SSO) con proveedores de servicios basados en SAML (sus aplicaciones)
* Actuando como *proveedor de servicios* (SP) e interactuando con proveedores de identidades basados en SAML, como Salesforce y ADFS

![Diagrama con B2C como proveedor de identidades a la izquierda y B2C como proveedor de servicios a la derecha](media/saml-identity-provider/saml-idp-diagram-01.jpg)

Resumen de los dos escenarios principales no exclusivos con SAML:

| Escenario | Rol de Azure AD B2C | Instrucciones |
| -------- | ----------------- | ------- |
| Mi aplicación espera una aserción de SAML para completar una autenticación. | **Azure AD B2C actúa como proveedor de identidades (IdP)**<br />Azure AD B2C actúa como IdP de SAML para las aplicaciones. | Este artículo. |
| Mis usuarios necesitan un inicio de sesión único con un proveedor de identidades compatible con SAML, como ADFS, Salesforce o Shibboleth.  | **Azure AD B2C actúa como proveedor de servicios (SP)**<br />Azure AD B2C actúa como proveedor de servicios al conectarse al proveedor de identidades de SAML. Es un proxy de federación entre la aplicación y el proveedor de identidades de SAML.  | <ul><li>[Configuración del inicio de sesión con IdP de SAML y ADFS mediante directivas personalizadas](identity-provider-adfs.md)</li><li>[Configuración del inicio de sesión con un proveedor de SAML Salesforce mediante directivas personalizadas](identity-provider-salesforce-saml.md)</li></ul> |

## <a name="prerequisites"></a>Requisitos previos

* Realice los pasos del artículo [Introducción a las directivas personalizadas en Azure AD B2C](custom-policy-get-started.md). Necesita la directiva personalizada *SocialAndLocalAccounts* del paquete de inicio de la directiva personalizada que se describe en el artículo.
* Conocimientos básicos del protocolo SAML (Lenguaje de marcado de aserción de seguridad).
* Una aplicación web configurada como un proveedor de servicios SAML (SP). En este tutorial, puede usar una [aplicación de prueba SAML][samltest] que proporcionamos.

## <a name="components-of-the-solution"></a>Componentes de la solución

Hay tres componentes principales necesarios para este escenario:

* El proveedor de servicios **SAML** con la posibilidad de enviar solicitudes de SAML y recibir, descodificar y responder a las aserciones de SAML de Azure AD B2C. El proveedor de servicios también se conoce como la aplicación de usuario de confianza.
* El **punto de conexión de metadatos** de SAML disponible públicamente para el proveedor de servicios.
* [Inquilino de Azure AD B2C](tutorial-create-tenant.md)

Si aún no tiene un proveedor de servicios SAML y un punto de conexión de metadatos asociado, puede usar esta aplicación SAML de ejemplo que hemos puesto a disposición para las pruebas:

[Aplicación de prueba SAML][samltest]

## <a name="1-set-up-certificates"></a>1. Configurar certificados

Para crear una relación de confianza entre el proveedor de servicios y Azure AD B2C, debe proporcionar los certificados X509 de la aplicación web.

* **Certificados del proveedor de servicios**
  * Certificado con una clave privada almacenada en la aplicación web. El proveedor de servicios usa este certificado para firmar la solicitud de SAML enviada a Azure AD B2C. Azure AD B2C lee la clave pública de los metadatos del proveedor de servicios para validar la firma.
  * (Opcional) Certificado con una clave privada almacenada en la aplicación web. Azure AD B2C lee la clave pública de los metadatos del proveedor de servicios para cifrar la aserción de SAML. A continuación, el proveedor de servicios utiliza la clave privada para descifrar la aserción.
* **Certificados de Azure AD B2C**
  * Certificado con una clave privada en Azure AD B2C. Azure AD B2C usa este certificado para firmar la respuesta de SAML enviada al proveedor de servicios. El proveedor de servicios lee la clave pública de metadatos de Azure AD B2C para validar la firma de la respuesta de SAML.

Puede usar un certificado emitido por una entidad de certificación pública o, para este tutorial, un certificado autofirmado.

### <a name="11-create-a-self-signed-certificate"></a>1.1 Crear un certificado autofirmado

[!INCLUDE [active-directory-b2c-create-self-signed-certificate](../../includes/active-directory-b2c-create-self-signed-certificate.md)]

### <a name="12-upload-the-certificate"></a>1.2 Cargar el certificado

A continuación, cargue el certificado de firma de respuesta y aserción de SAML en Azure AD B2C.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al inquilino de Azure AD B2C.
1. En **Directivas**, seleccione **Identity Experience Framework** y, posteriormente, **Claves de directiva**.
1. Seleccione **Agregar** y, posteriormente, **Opciones** > **Cargar**.
1. Escriba un **Nombre**, por ejemplo, *SamlIdpCert*. El prefijo *B2C_1A_* se agrega automáticamente al nombre de la clave.
1. Use el control de archivos de carga para cargar el certificado.
1. Escriba la contraseña del certificado.
1. Seleccione **Crear**.
1. Compruebe que la clave aparece como se esperaba. Por ejemplo, *B2C_1A_SamlIdpCert*.

## <a name="2-prepare-your-policy"></a>2. Preparar la directiva

### <a name="21-create-the-saml-token-issuer"></a>2.1 Crear el emisor de tokens SAML

Ahora, agregue la funcionalidad para que el inquilino emita tokens de SAML, mediante los perfiles técnicos [emisor de tokens SAML](saml-issuer-technical-profile.md) y [proveedor de sesión de SAML](custom-policy-reference-sso.md#samlssosessionprovider).

Abra `SocialAndLocalAccounts\`**`TrustFrameworkExtensions.xml`** en el paquete de inicio de directivas personalizadas.

Busque la sección `<ClaimsProviders>` y agregue el siguiente fragmento de código XML.

Puede cambiar el valor de los metadatos de `IssuerUri`. Este es el URI del emisor que se devuelve en la respuesta de SAML desde Azure AD B2C. La aplicación de usuario de confianza debe configurarse para aceptar un URI de emisor durante la validación de aserciones de SAML.

```xml
<ClaimsProvider>
  <DisplayName>Token Issuer</DisplayName>
  <TechnicalProfiles>

    <!-- SAML Token Issuer technical profile -->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>Token Issuer</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      <Metadata>
        <!-- The issuer contains the policy name; it should be the same name as configured in the relying party application. B2C_1A_signup_signin_SAML is used below. -->
        <!--<Item Key="IssuerUri">https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/B2C_1A_signup_signin_saml</Item>-->
      </Metadata>
      <CryptographicKeys>
        <Key Id="MetadataSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SamlIdpCert"/>
      </CryptographicKeys>
      <InputClaims/>
      <OutputClaims/>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer"/>
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
    </TechnicalProfile>

  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="3-add-the-saml-relying-party-policy"></a>3. Agregar la directiva de usuario de confianza de SAML

Ahora que el inquilino puede emitir aserciones de SAML, debe crear la directiva de usuario de confianza de SAML y modificar el recorrido del usuario para que emita una aserción de SAML en lugar de un JWT.

### <a name="31-create-sign-up-or-sign-in-policy"></a>3.1. Crear una directiva de registro o de inicio de sesión

1. Cree una copia del archivo *SignUpOrSignin.xml* en el directorio de trabajo del paquete de inicio y guárdelo con un nuevo nombre. Por ejemplo, *SignUpOrSigninSAML.xml*. Este es su archivo de directiva de usuario de confianza.

1. Abra el archivo *SignUpOrSigninSAML.xml* en el editor que prefiera.

1. Cambie los valores de `PolicyId` y `PublicPolicyUri` de la directiva a _B2C_1A_signup_signin_saml_ y `http://tenant-name.onmicrosoft.com/B2C_1A_signup_signin_saml`, como se muestra a continuación.

    ```xml
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="tenant-name.onmicrosoft.com"
    PolicyId="B2C_1A_signup_signin_saml"
    PublicPolicyUri="http://tenant-name.onmicrosoft.com/B2C_1A_signup_signin_saml">
    ```

1. Agregue el siguiente fragmento de código XML justo antes del elemento `<RelyingParty>`. Este XML sobrescribe el paso 7 de la orquestación del recorrido del usuario _SignUpOrSignIn_. Si ha iniciado desde una carpeta diferente en el paquete de inicio o ha personalizado el recorrido del usuario mediante la adición o eliminación de pasos de orquestación, asegúrese de que el número (en el elemento `order`) está alineado con el especificado en el recorrido del usuario para el paso del emisor de tokens (por ejemplo, en las otras carpetas del paquete de inicio corresponde al paso número 4 para `LocalAccounts`, 6 para `SocialAccounts` y 9 para `SocialAndLocalAccountsWithMfa`).

    ```xml
    <UserJourneys>
      <UserJourney Id="SignUpOrSignIn">
        <OrchestrationSteps>
          <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="Saml2AssertionIssuer"/>
        </OrchestrationSteps>
      </UserJourney>
    </UserJourneys>
    ```

1. Reemplace el elemento `<TechnicalProfile>` completo en el elemento `<RelyingParty>` por el siguiente XML de perfil técnico.

    ```xml
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" DefaultValue="" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="objectId"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="objectId" ExcludeAsClaim="true"/>
    </TechnicalProfile>
    ```

1. Actualice `tenant-name` con el nombre del inquilino de Azure AD B2C.

El archivo de directiva de usuario de confianza final debe tener un aspecto similar al código XML siguiente:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="contoso.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin_saml"
  PublicPolicyUri="http://contoso.onmicrosoft.com/B2C_1A_signup_signin_saml">

  <BasePolicy>
    <TenantId>contoso.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>

  <UserJourneys>
    <UserJourney Id="SignUpOrSignIn">
      <OrchestrationSteps>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="Saml2AssertionIssuer"/>
      </OrchestrationSteps>
    </UserJourney>
  </UserJourneys>

  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="SAML2"/>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" DefaultValue="" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="objectId"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="objectId" ExcludeAsClaim="true"/>
    </TechnicalProfile>
  </RelyingParty>
</TrustFrameworkPolicy>
```

> [!NOTE]
> Al implementar otros tipos de flujos de usuario (por ejemplo,inicio de sesión, restablecimiento de contraseña o edición de perfiles), el proceso es esencialmente el mismo que se describe en esta sección. En el paso 4 anterior, cambiará el último paso del recorrido del usuario de `JWTIssuer` a `Saml2AssertionIssuer`. Y en el paso 6 anterior, en la sección de usuario de confianza, cambiará **Protocolo** de `OpenIdConnect` a `SAML2`.

### <a name="32-upload-and-test-your-policy-metadata"></a>3.2 Cargar y probar los metadatos de la directiva

Guarde los cambios y cargue el nuevo archivo de directiva. Una vez que haya cargado ambas directivas (la extensión y los archivos de usuario de confianza), abra un explorador web y vaya a los metadatos de la directiva.

Los metadatos de IDP de directivas de Azure AD B2C son información que se usa en el protocolo SAML para exponer la configuración de un proveedor de entidades SAML. Los metadatos definen la ubicación de los servicios, como el inicio y el cierre de sesión, los certificados, el método de inicio de sesión y mucho más. Los metadatos de la directiva de Azure AD B2C están disponibles en la siguiente dirección URL. Reemplace `tenant-name` por el nombre de su inquilino de Azure AD B2C y `policy-name` por el nombre (ID) de la directiva, como, por ejemplo, .../B2C_1A_signup_signin_saml/samlp/metadata:

`https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/policy-name/Samlp/metadata`

La directiva personalizada y el inquilino de Azure AD B2C ya están listos. A continuación, cree un registro de aplicación en Azure AD B2C.

## <a name="4-setup-application-in-the-azure-ad-b2c-directory"></a>4. Configurar la aplicación en el directorio de Azure AD B2C

### <a name="41-register-your-application-in-azure-ad-b2c"></a>4.1 Registrar la aplicación en Azure AD B2C

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones** y luego **Nuevo registro**.
1. Escriba un **Nombre** para la aplicación. Por ejemplo, *SAMLApp1*.
1. En **Tipos de cuenta admitidos**, seleccione **Solo las cuentas de este directorio organizativo**.
1. En **URI de redirección**, seleccione **Web** y escriba `https://localhost`. Modificará este valor más adelante en el manifiesto del registro de aplicación.
1. Seleccione **Registrar**.

### <a name="42-update-the-app-manifest"></a>4.2 Actualizar el manifiesto de la aplicación

En el caso de las aplicaciones SAML, hay varias propiedades que debe configurar en el manifiesto del registro de aplicación.

1. En [Azure Portal](https://portal.azure.com), vaya al registro de aplicación que creó en la sección anterior.
1. En **Administrar**, seleccione **Manifiesto** para abrir el editor de manifiestos. Modificará varias propiedades en las secciones siguientes.

#### <a name="identifieruris"></a>identifierUris

`identifierUris` es una colección de cadenas que contiene los URI definidos por el usuario que identifican de forma única una aplicación web en su inquilino de Azure AD B2C. El URI debe coincidir con el nombre `Issuer` de la solicitud SAML. El URI definido por el usuario suele ser el mismo valor que el de los metadatos `entityID` del proveedor de servicios.

#### <a name="samlmetadataurl"></a>samlMetadataUrl

Esta propiedad representa la dirección URL de metadatos disponibles públicamente del proveedor de servicios. La dirección URL de metadatos puede apuntar a un archivo de metadatos cargado en cualquier punto de conexión accesible anónimamente, por ejemplo, Blob Storage.

Los metadatos son información que se usa en el protocolo SAML para exponer la configuración de una entidad SAML, como un proveedor de servicios. Los metadatos definen la ubicación de los servicios, como el inicio y el cierre de sesión, los certificados, el método de inicio de sesión y mucho más. Azure AD B2C lee los metadatos del proveedor de servicios y actúa del modo correspondiente. Los metadatos no son necesarios. También puede especificar algunos de los atributos, como el URI de respuesta y el URI de cierre de sesión, directamente en el manifiesto de la aplicación.

Si hay propiedades especificadas *tanto* en la dirección URL de metadatos de SAML como en el manifiesto del registro de la aplicación, se **combinan**. Las propiedades especificadas en la dirección URL de metadatos se procesan primero y tienen prioridad.

Para este tutorial, en el que se usa la aplicación de prueba de SAML, utilice el siguiente valor para `samlMetadataUrl`:

```json
"samlMetadataUrl":"https://samltestapp2.azurewebsites.net/Metadata",
```

#### <a name="replyurlswithtype-optional"></a>replyUrlsWithType (opcional)

Si no proporciona un URI de metadatos, puede especificar explícitamente la dirección URL de respuesta. Esta propiedad opcional representa el valor de `AssertionConsumerServiceUrl` (dirección URL `SingleSignOnService` en los metadatos del proveedor de servicios) y se supone que `BindingType` es `HTTP POST`.

Si elige no configurar la dirección URL de respuesta y la dirección URL de cierre de sesión en el manifiesto de la aplicación sin usar los metadatos del proveedor de servicios, Azure AD B2C no validará la firma de la solicitud de SAML ni cifrará la respuesta de SAML.

En este tutorial, en el que se usa la aplicación de prueba de SAML, establezca la propiedad `url` de `replyUrlsWithType` en el valor que se muestra en el siguiente fragmento de código JSON.

```json
"replyUrlsWithType":[
  {
    "url":"https://samltestapp2.azurewebsites.net/SP/AssertionConsumer",
    "type":"Web"
  }
],
```

#### <a name="logouturl-optional"></a>logoutUrl (opcional)

Esta propiedad opcional representa la dirección URL `Logout` (dirección URL `SingleLogoutService` en los metadatos del usuario de confianza) y se supone que `BindingType` es `Http-Redirect`.

Para este tutorial, en el que se usa la aplicación de prueba de SAML, deje `logoutUrl` establecido en `https://samltestapp2.azurewebsites.net/logout`:

```json
"logoutUrl": "https://samltestapp2.azurewebsites.net/logout",
```

## <a name="5-update-your-application-code"></a>5. Actualizar el código de la aplicación

El último paso es habilitar Azure AD B2C como IdP de SAML en la aplicación de usuario de confianza de SAML. Cada aplicación es diferente y los pasos para hacerlo varían. Consulte la documentación de la aplicación para obtener información detallada.

Los metadatos se pueden configurar en el proveedor de servicio como "Metadatos estáticos" o "Metadatos dinámicos". En el modo estático, se copian todos o parte de los metadatos de la directiva de Azure AD B2C. En el modo dinámico, se establece la dirección URL en los metadatos y se permite que la aplicación los lea de forma dinámica.

Normalmente se requieren algunos o todos los siguientes elementos:

* **Metadatos**: `https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/policy-name/Samlp/metadata`
* **Emisor**:  El valor `issuer` de la solicitud SAML debe coincidir con uno de los URI configurados en el elemento `identifierUris` del manifiesto de registro de la aplicación. Si el nombre `issuer` de la solicitud SAML no existe en el elemento `identifierUris`, [agréguelo al manifiesto de registro de la aplicación](#identifieruris). Por ejemplo, `https://contoso.onmicrosoft.com/app-name`. 
* **Dirección URL de inicio de sesión/Punto de conexión SAML/Dirección URL de SAML**: Compruebe el valor en el archivo de metadatos de la directiva SAML de Azure AD B2C para el elemento XML `<SingleSignOnService>`
* **Certificate**: es *B2C_1A_SamlIdpCert*, pero sin la clave privada. Para obtener la clave pública del certificado:

    1. Vaya a la dirección URL de metadatos especificada anteriormente.
    1. Copie el valor en el elemento `<X509Certificate>`.
    1. Péguelo en un archivo de texto.
    1. Guarde el archivo de texto como archivo *.cert*.

### <a name="51-test-with-the-saml-test-app-optional"></a>5.1 Probar con la aplicación de prueba SAML (opcional)

Para completar este tutorial con la [aplicación de prueba SAML][samltest]:

* Actualizar el nombre del inquilino
* Actualice el nombre de la directiva, por ejemplo *B2C_1A_signup_signin_saml*
* Especifique el URI de este emisor. Use uno de los URI que se encuentran en el elemento `identifierUris` del manifiesto de registro de la aplicación, por ejemplo `https://contoso.onmicrosoft.com/app-name`.

Seleccione **Iniciar sesión**; debe aparecer una pantalla de inicio de sesión de usuario. Tras el inicio de sesión, se vuelve a emitir una aserción de SAML a la aplicación de ejemplo.

## <a name="enable-encrypted-assertions-optional"></a>Habilitación de aserciones cifradas (opcional)

Para cifrar las aserciones de SAML enviadas de vuelta al proveedor de servicios, Azure AD B2C usa el certificado de clave pública de los proveedores de servicios. La clave pública debe existir en los metadatos de SAML explicados en la sección anterior ["samlMetadataUrl"](#samlmetadataurl) como KeyDescriptor con un uso de "Encryption".

El código XML siguiente es un ejemplo de KeyDescriptor en los metadatos de SAML con un uso establecido en Encryption:

```xml
<KeyDescriptor use="encryption">
  <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```

Para habilitar Azure AD B2C para enviar aserciones cifradas, establezca el elemento de los metadatos **WantsEncryptedAssertion** en `true` en el [perfil técnico del usuario de confianza](relyingparty.md#technicalprofile). También puede configurar el algoritmo utilizado para cifrar la aserción de SAML. Para más información, consulte [Metadatos del perfil técnico del usuario de confianza](relyingparty.md#metadata). 

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="WantsEncryptedAssertions">true</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

## <a name="enable-identity-provider-initiated-flow-optional"></a>Habilitación del flujo iniciado por el proveedor de identidades (opcional)

En el flujo iniciado por el proveedor de identidades, el proveedor de identidades (Azure AD B2C) inicia el proceso de inicio de sesión, que envía una respuesta SAML no solicitada al proveedor de servicios (la aplicación de usuario de confianza). Actualmente no se admiten escenarios en los que el proveedor de identidad de inicio sea un proveedor de identidades externo, por ejemplo [AD-FS](identity-provider-adfs.md) o [Salesforce](identity-provider-salesforce-saml.md).

Para habilitar el flujo iniciado por el proveedor de identidades (Azure AD B2C), establezca el elemento de metadatos **IdpInitiatedProfileEnabled** en `true` en el [perfil técnico del usuario de confianza](relyingparty.md#technicalprofile).

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="SAML2"/>
    <Metadata>
      <Item Key="IdpInitiatedProfileEnabled">true</Item>
    </Metadata>
   ..
  </TechnicalProfile>
</RelyingParty>
```

Para iniciar sesión o registrar un usuario mediante el flujo iniciado por el proveedor de identidades, use la siguiente dirección URL:

```
https://tenant-name.b2clogin.com/tenant-name.onmicrosoft.com/policy-name/generic/login?EntityId=app-identifier-uri 
```

Reemplace los siguientes valores:

* **tenant-name** por el nombre de inquilino
* **policy-name** por el nombre de la directiva de usuario de confianza de SAML
* **app-identifier-uri** con `identifierUris` en el archivo de metadatos, por ejemplo `https://contoso.onmicrosoft.com/app-name`
## <a name="sample-policy"></a>Directiva de ejemplo

Se proporciona una directiva de ejemplo completa que se puede usar para realizar pruebas con la aplicación de prueba SAML.

1. Descargue la [directiva de ejemplo de inicio de sesión iniciada por el SP de SAML](https://github.com/azure-ad-b2c/saml-sp/tree/master/policy/SAML-SP-Initiated)
1. Actualice `TenantId` para que coincida con el nombre del inquilino, por ejemplo  *contoso.b2clogin.com*
1. Mantenga el nombre de la directiva *B2C_1A_signup_signin_saml*

## <a name="supported-and-unsupported-saml-modalities"></a>Modalidades de SAML admitidas y no admitidas

Los siguientes escenarios de usuario de confianza (RP) de SAML se admiten a través de su propio punto de conexión de metadatos:

* Varias direcciones URL de cierre de sesión o enlace POST para la dirección URL de cierre de sesión en el objeto de entidad de servicio o aplicación.
* Especificación de la clave de firma para comprobar las solicitudes de RP en el objeto de entidad de servicio o aplicación.
* Especificación de una clave de cifrado de tokens en el objeto de entidad de servicio o aplicación.
* Inicio de sesión iniciado por el proveedor de identidades, donde el proveedor de identidades es Azure AD B2C.

## <a name="saml-token"></a>Token SAML

Un token SAML es un token de seguridad que emite Azure AD B2C después de un inicio de sesión correcto. Contiene información sobre el usuario, el proveedor de servicios para el que está previsto el token, la firma y el tiempo de validez. En la tabla siguiente se enumeran las notificaciones y propiedades que puede esperar de los tokens SAML emitidos por Azure AD B2C.

|Elemento  |Propiedad  |Notas  |
|---------|---------|---------|
|`<Response>`| `ID` | Identificador único de la respuesta generado automáticamente. | 
|`<Response>`| `InResponseTo` | Identificador de la solicitud SAML de la que es respuesta este mensaje. | 
|`<Response>` | `IssueInstant` | Instante de tiempo de emisión de la respuesta. El valor de tiempo está codificado en UTC.    Para cambiar la configuración de las vigencias de los tokens, establezca los [metadatos](saml-issuer-technical-profile.md#metadata) de `TokenNotBeforeSkewInSeconds` del perfil técnico del emisor del token SAML. | 
|`<Response>` | `Destination`| Una referencia de URI que indica la dirección a la que se ha enviado esta respuesta. El valor es idéntico a la solicitud de SAML `AssertionConsumerServiceURL`. | 
|`<Response>` `<Issuer>` | |Identifica al emisor del token. Se trata de un URI arbitrario definido por los [metadatos](saml-issuer-technical-profile.md#metadata)`IssuerUri` del emisor del token SAML.     |
|`<Response>` `<Assertion>` `<Subject>` `<NameID>`     |         |La entidad de seguridad sobre la que el token declara información, por ejemplo, el id. de objeto del usuario. Este valor es inmutable y no se puede reasignar ni volver a usar. Se puede usar para realizar comprobaciones de autorización de forma segura, por ejemplo, cuando el token se usa para acceder a un recurso. De manera predeterminada, la notificación del asunto se rellena con el identificador de objeto del usuario del directorio.|
|`<Response>` `<Assertion>` `<Subject>` `<NameID>`     | `Format` | Referencia de URI que representa la clasificación de la información de identificador basada en cadenas. De manera predeterminada, esta propiedad se omite. Puede establecer el usuario de confianza [SubjectNamingInfo](relyingparty.md#subjectnaminginfo) para especificar el formato `NameID`, como `urn:oasis:names:tc:SAML:2.0:nameid-format:transient`. |
|`<Response>` `<Assertion>` `<Subject>` `<Conditions>` |`NotBefore` |Hora a la que el token es válido. El valor de tiempo está codificado en UTC. La aplicación tiene que usar esta notificación para comprobar la validez de la duración del token. Para cambiar la configuración de las vigencias de los tokens, establezca los [metadatos](saml-issuer-technical-profile.md#metadata) de `TokenNotBeforeSkewInSeconds` del perfil técnico de emisión del token SAML. |
|`<Response>` `<Assertion>` `<Subject>` `<Conditions>` | `NotOnOrAfter` | Hora a la que el token deja de ser válido. La aplicación tiene que usar esta notificación para comprobar la validez de la duración del token. El valor predeterminado es de 5 minutos después de `NotBefore`, y se puede actualizar agregando los `TokenLifeTimeInSeconds` [metadatos](saml-issuer-technical-profile.md#metadata) del perfil técnico de emisión del token SAML.|
|`<Response>` `<Assertion>` `<Conditions>` `<AudienceRestriction>` `<Audience>` | |Referencia de URI que identifica una audiencia prevista. Identifica al destinatario previsto del token. El valor es idéntico a la solicitud de SAML `AssertionConsumerServiceURL`.|
|Colección `<Response>` `<Assertion>` `<AttributeStatement>` de `<Attribute>` | | Colección de aserciones (notificaciones), tal y como están configuradas en las notificaciones de salida del [perfil técnico del usuario de confianza](relyingparty.md#technicalprofile). Para configurar el nombre de la aserción, puede establecer `PartnerClaimType` de la notificaciones de salida. |

## <a name="next-steps"></a>Pasos siguientes

- Puede encontrar más información sobre el [protocolo SAML en el sitio web de OASIS](https://www.oasis-open.org/).
- Obtenga la aplicación web de prueba de SAML en el [repositorio de la comunidad de Azure AD B2C de GitHub](https://github.com/azure-ad-b2c/saml-sp-tester).

<!-- LINKS - External -->
[samltest]: https://aka.ms/samltestapp
