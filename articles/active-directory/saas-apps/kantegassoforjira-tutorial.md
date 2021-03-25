---
title: 'Tutorial: Integración de Azure Active Directory con Kantega SSO for JIRA | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Kantega SSO for JIRA.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: 9643d0e63e85a9b500021a415e3cdaf3edc756c5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97608738"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a>Tutorial: Integración de Azure Active Directory con Kantega SSO for JIRA

En este tutorial obtendrá información sobre cómo integrar Kantega SSO for JIRA con Azure Active Directory (Azure AD).
La integración de Kantega SSO for JIRA con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Kantega SSO for JIRA.
* Puede permitir que los usuarios inicien sesión automáticamente en Kantega SSO for JIRA (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con Kantega SSO for JIRA, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único de Kantega SSO for JIRA

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Kantega SSO for JIRA admite SSO iniciado por **SP e IDP**

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a>Agregar Kantega SSO for JIRA desde la galería

Para configurar la integración de Kantega SSO for JIRA en Azure AD, tiene que agregar Kantega SSO for JIRA desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Kantega SSO for JIRA desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Kantega SSO for JIRA**, seleccione **Kantega SSO for JIRA** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Kantega SSO for JIRA en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Kantega SSO for JIRA con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Kantega SSO for JIRA.

Para configurar y probar el inicio de sesión único de Azure AD con Kantega SSO for JIRA, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Kantega SSO for JIRA](#configure-kantega-sso-for-jira-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Kantega SSO for JIRA](#create-kantega-sso-for-jira-test-user)** : para tener un homólogo de Britta Simon en Kantega SSO for JIRA que esté vinculado a la representación de Azure AD del usuario.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único en Azure AD con Kantega SSO for JIRA, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Kantega SSO for JIRA**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Captura de pantalla que muestra la sección "Configuración básica de S A M L" con el cuadro de texto "Identificador" y "U R L de respuesta" resaltados y el botón "Guardar" seleccionado.](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Kantega SSO for JIRA](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Estos valores se reciben durante la configuración del complemento de Jira, que se explica más adelante en el tutorial.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

7. En la sección **Set up Kantega SSO for JIRA** (Configurar Kantega SSO for JIRA), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-kantega-sso-for-jira-single-sign-on"></a>Configuración del inicio de sesión único en Kantega SSO for JIRA

1. En otra ventana del explorador web, inicie sesión en el servidor local de JIRA como administrador.

1. Mantenga el mouse encima del icono de engranaje y haga clic en **Complementos**.

    ![Captura de pantalla que muestra el icono de engranaje seleccionado y la opción "Complementos" seleccionada en el menú desplegable.](./media/kantegassoforjira-tutorial/addon1.png)

1. En la sección de la pestaña Complementos, haga clic en **Find new add-ons** (Buscar nuevos complementos). Busque **Kantega SSO for JIRA (SAML & Kerberos)** y haga clic en el botón **Instalar** para instalar el nuevo complemento SAML.

    ![Captura de pantalla que muestra la sección "Find new Add-ons" (Buscar nuevos complementos) con "Kantego S S O for JIRA (S A M L & Kerberos)" en el cuadro de búsqueda y el botón "Instalar" seleccionado.](./media/kantegassoforjira-tutorial/addon2.png)

1. Se inicia la instalación del complemento.

    ![Captura de pantalla que muestra el cuadro de diálogo "Instalando" del complemento.](./media/kantegassoforjira-tutorial/addon3.png)

1. Una vez completada la instalación. Haga clic en **Cerrar**.

    ![Captura de pantalla que muestra el cuadro de diálogo "Installed and ready to go!" (Instalado y listo para usarse) con la acción "Cerrar" seleccionada.](./media/kantegassoforjira-tutorial/addon33.png)

1.  Haga clic en **Administrar**.

    ![Captura de pantalla que muestra la página de aplicación "Kantega S S O" con el botón "Administrar" seleccionado.](./media/kantegassoforjira-tutorial/addon34.png)
    
1. El nuevo complemento aparece en **INTEGRACIONES**. Haga clic en **Configurar** para configurar el nuevo complemento.

    ![Captura de pantalla que muestra "INTEGRACIONES" en el menú de navegación de la izquierda resaltado y el botón "Configurar" seleccionado en la sección "Administrar complementos".](./media/kantegassoforjira-tutorial/addon35.png)

1. En la sección **SAML**. Seleccione **Azure Active Directory (Azure AD)** en la lista desplegable **Agregar proveedor de identidades**.

    ![Captura de pantalla que muestra el menú desplegable "Agregar proveedor de identidades" con la opción "Azure Active Directory (Azure A D)" seleccionada.](./media/kantegassoforjira-tutorial/addon4.png)

1. Seleccione el nivel de suscripción **Básica**.

    ![Captura de pantalla que muestra la sección "Preparación de Azure A D" con la opción "Básica" seleccionada.](./media/kantegassoforjira-tutorial/addon5.png)

1. En la sección **Agregar propiedades**, siga estos pasos: 

    ![Captura de pantalla que muestra la sección "Propiedades de la aplicación" con el cuadro de texto "U R L de I D. de aplicación" y el botón Copiar resaltados, y el botón "Siguiente" seleccionado.](./media/kantegassoforjira-tutorial/addon6.png)

    1. Copie el valor de **URI de id. de aplicación** y úselo en los campos **Identificador, URL de respuesta y URL de inicio de sesión** de la sección **Configuración básica de SAML**  de Azure Portal.

    1. Haga clic en **Next**.

1. En la sección **Importar metadatos**, siga estos pasos: 

    ![Captura de pantalla que muestra la sección "Importación de metadatos" con la opción "Archivo de metadatos en mi equipo" seleccionada.](./media/kantegassoforjira-tutorial/addon7.png)

    1. Seleccione **Archivo de metadatos en el equipo** y cargue el archivo de metadatos que descargó desde Azure Portal.

    1. Haga clic en **Next**.

1. En la sección **Name and SSO location** (Nombre y ubicación de SSO), siga estos pasos:

    ![Captura de pantalla que muestra la opción "Name and S S O location" (Nombre y ubicación de S S O) y el botón "Siguiente" seleccionado.](./media/kantegassoforjira-tutorial/addon8.png)

    1. Agregue el nombre del proveedor de identidades en el cuadro de texto **Nombre del proveedor de identidades** (por ejemplo, Azure AD).

    1. Haga clic en **Next**.

1. Compruebe el certificado de firma y haga clic en **Siguiente**.

    ![Captura de pantalla que muestra la sección "Comprobación de firmas" con el botón "Siguiente" seleccionado.](./media/kantegassoforjira-tutorial/addon9.png)

1. En la sección **Cuentas de usuario de JIRA**, siga estos pasos:

    ![Captura de pantalla que muestra la sección "Cuentas de usuario de JIRA" con la opción "Create users in JIRA's Internal Directory if needed" (Crear usuarios en el directorio interno de JIRA si es necesario) resaltada y el botón "Siguiente" seleccionado.](./media/kantegassoforjira-tutorial/addon10.png)

    1. Seleccione **Create users in JIRA's internal Directory if needed** (Crear usuarios en el directorio interno de JIRA si es necesario) y escriba el nombre adecuado del grupo de usuarios (puede ser un número múltiple de grupos separados por coma).

    1. Haga clic en **Next**.

1. Haga clic en **Finalizar**

    ![Captura de pantalla que muestra la sección "Resumen" con el botón "Finalizar" seleccionado.](./media/kantegassoforjira-tutorial/addon11.png)

1. En la sección **Known domains for Azure AD** (Dominios conocidos para Azure AD), siga estos pasos:

    ![Configurar inicio de sesión único](./media/kantegassoforjira-tutorial/addon12.png)

    1. Seleccione **Known domains** (Dominios conocidos) en el panel izquierdo de la página.

    2. Escriba el nombre de dominio en el cuadro de texto **Known domains** (Dominios conocidos).

    3. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    1. En el campo **Nombre**, escriba **BrittaSimon**.

    1. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`. Por ejemplo: BrittaSimon@contoso.com

    1. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Kantega SSO for JIRA.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Kantega SSO for JIRA**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Kantega SSO for JIRA**.

    ![Vínculo a Kantega SSO for JIRA en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-kantega-sso-for-jira-test-user"></a>Creación de un usuario de prueba en Kantega SSO for JIRA

Para que los usuarios de Azure AD puedan iniciar sesión en JIRA, es preciso aprovisionarlos en JIRA. En Kantega SSO for JIRA, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión como administrador en el servidor local de JIRA.

1. Mantenga el mouse encima del icono de engranaje y haga clic en **Administración de usuarios**.

    ![Captura de pantalla que muestra el icono de engranaje seleccionado y la opción "Administración de usuarios" seleccionada en el menú desplegable.](./media/kantegassoforjira-tutorial/user1.png) 

1. En la sección de la pestaña **Administración de usuarios**, haga clic en **Crear usuario**.

    ![Captura de pantalla que muestra la sección "Administración de usuarios" con el botón "Crear usuario" seleccionado.](./media/kantegassoforjira-tutorial/user2.png) 

1. En la página del cuadro de diálogo **"Create New User"** (Crear nuevo usuario), realice los pasos siguientes:

    ![Agregar empleado](./media/kantegassoforjira-tutorial/user3.png) 

    1. En el cuadro de texto **Dirección de correo electrónico**, escriba la dirección de correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.

    2. En el cuadro de texto **Nombre completo**, escriba el nombre completo de un usuario, por ejemplo, Britta Simon.

    3. En el cuadro de texto **Nombre de usuario**, escriba el correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.

    4. En el cuadro de texto **Contraseña**, escriba la contraseña del usuario.

    5. Haga clic en **Crear usuario**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Kantega SSO for JIRA en el panel de acceso, debería iniciar sesión automáticamente en la versión de Kantega SSO for JIRA para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)