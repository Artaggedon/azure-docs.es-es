---
title: 'Tutorial: Aprovisionamiento de usuarios para ThousandEyes: Azure AD'
description: Aprenda a configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de ThousandEyes.
services: active-directory
author: ArvindHarinder1
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: arvinh
ms.openlocfilehash: fcb53dc22cfb4bf7308b92ee76e5aaaf3bb0dead
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98735119"
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Tutorial: Configuración de ThousandEyes para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es explicar los pasos que hay que realizar en ThousandEyes y Azure AD para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Azure AD en ThousandEyes. 

## <a name="prerequisites"></a>Prerrequisitos

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

* Un inquilino de Azure Active Directory
* Un inquilino de ThousandEyes con el [plan estándar](https://www.thousandeyes.com/pricing) o uno superior habilitado 
* Una cuenta de usuario de ThousandEyes con permisos de administrador 

> [!NOTE]
> Nota: la integración del aprovisionamiento de Azure AD se basa en la [API de ThousandEyes SCIM](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), que está disponible para los equipos de ThousandEyes con el plan Standard o uno superior.

## <a name="assigning-users-to-thousandeyes"></a>Asignación de usuarios a ThousandEyes

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de cuentas de usuario, solo se sincronizarán los usuarios y grupos que se han "asignado" a una aplicación de Azure AD. 

Antes de configurar y habilitar el servicio de aprovisionamiento, debe decidir qué usuarios o grupos de Azure AD representan a los usuarios que necesitan acceso a la aplicación ThousandEyes. Una vez decidido, puede asignar estos usuarios a la aplicación de ThousandEyes siguiendo estas instrucciones:

[Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>Sugerencias importantes para asignar usuarios a ThousandEyes

* Se recomienda asignar un único usuario de Azure AD a ThousandEyes para probar la configuración de aprovisionamiento. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a ThousandEyes, debe seleccionar el rol **Usuario** u otro válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. El rol **Acceso predeterminado** no funciona para realizar el aprovisionamiento y estos usuarios se omiten.

## <a name="configuring-user-provisioning-to-thousandeyes"></a>Configuración del aprovisionamiento de usuarios en ThousandEyes 

Esta sección lo guía a través de los pasos necesarios para conectar la API de aprovisionamiento de cuentas de usuario de ThousandEyes, así como para configurar el servicio de aprovisionamiento con el fin de crear, actualizar y deshabilitar cuentas de usuario asignadas en ThousandEyes en función de la asignación de grupos y usuarios de Azure AD.

> [!TIP]
> También puede habilitar el inicio de sesión único basado en SAML para ThousandEyes siguiendo las instrucciones de [Azure Portal](https://portal.azure.com). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático, aunque estas dos características se complementan entre sí.

### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Configuración del aprovisionamiento de cuentas de usuario automático para ThousandEyes en Azure AD

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. Si ya ha configurado ThousandEyes para el inicio de sesión único, busque la instancia de ThousandEyes mediante el campo de búsqueda. En caso contrario, seleccione **Agregar** y busque **ThousandEyes** en la galería de aplicaciones. Seleccione ThousandEyes en los resultados de búsqueda y agréguelo a la lista de aplicaciones.

    ![Vínculo a ThousandEyes en la lista de aplicaciones](common/all-applications.png)
    
3. Seleccione la instancia de ThousandEyes y luego, la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

![Captura de pantalla que muestra la pestaña Aprovisionamiento de ThousandEyes con la opción Automático seleccionada para el modo de aprovisionamiento.](./media/thousandeyes-provisioning-tutorial/ThousandEyes1.png)
    

5. En la sección **Credenciales de administrador**, escriba el **token de portador de OAuth** generado por la cuenta de ThousandEyes (puede buscar el token o generar uno en la sección **Perfil** de la cuenta de ThousandEyes).

    ![Captura de pantalla que muestra dónde encontrar el vínculo de configuración de la cuenta para el grupo de cuentas actual.](./media/thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. En Azure Portal, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a la aplicación de ThousandEyes. Si la conexión no se establece, asegúrese de que la cuenta de ThousandEyes tiene permisos de administrador de equipo e intente el paso 5 de nuevo.

7. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

8. Haga clic en **Save**(Guardar).

9. En la sección Asignaciones, seleccione **Synchronize Azure Active Directory Users to ThousandEyes** (Sincronizar usuarios de Azure Active Directory con ThousandEyes).

10. En la sección **Asignaciones de atributos**, revise los atributos de usuario que se sincronizan desde Azure AD hacia ThousandEyes. Los atributos seleccionados como propiedades de **Coincidencia** se usan para establecer coincidencias con las cuentas de usuario de Parsable con el objetivo de realizar operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de Parsable admite el filtrado de usuarios basado en ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

     |Atributo|Tipo|Compatible con el filtrado|
     |---|---|---|
     |externalId|String|&check;|
     |userName|String|&check;|
     |active|Boolean|
     |DisplayName|String|
     |emails[type eq "work"].value|String|
     |name.formatted|String|


11. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

12. Para habilitar el servicio de aprovisionamiento de Azure AD para ThousandEyes, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

13. Defina los usuarios o grupos que quiere que se aprovisionen en ThousandEyes; para ello, elija los valores deseados en **Ámbito**, en la sección **Configuración**.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

14. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia el ciclo de sincronización inicial de todos los usuarios y grupos definidos en **Ámbito** en la sección **Configuración**. El ciclo de sincronización inicial tarda más tiempo en realizarse que los ciclos posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. 

## <a name="step-6-monitor-your-deployment"></a>Paso 6. Supervisión de la implementación
Una vez configurado el aprovisionamiento, use los recursos siguientes para supervisar la implementación:

1. Use los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md) para determinar qué usuarios se han aprovisionado correctamente o sin éxito.
2. Consulte la [barra de progreso](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) para ver el estado del ciclo de aprovisionamiento y cuánto falta para que finalice.
3. Si la configuración de aprovisionamiento parece estar en mal estado, la aplicación pasará a estar en cuarentena. Más información sobre los estados de cuarentena [aquí](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)