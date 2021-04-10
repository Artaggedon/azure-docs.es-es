---
title: Creación y administración de credenciales para exámenes
description: Conozca los pasos necesarios para crear y administrar credenciales en Azure Purview.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 02/11/2021
ms.openlocfilehash: 3802d25ebd8f21ab5b8991a66ceb6650f2f276a9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "103461715"
---
# <a name="credentials-for-source-authentication-in-azure-purview"></a>Credenciales para la autenticación de origen en Azure Purview

En este artículo se describe cómo se pueden crear credenciales en Azure Purview. Estas credenciales guardadas le permiten volver a usar y aplicar rápidamente la información de autenticación guardada a los exámenes del origen de datos.

## <a name="prerequisites"></a>Requisitos previos

- Un almacén de claves de Azure. Para aprender a crear una, consulte el [Inicio rápido: Creación de un almacén de claves mediante Azure Portal](../key-vault/general/quick-create-portal.md).

## <a name="introduction"></a>Introducción

Una credencial es información de autenticación que Azure Purview puede usar para autenticarse en los orígenes de datos registrados. Se puede crear un objeto de credencial para varios tipos de escenarios de autenticación, como la autenticación básica que requiere el nombre de usuario y la contraseña. La credencial captura información específica necesaria para autenticarse, en función del tipo de método de autenticación elegido. Las credenciales usan los secretos existentes de Azure Key Vault para recuperar información confidencial de autenticación durante el proceso de creación de la credencial.

## <a name="use-purview-managed-identity-to-set-up-scans"></a>Uso de una identidad administrada de Purview para configurar exámenes

Si usa la identidad administrada de Purview para configurar exámenes, no tendrá que crear explícitamente una credencial y vincular el almacén de claves a Purview para almacenarlos. Para encontrar instrucciones detalladas sobre cómo agregar la identidad administrada de Purview para acceder a los orígenes de datos y examinarlos, consulte a continuación las secciones de autenticación específicas de los orígenes de datos:

- [Azure Blob Storage](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen2](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL Database](register-scan-azure-sql-database.md)
- [Instancia administrada de Azure SQL Database](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)

## <a name="create-azure-key-vaults-connections-in-your-azure-purview-account"></a>Creación de conexiones de Azure Key Vault en la cuenta de Azure Purview

Para crear una credencial, primero debe asociar una o varias de las instancias de Azure Key Vault existentes a su cuenta de Azure Purview.

1. En [Azure Portal](https://portal.azure.com), seleccione su cuenta de Azure Purview y abra Azure Purview Studio. Vaya al **Management Center** (Centro de administración) de Azure Purview Studio y, luego, a **Credentials** (Credenciales).

2. En la página **Credentials** (Credenciales), seleccione **Manage Key Vault connections** (Administrar conexiones de Key Vault).

   :::image type="content" source="media/manage-credentials/manage-kv-connections.png" alt-text="Administración de conexiones de Azure Key Vault":::

3. Seleccione **+ New** (+ Nuevo) en la página Manage Key Vault connections (Administrar conexiones de Key Vault).

4. Rellene la información necesaria y seleccione **Create** (Crear).

5. Confirme que el almacén de claves se ha asociado correctamente a la cuenta de Azure Purview, como se muestra en este ejemplo:

   :::image type="content" source="media/manage-credentials/view-kv-connections.png" alt-text="Vista de conexiones de Azure Key Vault para confirmar.":::

## <a name="grant-the-purview-managed-identity-access-to-your-azure-key-vault"></a>Concesión de acceso a la identidad administrada de Purview a la instancia de Azure Key Vault

1. Vaya a Azure Key Vault.

2. Seleccione la página **Directivas de acceso**.

3. Seleccione **Agregar directiva de acceso**.

   :::image type="content" source="media/manage-credentials/add-msi-to-akv.png" alt-text="Incorporación de Purview MSI a AKV.":::

4. En el menú desplegable **Secrets permissions** (Permisos de secretos), seleccione los permisos **Obtener** y **Enumerar**.

5. En **Seleccionar la entidad de seguridad**, elija la identidad administrada de Purview. Puede buscar el MSI de Purview con el nombre de la instancia de Purview **o** el identificador de aplicación de la identidad administrada. Actualmente no se admiten identidades compuestas (nombre de identidad administrada + identificador de la aplicación).

   :::image type="content" source="media/manage-credentials/add-access-policy.png" alt-text="Agregar directiva de acceso":::

6. Seleccione **Agregar**.

7. Seleccione **Guardar** para guardar la directiva de acceso.

   :::image type="content" source="media/manage-credentials/save-access-policy.png" alt-text="Guardado de la directiva de acceso.":::

## <a name="create-a-new-credential"></a>Creación de una credencial

Estos tipos de credenciales se admiten en Purview:

- Autenticación básica: se agrega la **contraseña** como un secreto en el almacén de claves.
- Entidad de servicio: se agrega la **clave de entidad de servicio** como un secreto en el almacén de claves.
- Autenticación de SQL: se agrega la **contraseña** como un secreto en el almacén de claves.
- Clave de cuenta: se agrega la **clave de cuenta** como un secreto en el almacén de claves.
- ARN de rol: en los orígenes de datos de Amazon S3, agregue su **ARN de rol** en AWS. 

Para más información, consulte [Incorporación de un secreto a Key Vault](../key-vault/secrets/quick-create-portal.md#add-a-secret-to-key-vault) y [Creación de un rol de AWS para Purview](register-scan-amazon-s3.md#create-a-new-aws-role-for-purview).

Después de almacenar los secretos en el almacén de claves:

1. En Azure Purview, vaya a la página Credentials (Credenciales).

2. Cree la credencial seleccionando **+ New** (+ Nuevo).

3. Proporcione la información necesaria. Seleccione el **método de autenticación** y una **conexión a Key Vault** desde la que seleccionar un secreto.

4. Una vez rellenados todos los detalles, seleccione **Create** (Crear).

   :::image type="content" source="media/manage-credentials/new-credential.png" alt-text="Nueva credencial.":::

5. Compruebe que la nueva credencial aparece en la vista de lista y que está preparada para usarse.

   :::image type="content" source="media/manage-credentials/view-credentials.png" alt-text="Visualización de la credencial.":::

## <a name="manage-your-key-vault-connections"></a>Administración de las conexiones del almacén de claves

1. Busque las conexiones del almacén de claves por el nombre.

   :::image type="content" source="media/manage-credentials/search-kv.png" alt-text="Búsqueda del almacén de claves":::

2. Elimine una o más conexiones de almacén de claves.

   :::image type="content" source="media/manage-credentials/delete-kv.png" alt-text="Eliminar el almacén de claves":::

## <a name="manage-your-credentials"></a>Administración de las credenciales

1. Busque las credenciales por el nombre.
  
2. Seleccione y actualice una credencial existente.

3. Elimine una o varias credenciales.

## <a name="next-steps"></a>Pasos siguientes

[Creación de un conjunto de reglas de examen](create-a-scan-rule-set.md)
