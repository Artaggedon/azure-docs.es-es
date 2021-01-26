---
title: 'Administración de contraseñas de aplicaciones: Azure Active Directory | Microsoft Docs'
description: Comprenda qué son las contraseñas de aplicaciones y para qué se utilizan con respecto la verificación en dos pasos.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 05/28/2020
ms.author: curtand
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 07303a0b0b3007ade9adb90af7397855a5014cc0
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98179429"
---
# <a name="manage-app-passwords-for-two-step-verification"></a>Administración de las contraseñas de aplicaciones para la verificación en dos pasos

> [!Important]
>Es posible que el administrador no le permita utilizar contraseñas de aplicaciones. Si no ve **Contraseñas de aplicaciones** como una opción, no están disponibles en la organización.

Al utilizar las contraseñas de aplicaciones, es importante recordar:

- Las contraseñas de aplicaciones se generan automáticamente y deben crearse y escribirse una vez por cada aplicación.

- Hay un límite de 40 contraseñas por usuario. Si intenta crear una después de ese límite, se le pedirá que elimine una contraseña existente antes de que se le permita crear una nueva.

    >[!Note]
    >Los clientes de Office 2013 (incluido Outlook) admiten nuevos protocolos de autenticación que se pueden usar con la verificación en dos pasos. Esta compatibilidad significa que después de activar la verificación en dos pasos, ya no necesitará contraseñas de aplicaciones para clientes de Office 2013. Para obtener más información, consulte el artículo [Cómo funciona la autenticación moderna para las aplicaciones de cliente de Office 2013 y Office 2016](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517).

## <a name="create-new-app-passwords"></a>Creación de una nueva contraseña de aplicaciones

Durante el proceso inicial de registro de verificación en dos fases, se le proporcionará una sola contraseña para la aplicación. Si necesita más de una, tendrá que crearla usted mismo. Puede crear contraseñas de aplicación desde varias áreas, en función de cómo se configure la verificación en dos fases en la organización. Para obtener más información sobre el registro para usar la verificación en dos fases con su cuenta profesional o educativa, consulte [Información general de la verificación en dos fases y la cuenta profesional o educativa](multi-factor-authentication-end-user-first-time.md) y sus artículos relacionados.

### <a name="where-to-create-and-delete-your-app-passwords"></a>Dónde crear y eliminar las contraseñas de aplicaciones

Puede crear y eliminar contraseñas de la aplicación, en función de cómo use la verificación en dos fases:

