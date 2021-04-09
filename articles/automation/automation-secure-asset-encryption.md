---
title: Cifrado de recursos seguros en Azure Automation
description: En este artículo se proporcionan conceptos para configurar la cuenta de Automation para usar el cifrado.
services: automation
ms.service: automation
ms.subservice: process-automation
author: snehithm
ms.author: snmuvva
ms.date: 01/11/2020
ms.topic: conceptual
manager: kmadnani
ms.openlocfilehash: b46cf3a742158a3347b43a1e9bc6d62d0b2160d4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "104773698"
---
# <a name="encryption-of-secure-assets-in-azure-automation"></a>Cifrado de recursos seguros en Azure Automation

Los recursos protegidos en Azure Automation incluyen credenciales, certificados, conexiones y variables cifradas. Estos recursos se protegen en Azure Automation mediante varios niveles de cifrado. En función de la clave de nivel superior utilizada para el cifrado, hay dos modelos de cifrado:

- Usando claves administradas por Microsoft
- Uso de claves que administra

## <a name="microsoft-managed-keys"></a>Claves administradas por Microsoft

De forma predeterminada, la cuenta de Azure Automation usa claves de cifrado administradas por Microsoft.

Cada recurso seguro se cifra y se almacena en Azure Automation con una clave única(clave de cifrado de datos) que se genera para cada cuenta de Automation. Estas claves se cifran y almacenan en Azure Automation con otra clave única que se genera para cada cuenta, denominada clave de cifrado de cuenta (AEK). Estas claves de cifrado de cuenta se cifran y almacenan en Azure Automation mediante claves administradas por Microsoft. 

## <a name="keys-that-you-manage-with-key-vault-preview"></a>Claves que se administra con Key Vault (versión preliminar)

Puede administrar el cifrado de recursos seguros para su cuenta de Automation con sus propias claves. Cuando especifica una clave administrada por el cliente en el nivel de la cuenta de Automation, esa clave se usa para proteger y controlar el acceso a la clave de cifrado de cuenta para la cuenta de Automation. Esta, a su vez, se usa para cifrar y descifrar todos los recursos seguros. Las claves administradas por el cliente ofrecen más flexibilidad para crear, rotar, deshabilitar y revocar controles de acceso. También puede auditar las claves de cifrado que se usan para proteger los recursos seguros.

Use Azure Key Vault para almacenar las claves administradas por el cliente. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas.  Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../key-vault/general/overview.md)

## <a name="use-of-customer-managed-keys-for-an-automation-account"></a>Uso de las claves administradas por el cliente para una cuenta de Automation

Al usar el cifrado con claves administradas por el cliente para una cuenta de Automation, Azure Automation encapsula la clave de cifrado de la cuenta con la clave administrada por el cliente en el almacén de claves asociado. La habilitación de claves administradas por el cliente no afecta al rendimiento, y la cuenta se cifra con la nueva clave inmediatamente sin retraso alguno.

Las cuentas de Automation nuevas siempre se cifran mediante claves administradas por Microsoft. No es posible habilitar claves administradas por el cliente en el momento en que se crea la cuenta. Las claves administradas por el cliente se almacenan en Azure Key Vault, y el almacén de claves se debe aprovisionar con directivas de acceso que concedan permisos de clave a la identidad administrada que está asociada a la cuenta de Automation. La identidad administrada solo está disponible después de crear la cuenta de almacenamiento.

Al modificar la clave que se usa para el cifrado de recursos seguros de Azure Automation habilitando o deshabilitando las claves administradas por el cliente, actualizando la versión de la clave o especificando una clave diferente, el cifrado de la clave de cifrado de cuenta cambia, pero los recursos seguros de su cuenta de Azure Storage no tienen que volver a cifrarse.

> [!NOTE] 
> Para habilitar las claves administradas por el cliente, tiene que realizar llamadas a la API REST de Azure Automation con la versión de API 2020-01-13-preview.

### <a name="prerequisites-for-using-customer-managed-keys-in-azure-automation"></a>Requisitos previos para el uso de claves administradas por el cliente en Azure Automation

Antes de habilitar las claves administradas por el cliente para una cuenta de Automation, debe asegurarse de que se cumplen los siguientes requisitos previos:

 - La clave administrada por el cliente se almacena en Azure Key Vault. 
 - Habilite las propiedades **Eliminación temporal** y **No purgar** en el almacén de claves. Estas características son necesarias para permitir la recuperación de claves en caso de que se eliminen accidentalmente.
 - Solo se admiten claves RSA con para el cifrado de Azure Automation. Para obtener más información acerca de las claves, consulte [Información acerca de claves, secretos y certificados de Azure Key Vault](../key-vault/general/about-keys-secrets-certificates.md).
- La cuenta de Automation y el almacén de claves pueden estar en distintas suscripciones, pero deben estar en el mismo inquilino de Azure Active Directory.

