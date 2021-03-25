---
title: Creación de una cuenta de almacenamiento con el cifrado de infraestructura habilitado para poder realizar el cifrado doble de datos
titleSuffix: Azure Storage
description: Los clientes que necesiten más seguridad de que sus datos estén seguros también pueden habilitar el cifrado AES de 256 bits en el nivel de infraestructura de Azure Storage. Cuando se habilita el cifrado de la infraestructura, los datos de una cuenta de almacenamiento se cifran dos veces con dos algoritmos de cifrado y dos claves diferentes.
services: storage
author: tamram
ms.service: storage
ms.date: 09/17/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-azurecli
ms.openlocfilehash: 612ba18ba71a22ad6c346b26008e688195c1d1e4
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92746576"
---
# <a name="create-a-storage-account-with-infrastructure-encryption-enabled-for-double-encryption-of-data"></a>Creación de una cuenta de almacenamiento con el cifrado de infraestructura habilitado para poder realizar el cifrado doble de datos

Azure Storage cifra de forma automática todos los datos de una cuenta de almacenamiento en el nivel de servicio mediante el cifrado AES de 256 bits, uno de los cifrados de bloques más sólidos que hay disponibles y compatible con FIPS 140-2. Los clientes que necesiten más seguridad de que sus datos estén seguros también pueden habilitar el cifrado AES de 256 bits en el nivel de infraestructura de Azure Storage. Cuando se habilita el cifrado de la infraestructura, los datos de las cuentas de almacenamiento se cifran dos veces, una vez en el nivel de servicio y otra en el nivel de infraestructura, con dos algoritmos de cifrado y dos claves diferentes. El doble cifrado de los datos de Azure Storage sirve de protección en caso de que uno de los algoritmos de cifrado o las claves puedan estar en peligro. En este escenario, la capa adicional de cifrado también protege los datos.

