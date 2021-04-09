---
title: 'Control de seguridad de Azure: recuperación de datos'
description: Recuperación de datos del control de seguridad de Azure
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 5006a592b410685ad808ff01112234bc48f5d267
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105728103"
---
# <a name="security-control-data-recovery"></a>Control de seguridad: Recuperación de datos

Asegúrese de que se realiza una copia de seguridad de todos los datos, configuraciones y secretos del sistema de forma periódica.

## <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantía de copias de seguridad automáticas periódicas

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 9.1 | 10.1 | Customer |

Habilite Azure Backup y configure el origen de la copia de seguridad (máquinas virtuales de Azure, SQL Server o recursos compartidos de archivos), así como la frecuencia y el período de retención deseados.

- [Habilitación de Azure Backup](../../backup/index.yml)

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Realización de copias de seguridad completas del sistema y copia de seguridad de las claves administradas por el cliente

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 9.2 | 10.2 | Customer |

Habilite Azure Backup y las máquinas virtuales de destino, así como la frecuencia y los períodos de retención deseados. Realice una copia de seguridad de las claves administradas del cliente con Azure Key Vault.

- [Habilitación de Azure Backup](../../backup/index.yml)

- [Creación de una copia de seguridad de las claves del almacén de claves en Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

## <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Validación de todas las copias de seguridad, incluidas las claves administradas por el cliente

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 9.3 | 10,3 | Customer |

Garantice la capacidad de realizar la restauración de datos de contenido en Azure Backup de forma periódica. Pruebe la restauración de las claves administradas por el cliente de la copia de seguridad.

- [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](../../backup/backup-azure-restore-files-from-vm.md)

- [Restauración de las claves del almacén de claves en Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey)

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Garantía de la protección de las copias de seguridad y las claves administradas del cliente

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 9,4 | 10,4 | Customer |

Para la copia de seguridad local, el cifrado en reposo se proporciona con la frase de contraseña que proporcione durante la copia de seguridad en Azure. Para máquinas virtuales de Azure, los datos están cifrados en reposo con Storage Service Encryption (SSE). Use el control de acceso basado en roles de Azure para proteger las copias de seguridad y las claves administradas por el cliente.  

Habilite la eliminación temporal y la protección de purga en Key Vault para proteger las claves contra la eliminación accidental o malintencionada.  Si Azure Storage se usa para almacenar las copias de seguridad, habilite la eliminación temporal para guardar y recuperar los datos cuando se eliminen blobs o instantáneas de blobs. 

- [Descripción de Azure RBAC](../../role-based-access-control/overview.md)

- [Procedimiento para habilitar la eliminación temporal y la protección de purga en Key Vault](../../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

- [Eliminación temporal de blobs de Azure Storage](../../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)


## <a name="next-steps"></a>Pasos siguientes

- Consulte el control de seguridad siguiente:  [Respuesta a los incidentes](security-control-incident-response.md)