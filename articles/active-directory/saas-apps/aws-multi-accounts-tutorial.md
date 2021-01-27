---
title: 'Tutorial: Integración de Azure Active Directory con Amazon Web Services (AWS) para conectar varias cuentas | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure AD y Amazon Web Services (AWS) (tutorial heredado).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/24/2020
ms.author: jeedes
ms.openlocfilehash: 440fe52689a345eec36c3ac613d6bc2cc2dccc13
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735484"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws-legacy-tutorial"></a>Tutorial: Integración de Azure Active Directory con Amazon Web Services (AWS) (tutorial heredado)

En este tutorial, aprenderá a integrar Azure Active Directory (Azure AD) con Amazon Web Services (AWS) (tutorial heredado).

La integración de Amazon Web Services (AWS) con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Amazon Web Services (AWS).
- Puede permitir que los usuarios inicien sesión automáticamente en Amazon Web Services (AWS) (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

![Amazon Web Services (AWS) en la lista de resultados](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

> [!NOTE]
> Tenga en cuenta que conectar una aplicación de AWS a todas las cuentas de AWS no es nuestro enfoque recomendado. En su lugar, se recomienda usar [este](./amazon-web-service-tutorial.md) enfoque para configurar varias instancias de la cuenta de AWS en varias instancias de aplicaciones de AWS en Azure AD. Solo debe usar este enfoque si tiene pocas cuentas y roles de AWS, ya que este modelo no es escalable a medida que crecen las cuentas y los roles de AWS dentro de ellas. Este enfoque no usa la funcionalidad de importación de roles de AWS con el Aprovisionamiento de usuarios de Azure AD, por lo que tiene que agregar, actualizar y eliminar los roles manualmente. Para conocer otras limitaciones de este enfoque, consulte los detalles siguientes.

**Tenga en cuenta que no se recomienda usar este enfoque por los siguientes motivos:**

* Tiene que usar el enfoque de Probador de Microsoft Graph para aplicar revisiones a todos los roles de la aplicación. No se recomienda usar el enfoque de archivo de manifiesto.

* Hemos visto clientes que informan de que tras agregar ~1200 roles de aplicación para una única aplicación de AWS, todas las operaciones en la aplicación comenzaban a generar los errores relacionados con el tamaño. Hay un límite de tamaño fijo sobre el objeto de aplicación.

* Tiene que actualizar manualmente el rol a medida que se agregan roles a cualquiera de las cuentas; este es, lamentablemente, un enfoque de sustitución y no de asociación. También, si las cuentas crecen, esta se convierte en una relación de equivalencia con las cuentas y roles.

* Todas las cuentas de AWS van a usar el mismo archivo XML de metadatos de federación y, en el momento de la sustitución del certificado, tiene que controlar este ejercicio masivo para actualizar el certificado en todas las cuentas AWS al mismo tiempo.

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure AD con Amazon Web Services (AWS), necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Suscripción habilitada para el inicio de sesión único en Amazon Web Services (AWS)

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Amazon Web Services (AWS) admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Adición de Amazon Web Services (AWS) desde la galería

Para configurar la integración de Amazon Web Services (AWS) en Azure AD, es preciso agregar Amazon Web Services (AWS) desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Amazon Web Services (AWS)** en el cuadro de búsqueda.
1. Seleccione **Amazon Web Services (AWS)** en el panel de resultados y, luego, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

1. Una vez agregada la aplicación, vaya a la página **Propiedades** y copie el **identificador de objeto**.

    ![Id. de objeto](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-properties.png)

## <a name="configure-and-test-azure-ad-sso"></a>Configuración y prueba del inicio de sesión único de Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Amazon Web Services (AWS) con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Amazon Web Services (AWS) para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Amazon Web Services (AWS).

En Amazon Web Services (AWS), asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario** para establecer la relación de vínculo.

Para configurar y probar el inicio de sesión único de Azure AD con Amazon Web Services (AWS), realice los pasos siguientes:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración del inicio de sesión único en Amazon Web Services (AWS)](#configure-amazon-web-services-aws-sso)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación de Amazon Web Services (AWS).

**Para configurar el inicio de sesión único de Azure AD con Amazon Web Services (AWS), realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de aplicaciones de **Amazon Web Services (AWS)** , seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono del **lapicero** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure, por lo que puede hacer clic en **Guardar**.

5. La aplicación Amazon Web Services (AWS) espera las aserciones de SAML en un formato concreto. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![Captura de pantalla que muestra User Attributes (Atributos de usuario) con el control de edición seleccionado.](common/edit-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | Nombre  | Atributo de origen  | Espacio de nombres |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | `https://aws.amazon.com/SAML/Attributes` |
    | Role | user.assignedroles | `https://aws.amazon.com/SAML/Attributes`|
    | SessionDuration | "proporcione un valor comprendido entre 900 segundos (15 minutos) y 43200 segundos (12 horas)" |  `https://aws.amazon.com/SAML/Attributes` |

    1. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

        ![Captura de pantalla que muestra User claims (Reclamaciones de usuario) con las opciones Add new claim (Agregar nueva reclamación) y Save (Guardar) seleccionadas.](common/new-save-attribute.png)

        ![Captura de pantalla que muestra Manage user claims (Administrar reclamaciones de usuario) para escribir los valores que se describen en este paso.](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En el cuadro de texto **Espacio de nombres**, escriba el valor del espacio de nombres que se muestra para esa fila.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

    >[!NOTE]
    >Para más información sobre los roles de Azure AD, haga clic [aquí](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui--preview).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el archivo **XML de metadatos de federación**  y guárdelo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-amazon-web-services-aws-sso"></a>Configuración del inicio de sesión único en Amazon Web Services (AWS)

1. En otra ventana del explorador, inicie sesión en su sitio de la compañía de Amazon Web Services (AWS) como administrador.

1. Haga clic en **AWS Home** (Página principal de AWS).

    ![Configurar página principal de inicio de sesión único][11]

1. Haga clic en **Administración de identidades y acceso**.

    ![Configurar identidad de inicio de sesión único][12]

1. Haga clic en **Proveedores de identidades** y, después, en **Create Provider** (Crear proveedor).

    ![Configurar proveedor de inicio de sesión único][13]

1. En la página de diálogo **Configure Provider** (Configurar proveedor), realice los pasos siguientes:

    ![Configurar cuadro de diálogo de inicio de sesión único][14]

    a. En **Tipo de proveedor**, seleccione **SAML**.

    b. En el cuadro de texto **Provider Name** (Nombre de proveedor), escriba un nombre de proveedor (por ejemplo, *WAAD*).

    c. Para cargar el **archivo de metadatos** descargado de Azure Portal, haga clic en **Choose file** (Elegir archivo).

    d. Haga clic en **Siguiente paso**.

1. En la página de diálogo **Verify Provider Information** (Comprobar la información del proveedor), haga clic en **Crear**.

    ![Configurar verificación de inicio de sesión único][15]

1. Haga clic en **Roles** y, después, en **Crear rol**.

    ![Configurar roles de inicio de sesión único][16]

    > [!NOTE]
    > La longitud combinada del ARN de rol y el ARN del proveedor de SAML para un rol que se va a importar debe ser de 240 caracteres o menos.

1. En la página **Crear rol**, realice los pasos siguientes:  

    ![Configurar confianza de inicio de sesión único][19]

    a. Seleccione **SAML 2.0 federation** (Federación de SAML 2.0) en **Select type of trusted entity** (Seleccionar tipo de entidad de confianza).

    b. En la sección **Choose a SAML 2.0 Provider** (Elegir un proveedor de SAML 2.0), seleccione **SAML provider** (Proveedor de SAML) que ha creado anteriormente (por ejemplo, *WAAD*)

    c. Seleccione **Allow programmatic and AWS Management Console access** (Permitir acceso mediante programación y a consola de AWS Management Console).

    d. Haga clic en **Siguiente: Permisos**.

1. Busque **Acceso de administrador** en la barra de búsqueda, seleccione la casilla **AdministratorAccess** y, a continuación, haga clic en **Siguiente: Etiquetas**.

    ![Captura de pantalla en la que se muestra AdministratorAccess seleccionado como nombre de directiva.](./media/aws-multi-accounts-tutorial/administrator-access.png)

1. En la sección **Add tags (optional)** (Agregar etiquetas [opcional]), lleve a cabo estos pasos:

    ![Incorporación de etiquetas](./media/aws-multi-accounts-tutorial/config2.png)

    a. En el cuadro de texto **Clave**, escriba el nombre de la clave, por ejemplo, Azureadtest.

    b. En el cuadro de texto **Valor (opcional)** , escriba el valor de la clave con el siguiente formato `accountname-aws-admin`. El nombre de cuenta se debe escribir todo en minúsculas.

    c. Haga clic en **Next: Review** (Siguiente: revisar).

1. En el cuadro de diálogo **Revisar** , realice los pasos siguientes:

    ![Configurar revisión de inicio de sesión único][34]

    a. En el cuadro de texto **Nombre de rol**, escriba el valor con el patrón `accountname-aws-admin`.

    b. En el cuadro de texto **Descripción del rol**, escriba el mismo valor que ha usado para el nombre del rol.

    c. Haga clic en **Crear rol**.

    d. Cree tantos roles como sea necesario y asígnelos al proveedor de identidades.

    > [!NOTE]
    > De forma similar, cree los otros roles restantes, como accountname-finance-admin, accountname-read-only-user, accountname-devops-user y accountname-tpm-user, con las distintas directivas que se deben adjuntar. Más adelante, también se pueden cambiar estas directivas de rol según los requisitos de la cuenta de AWS, pero siempre es mejor mantener las mismas para cada rol en las cuentas de AWS.

1. Tome nota del identificador de la cuenta de AWS en las propiedades de EC2 o en el panel de IAM, tal y como se indica a continuación:

    ![Captura de pantalla en la que se muestra dónde aparece el identificador de la cuenta en la ventana de AWS.](./media/aws-multi-accounts-tutorial/aws-accountid.png)

1. Ahora inicie sesión en Azure Portal y vaya a **Grupos**.

1. Cree nuevos grupos con el mismo nombre que el de los roles de IAM creados anteriormente y anote los **identificadores de objeto** de estos nuevos grupos.

    ![Selección de acceso de administrador 1](./media/aws-multi-accounts-tutorial/copy-objectids.png)

1. Cerrar la sesión de la cuenta de AWS actual e inicie sesión con la cuenta en la que desee configurar el inicio de sesión único con Azure AD.

1. Una vez que se crean todos los roles en las cuentas, se muestran en la lista **Roles** de dichas cuentas.

    ![Captura de pantalla en la que se muestra la lista de roles con el nombre del rol, la descripción y las entidades de confianza.](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-listofroles.png)

1. Es necesario capturar los valores de ARN de rol y Entidades de confianza de los roles, que necesitamos para asignar manualmente con la aplicación de Azure AD.

1. Haga clic en los roles para copiar los valores de **ARN de rol** y **Entidades de confianza**. Estos valores son necesarios para todos los roles que se deben crear en Azure AD.

    ![Configuración de roles 2](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-role-summary.png)

1. Realice el paso anterior en todos los roles de todas las cuentas y almacenar todas ellas en el formato **ARN de rol,Entidades de confianza** en el Bloc de notas.

1. Abra el [Explorador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer) en otra ventana.

    1. Inicie sesión en el sitio del Probador de Microsoft Graph con las credenciales de administrador o coadministrador globales del inquilino.

    1. Debe tener permisos suficientes para crear los roles. Haga clic en **Modificar permisos** para obtener los permisos necesarios.

        ![Cuadro de diálogo del Explorador de Microsoft Graph 1](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    1. Seleccione los siguientes permisos de la lista (si no los tiene ya) y haga clic en "Modificar permisos" 

        ![Cuadro de diálogo del Explorador de Microsoft Graph 2](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    1. En este se le pedirá que vuelva a iniciar la sesión y acepte el consentimiento. Después de aceptar el consentimiento, habrá iniciado sesión en el Probador de Microsoft Graph de nuevo.

    1. En la lista desplegable de la versión, cambie a **beta**. Para capturar todas las entidades de seguridad de su inquilino, utilice la siguiente consulta: `https://graph.microsoft.com/beta/servicePrincipals`. Si usa varios directorios, puede utilizar entonces el siguiente patrón, que contiene el dominio principal: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`.

        ![Cuadro de diálogo del Explorador de Microsoft Graph 3](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    1. En la lista de entidades de servicio que se capturan, seleccione la que debe modificar. También puede usar CTRL + F para buscar la aplicación en todas las entidades de servicio enumeradas. Para usar la siguiente consulta, utilice el valor de **Id. de objeto de entidad de servicio** que ha copiado de la página Propiedades de Azure AD y acceda a la entidad de servicio correspondiente.

        `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

        ![Cuadro de diálogo del Explorador de Microsoft Graph 4](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    1. Extraiga la propiedad appRoles del objeto de entidad de servicio.

        ![Cuadro de diálogo del Explorador de Microsoft Graph 5](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    1. Ahora debe generar roles nuevos para la aplicación. 

    1. En el JSON que aparece a continuación, se muestra un ejemplo del objeto appRoles. Cree un objeto similar para agregar los roles que desee para su aplicación.

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > Solo puede agregar nuevos roles además de **msiam_access** para la operación de revisión. Además, puede agregar tantos roles como desee según las necesidades de su organización. Azure AD enviará el **valor** de estos roles como valor de notificación en la respuesta de SAML.

    1. Vuelva al Probador de Microsoft Graph y cambie el método de **GET** a **PATCH**. Revise el objeto de entidad de servicio para obtener los roles deseados mediante la actualización de la propiedad appRoles de manera similar a la que aparece anteriormente en el ejemplo. Haga clic en **Ejecutar consulta** para ejecutar la operación de revisión. Un mensaje de operación correcta confirma la creación del rol en la aplicación de Amazon Web Services.

        ![Cuadro de diálogo del Explorador de Microsoft Graph 6](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

1. Una vez que haya revisado la entidad de servicio con más roles, podrá asignar usuarios o grupos a los roles correspondientes. Para hacer esto, vaya al portal y navegue hasta la aplicación Amazon Web Services. En la parte superior, haga clic en la pestaña **Usuarios y grupos**.

1. Se recomienda crear grupos nuevos para cada rol de AWS, con el fin de que pueda asignar dicho rol concreto en ese grupo. Tenga en cuenta que se trata de una asignación uno a uno de un grupo a un rol. Luego, puede agregar los miembros que pertenecen al grupo.

1. Una vez que se crean los grupos, seleccione el grupo y asígnelo a la aplicación.

    ![Configuración de la adición del inicio de sesión único 1](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > Al asignar grupos, no se admiten los grupos anidados.

1. Para asignar el rol al grupo, seleccione el rol y haga clic en el botón **Asignar** de la parte inferior de la página.

    ![Configuración de la adición del inicio de sesión único 2](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > Tenga en cuenta que debe actualizar la sesión en Azure Portal para ver los nuevos roles.

### <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante Mis aplicaciones.

Al hacer clic en el icono de Amazon Web Services (AWS) de Aplicaciones, debería acceder a la página de la aplicación Amazon Web Services (AWS) con la opción para seleccionar el rol.

![Prueba de inicio de sesión único 1](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-screen.png)

También puede comprobar la respuesta de SAML para ver los roles que se usan como notificaciones.

![Prueba de inicio de sesión único 2](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-saml.png)

Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Amazon Web Services (AWS), puede aplicar el control de sesión, que protege en tiempo real la filtración y la infiltración de la información confidencial de la organización. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

<!--Image references-->

[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/config3.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials-continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/
