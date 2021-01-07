---
title: Control del acceso a la cuenta de almacenamiento para el grupo de SQL sin servidor
description: Se describe el modo en que el grupo de SQL sin servidor accede a Azure Storage y cómo puede controlar su acceso al almacenamiento en Azure Synapse Analytics.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 06/11/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: edb1d419900147b586ba1ff257d4307b237be537
ms.sourcegitcommit: 6e2d37afd50ec5ee148f98f2325943bafb2f4993
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97746735"
---
# <a name="control-storage-account-access-for-serverless-sql-pool-in-azure-synapse-analytics"></a>Control del acceso a la cuenta de almacenamiento del grupo de SQL sin servidor en Azure Synapse Analytics

Una consulta del grupo de SQL sin servidor lee los archivos directamente desde Azure Storage. Los permisos para tener acceso a los archivos en Azure Storage se controlan en dos niveles:
- **Nivel de almacenamiento**: el usuario debe tener permiso de acceso a los archivos de almacenamiento subyacentes. El administrador de almacenamiento debe permitir que la entidad de seguridad de Azure AD lea y escriba archivos o que genere una clave SAS que se usará para acceder al almacenamiento.
- **Nivel de servicio de SQL**: el usuario debe tener permiso para leer datos mediante una [tabla externa](develop-tables-external-tables.md) o para ejecutar la función `OPENROWSET`. Conozca más información sobre [los permisos necesarios en esta sección](develop-storage-files-overview.md#permissions).

En este artículo se describen los tipos de credenciales que puede usar y cómo se realiza la búsqueda de credenciales para los usuarios de SQL y Azure AD.

## <a name="supported-storage-authorization-types"></a>Tipos de autorización de almacenamiento admitidos

Un usuario que haya iniciado sesión en un grupo de SQL sin servidor debe estar autorizado para acceder y consultar los archivos de Azure Storage si los archivos no están disponibles públicamente. Para acceder al almacenamiento no público puede usar tres tipos de autorización: [Identidad de usuario](?tabs=user-identity), [Firma de acceso compartido](?tabs=shared-access-signature) e [Identidad administrada](?tabs=managed-identity).

> [!NOTE]
> **Paso a través de Azure AD** es el comportamiento predeterminado cuando se crea un área de trabajo.

### <a name="user-identity"></a>[Identidad de usuario](#tab/user-identity)

La **identidad de usuario**, conocida también como "paso a través de Azure AD", es un tipo de autorización en el que se usa la identidad del usuario de Azure AD que inició sesión en el grupo de SQL sin servidor para autorizar el acceso a los datos. Antes de acceder a los datos, el administrador de Azure Storage debe conceder permisos al usuario de Azure AD. Como se indica en la tabla siguiente, no se admite para el tipo de usuario de SQL.

> [!IMPORTANT]
> Debe tener un rol de Propietario, Colaborador o Lector de datos de un blob de almacenamiento para usar su identidad para acceder a los datos.
> Incluso si es propietario de una cuenta de almacenamiento, deberá agregarse a uno de los roles de datos del blob de almacenamiento.
>
> Para más información sobre el control de acceso en Azure Data Lake Store Gen2, revise el artículo [Control de acceso en Azure Data Lake Storage Gen2](../../storage/blobs/data-lake-storage-access-control.md).
>

### <a name="shared-access-signature"></a>[Firma de acceso compartido](#tab/shared-access-signature)

La **Firma de acceso compartido (SAS)** ofrece acceso delegado a los recursos de una cuenta de almacenamiento. Con SAS, un cliente puede conceder a los clientes acceso a los recursos de una cuenta de almacenamiento sin compartir las claves de la cuenta. SAS le ofrece un control detallado sobre el tipo de acceso que concede a los clientes que tienen una SAS, incluido el intervalo de validez, los permisos concedidos, el intervalo de direcciones IP aceptable y el protocolo aceptable (https/http).

Para obtener un token de SAS, vaya a **Azure portal -> Cuenta de Storage -> Firma de acceso compartido -> Configurar permisos -> Generar la cadena de conexión y SAS.**

> [!IMPORTANT]
> Cuando se genera un token de SAS, este incluye un signo de interrogación ("?") al principio del token. Para usar el token en el grupo de SQL sin servidor, debe quitar el signo de interrogación ("?") al crear una credencial. Por ejemplo:
>
> SAS token: ?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D

Para habilitar el acceso mediante un token de SAS, debe crear una credencial con ámbito en la base de datos o con ámbito en el servidor. 

### <a name="managed-identity"></a>[Identidad administrada](#tab/managed-identity)

La **identidad administrada** también se conoce como MSI. Es una característica de Azure Active Directory (Azure AD) que proporciona servicios de Azure para el grupo de SQL sin servidor. Además, implementa una identidad administrada automáticamente en Azure AD. Esta identidad se puede usar para autorizar la solicitud de acceso a los datos de Azure Storage.

Antes de acceder a los datos, el administrador de Azure Storage debe conceder permisos a la identidad administrada para acceder a los datos. La concesión de permisos a la identidad administrada se realiza de la misma forma que la concesión de permisos a cualquier otro usuario de Azure AD.

### <a name="anonymous-access"></a>[Acceso anónimo](#tab/public-access)

Puede tener acceso a los archivos disponibles públicamente ubicados en cuentas de almacenamiento de Azure que [permitan el acceso anónimo](/azure/storage/blobs/storage-manage-access-to-resources).

---

### <a name="supported-authorization-types-for-databases-users"></a>Tipos de autorización admitidos para usuarios de bases de datos

En la tabla siguiente puede encontrar los tipos de autorización disponibles:

| Tipo de autorización                    | *Usuario de SQL*    | *Usuario de Azure AD*     |
| ------------------------------------- | ------------- | -----------    |
| [Identidad de usuario](?tabs=user-identity#supported-storage-authorization-types)       | No compatible | Compatible      |
| [SAS](?tabs=shared-access-signature#supported-storage-authorization-types)       | Compatible     | Compatible      |
| [Identidad administrada](?tabs=managed-identity#supported-storage-authorization-types) | No compatible | Compatible      |

### <a name="supported-storages-and-authorization-types"></a>Tipos de almacenamiento y autorización admitidos

Puede usar las siguientes combinaciones de tipos de autorización y almacenamiento de Azure:

| Tipo de autorización  | Blob Storage   | ADLS Gen1        | ADLS Gen2     |
| ------------------- | ------------   | --------------   | -----------   |
| [SAS](?tabs=shared-access-signature#supported-storage-authorization-types)    | Compatible\*      | No compatible   | Compatible\*     |
| [Identidad administrada](?tabs=managed-identity#supported-storage-authorization-types) | Compatible      | Compatible        | Compatible     |
| [Identidad de usuario](?tabs=user-identity#supported-storage-authorization-types)    | Compatible\*      | Compatible\*        | Compatible\*     |

\* El token de SAS y la identidad de Azure AD se pueden usar para tener acceso a un almacenamiento que no esté protegido con el firewall.


### <a name="querying-firewall-protected-storage"></a>Consulta del almacenamiento protegido por firewall

Al acceder al almacenamiento protegido con firewall, solo se puede usar **Identidad de usuario** o **Identidad administrada**.

#### <a name="user-identity"></a>Identidad del usuario

Para acceder a un almacenamiento que está protegido por firewall mediante Identidad de usuario, puede usar el módulo Az.Storage de PowerShell.
#### <a name="configuration-via-powershell"></a>Configuración mediante PowerShell

Siga estos pasos para configurar el firewall de la cuenta de almacenamiento y agregar una excepción para el área de trabajo de Synapse.

1. Abra o [instale PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7.1&preserve-view=true ).
2. Instale el módulo Az. Storage actualizado: 
    ```powershell
    Install-Module -Name Az.Storage -RequiredVersion 3.0.1-preview -AllowPrerelease
    ```
    > [!IMPORTANT]
    > Asegúrese de que usa la versión 3.0.1 o posterior. Puede comprobar la versión de Az.Storage con este comando:  
    > ```powershell 
    > Get-Module -ListAvailable -Name  Az.Storage | select Version
    > ```
    > 

3. Conéctese a su inquilino de Azure: 
    ```powershell
    Connect-AzAccount
    ```
4. Defina las variables en PowerShell: 
    - Nombre del grupo de recursos: puede encontrarlo en Azure Portal, en la información general del área de trabajo de Synapse.
    - Nombre de cuenta: nombre de la cuenta de almacenamiento que está protegida por reglas de firewall.
    - Id. de inquilino: puede encontrarlo en Azure Portal, en la información del inquilino de Azure Active Directory.
    - Id. de recurso: puede encontrarlo en Azure Portal, en la información general del área de trabajo de Synapse.

    ```powershell
        $resourceGroupName = "<resource group name>"
        $accountName = "<storage account name>"
        $tenantId = "<tenant id>"
        $resourceId = "<Synapse workspace resource id>"
    ```
    > [!IMPORTANT]
    > Asegúrese de que el identificador de recurso coincida con el de esta plantilla.
    >
    > Es importante escribir **resourcegroups** en minúsculas.
    > Ejemplo de un identificador de recurso: 
    > ```
    > /subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Synapse/workspaces/{name-of-workspace}
    > ```
    > 
5. Agregue una regla de red de almacenamiento: 
    ```powershell
        Add-AzStorageAccountNetworkRule -ResourceGroupName $resourceGroupName -Name $accountName -TenantId $tenantId -ResourceId $resourceId
    ```
6. Compruebe que la regla se aplicó en la cuenta de almacenamiento: 
    ```powershell
        $rule = Get-AzStorageAccountNetworkRuleSet -ResourceGroupName $resourceGroupName -Name $accountName
        $rule.ResourceAccessRules
    ```

#### <a name="managed-identity"></a>Identidad administrada
Debe establecer [Permitir servicios de Microsoft de confianza…](../../storage/common/storage-network-security.md#trusted-microsoft-services) y [asignar un rol de Azure](../../storage/common/storage-auth-aad.md#assign-azure-roles-for-access-rights) de manera explícita a la [identidad administrada asignada por el sistema](../../active-directory/managed-identities-azure-resources/overview.md) para esa instancia del recurso. En ese caso, el ámbito de acceso de la instancia corresponde al rol de Azure que se asigna a la identidad administrada.

## <a name="credentials"></a>Credenciales

Para consultar un archivo ubicado en Azure Storage, el punto de conexión del grupo de SQL sin servidor necesita una credencial que contenga la información de autenticación. Se usan dos tipos de credenciales:
- La CREDENCIAL de nivel de servidor se utiliza para las consultas ad hoc ejecutadas mediante la función `OPENROWSET`. El nombre de la credencial debe coincidir con la dirección URL de almacenamiento.
- LA CREDENCIAL CON ÁMBITO EN LA BASE DE DATOS se usa para las tablas externas. La tabla externa hace referencia a `DATA SOURCE` con la credencial que se debe usar para tener acceso al almacenamiento.

Para permitir a un usuario crear o anular una credencial, el administrador puede CONCEDER O DENEGAR un permiso para MODIFICAR CUALQUIER CREDENCIAL a un usuario:

```sql
GRANT ALTER ANY CREDENTIAL TO [user_name];
```

Los usuarios de bases de datos que tienen acceso a almacenamiento externo deben tener permiso para usar las credenciales.

### <a name="grant-permissions-to-use-credential"></a>Concesión de permisos para usar credenciales

Para usar las credenciales, un usuario debe tener el permiso `REFERENCES` sobre una credencial específica. Para conceder un permiso `REFERENCES` SOBRE una credencial de almacenamiento para un usuario específico, ejecute:

```sql
GRANT REFERENCES ON CREDENTIAL::[storage_credential] TO [specific_user];
```

Para garantizar una experiencia de paso a través de Azure AD sin interrupciones, todos los usuarios tendrán, de forma predeterminada, derecho a usar la credencial de `UserIdentity`.

## <a name="server-scoped-credential"></a>Credencial con ámbito en el servidor

Las credenciales con ámbito en el servidor se usan cuando el inicio de sesión de SQL llama a la función `OPENROWSET` sin `DATA_SOURCE` para leer archivos en alguna cuenta de almacenamiento. El nombre de la credencial con ámbito en el servidor **debe** coincidir con la dirección URL de Azure Storage. Para agregar una credencial, ejecute [CREATE CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). Deberá proporcionar un argumento CREDENTIAL NAME. Debe coincidir con una parte de la ruta de acceso o con toda la ruta de acceso a los datos de almacenamiento (consulte a continuación).

> [!NOTE]
> El argumento `FOR CRYPTOGRAPHIC PROVIDER` no se admite.

El nombre de la CREDENCIAL de nivel de servidor debe coincidir con la ruta de acceso completa de la cuenta de almacenamiento (y opcionalmente, del contenedor) en el formato siguiente: `<prefix>://<storage_account_path>/<storage_path>`. Las rutas de acceso de la cuenta de almacenamiento se describen en la tabla siguiente:

| Origen de datos externo       | Prefijo | Ruta de acceso a la cuenta de almacenamiento                                |
| -------------------------- | ------ | --------------------------------------------------- |
| Azure Blob Storage         | https  | <cuentaDeAlmacenamiento>.blob.core.windows.net             |
| Azure Data Lake Storage Gen1 | https  | <cuentaDeAlmacenamiento>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Storage Gen2 | https  | <cuentaDeAlmacenamiento>.dfs.core.windows.net              |

Las credenciales con ámbito en el servidor permiten el acceso a Azure Storage mediante los siguientes tipos de autenticación:

### <a name="user-identity"></a>[Identidad de usuario](#tab/user-identity)

Los usuarios de Azure AD que tengan los roles `Storage Blob Data Owner`, `Storage Blob Data Contributor`o `Storage Blob Data Reader` pueden acceder a cualquier archivo de Azure Storage. Los usuarios de Azure AD no necesitan credenciales para acceder al almacenamiento. 

Los usuarios de SQL no pueden utilizar la autenticación de Azure AD para acceder al almacenamiento.

### <a name="shared-access-signature"></a>[Firma de acceso compartido](#tab/shared-access-signature)

El script siguiente crea una credencial de nivel de servidor que puede usar la función `OPENROWSET` para tener acceso a cualquier archivo de Azure Storage mediante el token de SAS. Cree esta credencial para habilitar la entidad de seguridad de SQL que ejecuta la función `OPENROWSET` para leer archivos protegidos con clave SAS en la instancia de Azure Storage que coincida con la dirección URL del nombre de la credencial.

Reemplace <*mystorageaccountname*> por el nombre de la cuenta de almacenamiento real y <*mystorageaccountcontainername*> por el nombre real del contenedor:

```sql
CREATE CREDENTIAL [https://<storage_account>.dfs.core.windows.net/<container>]
WITH IDENTITY='SHARED ACCESS SIGNATURE'
, SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
```

### <a name="managed-identity"></a>[Identidad administrada](#tab/managed-identity)

El script siguiente crea una credencial de nivel de servidor que puede usar la función `OPENROWSET` para tener acceso a cualquier archivo de Azure Storage mediante la identidad administrada del área de trabajo.

```sql
CREATE CREDENTIAL [https://<storage_account>.dfs.core.windows.net/<container>]
WITH IDENTITY='Managed Identity'
```

### <a name="public-access"></a>[Acceso público](#tab/public-access)

La credencial con ámbito en la base de datos no es necesaria para permitir el acceso a archivos disponibles públicamente. Cree un [origen de datos sin credenciales con ámbito en la base de datos](develop-tables-external-tables.md?tabs=sql-ondemand#example-for-create-external-data-source) para tener acceso a los archivos disponibles públicamente en Azure Storage.

---

## <a name="database-scoped-credential"></a>Credencial con ámbito en la base de datos

Las credenciales con ámbito en la base de datos se usan cuando cualquier entidad de seguridad llama a la función `OPENROWSET` con `DATA_SOURCE` o selecciona datos de [tabla externa](develop-tables-external-tables.md) que no tienen acceso a archivos públicos. No es necesario que la credencial con ámbito en la base de datos coincida con el nombre de la cuenta de almacenamiento. Se usará explícitamente en el ORIGEN DE DATOS que define la ubicación del almacenamiento.

Las credenciales con ámbito en la base de datos permiten el acceso a Azure Storage mediante los siguientes tipos de autenticación:

### <a name="azure-ad-identity"></a>[Identidad de Azure AD](#tab/user-identity)

Los usuarios de Azure AD que tengan, como mínimo, los roles `Storage Blob Data Owner`, `Storage Blob Data Contributor`o `Storage Blob Data Reader` pueden acceder a cualquier archivo de Azure Storage. Los usuarios de Azure AD no necesitan credenciales para acceder al almacenamiento.

```sql
CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>'
)
```

Los usuarios de SQL no pueden utilizar la autenticación de Azure AD para acceder al almacenamiento.

### <a name="shared-access-signature"></a>[Firma de acceso compartido](#tab/shared-access-signature)

El script siguiente crea una credencial que se usa para tener acceso a los archivos del almacenamiento mediante el token de SAS especificado en la credencial. El script creará un origen de datos externo de ejemplo que usa este token de SAS para acceder al almacenamiento.

```sql
-- Optional: Create MASTER KEY if not exists in database:
-- CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Very Strong Password>'
GO
CREATE DATABASE SCOPED CREDENTIAL [SasToken]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
     SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO
CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>',
          CREDENTIAL = SasToken
)
```

### <a name="managed-identity"></a>[Identidad administrada](#tab/managed-identity)

El script siguiente crea una credencial con ámbito en la base de datos que se puede usar para suplantar al usuario actual de Azure AD como identidad administrada del servicio. El script creará un origen de datos externo de ejemplo que usa la identidad del área de trabajo para acceder al almacenamiento.

```sql
-- Optional: Create MASTER KEY if not exists in database:
-- CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Very Strong Password>
CREATE DATABASE SCOPED CREDENTIAL SynapseIdentity
WITH IDENTITY = 'Managed Identity';
GO
CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>',
          CREDENTIAL = SynapseIdentity
)
```

No es necesario que la credencial con ámbito en la base de datos coincida con el nombre de la cuenta de almacenamiento porque se usará explícitamente en el ORIGEN DE DATOS que define la ubicación del almacenamiento.

### <a name="public-access"></a>[Acceso público](#tab/public-access)

La credencial con ámbito en la base de datos no es necesaria para permitir el acceso a archivos disponibles públicamente. Cree un [origen de datos sin credenciales con ámbito en la base de datos](develop-tables-external-tables.md?tabs=sql-ondemand#example-for-create-external-data-source) para tener acceso a los archivos disponibles públicamente en Azure Storage.

```sql
CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://<storage_account>.blob.core.windows.net/<container>/<path>'
)
```
---

Las credenciales con ámbito en la base de datos se usan en orígenes de datos externos para especificar el método de autenticación que se utilizará para tener acceso a este almacenamiento:

```sql
CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>',
          CREDENTIAL = <name of database scoped credential> 
)
```

## <a name="examples"></a>Ejemplos

### <a name="access-a-publicly-available-data-source"></a>**Acceso a un origen de datos disponible públicamente**

Use el siguiente script para crear una tabla que tenga acceso al origen de datos disponible públicamente.

```sql
CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat]
       WITH ( FORMAT_TYPE = PARQUET)
GO
CREATE EXTERNAL DATA SOURCE publicData
WITH (    LOCATION   = 'https://<storage_account>.dfs.core.windows.net/<public_container>/<path>' )
GO

CREATE EXTERNAL TABLE dbo.userPublicData ( [id] int, [first_name] varchar(8000), [last_name] varchar(8000) )
WITH ( LOCATION = 'parquet/user-data/*.parquet',
       DATA_SOURCE = [publicData],
       FILE_FORMAT = [SynapseParquetFormat] )
```

El usuario de la base de datos puede leer el contenido de los archivos desde el origen de datos mediante la tabla externa o la función [OPENROWSET](develop-openrowset.md) que hace referencia al origen de datos:

```sql
SELECT TOP 10 * FROM dbo.userPublicData;
GO
SELECT TOP 10 * FROM OPENROWSET(BULK 'parquet/user-data/*.parquet',
                                DATA_SOURCE = 'mysample',
                                FORMAT='PARQUET') as rows;
GO
```

### <a name="access-a-data-source-using-credentials"></a>**Acceso a un origen de datos mediante credenciales**

Modifique el script siguiente para crear una tabla externa que tenga acceso a Azure Storage mediante el token de SAS, la identidad de Azure AD del usuario o la identidad administrada del área de trabajo.

```sql
-- Create master key in databases with some password (one-off per database)
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Y*********0'
GO

-- Create databases scoped credential that use Managed Identity or SAS token. User needs to create only database-scoped credentials that should be used to access data source:

CREATE DATABASE SCOPED CREDENTIAL WorkspaceIdentity
WITH IDENTITY = 'Managed Identity'
GO
CREATE DATABASE SCOPED CREDENTIAL SasCredential
WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sv=2019-10-1********ZVsTOL0ltEGhf54N8KhDCRfLRI%3D'

-- Create data source that one of the credentials above, external file format, and external tables that reference this data source and file format:

CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] WITH ( FORMAT_TYPE = PARQUET)
GO

CREATE EXTERNAL DATA SOURCE mysample
WITH (    LOCATION   = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>'
-- Uncomment one of these options depending on authentication method that you want to use to access data source:
--,CREDENTIAL = WorkspaceIdentity 
--,CREDENTIAL = SasCredential 
)

CREATE EXTERNAL TABLE dbo.userData ( [id] int, [first_name] varchar(8000), [last_name] varchar(8000) )
WITH ( LOCATION = 'parquet/user-data/*.parquet',
       DATA_SOURCE = [mysample],
       FILE_FORMAT = [SynapseParquetFormat] );

```

El usuario de la base de datos puede leer el contenido de los archivos desde el origen de datos mediante la [tabla externa](develop-tables-external-tables.md) o la función [OPENROWSET](develop-openrowset.md) que hace referencia al origen de datos:

```sql
SELECT TOP 10 * FROM dbo.userdata;
GO
SELECT TOP 10 * FROM OPENROWSET(BULK 'parquet/user-data/*.parquet', DATA_SOURCE = 'mysample', FORMAT='PARQUET') as rows;
GO
```

## <a name="next-steps"></a>Pasos siguientes

Los artículos que se enumeran a continuación le ayudarán a aprender a consultar diferentes tipos de carpeta y de archivo, y a crear y usar vistas:

- [Consulta de archivos .csv](query-single-csv-file.md)
- [Consulta de carpetas y varios archivos .csv](query-folders-multiple-csv-files.md)
- [Consulta de archivos específicos](query-specific-files.md)
- [Consulta de archivos Parquet](query-parquet-files.md)
- [Creación y uso de vistas](create-use-views.md)
- [Consulta de archivos JSON](query-json-files.md)
- [Consulta de tipos anidados de Parquet](query-parquet-nested-types.md)