El cifrado de nivel de servicio permite usar claves administradas por Microsoft o claves administradas por el cliente con Azure Key Vault o el modelo de seguridad de hardware (HSM) administrado de Key Vault (versión preliminar). El cifrado en el nivel de infraestructura se basa en las claves administradas por Microsoft y siempre usa una clave independiente. Para más información sobre la administración de claves con el cifrado de Azure Storage, consulte la sección [Información sobre la administración de claves de cifrado](storage-service-encryption.md#about-encryption-key-management).

Para cifrar dos veces los datos, primero debe crear una cuenta de almacenamiento que esté configurada para el cifrado de la infraestructura. En este artículo se describe cómo crear una cuenta de almacenamiento que habilite el cifrado de la infraestructura.

## <a name="register-to-use-infrastructure-encryption"></a>Registro para usar el cifrado de infraestructura

Para crear una cuenta de almacenamiento que tenga habilitado el cifrado de infraestructura, primero debe registrarse para usar esta característica con Azure mediante PowerShell o la CLI de Azure.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

N/D

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Para registrarse con PowerShell, llame al comando [Register-AzProviderFeature](/powershell/module/az.resources/register-azproviderfeature).

```powershell
Register-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowRequireInfraStructureEncryption
```

Para comprobar el estado del registro con PowerShell, llame al comando [Get-AzProviderFeature](/powershell/module/az.resources/get-azproviderfeature).

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowRequireInfraStructureEncryption
```

Una vez aprobado el registro, debe volver a registrar el proveedor de recursos de Azure Storage. Para volver a registrar el proveedor de recursos con PowerShell, llame al comando [Register-AzResourceProvider](/powershell/module/az.resources/register-azresourceprovider).

```powershell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Storage'
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para registrarse con la CLI de Azure, llame al comando [az feature register](/cli/azure/feature#az-feature-register).

```azurecli
az feature register --namespace Microsoft.Storage \
    --name AllowRequireInfraStructureEncryption
```

Para comprobar el estado del registro con la CLI de Azure, llame al comando [az feature](/cli/azure/feature#az-feature-show).

```azurecli
az feature show --namespace Microsoft.Storage \
    --name AllowRequireInfraStructureEncryption
```

Una vez aprobado el registro, debe volver a registrar el proveedor de recursos de Azure Storage. Para volver a registrar el proveedor de recursos con la CLI de Azure, llame al comando [az provider register](/cli/azure/provider#az-provider-register).

```azurecli
az provider register --namespace 'Microsoft.Storage'
```

# <a name="template"></a>[Plantilla](#tab/template)

N/D

---

## <a name="create-an-account-with-infrastructure-encryption-enabled"></a>Creación de una cuenta con el cifrado de infraestructura habilitado

Para usar el cifrado de infraestructura debe configurar una cuenta de almacenamiento en el momento en que se crea la cuenta. La cuenta de almacenamiento debe ser de tipo de uso general v2.

Una vez que la cuenta se ha creado, no se puede habilitar o deshabilitar el cifrado de infraestructura.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Para usar PowerShell con el fin de crear una cuenta de almacenamiento con el cifrado de infraestructura habilitado, siga estos pasos:

1. En Azure Portal, vaya a la página **Cuentas de almacenamiento**.
1. Elija el botón **Agregar** para agregar una cuenta de almacenamiento v2 de uso general.
1. En la pestaña **Avanzado**, busque **Cifrado de infraestructura** y seleccione **Habilitado**.
1. Seleccione **Revisar y crear** para terminar de crear la cuenta de almacenamiento.

    :::image type="content" source="media/infrastructure-encryption-enable/create-account-infrastructure-encryption-portal.png" alt-text="Captura de pantalla que muestra cómo habilitar el cifrado de infraestructura al crear una la cuenta":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Para usar PowerShell para crear una cuenta de almacenamiento con el cifrado de infraestructura habilitado, asegúrese de haber instalado la versión 2.2.0 del módulo [Az.Storage PowerShell](https://www.powershellgallery.com/packages/Az.Storage), o cualquier versión posterior. Para más información, vea [Instalar Azure PowerShell](/powershell/azure/install-az-ps).

A continuación, cree una cuenta de almacenamiento v2 de uso general. Para ello, debe llamar al comando [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount). Incluya la opción `-RequireInfrastructureEncryption` para habilitar el cifrado de infraestructura.

En el ejemplo siguiente se muestra cómo crear una cuenta de almacenamiento v2 de uso general configurada para el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) y que tenga el cifrado de infraestructura habilitado para poder realizar un cifrado doble de los datos. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```powershell
New-AzStorageAccount -ResourceGroupName <resource_group> `
    -AccountName <storage-account> `
    -Location <location> `
    -SkuName "Standard_RAGRS" `
    -Kind StorageV2 `
    -RequireInfrastructureEncryption
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para usar la CLI de Azure para crear una cuenta de almacenamiento que tenga el cifrado de infraestructura habilitado, asegúrese de que esté instalada la versión 2.8.0 de la CLI de Azure, o cualquier versión posterior. Para más información, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

A continuación, cree una cuenta de almacenamiento v2 de uso general. Para ello, llame al comando [az storage account create](/cli/azure/storage/account#az-storage-account-create) e incluya `--require-infrastructure-encryption option` para habilitar el cifrado de infraestructura.

En el ejemplo siguiente se muestra cómo crear una cuenta de almacenamiento v2 de uso general configurada para el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) y que tenga el cifrado de infraestructura habilitado para poder realizar un cifrado doble de los datos. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```azurecli-interactive
az storage account create \
    --name <storage-account> \
    --resource-group <resource-group> \
    --location <location> \
    --sku Standard_RAGRS \
    --kind StorageV2 \
    --require-infrastructure-encryption
```

# <a name="template"></a>[Plantilla](#tab/template)

En el ejemplo siguiente de JSON se crea una cuenta de almacenamiento v2 de uso general configurada para el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS) y que tenga el cifrado de infraestructura habilitado para poder realizar un cifrado doble de los datos. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```json
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2019-06-01",
        "name": "[parameters('<storage-account>')]",
        "location": "[parameters('<location>')]",
        "dependsOn": [],
        "tags": {},
        "sku": {
            "name": "[parameters('Standard_RAGRS')]"
        },
        "kind": "[parameters('StorageV2')]",
        "properties": {
            "accessTier": "[parameters('<accessTier>')]",
            "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]",
            "largeFileSharesState": "[parameters('<largeFileSharesState>')]",
            "encryption": {
                "keySource": "Microsoft.Storage",
                "requireInfrastructureEncryption": true,
                "services": {
                    "blob": { "enabled": true },
                    "file": { "enabled": true }
              }
            }
        }
    }
],
```

---

## <a name="verify-that-infrastructure-encryption-is-enabled"></a>Comprobación de que el cifrado de infraestructura está habilitado

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Para comprobar que el cifrado de infraestructura está habilitado para una cuenta de almacenamiento con Azure Portal, siga estos pasos:

1. Vaya a la cuenta de almacenamiento en Azure Portal.
1. En **Configuración**, elija **Cifrado**.

    :::image type="content" source="media/infrastructure-encryption-enable/verify-infrastructure-encryption-portal.png" alt-text="Captura de pantalla que muestra cómo comprobar que el cifrado de infraestructura está habilitado para la cuenta":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Para comprobar que el cifrado de infraestructura está habilitado en una cuenta de almacenamiento con PowerShell, llame al comando [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount). Este comando devuelve un conjunto de propiedades de la cuenta de almacenamiento y sus valores. Recupere el campo `RequireInfrastructureEncryption` dentro de la propiedad `Encryption` y compruebe que esté establecida en `True`.

En el ejemplo siguiente se recupera el valor de la propiedad `RequireInfrastructureEncryption`. No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

```powershell
$account = Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -StorageAccountName <storage-account>
$account.Encryption.RequireInfrastructureEncryption
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para comprobar que el cifrado de infraestructura está habilitado en una cuenta de almacenamiento con la CLI de Azure, llame al comando [az storage account show](/cli/azure/storage/account#az-storage-account-show). Este comando devuelve un conjunto de propiedades de la cuenta de almacenamiento y sus valores. Busque el campo `requireInfrastructureEncryption` dentro de la propiedad `encryption` y compruebe que esté establecida en `true`.

En el ejemplo siguiente se recupera el valor de la propiedad `requireInfrastructureEncryption`. No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

```azurecli-interactive
az storage account show /
    --name <storage-account> /
    --resource-group <resource-group>
```

# <a name="template"></a>[Plantilla](#tab/template)

N/D

---

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](storage-service-encryption.md)
- [Claves administradas por el cliente para el cifrado de Azure Storage](customer-managed-keys-overview.md)
