---
title: Registro y examen de un inquilino de Power BI (versión preliminar)
description: Aprenda a usar el portal de Azure Purview para registrar y examinar un inquilino de Power BI.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/19/2020
ms.openlocfilehash: 5f42b5f2b2a7660ee0de975068f64572c470503a
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98874913"
---
# <a name="register-and-scan-a-power-bi-tenant-preview"></a>Registro y examen de un inquilino de Power BI (versión preliminar)

En este artículo se muestra cómo usar el portal de Azure Purview para registrar y examinar un inquilino de Power BI.

> [!Note]
> Si la instancia de Purview y el inquilino de Power BI están en el mismo inquilino de Azure, solo puede usar la autenticación de identidad administrada (MSI) para configurar un examen de un inquilino de Power BI. 

## <a name="create-a-security-group-for-permissions"></a>Creación de un grupo de seguridad de permisos

Para configurar la autenticación, cree un grupo de seguridad y agréguele la identidad administrada del catálogo.

1. En [Azure Portal](https://portal.azure.com), busque **Azure Active Directory**.
1. Cree un grupo de seguridad en Azure Active Directory mediante las indicaciones de [Creación de un grupo básico e incorporación de miembros con Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

    > [!Tip]
    > Puede omitir este paso si ya tiene un grupo de seguridad que quiera usar.

1. Seleccione **Seguridad** como **Tipo de grupo**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/security-group.png" alt-text="Tipo de grupo de seguridad":::

1. Agregue la identidad administrada del catálogo a este grupo de seguridad. Seleccione **Miembros** y, a continuación, seleccione **+ Agregar miembros**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-group-member.png" alt-text="Incorporación de la instancia administrada del catálogo al grupo.":::

1. Busque el catálogo y selecciónelo.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-catalog-to-group-by-search.png" alt-text="Búsqueda del catálogo para incorporarlo":::

    Verá una notificación de operación correcta que le muestra que se ha agregado.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/success-add-catalog-msi.png" alt-text="Incorporación de MSI del catálogo":::

## <a name="associate-the-security-group-with-the-tenant"></a>Asociación del grupo de seguridad con el inquilino

1. Inicie sesión en el [portal de administración de Power BI](https://app.powerbi.com/admin-portal/tenantSettings).
1. Seleccione la página **Configuración de inquilinos**.

    > [!Important]
    > Debe ser administrador de Power BI para ver la página de configuración de inquilinos.

1. Seleccione **Configuración de la API de administración** > **Concesión de permisos a las entidades de servicio para utilizar las API de administración de Power BI de solo lectura (versión preliminar)** .
1. Seleccione **Grupos de seguridad específicos**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/allow-service-principals-power-bi-admin.png" alt-text="Imagen en la que se muestra cómo permitir a las entidades de servicio obtener permisos de API de administración de Power BI de solo lectura":::

    > [!Caution]
    > Cuando permite que el grupo de seguridad que ha creado (que tiene como miembro la identidad administrada del catálogo de datos) use las API de administración de Power BI de solo lectura, también le permite acceder a los metadatos (por ejemplo, los nombres de los paneles y los informes, los propietarios, las descripciones, etc.) de todos los artefactos de Power BI de este inquilino. Una vez que los metadatos se han extraído en Azure Purview, los permisos de Purview, no los permisos de Power BI, determinan quién puede ver esos metadatos.

    > [!Note]
    > Puede quitar el grupo de seguridad de la configuración del desarrollador, pero los metadatos extraídos previamente no se quitarán de la cuenta de Purview. Puede eliminarlo por separado, si quiere.

## <a name="register-your-power-bi-and-set-up-a-scan"></a>Registro de Power BI y configuración de un examen

Ahora que le han concedido los permisos del catálogo para conectarse a la API de administración de su inquilino de Power BI, puede configurar el examen desde el portal del catálogo.

En primer lugar, agregue una marca de característica especial a la dirección URL de Purview. 

1. Seleccione el icono del **Centro de administración**.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/management-center.png" alt-text="Icono del Centro de administración":::.

1. A continuación, seleccione **+ Nuevo** en **Orígenes de datos**.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/data-sources.png" alt-text="Imagen del botón para agregar un nuevo origen de datos":::

    Seleccione **Power BI** como origen de datos.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/select-power-bi-data-source.png" alt-text="Imagen en la que se muestra la lista de orígenes de datos disponibles para elegir":::

1. Asigne un nombre descriptivo a la instancia de Power BI.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/power-bi-friendly-name.png" alt-text="Imagen en la que se muestra el nombre descriptivo del origen de datos de Power BI":::

    El nombre debe tener entre 3 y 63 caracteres, y solo puede contener letras, números, guiones bajos y guiones.  No están permitidos los espacios.

    De manera predeterminada, el sistema buscará el inquilino de Power BI que exista en la misma suscripción de Azure.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/power-bi-datasource-registered.png" alt-text="Origen de datos de Power BI registrado":::

1. Asigne un nombre al examen. Tenga en cuenta que el único método de autenticación admitido es **Identidad administrada**.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/power-bi-scan-setup.png" alt-text="Imagen en la que se muestra la configuración del examen de Power BI":::

    El nombre del examen debe tener entre 3 y 63 caracteres, y solo puede contener letras, números, guiones bajos y guiones.  No están permitidos los espacios.

1. Configure un desencadenador de examen. Las opciones disponibles son: **Una sola vez**, **Cada 7 días** y **Cada 30 días**.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/scan-trigger.png" alt-text="Imagen de desencadenador de examen":::

1. En **Revisar nuevo examen**, seleccione **Guardar y ejecutar** para iniciar el examen.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/save-run-power-bi-scan.png" alt-text="Imagen de la pantalla de Power BI Guardar y ejecutar":::

## <a name="next-steps"></a>Pasos siguientes

- [Examen del catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en el catálogo de datos de Azure Purview](how-to-search-catalog.md)