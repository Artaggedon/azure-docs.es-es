---
title: 'Actualización de la información de los grupos en el portal Mis aplicaciones: Azure AD'
description: Aprenda a ver y actualizar la información de sus grupos, por ejemplo, ver los grupos que posee, crear otros, ver los grupos de los que es miembro y unirse a grupos de los que aún no forme parte.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 01/19/2021
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: fc8d211160e757c78a8e2adafad703118628e6dc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "100095051"
---
# <a name="update-your-groups-info-on-the-my-apps-portal"></a>Actualización de la información de los grupos en el portal Mis aplicaciones

Puede usar su cuenta profesional o educativa con el portal **Aplicaciones** basado en web para ver e iniciar muchas de las aplicaciones de su organización basadas en la nube, actualizar información de su perfil y cuenta, ver su información de **Grupos** y realizar **revisiones de acceso** para sus aplicaciones y grupos. Si no tiene acceso al portal **Aplicaciones**, debe ponerse en contacto con su departamento de soporte técnico para obtener permiso.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-my-apps-portal.md)]

>[!Important]
>Este contenido está diseñado para usuarios. Si es administrador, puede encontrar más información acerca de cómo configurar y administrar aplicaciones basadas en la nube en la [Documentación sobre la administración de aplicaciones](../manage-apps/index.yml).
>
> Si ve un error al iniciar sesión con un cuenta Microsoft personal, puede seguir iniciando sesión con el nombre de dominio de su organización (por ejemplo, contoso.com) o el **Id. de inquilino** de su organización desde el administrador en una de las siguientes direcciones URL:
>
>   - https://myapplications.microsoft.com?tenantId=*nombre_de_dominio*
>   - https://myapplications.microsoft.com?tenant=*su_identificador_de_inquilino*

## <a name="view-your-groups-information"></a>Visualización de la información de los grupos

Si el administrador le ha dado permiso para ver el icono **Grupos**, puede:

- **Como miembro de un grupo**, ver los detalles o abandonar cualquier grupo.

- **Como propietario de un grupo**, ver los detalles, crear uno nuevo, agregar o eliminar usuarios, o eliminar el grupo.

### <a name="to-view-your-groups-information"></a>Para ver la información de los grupos

1. Inicie sesión en su cuenta profesional o educativa.

2. Abra el explorador web y vaya a https://myapps.microsoft.com o use el vínculo proporcionado por su organización. Por ejemplo, quizá se le redirija a una página personalizada para la organización, como https://myapps.microsoft.com/contoso.com. Aunque inicie sesión con una cuenta Microsoft personal, puede acudir a su administrador para iniciar sesión con el nombre de dominio (por ejemplo, contoso.com) o el **identificador de inquilino** de la organización en una de las siguientes direcciones URL:

   - https://myapplications.microsoft.com?tenantId=*nombre_de_dominio*
   - https://myapplications.microsoft.com?tenant=*su_identificador_de_inquilino*


    Aparecerá la página **Aplicaciones**, que muestra todas las aplicaciones basadas en la nube disponibles para su uso y propiedad de su organización.

    ![Página de aplicaciones en el portal Mis aplicaciones](media/my-apps-portal-end-user-groups/my-apps-home-large.png)

3. Despliegue el menú **Mis aplicaciones** y seleccione **Mis grupos** para ver la información relacionada con el grupo.

