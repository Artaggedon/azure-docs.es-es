---
title: Registro y examen de un servidor de SQL Server local
description: En este tutorial se describe cómo examinar un servidor de SQL Server local mediante un entorno de ejecución de integración autohospedado.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 09/18/2020
ms.openlocfilehash: 0d282ee805ac61ba17ceb3ecc6a3d8179ea7b319
ms.sourcegitcommit: 6628bce68a5a99f451417a115be4b21d49878bb2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2021
ms.locfileid: "98555906"
---
# <a name="register-and-scan-an-on-premises-sql-server"></a>Registro y examen de un servidor de SQL Server local

En este artículo se describe cómo registrar un origen de datos de SQL Server en Purview y cómo configurar un examen en él.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

El origen de datos local de SQL Server admite la funcionalidad siguiente:

- **Exámenes completos e incrementales** para capturar metadatos y su clasificación en una red local o un servidor de SQL Server que esté instalado en una VM de Azure.

- **Linaje** entre los recursos de datos de las actividades de copia y flujo de datos de ADF.

El origen de datos local de SQL Server admite:

- todas las versiones desde SQL Server 2019 hasta SQL Server 2000

- Método de autenticación: Autenticación SQL

### <a name="known-limitations"></a>Restricciones conocidas

Azure Purview no admite el examen de [vistas](/sql/relational-databases/views/views) en SQL Server.

## <a name="prerequisites"></a>Requisitos previos

- Antes de registrar los orígenes de datos, cree una cuenta de Azure Purview. Para más información sobre cómo crear una cuenta de Purview, consulte [Inicio rápido: creación de una cuenta de Azure Purview](create-catalog-portal.md).

- Configure un [entorno de ejecución de integración autohospedado](manage-integration-runtimes.md) para examinar el origen de datos.

## <a name="setting-up-authentication-for-a-scan"></a>Configuración de la autenticación para un examen

Solo hay una manera de configurar la autenticación para la instancia local de SQL Server:

- Autenticación de SQL

### <a name="sql-authentication"></a>Autenticación SQL

La identidad de SQL debe tener acceso a la base de datos principal. Esta ubicación es donde se almacena `sys.databases`. El analizador de Purview necesita enumerar `sys.databases` para buscar todas las instancias de SQL DB en el servidor.

#### <a name="using-an-existing-server-administrator"></a>Uso del administrador del servidor existente

Si tiene previsto utilizar un administrador del servidor (sa) existente para examinar el servidor local de SQL Server, asegúrese de lo siguiente:

1. `sa` no es un tipo de autenticación de Windows.

2. El usuario de nivel de servidor que quiera usar debe tener roles de servidor de tipo público y sysadmin. Para comprobar estos roles, vaya a SQL Server Management Studio (SSMS), conéctese al servidor, vaya a seguridad, seleccione el inicio de sesión que va a usar, haga clic con el botón derecho en **Propiedades** y seleccione **Roles de servidor**.

   :::image type="content" source="media/register-scan-on-premises-sql-server/server-level-login.png" alt-text="Inicio de sesión en el nivel de servidor.":::

3. Las bases de datos se asignan a un usuario que tiene al menos acceso de nivel db_datareader en cada base de datos.

   :::image type="content" source="media/register-scan-on-premises-sql-server/user-mapping-sa.png" alt-text="asignación de usuarios para sa.":::

#### <a name="creating-a-new-login-and-user"></a>Creación de un inicio de sesión y un usuario nuevos

Si quiere crear un nuevo inicio de sesión y un usuario para poder examinar el servidor de SQL Server, siga estos pasos:

1. Vaya a SQL Server Management Studio (SSMS), conéctese al servidor, vaya a seguridad, haga clic con el botón derecho en Inicio de sesión y cree un nuevo inicio de sesión. Asegúrese de seleccionar la autenticación de SQL.

   :::image type="content" source="media/register-scan-on-premises-sql-server/create-new-login-user.png" alt-text="Creación de un nuevo inicio de sesión y un usuario.":::

2. Seleccione los roles de servidor en el panel de navegación izquierdo y seleccione los tipos público y sysadmin.

3. Seleccione la opción de asignación de usuarios en el panel de navegación izquierdo y seleccione todas las bases de datos de al asignación.

   :::image type="content" source="media/register-scan-on-premises-sql-server/user-mapping.png" alt-text="asignación de usuarios.":::

4. Haga clic en Aceptar para guardarlo.

5. Vuelva al usuario que creó; para ello, haga clic con el botón derecho y seleccione **Propiedades**. Escriba una contraseña nueva y confírmela. Seleccione "Especificar la contraseña anterior" y escríbala. **Es necesario cambiar la contraseña en cuanto cree un nuevo inicio de sesión.**

   :::image type="content" source="media/register-scan-on-premises-sql-server/change-password.png" alt-text="cambiar contraseña.":::

#### <a name="storing-your-sql-login-password-in-a-key-vault-and-creating-a-credential-in-purview"></a>Almacenaje de la contraseña de inicio de sesión de SQL en un almacén de claves y creación una credencial en Purview

1. Vaya al almacén de claves en Azure Portal.
1. Seleccione **Configuración > Secretos**.
1. Seleccione **+ Generate/Import** (+ Generar/Importar) y escriba el **nombre** y el **valor** como la *contraseña* del inicio de sesión de SQL Server.
1. Seleccione **Crear** para completar la acción.
1. Si el almacén de claves no está conectado todavía a Purview, necesitará [crear una conexión del almacén de claves](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account).
1. Por último, [cree una nueva credencial](manage-credentials.md#create-a-new-credential) mediante el **nombre de usuario** y la **contraseña** para configurar el examen.

## <a name="register-a-sql-server-data-source"></a>Registro de un origen de datos de SQL Server

1. Vaya a la cuenta de Purview.

1. En el panel de navegación izquierdo de Orígenes y análisis, seleccione **Entornos de ejecución de integración**. Asegúrese de que está configurado un entorno de ejecución de integración autohospedado. Si no está configurado, siga los pasos que se mencionan [aquí](manage-integration-runtimes.md) para crear un entorno de ejecución de integración autohospedado para realizar exámenes en una VM o una instancia local de Azure que tenga acceso a la red local.

1. Seleccione **Sources** (Orígenes) en el panel de navegación izquierdo.

1. Seleccione **Registrar**.

1. Seleccione **SQL Server** y, a continuación, **Continuar**.

   :::image type="content" source="media/register-scan-on-premises-sql-server/set-up-sql-data-source.png" alt-text="Configure el origen de datos de SQL.":::

5. Proporcione un nombre descriptivo y un punto de conexión de servidor y, a continuación, seleccione **Finalizar** para registrar el origen de datos. Si, por ejemplo, el FQDN de SQL Server es **foobar.database.windows.net**, escriba *foobar* como punto de conexión del servidor.

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Examen del catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en el catálogo de datos de Azure Purview](how-to-search-catalog.md)