### <a name="assignment-of-an-identity-to-the-automation-account"></a>Asignación de una identidad a la cuenta de Automation

Para usar las claves administradas por el cliente con una cuenta de Automation, la cuenta de Automation debe autenticarse en el almacén de claves que contiene las claves administradas por el cliente. Azure Automation usa identidades administradas asignadas por el sistema para autenticar la cuenta con Azure Key Vault. Para más información sobre las identidades administradas, consulte el artículo sobre [Qué son las identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md).

Configure una identidad administrada asignada por el sistema a la cuenta de Automation mediante la siguiente llamada a la API REST:

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

Cuerpo de la solicitud:

```json
{ 
 "identity": 
 { 
  "type": "SystemAssigned" 
  } 
}
```

La identidad asignada por el sistema para la cuenta de Automation se devuelve en una respuesta similar a la siguiente:

```json
{
 "name": "automation-account-name",
 "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
 ..
 "identity": {
    "type": "SystemAssigned",
    "principalId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
    "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb"
 },
..
}
```

### <a name="configuration-of-the-key-vault-access-policy"></a>Configuración de la directiva de acceso de Key Vault

Una vez que se asigna una identidad administrada a la cuenta de Automation, se configura el acceso al almacén de claves que contiene las claves administradas por el cliente. Azure Automation requiere **get**, **recover**, **wrapKey** y **UnwrapKey** en las claves administradas por el cliente.

Este tipo de directiva de acceso se puede establecer mediante la siguiente llamada a la API REST:

```http
PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sample-group/providers/Microsoft.KeyVault/vaults/sample-vault/accessPolicies/add?api-version=2018-02-14
```

Cuerpo de la solicitud:

```json
{
  "properties": {
    "accessPolicies": [
      {
        "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
        "objectId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
        "permissions": {
          "keys": [
            "get",
            "recover",
            "wrapKey",
            "unwrapKey"
          ],
          "secrets": [],
          "certificates": []
        }
      }
    ]
  }
}
```

> [!NOTE]
> Se deben proporcionar los campos **tenantId** y **objectId** con valores de **identity.tenantId** y **identity.principalId** respectivamente de la respuesta de la identidad administrada para la cuenta de Automation.

### <a name="change-the-configuration-of-automation-account-to-use-customer-managed-key"></a>Cambio de la configuración de la cuenta de Automation para usar la clave administrada por el cliente

Por último, puede cambiar la cuenta de Automation de las claves administradas por Microsoft a las claves administradas por el cliente, mediante la siguiente llamada a la API REST:

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

Cuerpo de la solicitud:

```json
{
    "identity": {
    "type": "SystemAssigned"
    },
    "properties": {
        "encryption": {
            "keySource": "Microsoft.Keyvault",
            "keyvaultProperties": {
                "keyName": "sample-vault-key",
                "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
                "keyVersion": "7c73556c521340209371eaf623cc099d"
            }
        }
    }
}
```

Respuesta de muestra

```json
{
  "name": "automation-account-name",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
  ..
  "properties": {
    ..
    "encryption": {
      "keyvaultProperties": {
         "keyName": "sample-vault-key",
          "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
          "keyVersion": "7c73556c521340209371eaf623cc099d"
      },
      "keySource": "Microsoft.Keyvault"
    },
    ..
  }
}
```

## <a name="rotation-of-a-customer-managed-key"></a>Rotación de una clave administrada por el cliente

Las claves administradas por el cliente se pueden rotar en Azure Key Vault según las directivas de cumplimiento. Cuando la clave rota, hay que actualizar la cuenta de Automation para usar el nuevo identificador URI de la clave.

La rotación de la clave no desencadena un nuevo cifrado de los recursos seguros en la cuenta de Automation. No se requiere ninguna acción adicional.

## <a name="revocation-of-access-to-a-customer-managed-key"></a>Revocación del acceso de una clave administrada por el cliente

Para revocar el acceso a las claves administradas por el cliente, use PowerShell o la CLI de Azure. Para más información, consulte la referencia de [PowerShell para Azure Key Vault](/powershell/module/az.keyvault/) o la referencia de la [CLI para Azure Key Vault](/cli/azure/keyvault). La revocación del acceso bloquea de manera eficaz el acceso a todos los recursos seguros de la cuenta de Automation, ya que Azure Automation no puede acceder a la clave de cifrado.

## <a name="next-steps"></a>Pasos siguientes

- Para comprender Azure Key Vault, vea [¿Qué es Azure Key Vault?](../key-vault/general/overview.md).
- Para trabajar con certificados, vea [Administración de certificados en Azure Automation](shared-resources/certificates.md).
- Para manipular las credenciales, vea [Administración de credenciales en Azure Automation](shared-resources/credentials.md).
- Para usar variables de automatización, [Administración de variables en Azure Automation](shared-resources/variables.md).
- Para obtener ayuda al trabajar con conexiones, vea [Administración de conexiones en Azure Automation](automation-connections.md).