- **Su organización usa la verificación en dos fases y la página de verificación de seguridad adicional.** Si usa una cuenta profesional o educativa (por ejemplo, alain@contoso.com) con la verificación en dos fases en su organización, puede administrar las contraseñas de la aplicación en la [página de comprobación de seguridad adicional](https://account.activedirectory.windowsazure.com/Proofup.aspx). Para obtener instrucciones detalladas, consulte en este artículo la sección sobre [creación y eliminación de contraseñas de aplicaciones mediante la página de comprobación de seguridad adicional](#create-and-delete-app-passwords-from-the-additional-security-verification-page).

- **Su organización usa la verificación en dos fases y el portal de Office 365.** Si usa una cuenta profesional o educativa (por ejemplo, alain@contoso.com), la verificación en dos fases y aplicaciones de Microsoft 365 en su organización, puede administrar las contraseñas de la aplicación en la [página del portal de Office 365](https://www.office.com). Para obtener instrucciones detalladas, consulte en este artículo la sección sobre [creación y eliminación de contraseñas de aplicaciones mediante el portal de Office 365](#create-and-delete-app-passwords-using-the-office-365-portal).

- **Está usando la verificación en dos fases con una cuenta Microsoft personal.** Si usa una cuenta Microsoft personal (como, por ejemplo, alain@outlook.com) con la verificación en dos fases, puede administrar las contraseñas de aplicación en la [página de aspectos básicos de seguridad](https://account.microsoft.com/security/). Para obtener instrucciones detalladas, consulte [Uso de contraseñas de aplicaciones con aplicaciones que no admiten la verificación en dos pasos](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-and-two-step-verification).

## <a name="create-and-delete-app-passwords-from-the-additional-security-verification-page"></a>Creación y eliminación de contraseñas de la aplicación en la página de comprobación de seguridad adicional

Puede crear y eliminar contraseñas de la aplicación en la página de **comprobación de seguridad adicional** de su cuenta profesional o educativa.

1. Inicie sesión en la [página de comprobación de la seguridad adicional](https://account.activedirectory.windowsazure.com/Proofup.aspx) y seleccione **Contraseñas de aplicación**.

    ![Página de contraseñas de aplicación, con la pestaña de contraseñas de aplicación resaltada](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page.png)

2. Seleccione **Crear**, escriba el nombre de la aplicación que requiere la contraseña de la aplicación y, a continuación, seleccione **Siguiente**.

    ![Página de creación de contraseñas de aplicación, con el nombre de la aplicación que necesita contraseña](media/multi-factor-authentication-end-user-app-passwords/mfa-create-app-password-page.png)

3. Copie la contraseña de la página **Su contraseña de aplicación** y, luego, seleccione **Cerrar**.

    ![Página Su contraseña de aplicación con la contraseña de la aplicación especificada](media/multi-factor-authentication-end-user-app-passwords/mfa-your-app-password-page.png)

4. En la página **Contraseñas de aplicación**, asegúrese de que aparece su aplicación.

    ![Página Contraseñas de aplicación, con la nueva aplicación mostrada en la lista](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-with-new-password.png)  

5. Abra la aplicación para la que creó la contraseña de aplicación (por ejemplo, Outlook 2010) y, a continuación, pegue la contraseña de aplicación cuando se le solicite. Solo debe hacer esto una vez por aplicación.

### <a name="to-delete-an-app-password-using-the-app-passwords-page"></a>Para eliminar una contraseña de aplicación mediante la página Contraseñas de aplicación

1. En la página **Contraseñas de aplicación**, seleccione **Eliminar** junto a la contraseña de la aplicación que quiere eliminar.

   ![Captura de pantalla que muestra la eliminación de una contraseña de aplicación en la página Contraseñas de aplicación](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-delete.png)

2. Seleccione **Sí** para confirmar que desea eliminar la contraseña y, a continuación, seleccione **Cerrar**.

    La contraseña de aplicación se eliminó correctamente.

## <a name="create-and-delete-app-passwords-using-the-office-365-portal"></a>Creación y eliminación de contraseñas de aplicaciones en el portal de Office 365

Si utiliza la verificación en dos pasos con la cuenta profesional o educativa y sus aplicaciones de Microsoft 365, puede crear y eliminar las contraseñas de las aplicaciones mediante el portal de Office 365.

### <a name="to-create-app-passwords-using-the-office-365-portal"></a>Para crear contraseñas de aplicaciones en el portal de Office 365

1. Inicie sesión en su cuenta profesional o educativa, vaya a la [página Mi cuenta](https://myaccount.microsoft.com) y seleccione **Información de seguridad**.

    ![Portal de Office que muestra la pestaña Información de seguridad](media/multi-factor-authentication-end-user-app-passwords/mfa-security-info.png)

2. Seleccione **Agregar método**, **Contraseña de aplicación** en la lista desplegable y, a continuación, haga clic en **Agregar**.

    ![Página Información de seguridad con la lista desplegable Agregar método](media/multi-factor-authentication-end-user-app-passwords/mfa-add-method.png)

3. Escriba un nombre para la contraseña de aplicación y seleccione **Siguiente**.

    ![Página de creación de contraseñas de aplicación con el nombre de la contraseña de aplicación](media/multi-factor-authentication-end-user-app-passwords/mfa-enter-app-password-name.png)

4. Copie la contraseña de la página **Contraseña de aplicación** y, luego, seleccione **Listo**.

    ![Página Contraseña de aplicación con la nueva contraseña de aplicación que creó](media/multi-factor-authentication-end-user-app-passwords/mfa-copy-app-password.png)

5. En la página **Información de seguridad**, asegúrese de que aparece su contraseña de aplicación.

    ![Página Información de seguridad con la nueva contraseña de aplicación mostrada en la lista](media/multi-factor-authentication-end-user-app-passwords/mfa-verify-app-password.png)  

6. Abra la aplicación para la que creó la contraseña de aplicación (por ejemplo, Outlook 2016) y, a continuación, pegue la contraseña de aplicación cuando se le solicite. Solo debe hacer esto una vez por aplicación.

### <a name="to-delete-app-passwords-using-the-security-info-page"></a>Eliminación de contraseñas de aplicación mediante la página Información de seguridad

1. En la página **Información de seguridad**, seleccione **Eliminar** junto a la contraseña de aplicación que quiere eliminar.

   ![Captura de pantalla que muestra la eliminación de una contraseña de aplicación en la página Información de seguridad](media/multi-factor-authentication-end-user-app-passwords/mfa-delete-app-password.png)

2. En el cuadro de confirmación, seleccione **Aceptar**.

    La contraseña de aplicación se eliminó correctamente.

## <a name="if-your-app-passwords-arent-working-properly"></a>Si las contraseñas de aplicaciones no funcionan correctamente

Asegúrese de escribir correctamente la contraseña. Si está seguro de que ha introducido su contraseña correctamente, vuelva a iniciar sesión y cree una nueva contraseña de la aplicación. Si ninguna de estas opciones soluciona el problema, póngase en contacto con el departamento de soporte técnico de la empresa para que pueda eliminar las contraseñas de aplicaciones existentes, permitiéndole crear otras nuevas.

## <a name="next-steps"></a>Pasos siguientes

- [Administrar la configuración de la verificación en dos pasos](multi-factor-authentication-end-user-manage-settings.md)

- Pruebe la [aplicación Microsoft Authenticator](user-help-auth-app-download-install.md) para verificar los inicios de sesión con las notificaciones de aplicación, en lugar de recibir llamadas o textos.
