---
title: Definición de varias instancias de un valor de salida
description: Use la operación de copia en una plantilla de Azure Resource Manager (plantilla de ARM) para realizar varias iteraciones al devolver un valor desde una implementación.
ms.topic: conceptual
ms.date: 04/17/2020
ms.openlocfilehash: 5ae20ed9ec3fdb3b76adbd370f5ba22f9386d613
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96905951"
---
# <a name="output-iteration-in-arm-templates"></a>Iteración de salida en las plantillas de ARM

En este artículo se muestra cómo crear más de un valor para una salida en la plantilla de Azure Resource Manager (plantilla de ARM). Al agregar el elemento `copy` a la sección de salidas de la plantilla, puede devolver de forma dinámica varios elementos durante la implementación.

También puede utilizar el elemento copy con [recursos](copy-resources.md), [propiedades de un recurso](copy-properties.md) y [variables](copy-variables.md).

## <a name="syntax"></a>Sintaxis

El elemento copy tiene el siguiente formato general:

```json
"copy": {
  "count": <number-of-iterations>,
  "input": <values-for-the-output>
}
```

La propiedad `count` especifica el número de iteraciones que quiere para el valor de salida.

La propiedad `input` especifica las propiedades que desea repetir. Tiene que crear una matriz de elementos construida a partir del valor de la propiedad `input`. Puede tratarse de una propiedad única (como una cadena) o de un objeto con varias propiedades.

## <a name="copy-limits"></a>Límites de copia

El valor de count no puede superar 800.

El valor de count no puede ser un número negativo. Puede ser cero si implementa la plantilla con una versión reciente de la CLI de Azure, PowerShell o la API de REST. Concretamente, se debe usar:

* Azure PowerShell **2.6** o posterior
* CLI de Azure **2.0.74** o posterior
* API de REST versión **2019-05-10** o posterior
* Las [implementaciones vinculadas](linked-templates.md) deben usar la versión **10-05-2019** o posterior de la API para el tipo de recurso de implementación

Las versiones anteriores de PowerShell, la CLI y API REST no admiten un valor de count de cero.

## <a name="outputs-iteration"></a>Iteración de salidas

En el ejemplo siguiente se crea un número variable de cuentas de almacenamiento y se devuelve un punto de conexión para cada cuenta de almacenamiento:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageEndpoints": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
            }
        }
    }
}
```

La plantilla anterior devuelve una matriz con los siguientes valores:

```json
[
    "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
    "https://1storagecfrbqnnmpeudi.blob.core.windows.net/"
]
```

En el ejemplo siguiente se devuelven tres propiedades de las nuevas cuentas de almacenamiento.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageCount": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "variables": {
        "baseName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[concat(copyIndex(), variables('baseName'))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": "[parameters('storageCount')]"
            }
        }
    ],
    "outputs": {
        "storageInfo": {
            "type": "array",
            "copy": {
                "count": "[parameters('storageCount')]",
                "input": {
                    "id": "[reference(concat(copyIndex(), variables('baseName')), '2019-04-01', 'Full').resourceId]",
                    "blobEndpoint": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]",
                    "status": "[reference(concat(copyIndex(), variables('baseName'))).statusOfPrimary]"
                }
            }
        }
    }
}
```

El ejemplo anterior devuelve una matriz con los valores siguientes:

```json
[
    {
        "id": "Microsoft.Storage/storageAccounts/0storagecfrbqnnmpeudi",
        "blobEndpoint": "https://0storagecfrbqnnmpeudi.blob.core.windows.net/",
        "status": "available"
    },
    {
        "id": "Microsoft.Storage/storageAccounts/1storagecfrbqnnmpeudi",
        "blobEndpoint": "https://1storagecfrbqnnmpeudi.blob.core.windows.net/",
        "status": "available"
    }
]
```

## <a name="next-steps"></a>Pasos siguientes

* Para realizar un tutorial, consulte [Tutorial: Creación de varias instancias de recursos con plantillas de Resource Manager](template-tutorial-create-multiple-instances.md).
* Para otros usos del elemento copy, consulte:
  * [Iteración de recursos en las plantillas de ARM](copy-resources.md)
  * [Iteración de propiedades en las plantillas de ARM](copy-properties.md)
  * [Iteración de variables en las plantillas de ARM](copy-variables.md)
* Si quiere obtener más información sobre las secciones de una plantilla, vea [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
* Para información sobre cómo implementar una plantilla, consulte [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](deploy-powershell.md).
