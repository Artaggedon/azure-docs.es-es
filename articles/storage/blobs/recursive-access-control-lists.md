---
title: Establecimiento de listas de control de acceso (ACL) de forma recursiva para Azure Data Lake Storage Gen2 | Microsoft Docs
description: Puede agregar, actualizar y quitar listas de control de acceso de forma recursiva para los elementos secundarios existentes de un directorio primario.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 01/22/2021
ms.author: normesta
ms.reviewer: prishet
ms.custom: devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: 626e626cbd8fa86bd0366516cbaf5a54789f3988
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98741050"
---
# <a name="set-access-control-lists-acls-recursively-for-azure-data-lake-storage-gen2"></a>Establecimiento de listas de control de acceso (ACL) de forma recursiva para Azure Data Lake Storage Gen2

La herencia de ACL ya está disponible para los nuevos elementos secundarios que se crean en un directorio primario. Ahora también puede agregar, actualizar y quitar las ACL de forma recursiva para los elementos secundarios existentes de un directorio primario sin tener que realizar estos cambios individualmente para cada elemento secundario.

[Bibliotecas](#libraries) | [Ejemplos](#code-samples) | [Procedimientos recomendados](#best-practice-guidelines)

## <a name="prerequisites"></a>Requisitos previos

- Suscripción a Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

- Una cuenta de almacenamiento que tenga habilitado el espacio de nombres jerárquico (HNS). Siga [estas](create-data-lake-storage-account.md) instrucciones para crear uno.

- Los permisos correctos para ejecutar el proceso de ACL recursivo. El permiso correcto incluye una de las siguientes opciones: 

  - Una [entidad de seguridad](../../role-based-access-control/overview.md#security-principal) aprovisionada de Azure Active Directory a la que se ha asignado el rol [Propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) en el ámbito del contenedor de destino, el grupo de recursos primario o la suscripción.   

  - El usuario propietario del contenedor o directorio de destino al que va a aplicar el proceso de ACL recursivo. Esto incluye todos los elementos secundarios del contenedor o directorio de destino. 

- Una descripción de cómo se aplican las ACL a los directorios y archivos. Consulte [Control de acceso en Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md). 

Consulte la sección **Configurar su proyecto** de este artículo para ver la guía de instalación de PowerShell, el SDK de .NET y el SDK de Python.

## <a name="set-up-your-project"></a>Configurar su proyecto

Instale las bibliotecas necesarias.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Use el siguiente comando para comprobar que la versión de PowerShell que ha instalado es `5.1` o posterior.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   Para actualizar la versión de PowerShell, vea [Actualización de Windows PowerShell existente](/powershell/scripting/install/installing-windows-powershell).
    
2. Instale el módulo **Az.Storage**.

   ```powershell
   Install-Module Az.Storage -Repository PSGallery -Force  
   ```

   Para más información sobre cómo instalar módulos de PowerShell, vea [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps).

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Abra [Azure Cloud Shell](../../cloud-shell/overview.md) o, si ha [instalado](/cli/azure/install-azure-cli) la CLI de Azure localmente, abra una aplicación de consola de comandos como Windows PowerShell.

2. Use el comando siguiente para verificar que la versión de la CLI de Azure que ha instalado sea `2.14.0` o posterior.

   ```azurecli
    az --version
   ```
   Si la versión de la CLI de Azure es anterior a `2.14.0`, instale una versión posterior. Consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

### <a name="net"></a>[.NET](#tab/dotnet)

1. Abra una ventana de comando (por ejemplo: Windows PowerShell).

2. En el directorio del proyecto, instale el paquete Azure.Storage.Files.DataLake de la versión preliminar mediante el comando `dotnet add package`.

   ```console
   dotnet add package Azure.Storage.Files.DataLake -v 12.5.0-preview.1 -s https://pkgs.dev.azure.com/azure-sdk/public/_packaging/azure-sdk-for-net/nuget/v3/index.json
   ```

3. Agregue estas instrucciones "using" al inicio del archivo de código.

   ```csharp
   using Azure;
   using Azure.Core;
   using Azure.Storage;
   using Azure.Storage.Files.DataLake;
   using Azure.Storage.Files.DataLake.Models;
   using System.Collections.Generic;
   using System.Threading.Tasks;
    ```

### <a name="java"></a>[Java](#tab/java)

Para empezar, abra [esta página](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) y busque la versión más reciente de la biblioteca de Java. Después, abra el archivo *pom.xml* en el editor de texto. Agregue un elemento de dependencia que haga referencia a esa versión.

Si planea autenticar la aplicación cliente mediante Azure Active Directory (AD), agregue una dependencia a la biblioteca cliente de secreto de Azure. Consulte [Adición del paquete de la biblioteca cliente de secreto al proyecto](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity#adding-the-package-to-your-project).

A continuación, agregue estas instrucciones Imports al archivo de código.

```java
import com.azure.identity.ClientSecretCredential;
import com.azure.identity.ClientSecretCredentialBuilder;
import com.azure.core.http.rest.PagedIterable;
import com.azure.core.http.rest.Response;
import com.azure.storage.common.StorageSharedKeyCredential;
import com.azure.storage.file.datalake.DataLakeDirectoryClient;
import com.azure.storage.file.datalake.DataLakeFileClient;
import com.azure.storage.file.datalake.DataLakeFileSystemClient;
import com.azure.storage.file.datalake.DataLakeServiceClient;
import com.azure.storage.file.datalake.DataLakeServiceClientBuilder;
import com.azure.storage.file.datalake.models.AccessControlChangeResult;
import com.azure.storage.file.datalake.models.AccessControlType;
import com.azure.storage.file.datalake.models.ListPathsOptions;
import com.azure.storage.file.datalake.models.PathAccessControl;
import com.azure.storage.file.datalake.models.PathAccessControlEntry;
import com.azure.storage.file.datalake.models.PathItem;
import com.azure.storage.file.datalake.models.PathPermissions;
import com.azure.storage.file.datalake.models.PathRemoveAccessControlEntry;
import com.azure.storage.file.datalake.models.RolePermissions;
import com.azure.storage.file.datalake.options.PathSetAccessControlRecursiveOptions;
```

### <a name="python"></a>[Python](#tab/python)

1. Descargue la [biblioteca cliente de Azure Data Lake Storage para Python](https://recursiveaclpr.blob.core.windows.net/privatedrop/azure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl?sv=2019-02-02&st=2020-08-24T07%3A47%3A01Z&se=2021-08-25T07%3A47%3A00Z&sr=b&sp=r&sig=H1XYw4FTLJse%2BYQ%2BfamVL21UPVIKRnnh2mfudA%2BfI0I%3D).

2. Instale la biblioteca que descargó mediante [pip](https://pypi.org/project/pip/).

   ```
   pip install azure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl
   ```

3. Agregue estas instrucciones import al inicio del archivo de código.

   ```python
   import os, uuid, sys
   from azure.storage.filedatalake import DataLakeServiceClient
   from azure.core._match_conditions import MatchConditions
   from azure.storage.filedatalake._models import ContentSettings
   ```

---

## <a name="connect-to-your-account"></a>Conexión a su cuenta

Puede conectarse mediante Azure Active Directory o mediante una clave de cuenta. 

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="connect-by-using-azure-active-directory-ad"></a>Conexión con Azure Active Directory (AD)

Con este enfoque, el sistema garantiza que su cuenta de usuario tiene los permisos de ACL y las asignaciones de control de acceso basado en rol de Azure (Azure RBAC) apropiados. 

En la tabla siguiente se muestra cada uno de los roles admitidos y su capacidad de configuración de ACL.

|Role|Capacidad de configuración de ACL|
|--|--|
|[Propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Todos los directorios y archivos de la cuenta.|
|[Colaborador de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Solo los directorios y archivos que pertenecen a la entidad de seguridad.|

1. Abra una ventana de comandos de Windows PowerShell y, a continuación, inicie sesión en su suscripción de Azure con el comando `Connect-AzAccount` y siga las instrucciones de la pantalla.

   ```powershell
   Connect-AzAccount
   ```

2. Si su identidad está asociada a más de una suscripción, establezca la suscripción activa en la suscripción de la cuenta de almacenamiento en la que quiere crear y administrar directorios. En este ejemplo, reemplace el valor de marcador de posición `<subscription-id>` por el identificador de la suscripción.

   ```powershell
   Select-AzSubscription -SubscriptionId <subscription-id>
   ```
3. Obtenga el contexto de la cuenta de almacenamiento.

   ```powershell
   $ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -UseConnectedAccount
   ```

#### <a name="connect-by-using-an-account-key"></a>Conexión con una clave de cuenta

Con este enfoque, el sistema no comprueba los permisos ACL o de RBAC. Obtenga el contexto de la cuenta de almacenamiento mediante una clave de cuenta.

```powershell
$ctx = New-AzStorageContext -StorageAccountName "<storage-account-name>" -StorageAccountKey "<storage-account-key>"
```

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Si usa la CLI de Azure localmente, ejecute el comando de inicio de sesión.

   ```azurecli
   az login
   ```

   Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión de Azure.

   En caso contrario, abra una página del explorador en [https://aka.ms/devicelogin](https://aka.ms/devicelogin) y escriba el código de autorización que se muestra en el terminal. Inicie sesión con las credenciales de su cuenta en el explorador.

   Para obtener más información sobre los distintos métodos de autenticación, consulte [Autorización del acceso a los datos de blobs o colas con la CLI de Azure](./authorize-data-operations-cli.md).

2. Si su identidad se asocia a más de una suscripción, establezca su suscripción activa en la suscripción de la cuenta de almacenamiento que hospedará el sitio web estático.

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   Reemplace el valor de marcador de posición `<subscription-id>` por el identificador de la suscripción.

> [!NOTE]
> En el ejemplo que se presenta en este artículo se muestra la autorización de Azure Active Directory (AD). Para obtener más información sobre los métodos de autorización, consulte [Autorización del acceso a los datos de blobs o colas con la CLI de Azure](./authorize-data-operations-cli.md).

### <a name="net"></a>[.NET](#tab/dotnet)

Para usar los fragmentos de código de este artículo, tiene que crear una instancia de [DataLakeServiceClient](/dotnet/api/azure.storage.files.datalake.datalakeserviceclient) que represente la cuenta de almacenamiento.

#### <a name="connect-by-using-azure-active-directory-ad"></a>Conexión con Azure Active Directory (AD)

Puede usar la [biblioteca cliente de identidad de Azure para .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity) para autenticar la aplicación con Azure AD.

Después de instalar el paquete, agregue esta instrucción "using" al principio del archivo de código.

```csharp
using Azure.Identity;
```

Obtenga un identificador de cliente, un secreto de cliente y un identificador de inquilino. Para ello, consulte [Adquisición de un token de Azure AD para la autorización de solicitudes desde una aplicación cliente](../common/storage-auth-aad-app.md). Como parte de ese proceso, tendrá que asignar uno de los siguientes roles de [control de acceso basado en roles de Azure (Azure RBAC)](../../role-based-access-control/overview.md) a la entidad de seguridad. 

|Role|Capacidad de configuración de ACL|
|--|--|
|[Propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Todos los directorios y archivos de la cuenta.|
|[Colaborador de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Solo los directorios y archivos que pertenecen a la entidad de seguridad.|

En este ejemplo se crea una instancia de [DataLakeServiceClient](/dotnet/api/azure.storage.files.datalake.datalakeserviceclient) con un identificador de cliente, un secreto de cliente y un identificador de inquilino.  

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Authorize_DataLake.cs" id="Snippet_AuthorizeWithAAD":::

#### <a name="connect-by-using-an-account-key"></a>Conexión con una clave de cuenta

Este método es la manera más sencilla de conectarse a una cuenta. 

En este ejemplo se crea una instancia de [DataLakeServiceClient](/dotnet/api/azure.storage.files.datalake.datalakeserviceclient) con una clave de cuenta.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Authorize_DataLake.cs" id="Snippet_AuthorizeWithKey":::

> [!NOTE]
> Para ver más ejemplos, consulte la documentación de la [biblioteca cliente de identidad de Azure para .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity).

### <a name="java"></a>[Java](#tab/java)

Para usar los fragmentos de código de este artículo, tiene que crear una instancia de **DataLakeServiceClient** que represente la cuenta de almacenamiento. 

#### <a name="connect-by-using-an-account-key"></a>Conexión con una clave de cuenta

Es la manera más sencilla de conectarse a una cuenta. 

En este ejemplo se crea una instancia de **DataLakeServiceClient** con una clave de cuenta.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/Authorize_DataLake.java" id="Snippet_AuthorizeWithKey":::

#### <a name="connect-by-using-azure-active-directory-azure-ad"></a>Conexión con Azure Active Directory (Azure AD)

Puede usar la [biblioteca cliente de identidad de Azure para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) para autenticar la aplicación con Azure AD.

En este ejemplo se crea una instancia de **DataLakeServiceClient** con un identificador de cliente, un secreto de cliente y un identificador de inquilino.  Para obtener estos valores, consulte [Adquisición de un token de Azure AD para la autorización de solicitudes desde una aplicación cliente](../common/storage-auth-aad-app.md).

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/Authorize_DataLake.java" id="Snippet_AuthorizeWithAzureAD":::

> [!NOTE]
> Para ver más ejemplos, consulte la documentación [Biblioteca cliente de identidad de Azure para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity).

### <a name="python"></a>[Python](#tab/python)

Para usar los fragmentos de código de este artículo, tiene que crear una instancia de **DataLakeServiceClient** que represente la cuenta de almacenamiento. 

### <a name="connect-by-using-azure-active-directory-ad"></a>Conexión con Azure Active Directory (AD)

Puede usar la [biblioteca cliente de identidad de Azure para Python](https://pypi.org/project/azure-identity/) para autenticar la aplicación con Azure AD.

En este ejemplo se crea una instancia de **DataLakeServiceClient** con un identificador de cliente, un secreto de cliente y un identificador de inquilino.  Para obtener estos valores, consulte [Adquisición de un token de Azure AD para la autorización de solicitudes desde una aplicación cliente](../common/storage-auth-aad-app.md). Como parte de ese proceso, tendrá que asignar uno de los siguientes roles de [control de acceso basado en roles de Azure (Azure RBAC)](../../role-based-access-control/overview.md) a la entidad de seguridad. 

|Role|Capacidad de configuración de ACL|
|--|--|
|[Propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Todos los directorios y archivos de la cuenta.|
|[Colaborador de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Solo los directorios y archivos que pertenecen a la entidad de seguridad.|

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_AuthorizeWithAAD":::

> [!NOTE]
> Para ver más ejemplos, consulte la documentación de la [biblioteca cliente de identidad de Azure para Python](https://pypi.org/project/azure-identity/).

### <a name="connect-by-using-an-account-key"></a>Conexión con una clave de cuenta

Es la manera más sencilla de conectarse a una cuenta. 

En este ejemplo se crea una instancia de **DataLakeServiceClient** con una clave de cuenta.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/crud_datalake.py" id="Snippet_AuthorizeWithKey":::
 
- Reemplace el valor de marcador de posición `storage_account_name` por el nombre de la cuenta de almacenamiento.

- Reemplace el valor de marcador de posición `storage_account_key` por la clave de acceso de la cuenta de almacenamiento.

---

## <a name="set-an-acl-recursively"></a>Establecimiento de una ACL de forma recursiva

Cuando *establece* una ACL, debe **reemplazar** toda la ACL, incluidas todas sus entradas. Si quiere cambiar el nivel de permiso de una entidad de seguridad o agregar una nueva entidad de seguridad a la ACL sin que esto afecte a otras entradas existentes, debe *actualizar* la ACL en su lugar. Para actualizar una ACL en lugar de reemplazarla, consulte la sección [Actualizar una ACL de forma recursiva](#update-an-acl-recursively) de este artículo.  

Si elige *establecer* la lista de control de acceso, debe agregar una entrada para el usuario propietario, otra para el grupo propietario y una tercera para los demás usuarios. Para más información sobre el usuario propietario, el grupo propietario y el resto de usuarios, consulte [Usuarios e identidades](data-lake-storage-access-control.md#users-and-identities). 

Esta sección contiene ejemplos de cómo establecer una lista de control de acceso.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Establezca una ACL de forma recursiva mediante el cmdlet **Set-AzDataLakeGen2AclRecursive**.

En este ejemplo se establece la ACL de un directorio denominado `my-parent-directory`. En estas entradas se concede al usuario propietario permisos de lectura, escritura y ejecución, permisos solo de lectura y ejecución al grupo propietario, y ningún permiso al resto. La última entrada de ACL de este ejemplo proporciona a un usuario específico con el identificador de objeto "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" permisos de lectura y ejecución.

```powershell
$filesystemName = "my-container"
$dirname = "my-parent-directory/"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rwx 
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission r-x -InputObject $acl 
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "---" -InputObject $acl
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission r-x -InputObject $acl 

Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl

```

> [!NOTE]
> Si desea establecer una entrada de la ACL **predeterminada**, use el parámetro **-DefaultScope** cuando ejecute el comando **Set-AzDataLakeGen2ItemAclObject**. Por ejemplo: `$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rwx -DefaultScope`.

Para ver un ejemplo que establezca ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el artículo de referencia [Set-AzDataLakeGen2AclRecursive](/powershell/module/az.storage/set-azdatalakegen2aclrecursive).

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Establezca una ACL de forma recursiva mediante el comando [az storage fs access set-recursive](/cli/azure/storage/fs/access#az_storage_fs_access_set_recursive).

En este ejemplo se establece la ACL de un directorio denominado `my-parent-directory`. En estas entradas se concede al usuario propietario permisos de lectura, escritura y ejecución, permisos solo de lectura y ejecución al grupo propietario, y ningún permiso al resto. La última entrada de ACL de este ejemplo proporciona a un usuario específico con el identificador de objeto "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" permisos de lectura y ejecución.

```azurecli
az storage fs access set-recursive --acl "user::rwx,group::r-x,other::---,user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x" -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login
```

> [!NOTE]
> Si desea establecer entradas de ACL **predeterminadas**, agregue el prefijo `default:` a cada una de ellas. Por ejemplo, `default:user::rwx` o `default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x`. 

### <a name="net"></a>[.NET](#tab/dotnet)

Establezca una ACL de forma recursiva llamando al método **DataLakeDirectoryClient.SetAccessControlRecursiveAsync**. Pase a este método una [lista](/dotnet/api/system.collections.generic.list-1) de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem). Cada [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) define una entrada de la ACL. 

Si desea establecer una entrada de la ACL **predeterminada**, puede establecer la propiedad [PathAccessControlItem.DefaultScope](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem.defaultscope#Azure_Storage_Files_DataLake_Models_PathAccessControlItem_DefaultScope) de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) en **true**. 

En este ejemplo se establece la ACL de un directorio denominado `my-parent-directory`. Este método acepta un parámetro booleano denominado `isDefaultScope` que especifica si se debe establecer la ACL predeterminada. Ese parámetro se utiliza en el constructor de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem). Las entradas de la ACL conceden al usuario propietario permisos de lectura, escritura y ejecución, permisos solo de lectura y ejecución al grupo propietario, y ningún permiso al resto. La última entrada de ACL de este ejemplo proporciona a un usuario específico con el identificador de objeto "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" permisos de lectura y ejecución.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/ACL_DataLake.cs" id="Snippet_SetACLRecursively":::

Para ver un ejemplo que establezca ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FRecursive-Acl-Sample-Net.zip%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A45%253A28Z%26se%3D2021-09-25T07%253A45%253A00Z%26sr%3Db%26sp%3Dr%26sig%3D2GI3f0KaKMZbTi89AgtyGg%252BJePgNSsHKCL68V6I5W3s%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C6eae76c57d224fb6de8908d848525330%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637338865714571853&sdata=%2FWom8iI3DSDMSw%2FfYvAaQ69zbAoqXNTQ39Q9yVMnASA%3D&reserved=0) de .NET.

### <a name="java"></a>[Java](#tab/java)

Establezca una ACL de forma recursiva mediante una llamada al método **DataLakeDirectoryClient.SetAccessControlRecursive**. Pase a este método una [lista](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) de objetos [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html). Cada objeto [PathAccessControlItem](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) define una entrada de la ACL. 

Si desea establecer una entrada de ACL **predeterminada**, puede llamar al método **setDefaultScope** del objeto [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) y pasar un valor de **true**. 

En este ejemplo se establece la ACL de un directorio denominado `my-parent-directory`. Este método acepta un parámetro booleano denominado `isDefaultScope` que especifica si se debe establecer la ACL predeterminada. Ese parámetro se usa en cada llamada al método **setDefaultScope** del objeto [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html). Las entradas de la ACL conceden al usuario propietario permisos de lectura, escritura y ejecución, permisos solo de lectura y ejecución al grupo propietario, y ningún permiso al resto. La última entrada de ACL de este ejemplo proporciona a un usuario específico con el identificador de objeto "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" permisos de lectura y ejecución.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_SetACLRecursively":::

### <a name="python"></a>[Python](#tab/python)

Establezca una ACL de forma recursiva llamando al método **DataLakeDirectoryClient.set_access_control_recursive**.

Si desea establecer una entrada de la ACL **predeterminada**, agregue la cadena `default:` al principio de cada cadena de entrada de ACL. 

En este ejemplo se establece la ACL de un directorio denominado `my-parent-directory`. 

Este método acepta un parámetro booleano denominado `is_default_scope` que especifica si se debe establecer la ACL predeterminada. Si ese parámetro es `True`, la lista de entradas de la ACL va precedida de la cadena `default:`. 

Las entradas de la ACL conceden al usuario propietario permisos de lectura, escritura y ejecución, permisos solo de lectura y ejecución al grupo propietario, y ningún permiso al resto. La última entrada de la ACL de este ejemplo proporciona a un usuario específico con el identificador de objeto "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" permisos de lectura y ejecución. En estas entradas se concede al usuario propietario permisos de lectura, escritura y ejecución, permisos solo de lectura y ejecución al grupo propietario, y ningún permiso al resto. La última entrada de ACL de este ejemplo proporciona a un usuario específico con el identificador de objeto "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" permisos de lectura y ejecución.

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/ACL_datalake.py" id="Snippet_SetACLRecursively":::

Para ver un ejemplo que procese ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control_recursive.py) de Python.

---

## <a name="update-an-acl-recursively"></a>Actualización de una ACL de forma recursiva

Cuando *actualiza* una ACL, modifica la ACL en lugar de reemplazarla. Por ejemplo, puede agregar una nueva entidad de seguridad a la ACL sin que esto afecte a otras entidades de seguridad que se enumeran en la ACL.  Para reemplazar la ACL en lugar de actualizarla, consulte la sección [Establecer una ACL de forma recursiva](#set-an-acl-recursively) de este artículo. 

Para actualizar una ACL, cree un nuevo objeto ACL con la entrada de la ACL que quiera actualizar y, a continuación, use ese objeto en la operación para actualizar la ACL. No debe obtener la ACL existente, simplemente proporcione las entradas de la ACL que se vayan a actualizar.

Esta sección contiene ejemplos de cómo actualizar una ACL.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Actualice una ACL de forma recursiva mediante el cmdlet **Update-AzDataLakeGen2AclRecursive**. 

En este ejemplo se actualiza una entrada de ACL con permiso de escritura. 

```powershell
$filesystemName = "my-container"
$dirname = "my-parent-directory/"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission rwx

Update-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl

```

> [!NOTE]
> Si desea actualizar una entrada de la ACL **predeterminada**, use el parámetro **-DefaultScope** cuando ejecute el comando **Set-AzDataLakeGen2ItemAclObject**. Por ejemplo: `$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission rwx -DefaultScope`.

Para ver un ejemplo que actualice ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el artículo de referencia [Update-AzDataLakeGen2AclRecursive](/powershell/module/az.storage/update-azdatalakegen2aclrecursive).

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Actualice una ACL de forma recursiva mediante el comando [az storage fs access update-recursive](/cli/azure/storage/fs/access#az_storage_fs_access_update_recursive). 

En este ejemplo se actualiza una entrada de ACL con permiso de escritura. 

```azurecli
az storage fs access update-recursive --acl "user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:rwx" -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login
```

> [!NOTE]
> Si desea actualizar entradas de ACL **predeterminadas**, agregue el prefijo `default:` a cada una de ellas. Por ejemplo, `default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x`.

### <a name="net"></a>[.NET](#tab/dotnet)

Actualice una ACL de forma recursiva llamando al método **DataLakeDirectoryClient.UpdateAccessControlRecursiveAsync**.  Pase a este método una [lista](/dotnet/api/system.collections.generic.list-1) de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem). Cada [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) define una entrada de la ACL. 

Si desea actualizar una entrada de la ACL **predeterminada**, puede establecer la propiedad [PathAccessControlItem.DefaultScope](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem.defaultscope#Azure_Storage_Files_DataLake_Models_PathAccessControlItem_DefaultScope) de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) en **true**. 

En este ejemplo se actualiza una entrada de ACL con permiso de escritura. Este método acepta un parámetro booleano denominado `isDefaultScope` que especifica si se debe actualizar la ACL predeterminada. Ese parámetro se utiliza en el constructor de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem).

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/ACL_DataLake.cs" id="Snippet_UpdateACLsRecursively":::

Para ver un ejemplo que actualice ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FRecursive-Acl-Sample-Net.zip%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A45%253A28Z%26se%3D2021-09-25T07%253A45%253A00Z%26sr%3Db%26sp%3Dr%26sig%3D2GI3f0KaKMZbTi89AgtyGg%252BJePgNSsHKCL68V6I5W3s%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C6eae76c57d224fb6de8908d848525330%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637338865714571853&sdata=%2FWom8iI3DSDMSw%2FfYvAaQ69zbAoqXNTQ39Q9yVMnASA%3D&reserved=0) de .NET.

### <a name="java"></a>[Java](#tab/java)

Actualice una ACL de forma recursiva mediante una llamada al método **DataLakeDirectoryClient.updateAccessControlRecursive**.  Pase a este método una [lista](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) de objetos [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html). Cada objeto [PathAccessControlItem](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) define una entrada de la ACL. 

Si desea establecer una entrada de ACL **predeterminada**, puede llamar al método **setDefaultScope** del objeto [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) y pasar un valor de **true**. 

En este ejemplo se actualiza una entrada de ACL con permiso de escritura. Este método acepta un parámetro booleano denominado `isDefaultScope` que especifica si se debe actualizar la ACL predeterminada. Ese parámetro se usa en la llamada al método **setDefaultScope** del objeto [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html). 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_UpdateACLRecursively":::

### <a name="python"></a>[Python](#tab/python)

Actualice una ACL de forma recursiva llamando al método **DataLakeDirectoryClient.update_access_control_recursive**. Si desea actualizar una entrada de la ACL **predeterminada**, agregue la cadena `default:` al principio de cada cadena de entrada de la ACL. 

En este ejemplo se actualiza una entrada de ACL con permiso de escritura.

En este ejemplo se establece la ACL de un directorio denominado `my-parent-directory`. Este método acepta un parámetro booleano denominado `is_default_scope` que especifica si se debe actualizar la ACL predeterminada. Si ese parámetro es `True`, la entrada de la ACL actualizada va precedida de la cadena `default:`.  

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/ACL_datalake.py" id="Snippet_UpdateACLsRecursively":::

Para ver un ejemplo que procese ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control_recursive.py) de Python.

---

## <a name="remove-acl-entries-recursively"></a>Eliminación de las entradas de ACL de forma recursiva

Puede quitar una o varias entradas de ACL de forma recursiva. Para quitar una entrada de ACL, cree un nuevo objeto de ACL para la entrada de ACL que se va a quitar y, a continuación, use ese objeto en la operación para quitar la ACL. No debe obtener la ACL existente; simplemente proporcione las entradas de la ACL que se van a quitar. 

Esta sección contiene ejemplos de cómo quitar una ACL. 

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Quite las entradas de ACL mediante el cmdlet **Remove-AzDataLakeGen2AclRecursive**. 

En este ejemplo se quita una entrada de ACL del directorio raíz del contenedor.  

```powershell
$filesystemName = "my-container"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission "---" 

Remove-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName  -Acl $acl
```

> [!NOTE]
> Si desea quitar una entrada de la ACL **predeterminada**, use el parámetro **-DefaultScope** cuando ejecute el comando **Set-AzDataLakeGen2ItemAclObject**. Por ejemplo: `$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission "---" -DefaultScope`.

Para ver un ejemplo que quite ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el artículo de referencia [Remove-AzDataLakeGen2AclRecursive](/powershell/module/az.storage/remove-azdatalakegen2aclrecursive).

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Quite las entradas de ACL mediante el comando [az storage fs access remove-recursive](/cli/azure/storage/fs/access#az_storage_fs_access_remove_recursive). 

En este ejemplo se quita una entrada de ACL del directorio raíz del contenedor.  

```azurecli
az storage fs access remove-recursive --acl "user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login
```

> [!NOTE]
> Si desea quitar entradas de ACL **predeterminadas**, agregue el prefijo `default:` a cada una de ellas. Por ejemplo, `default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.

### <a name="net"></a>[.NET](#tab/dotnet)

Elimine entradas de ACL llamando al método **DataLakeDirectoryClient.RemoveAccessControlRecursiveAsync**. Pase a este método una [lista](/dotnet/api/system.collections.generic.list-1) de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem). Cada [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) define una entrada de la ACL. 

Si desea quitar una entrada de la ACL **predeterminada**, puede establecer la propiedad [PathAccessControlItem.DefaultScope](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem.defaultscope#Azure_Storage_Files_DataLake_Models_PathAccessControlItem_DefaultScope) de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) en **true**. 

En este ejemplo se quita una entrada de la ACL del directorio llamado `my-parent-directory`. Este método acepta un parámetro booleano denominado `isDefaultScope` que especifica si se debe quitar la entrada de la ACL predeterminada. Ese parámetro se utiliza en el constructor de [PathAccessControlItem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem).

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/ACL_DataLake.cs" id="Snippet_RemoveACLRecursively":::

Para ver un ejemplo que quite ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FRecursive-Acl-Sample-Net.zip%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A45%253A28Z%26se%3D2021-09-25T07%253A45%253A00Z%26sr%3Db%26sp%3Dr%26sig%3D2GI3f0KaKMZbTi89AgtyGg%252BJePgNSsHKCL68V6I5W3s%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C6eae76c57d224fb6de8908d848525330%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637338865714571853&sdata=%2FWom8iI3DSDMSw%2FfYvAaQ69zbAoqXNTQ39Q9yVMnASA%3D&reserved=0) de .NET.

### <a name="java"></a>[Java](#tab/java)

Elimine entradas de ACL mediante una llamada al método **DataLakeDirectoryClient.removeAccessControlRecursive**. Pase a este método una [lista](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) de objetos [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html). Cada objeto [PathAccessControlItem](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) define una entrada de la ACL. 

Si desea quitar una entrada de ACL **predeterminada**, puede llamar al método **setDefaultScope** del objeto [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) y pasar un valor de **true**.  

En este ejemplo se quita una entrada de la ACL del directorio llamado `my-parent-directory`. Este método acepta un parámetro booleano denominado `isDefaultScope` que especifica si se debe quitar la entrada de la ACL predeterminada. Ese parámetro se usa en la llamada al método **setDefaultScope** del objeto [PathAccessControlEntry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html).

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_RemoveACLRecursively":::

### <a name="python"></a>[Python](#tab/python)

Elimine entradas de ACL llamando al método **DataLakeDirectoryClient.remove_access_control_recursive**. Si desea quitar una entrada de la ACL **predeterminada**, agregue la cadena `default:` al principio de la cadena de entrada de la ACL. 

En este ejemplo se quita una entrada de la ACL del directorio llamado `my-parent-directory`. Este método acepta un parámetro booleano denominado `is_default_scope` que especifica si se debe quitar la entrada de la ACL predeterminada. Si ese parámetro es `True`, la entrada de la ACL actualizada va precedida de la cadena `default:`. 

```python
def remove_permission_recursively(is_default_scope):
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")
              
        acl = 'user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

        if is_default_scope:
           acl = 'default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

        directory_client.remove_access_control_recursive(acl=acl)

    except Exception as e:
     print(e)
```

Para ver un ejemplo que procese ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control_recursive.py) de Python.

---

## <a name="recover-from-failures"></a>Recuperación de errores

Podría encontrar errores de tiempo de ejecución o de permisos. En el caso de los errores de tiempo de ejecución, reinicie el proceso desde el principio. Los errores de permisos pueden producirse si la entidad de seguridad no tiene permisos suficientes para modificar la ACL de un directorio o archivo que se encuentra en la jerarquía de directorios que se está modificando. Solucione el problema de permisos y, a continuación, elija reanudar el proceso desde el punto de error mediante un token de continuación, o bien reinicie el proceso desde el principio. No tiene que usar el token de continuación si prefiere reiniciar desde el principio. Puede volver a aplicar las entradas de ACL sin ningún efecto negativo.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Este ejemplo se devuelven los resultados a la variable y, a continuación, se canalizan las entradas con errores a una tabla con formato.

```powershell
$result = Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
$result
$result.FailedEntries | ft 
```

Según la salida de la tabla, puede corregir todos los errores de permisos y, a continuación, reanudar la ejecución mediante el token de continuación.

```powershell
$result = Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl -ContinuationToken $result.ContinuationToken
$result

```

Para ver un ejemplo que establezca ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el artículo de referencia [Set-AzDataLakeGen2AclRecursive](/powershell/module/az.storage/set-azdatalakegen2aclrecursive).

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

En caso de que se produzca un error, establezca el parámetro `--continue-on-failure` en `false` para devolver un token de continuación. Después de resolver los errores, puede reanudar el proceso desde el punto de error si ejecuta el comando de nuevo y, a continuación, establece el parámetro `--continuation` en el token de continuación. 

```azurecli
az storage fs access set-recursive --acl "user::rw-,group::r-x,other::---" --continue-on-failure false --continuation xxxxxxx -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login  
```

## <a name="net"></a>[.NET](#tab/dotnet)

Este ejemplo devuelve un token de continuación en caso de que se produzca un error. La aplicación puede volver a llamar a este método de ejemplo después de que se haya solucionado el error y pasar el token de continuación. Si se llama a este método de ejemplo por primera vez, la aplicación puede pasar un valor de `null` para el parámetro del token de continuación. 

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/ACL_DataLake.cs" id="Snippet_ResumeContinuationToken":::

Para ver un ejemplo que establezca ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FRecursive-Acl-Sample-Net.zip%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A45%253A28Z%26se%3D2021-09-25T07%253A45%253A00Z%26sr%3Db%26sp%3Dr%26sig%3D2GI3f0KaKMZbTi89AgtyGg%252BJePgNSsHKCL68V6I5W3s%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C6eae76c57d224fb6de8908d848525330%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637338865714571853&sdata=%2FWom8iI3DSDMSw%2FfYvAaQ69zbAoqXNTQ39Q9yVMnASA%3D&reserved=0) de .NET.

### <a name="java"></a>[Java](#tab/java)

Este ejemplo devuelve un token de continuación en caso de que se produzca un error. La aplicación puede volver a llamar a este método de ejemplo después de que se haya solucionado el error y pasar el token de continuación. Si se llama a este método de ejemplo por primera vez, la aplicación puede pasar un valor de `null` para el parámetro del token de continuación. 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_ResumeSetACLRecursively":::

### <a name="python"></a>[Python](#tab/python)

Este ejemplo devuelve un token de continuación en caso de que se produzca un error. La aplicación puede volver a llamar a este método de ejemplo después de que se haya solucionado el error y pasar el token de continuación. Si se llama a este método de ejemplo por primera vez, la aplicación puede pasar un valor de ``None`` para el parámetro del token de continuación. 

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/ACL_datalake.py" id="Snippet_ResumeContinuationToken":::

Para ver un ejemplo que procese ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control_recursive.py) de Python.

---

Si desea que el proceso se complete sin que se interrumpa por errores de permisos, puede especificarlo.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

En este ejemplo se usa el parámetro `ContinueOnFailure` para que la ejecución continúe incluso si la operación encuentra un error de permiso. 

```powershell
$result = Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl -ContinueOnFailure

echo "[Result Summary]"
echo "TotalDirectoriesSuccessfulCount: `t$($result.TotalFilesSuccessfulCount)"
echo "TotalFilesSuccessfulCount: `t`t`t$($result.TotalDirectoriesSuccessfulCount)"
echo "TotalFailureCount: `t`t`t`t`t$($result.TotalFailureCount)"
echo "FailedEntries:"$($result.FailedEntries | ft) 
```

Para ver un ejemplo que establezca ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el artículo de referencia [Set-AzDataLakeGen2AclRecursive](/powershell/module/az.storage/set-azdatalakegen2aclrecursive).

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para asegurarse de que el proceso se completa sin interrupciones, establezca el parámetro `--continue-on-failure` en `true`. 

```azurecli
az storage fs access set-recursive --acl "user::rw-,group::r-x,other::---" --continue-on-failure true --continuation xxxxxxx -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login  
```

### <a name="net"></a>[.NET](#tab/dotnet)

Para asegurarse de que el proceso se completa sin interrupciones, pase un objeto **AccessControlChangedOptions** y establezca la propiedad **ContinueOnFailure** de ese objeto en ``true``.

En este ejemplo se establecen las entradas de ACL de forma recursiva. Si este código se encuentra un error de permiso, lo registra y continúa la ejecución. En este ejemplo se imprime el número de errores en la consola. 

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/ACL_DataLake.cs" id="Snippet_ContinueOnFailure":::

Para ver un ejemplo que establezca ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FRecursive-Acl-Sample-Net.zip%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A45%253A28Z%26se%3D2021-09-25T07%253A45%253A00Z%26sr%3Db%26sp%3Dr%26sig%3D2GI3f0KaKMZbTi89AgtyGg%252BJePgNSsHKCL68V6I5W3s%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C6eae76c57d224fb6de8908d848525330%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637338865714571853&sdata=%2FWom8iI3DSDMSw%2FfYvAaQ69zbAoqXNTQ39Q9yVMnASA%3D&reserved=0) de .NET.

### <a name="java"></a>[Java](#tab/java)

Para asegurarse de que el proceso se completa de forma ininterrumpida, llame al método **setContinueOnFailure** de un objeto [PathSetAccessControlRecursiveOptions](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) y pase un valor de **true**.

En este ejemplo se establecen las entradas de ACL de forma recursiva. Si este código se encuentra un error de permiso, lo registra y continúa la ejecución. En este ejemplo se imprime el número de errores en la consola. 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_ContinueOnFailure":::

### <a name="python"></a>[Python](#tab/python)

Para asegurarse de que el proceso se completa sin interrupciones, no pase un token de continuación al **DataLakeDirectoryClient.set_access_control_recursive**.

En este ejemplo se establecen las entradas de ACL de forma recursiva. Si este código se encuentra un error de permiso, lo registra y continúa la ejecución. En este ejemplo se imprime el número de errores en la consola. 

:::code language="python" source="~/azure-storage-snippets/blobs/howto/python/python-v12/ACL_datalake.py" id="Snippet_ContinueOnFailure":::

Para ver un ejemplo que procese ACL de forma recursiva en lotes especificando un tamaño de lote, consulte el [ejemplo](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control_recursive.py) de Python.

---

## <a name="resources"></a>Recursos

Esta sección contiene vínculos a bibliotecas y ejemplos de código.

#### <a name="libraries"></a>Bibliotecas

- [PowerShell](https://www.powershellgallery.com/packages/Az.Storage/3.0.0)
- [CLI de Azure](/cli/azure/storage/fs/access)
- [.NET](https://pkgs.dev.azure.com/azure-sdk/public/_packaging/azure-sdk-for-net/nuget/v3/index.json)
- [Java](/java/api/overview/azure/storage-file-datalake-readme)
- [Python](https://recursiveaclpr.blob.core.windows.net/privatedrop/azure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl?sv=2019-02-02&st=2020-08-24T07%3A47%3A01Z&se=2021-08-25T07%3A47%3A00Z&sr=b&sp=r&sig=H1XYw4FTLJse%2BYQ%2BfamVL21UPVIKRnnh2mfudA%2BfI0I%3D)
- [REST](/rest/api/storageservices/datalakestoragegen2/path/update)

#### <a name="code-samples"></a>Ejemplos de código

- PowerShell: [Léame](https://recursiveaclpr.blob.core.windows.net/privatedrop/README.txt?sv=2019-02-02&st=2020-08-24T17%3A03%3A18Z&se=2021-08-25T17%3A03%3A00Z&sr=b&sp=r&sig=sPdKiCSXWExV62sByeOYqBTqpGmV2h9o8BLij3iPkNQ%3D) | [Ejemplo](https://recursiveaclpr.blob.core.windows.net/privatedrop/samplePS.ps1?sv=2019-02-02&st=2020-08-24T17%3A04%3A44Z&se=2021-08-25T17%3A04%3A00Z&sr=b&sp=r&sig=dNNKS%2BZcp%2F1gl6yOx6QLZ6OpmXkN88ZjBeBtym1Mejo%3D)

- CLI de Azure: [Ejemplo](https://github.com/Azure/azure-cli/blob/2a55a5350696a3a93a13f364f2104ec8bc82cdd3/src/azure-cli/azure/cli/command_modules/storage/docs/ADLS%20Gen2.md)

- NET: [Léame](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FREADME%2520for%2520net%3Fsv%3D2019-02-02%26st%3D2020-08-25T23%253A20%253A42Z%26se%3D2021-08-26T23%253A20%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DKrnHvasHoSoVeUyr2g%2fSc2aDVW3De4A%2fvx0lFWZs494%253D&data=02%7C01%7Cnormesta%40microsoft.com%7Cda902e4fe6c24e6a07d908d8494fd4bd%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339954503767961&sdata=gd%2B2LphTtDFVb7pZko9rkGO9OG%2FVvmeXprHB9IOEYXE%3D&reserved=0) | [Ejemplo](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FRecursive-Acl-Sample-Net.zip%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A45%253A28Z%26se%3D2021-09-25T07%253A45%253A00Z%26sr%3Db%26sp%3Dr%26sig%3D2GI3f0KaKMZbTi89AgtyGg%252BJePgNSsHKCL68V6I5W3s%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C6eae76c57d224fb6de8908d848525330%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637338865714571853&sdata=%2FWom8iI3DSDMSw%2FfYvAaQ69zbAoqXNTQ39Q9yVMnASA%3D&reserved=0)

- Python: [Léame](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FREADME%2520for%2520python%3Fsv%3D2019-02-02%26st%3D2020-08-25T23%253A21%253A47Z%26se%3D2021-08-26T23%253A21%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DRq6Bl5lXrtYk79thy8wX7UTbjyd2f%252B6xzVBFFVYbdYg%253D&data=02%7C01%7Cnormesta%40microsoft.com%7Cda902e4fe6c24e6a07d908d8494fd4bd%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339954503777915&sdata=3e46Lp2miOHj755Gh0odH3M0%2BdTF3loGCCBENrulVTM%3D&reserved=0) | [Ejemplo](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/storage/azure-storage-file-datalake/samples/datalake_samples_access_control_recursive.py)

## <a name="best-practice-guidelines"></a>Guías de procedimientos recomendados

En esta sección se proporcionan algunas directrices de procedimientos recomendados para configurar las ACL de forma recursiva. 

#### <a name="handling-runtime-errors"></a>Manejo de errores de tiempo de ejecución

Se puede producir un error de tiempo de ejecución por muchos motivos (por ejemplo: una interrupción o un problema de conectividad de cliente). Si se produce un error de tiempo de ejecución, reinicie el proceso de ACL recursivo. Las ACL se pueden volver a aplicar a los elementos sin provocar ningún efecto negativo. 

#### <a name="handling-permission-errors-403"></a>Manejo de errores de permisos (403)

Si experimenta una excepción de control de acceso al ejecutar un proceso de ACL recursivo, es posible que la [entidad de seguridad](../../role-based-access-control/overview.md#security-principal) de AD no tenga permisos suficientes para aplicar una ACL a uno o varios de los elementos secundarios de la jerarquía de directorios. Cuando se produce un error de permiso, el proceso se detiene y se proporciona un token de continuación. Repare el problema de permisos y, después, use el token de continuación para procesar el conjunto de datos restante. Los directorios y archivos que ya se han procesado correctamente no tendrán que procesarse de nuevo. También puede optar por reiniciar el proceso de ACL recursivo. Las ACL se pueden volver a aplicar a los elementos sin provocar ningún efecto negativo. 

#### <a name="credentials"></a>Credenciales 

Se recomienda que aprovisione una entidad de seguridad de Azure AD a la que se haya asignado el rol [Propietario de datos de blobs de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) en el ámbito de la cuenta de almacenamiento o el contenedor de destino. 

#### <a name="performance"></a>Rendimiento 

Para reducir la latencia, se recomienda ejecutar el proceso de ACL recursivo en una máquina virtual de Azure que se encuentre en la misma región que la cuenta de almacenamiento. 

#### <a name="acl-limits"></a>Límites de ACL

El número máximo de ACL que puede aplicar a un directorio o archivo es de 32 ACL de acceso y 32 ACL predeterminadas. Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md).

## <a name="see-also"></a>Consulte también

- [Control de acceso en Azure Data Lake Storage Gen2](./data-lake-storage-access-control.md)
- [Problemas conocidos](data-lake-storage-known-issues.md)
