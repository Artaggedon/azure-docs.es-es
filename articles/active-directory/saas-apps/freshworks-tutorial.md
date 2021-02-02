---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Freshworks | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Freshworks.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/20/2021
ms.author: jeedes
ms.openlocfilehash: 0070a91706fc7efe81a7679801e8c10ea9a05242
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98624748"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-freshworks"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Freshworks

En este tutorial, aprenderá a integrar Freshworks con Azure Active Directory (Azure AD). Cuando integre Freshworks con Azure AD, podrá hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Freshworks.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Freshworks con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Freshworks.

> [!NOTE]
> Esta integración también está disponible para usarse desde el entorno de la nube del gobierno de EE. UU de Azure AD. Puede encontrar esta aplicación en la galería de aplicaciones de la nube del gobierno de EE. UU. de Azure AD y configurarla de la misma manera que en la nube pública.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Freshworks admite inicio de sesión único iniciado por **SP**.

## <a name="add-freshworks-from-the-gallery"></a>Adición de Freshworks desde la galería

Para configurar la integración de Freshworks en Azure AD, será preciso que agregue Freshworks desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Freshworks** en el cuadro de búsqueda.
1. Seleccione **Freshworks** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-freshworks"></a>Configuración y prueba del inicio de sesión único de Azure AD para Freshworks

Configure y pruebe el inicio de sesión único de Azure AD con Freshworks mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Freshworks.

Para configurar y probar el inicio de sesión único de Azure AD con Freshworks, lleve a cabo los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Freshworks](#configure-freshworks-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Freshworks](#create-freshworks-test-user)** , para tener un homólogo de B.Simon en Freshworks que esté vinculado a su representación en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Freshworks**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.freshworks.com/login`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.freshworks.com/sp/SAML/<MODULE_ID>/metadata`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico al cliente de Freshworks](mailto:support@freshworks.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. Para modificar las opciones de **Firma** según sus propios requisitos, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Certificado de firma de SAML**.

     ![imagen](common/edit-certificate.png)

     ![Captura de pantalla que muestra el cuadro de diálogo "Certificado de firma de S A M L" con el botón "Editar" seleccionado.](./media/freshworks-tutorial/response.png)

    a. Seleccione **Firmar respuesta SAML** como **Opción de firma**.

    b. Haga clic en **Save**(Guardar).

1. En la sección **Set up Freshworks** (Configurar Freshworks), copie las direcciones URL adecuadas.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a Freshworks mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Freshworks**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-freshworks-sso"></a>Configuración del inicio de sesión único de Freshworks

1. Abra una nueva ventana del explorador web e inicie sesión en el sitio de empresa de Freshworks como administrador y haga lo siguiente:

2. En el lado izquierdo del menú, haga clic en el icono **Seguridad**, active la opción **Inicio de sesión único** y seleccione **SSO de SAML** en **Métodos de autenticación**.

    ![Captura de pantalla que muestra la sección "Métodos de autenticación de seguridad" con la opción "Inicio de sesión único" activada y la opción "S A M L S S O" (S S O d e S A M L) seleccionada.](./media/freshworks-tutorial/configure01.png)

3. En la sección **Inicio de sesión único**, siga estos pasos:

    ![Configuración de Freshworks](./media/freshworks-tutorial/configure02.png)

    a. Haga clic en **Copiar** para copiar el **identificador de entidad del proveedor de servicio (SP)** para su instancia y péguelo en el cuadro de texto **Identificador (Id. de entidad)** en **Configuración básica de SAML** de Azure Portal.

    b. En el cuadro de texto **Entity ID provided by the IdP** (Id. de entidad proporcionado por el IdP), pegue el valor de **Identificador de Azure AD** que copió en Azure Portal.

    c. En el cuadro de texto **Dirección URL de inicio de sesión único de SAML**, pegue el valor de **Dirección URL de inicio de sesión** que copió en Azure Portal.

    d. Abra el certificado codificado en Base64 con el Bloc de notas, copie su contenido y péguelo en el cuadro de texto **Certificado de seguridad**.

    e. Haga clic en **Save**(Guardar).

### <a name="create-freshworks-test-user"></a>Creación de un usuario de prueba de Freshworks

En esta sección, creará un usuario llamado B.Simon en Freshworks. Trabaje con el [equipo de soporte técnico al cliente de Freshworks](mailto:support@freshworks.com) para agregar los usuarios a la plataforma de Freshworks. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único. 

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Freshworks, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de Freshworks e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Freshworks en Aplicaciones, debería iniciar sesión automáticamente en la instancia de Freshworks para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Freshworks, puede aplicar el control de sesión, que protege contra la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).