4. Según sus permisos, puede usar la página **Mis grupos** para:

    - **Revisar los grupos que posee.** Puede ver información acerca de los grupos que posee en su organización desde el área **Grupos de mi propiedad**. Seleccionar un nombre de grupo específico proporciona más detalles sobre el grupo, incluido el tipo, el número de miembros, la directiva de unión y la lista de miembros activos.

    - **Crear un grupo.** Puede crear un grupo con usted como propietario en el área **Grupos de mi propiedad**. Para los pasos específicos, consulte la sección [Creación de un grupo nuevo](#create-a-new-group) de este artículo.

    - **Editar un grupo existente.** Puede editar los detalles de cualquiera de los grupos de su propiedad. Para los pasos específicos, consulte la sección [Edición de un grupo existente](#edit-an-existing-group) de este artículo.

    - **Agregar o eliminar miembros.** Puede agregar miembros de los grupos que posea o eliminarlos de estos. Para los pasos específicos, consulte la sección [Incorporación o eliminación de un miembro](#add-or-remove-a-member) de este artículo.

    - **Renovar un grupo de Office 365.** Si la organización lo permite, puede renovar los grupos de Office 365. Para los pasos específicos, consulte la sección [Renovación de un grupo de Office 365](#renew-an-office-365-group) de este artículo. 

    - **Eliminar un grupo.** Puede eliminar cualquier grupo de su propiedad. Para los pasos específicos, consulte la sección [Eliminar un grupo](#delete-a-group) de este artículo.

    - **Revisar los grupos de los que forma parte.** Puede ver los nombres de los grupos de los que es miembro en el área **Grupos en los que estoy**. Seleccionar un nombre de grupo específico proporciona más detalles sobre el grupo, incluido el tipo, el número de miembros, la directiva de unión y la lista de miembros activos.

    - **Unirse a un grupo.** Puede unirse a un grupo existente del que no sea miembro aún desde el área **Grupos en los que estoy**. Para los pasos específicos, consulte [Unión a un grupo existente](#join-an-existing-group).

## <a name="create-a-new-group"></a>Creación de un grupo nuevo

1. En la página **Grupos**, seleccione **Crear un grupo** desde el área **Grupos de mi propiedad**.

    Aparecerá el cuadro **Crear grupo**.

    ![Cuadro Crear grupo](media/my-apps-portal/my-apps-portal-create-group-page.png)

2. Escriba la información necesaria:

    - **Tipo de grupo:**

        - **Seguridad.** Se usa para administrar el acceso de miembros y del equipo a los recursos compartidos de un grupo de usuarios. Por ejemplo, puede crear un grupo de seguridad para una directiva de seguridad específica. De esta forma, puede conceder una serie de permisos a todos los miembros a la vez, en lugar de tener que agregar permisos a cada miembro individualmente.

        - **Office 365.** Ofrece oportunidades de colaboración al conceder acceso a los miembros a un correo compartido, calendarios, archivos, el sitio de SharePoint y mucho más. Esta opción también permite ofrecer a personas de fuera de su organización acceso al grupo.

    - **Nombre del grupo.** Agregue un nombre para el grupo, algo que sea fácil de recordar y que tenga sentido.

    - **Descripción del grupo (opcional).** Agregue una descripción opcional al grupo.

    - **Directiva del grupo.** Elija permitir que cualquiera se pueda unir al grupo o que el propietario agregue miembros.

3. Seleccione **Crear**.

    El nuevo grupo se crea con usted como propietario y aparece en la lista **Grupos de mi propiedad**. Dado que es el propietario, este grupo también aparece en la lista **Grupos en los que estoy**.

## <a name="edit-an-existing-group"></a>Edición de un grupo existente

Después de crear un grupo, puede editar sus detalles o actualizar cualquier dato existente.

1. Seleccione el grupo que desee editar en la página **Grupos** y **Editar detalles** en la página *&lt;nombre_grupo&gt;* .

    El cuadro **Editar detalles** aparecerá para que pueda actualizar la información que agregó al crear el grupo.

2. Realice todos los cambios y seleccione **Guardar**.

## <a name="add-or-remove-a-member"></a>Incorporación o eliminación de un miembro

Puede agregar o quitar miembros de los grupos de su propiedad.

1. Seleccione el grupo al que desea agregar miembros y **+** en la página *&lt;nombre_grupo&gt;* .

    ![Incorporación de un miembro al grupo con el signo + resaltado](media/my-apps-portal/my-apps-portal-add-member-link.png)

2. Busque el miembro que desee agregar en el cuadro **Agregar miembros** y seleccione **Agregar**.

    ![Cuadro Agregar miembros con el nuevo miembro para agregar](media/my-apps-portal/my-apps-portal-add-member-page.png)

    Se envía una invitación al nuevo miembro para que empiece por acceder a las aplicaciones de la organización.

3. Si ha agregado un miembro por error, o si un miembro ha dejado la organización, puede quitar el miembro al seleccionar **Quitar miembro** junto al nombre del miembro en la página *&lt;nombre_grupo&gt;* .

    ![Quitar un miembro, con el vínculo de eliminación resaltado](media/my-apps-portal/my-apps-portal-remove-member-link.png)

## <a name="renew-an-office-365-group"></a>Renovación de un grupo de Office 365

Si la organización lo permite, puede renovar un grupo de Office 365, al ampliar su fecha de expiración.

1. Seleccione el grupo de Office 365 que desee renovar y **Renovar grupo**.

    ![Renovar un grupo de Office 365 ampliando la fecha de expiración](media/my-apps-portal/my-apps-portal-renew-group-link.png)

2. Haga clic en **Aceptar** para cerrar el mensaje de confirmación.

    Después de actualizar la página verá las fechas **Última renovación** y **Expiración de grupo**.

## <a name="delete-a-group"></a>Eliminar un grupo

Puede eliminar cualquiera de los grupos de su propiedad en cualquier momento. Sin embargo, si elimina un grupo por error tendrá que volver a crearlo y a agregar los miembros.

1. Seleccione el grupo que desee eliminar de forma permanente y **Eliminar grupo** en la página *&lt;nombre_grupo&gt;* .

    ![Página <nombre_grupo> con el vínculo Eliminar grupo resaltado](media/my-apps-portal/my-apps-portal-delete-group-link.png)

2. Seleccione **Sí** en el mensaje de confirmación.

    El grupo se eliminará definitivamente.

## <a name="join-an-existing-group"></a>Unión a un grupo existente

Puede unirse a un grupo existente o abandonarlo desde la página **Grupos**.

1. En la página **Grupos**, seleccione **Unirse a un grupo** desde el área **Grupos en los que estoy**.

    La página **Unirse a grupos** aparecerá.

    ![Página Unirse a grupos con el botón Unirse a un grupo resaltado](media/my-apps-portal/my-apps-portal-join-group-link.png)

2. En la página **Unirse a grupos**, seleccione el nombre del grupo al que desee unirse, consulte los detalles del grupo y, si está disponible, seleccione **Unirse al grupo**.

    Si el grupo requiere que el propietario del grupo apruebe la incorporación, se le pedirá que escriba una justificación comercial de por qué necesita para unirse al grupo; a continuación, seleccione **Solicitar**. Si el grupo no requiere aprobación, se le agregará inmediatamente como miembro y el grupo aparecerá en su lista **Grupos en los que estoy**.

3. Si se unió a un grupo por error o si ya no necesita formar parte de él, puede seleccionar el nombre del grupo desde la pagina **Unirse a grupos** y **Abandonar el grupo**.

    ![Página Unirse a grupos con el botón Abandonar el grupo resaltado](media/my-apps-portal/my-apps-portal-leave-group-link.png)

## <a name="next-steps"></a>Pasos siguientes

- [Acceder y usar las aplicaciones del portal Aplicaciones](my-apps-portal-end-user-access.md).

- [Modificación de información de perfil](./my-account-portal-settings.md).

- [Realizar sus propias revisiones de acceso](my-apps-portal-end-user-access-reviews.md).