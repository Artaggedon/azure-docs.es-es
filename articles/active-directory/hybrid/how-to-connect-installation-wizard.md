---
title: Volver a ejecutar el Asistente para instalación de Azure AD Connect | Microsoft Docs
description: Explica cómo funciona el Asistente para la instalación la segunda vez que lo ejecute.
keywords: El Asistente para instalación de Azure AD Connect le permite configurar opciones de mantenimiento la segunda vez que se ejecuta
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/17/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d81836b47acb19f624075480aafef74c9c0934c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "91306130"
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Sincronización de Azure AD Connect: ejecución por segunda vez del Asistente para la instalación
La primera vez que ejecute el Asistente para la instalación de Azure AD Connect, este lo guiará a través de la configuración de la instalación. Si vuelve a ejecutarlo, le ofrecerá opciones para el mantenimiento.

>[!IMPORTANT]
>Tenga en cuenta que no puede ejecutar el Asistente para la instalación mientras haya una sincronización en curso.  Compruebe que no se está ejecutando una sincronización antes de iniciar el asistente.

Puede encontrar el Asistente para la instalación en el menú de inicio **Azure AD Connect**.

![Menú Inicio](./media/how-to-connect-installation-wizard/startmenu.png)

Cuando se inicia el Asistente para la instalación, aparece una página con estas opciones:

![Página con una lista de tareas adicionales](./media/how-to-connect-installation-wizard/additionaltasks.png)

Si ha instalado ADFS con Azure AD Connect podrá ver incluso más opciones. Estas opciones adicionales que tenga por ADFS se documentan en [Administración de AD FS](how-to-connect-fed-management.md#manage-ad-fs).

Seleccione una de las tareas y haga clic en **Siguiente** para continuar.

> [!IMPORTANT]
> Mientras tiene abierto el Asistente para la instalación, todas las operaciones en el motor de sincronización se suspenden. Asegúrese de cerrar el Asistente para instalación tan pronto como haya terminado los cambios de configuración que desea realizar.
>
>

## <a name="view-current-configuration"></a>Visualización de la configuración actual.
Esta opción le proporciona una vista rápida de las opciones configuradas en ese momento.

![Página con una lista de todas las opciones y su estado](./media/how-to-connect-installation-wizard/viewconfig.png)

Haga clic en **Anterior** para volver atrás. Si selecciona **Salir**, se cerrará el Asistente para la instalación.

## <a name="customize-synchronization-options"></a>Personalización de las opciones de sincronización
Esta opción se utiliza para realizar cambios en la configuración de sincronización. Se mostrará un subconjunto de opciones de la ruta de instalación de la configuración personalizada. Incluso si ha utilizado la instalación rápida inicialmente verá estas opciones.

* [Add more directories](how-to-connect-install-custom.md#connect-your-directories)(Agregar más directorios). Para quitar un directorio, consulte la sección sobre cómo [eliminar un conector](how-to-connect-sync-service-manager-ui-connectors.md#delete).
* [Change Domain and OU filtering](how-to-connect-install-custom.md#domain-and-ou-filtering)(Cambiar filtrado por dominio y unidad organizativa).
* Remove Group filtering (Quitar filtrado de grupo).
* [Change optional features](how-to-connect-install-custom.md#optional-features)(Cambiar las características opcionales).

Las demás opciones de la instalación inicial no se pueden cambiar y no están disponibles. Estas opciones son:

* Cambio del atributo que se utiliza para userPrincipalName y sourceAnchor.
* Cambio del método de unión para objetos de un bosque diferente.
* Habilitación del filtrado basado en grupo.

## <a name="refresh-directory-schema"></a>Actualización del esquema de directorio
Esta opción se utiliza si ha cambiado el esquema de una de sus instalaciones locales de bosques de AD DS. Por ejemplo, puede haber instalado Exchange o actualizado a un esquema de Windows Server 2012 con objetos de dispositivo. En este caso, tiene que indicar a Azure AD Connect que vuelva a leer el esquema de AD DS y actualice su memoria caché. Esta acción también vuelve a generar las reglas de sincronización. Si agrega el esquema de Exchange, por ejemplo, las reglas de sincronización para Exchange se agregan a la configuración.

Cuando se selecciona esta opción, se muestran todos los directorios en la configuración. Puede mantener la configuración predeterminada y actualizar todos los bosques o anular la selección de algunos de ellos.

![Página con una lista de todos los directorios en el entorno](./media/how-to-connect-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Configurar modo de almacenamiento provisional
Esta opción le permite habilitar y deshabilitar el modo de almacenamiento provisional en el servidor. Puede encontrar más información acerca del modo de almacenamiento provisional y cómo se utiliza en [Sincronización de Azure AD Connect: tareas y consideraciones operativas](how-to-connect-sync-staging-server.md).

La opción mostrará si el almacenamiento provisional está habilitado o deshabilitado:  
![Captura de pantalla que muestra el modo de almacenamiento provisional deshabilitado.](./media/how-to-connect-installation-wizard/stagingmodecurrentstate.png)

Para cambiar el estado, seleccione esta opción y seleccione o anule la selección de la casilla.  
![Opción que también muestra el estado actual del modo de almacenamiento provisional](./media/how-to-connect-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Cambiar inicio de sesión de usuario
Esta opción permite cambiar el método de inicio de sesión del usuario a y desde la sincronización de hash de contraseña, la autenticación de paso a través o la federación. No se puede cambiar a **No configurar**.

Para más información sobre esta opción, consulte [Opciones para el inicio de sesión de los usuarios en Azure AD Connect](plan-connect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre el modelo de configuración que emplea la sincronización de Azure AD Connect en el artículo de información sobre el [aprovisionamiento declarativo](concept-azure-ad-connect-sync-declarative-provisioning.md).

**Temas de introducción**

* [Sincronización de Azure AD Connect: comprender y personalizar la sincronización](how-to-connect-sync-whatis.md)
* [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md)
