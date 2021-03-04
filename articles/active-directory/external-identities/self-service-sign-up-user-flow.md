---
title: 'Incorporación de un flujo de usuario de registro de autoservicio: Azure AD'
description: Cree flujos de usuario para aplicaciones compiladas por la organización. Después, los usuarios que visitan esa aplicación pueden obtener una cuenta de invitado con las opciones configuradas en el flujo de usuario.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a08cc9f5b6bf7f02666406bcc541edb677196eeb
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101647343"
---
# <a name="add-a-self-service-sign-up-user-flow-to-an-app"></a>Incorporación de un flujo de usuario de registro de autoservicio a una aplicación

Puede crear flujos de usuario para aplicaciones compiladas por la organización. La asociación del flujo de usuario con una aplicación le permite habilitar el registro en esa aplicación. Puede optar por asociar más de una aplicación al flujo de usuario. Una vez asociado el flujo de usuario a una o más aplicaciones, los usuarios que visiten esa aplicación podrán registrarse y obtener una cuenta de invitado mediante las opciones configuradas en el flujo de usuario.

> [!NOTE]
> Puede asociar flujos de usuarios a las aplicaciones compiladas por la organización. Los flujos de usuario no se pueden usar para las aplicaciones de Microsoft, como SharePoint o Teams.

## <a name="before-you-begin"></a>Antes de empezar

### <a name="add-social-identity-providers-optional"></a>Adición de proveedores de identidades sociales (opcional)

Azure AD es el proveedor de identidades predeterminado para el registro de autoservicio. Esto significa que los usuarios pueden registrarse de manera predeterminada con una cuenta de Azure AD. Los proveedores de identidades sociales también pueden incluirse en estos flujos de registro para admitir cuentas de Google y Facebook.

- [Incorporación de Facebook a la lista de proveedores de identidades sociales](facebook-federation.md)
- [Incorporación de Google a la lista de proveedores de identidades sociales](google-federation.md)

### <a name="define-custom-attributes-optional"></a>Definición de atributos personalizados (opcional)

Los atributos de usuario son valores recopilados del usuario durante el registro de autoservicio. Azure AD incluye un conjunto de atributos integrado, pero puede crear atributos personalizados para utilizar en el flujo de usuario. También puede leer y escribir estos atributos mediante Microsoft Graph API. Consulte [Definición de atributos personalizados para flujos de usuario](user-flow-add-custom-attributes.md).

## <a name="enable-self-service-sign-up-for-your-tenant"></a>Habilitación del registro de autoservicio para el inquilino

Para poder agregar un flujo de usuario de registro de autoservicio a las aplicaciones, debe habilitar la característica para el inquilino. Cuando se haya habilitado, los controles estarán disponibles en el flujo de usuario, lo que le permite asociar el flujo de usuario con una aplicación.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de Azure AD.
2. En **Servicios de Azure**, seleccione **Azure Active Directory**.
3. Seleccione **Configuración de usuario** y, a continuación, en **Usuarios externos**, seleccione **Administrar la configuración de colaboración externa**.
4. Establezca el conmutador de alternancia **Enable guest self-service sign up via user flows** (Habilitación del registro de autoservicio de invitados mediante flujos de usuario) en **Yes** (Sí).

   ![Habilitación del registro de invitados de autoservicio](media/self-service-sign-up-user-flow/enable-self-service-sign-up.png)
5. Seleccione **Guardar**.
## <a name="create-the-user-flow-for-self-service-sign-up"></a>Creación del flujo de usuario para el registro de autoservicio

A continuación, debe crear el flujo de usuario para el registro de autoservicio y agregarlo a una aplicación.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de Azure AD.
2. En **Servicios de Azure**, seleccione **Azure Active Directory**.
3. En el menú de la izquierda, seleccione **External Identities**.
4. Seleccione **Flujos de usuario** y, después, **Nuevo flujo de usuario**.

   ![Botón para agregar un nuevo flujo de usuario](media/self-service-sign-up-user-flow/new-user-flow.png)

5. En la página **Crear**, escriba un **Nombre** para el flujo de usuario. Tenga en cuenta que el nombre incluye automáticamente el prefijo **B2X_1_** .
6. En la lista **Proveedores de identidades**, seleccione uno o varios proveedores de identidades que los usuarios externos puedan usar para iniciar sesión en la aplicación. La opción **Registro en Azure Active Directory** se selecciona de manera predeterminada. (Consulte la sección [Antes de empezar](#before-you-begin) que aparece más arriba en este artículo para obtener información sobre cómo agregar proveedores de identidades).
7. En **Atributos de usuario**, elija los atributos que quiere recopilar del usuario. En el caso de los atributos adicionales, seleccione **Mostrar más**. Por ejemplo, seleccione **Mostrar más** y elija los atributos y las notificaciones de **País o región**, **Nombre para mostrar** y **Código postal**. Seleccione **Aceptar**.

   ![Página de creación de un nuevo flujo de usuario](media/self-service-sign-up-user-flow/create-user-flow.png)

> [!NOTE]
> Solo puede recopilar atributos cuando un usuario se registra por primera vez. Una vez que un usuario se registra, ya no se le pedirá que recopile información de atributos, aunque cambie el flujo de usuario.

8. Seleccione **Crear**.
9. El nuevo flujo de usuario aparece en la lista **Flujos de usuario**. Si es necesario, actualice la página.

## <a name="select-the-layout-of-the-attribute-collection-form"></a>Selección del diseño del formulario de colección de atributos

Puede elegir el orden en que se muestran los atributos en la página de registro. 

1. En [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory**.
2. Select **Identidades externas** y, después, **Flujos de usuario**.
3. Seleccione el flujo de usuario de registro de autoservicio de la lista.
4. En **Personalizar**, seleccione **Diseños de página**.
5. Aparecen los atributos que eligió recopilar. Para cambiar el orden de visualización, seleccione un atributo y, a continuación, seleccione **Subir**, **Bajar**, **Mover a la parte superior** o **Mover a la parte inferior**.
6. Seleccione **Guardar**.

## <a name="add-applications-to-the-self-service-sign-up-user-flow"></a>Incorporación de aplicaciones al flujo de usuario de registro de autoservicio

Ya puede asociar aplicaciones al flujo de usuario.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de Azure AD.
2. En **Servicios de Azure**, seleccione **Azure Active Directory**.
3. En el menú de la izquierda, seleccione **External Identities**.
4. En **Self-service sign up** (Registro de autoservicio), seleccione **User flows** (Flujos de usuario).
5. Seleccione el flujo de usuario de registro de autoservicio de la lista.
6. En el menú de la izquierda, en **Usar**, seleccione **Aplicaciones**.
7. Seleccione **Agregar una aplicación**.

   ![Asignación de una aplicación a un flujo de usuario](media/self-service-sign-up-user-flow/assign-app-to-user-flow.png)

8. Seleccione la aplicación de la lista. O bien, use el cuadro de búsqueda para encontrar la aplicación y, a continuación, selecciónela.
9. Haga clic en **Seleccionar**.

## <a name="next-steps"></a>Pasos siguientes

- [Incorporación de Google a la lista de proveedores de identidades sociales](google-federation.md)
- [Incorporación de Facebook a la lista de proveedores de identidades sociales](facebook-federation.md)
- [Uso de conectores de API para personalizar y extender los flujos de usuario a través de las API web](api-connectors-overview.md)
- [Incorporación de un flujo de trabajo de aprobación personalizado al flujo del usuario](self-service-sign-up-add-approvals.md)
