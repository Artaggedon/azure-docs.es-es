---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: a6349188a2c6b4da68009df93fbea5fa6eabacf1
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102244654"
---
<!-- ### Create a storage account -->

Al crear una cuenta de Media Services, debe proporcionar el nombre de un recurso de cuenta de Azure Storage. La cuenta de almacenamiento especificada está asociada a su cuenta de Media Services. Para más información sobre cómo se utilizan las cuentas de almacenamiento en Media Services, consulte [Cuentas de almacenamiento](../storage-account-concept.md).

Debe tener una cuenta de almacenamiento **Principal** y puede tener cualquier número de cuentas de almacenamiento **Secundarias** asociadas a su cuenta de Media Services. Media Services admite cuentas de **Uso general v2** (GPv2) o **Uso general v1** (GPv1). No se permiten cuentas de solo BLOB como **Principal**. Si quiere obtener más información sobre las cuentas de almacenamiento, consulte [Opciones de la cuenta de Azure Storage](../../../storage/common/storage-account-overview.md). 

En este ejemplo, se va a crear una cuenta LRS estándar de uso general v2. Si quiere experimentar con las cuentas de almacenamiento, use `--sku Standard_LRS`. Sin embargo, al seleccionar una SKU de producción debe considerar `--sku Standard_RAGRS`, que proporciona replicación geográfica para la continuidad empresarial. Para más información, consulte los comandos [storage accounts](/cli/azure/storage/account).

El siguiente comando crea una cuenta de almacenamiento que se asociará a la cuenta de Media Services. En el siguiente script, puede sustituir `storageaccountforams` por su valor. `amsResourceGroup` debe coincidir con el valor asignado al grupo de recursos del paso anterior. El nombre de la cuenta de almacenamiento debe tener una longitud inferior a 24.

```azurecli
az storage account create --name storageaccountforams --kind StorageV2 --sku Standard_LRS -l westus2 -g amsResourceGroup
```
