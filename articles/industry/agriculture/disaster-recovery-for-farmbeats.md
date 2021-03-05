---
title: Recuperación ante desastres de FarmBeats
description: En este artículo se describe cómo la recuperación de datos protege frente a la pérdida de datos.
author: uhabiba04
ms.topic: article
ms.date: 04/13/2020
ms.author: v-ummehabiba
ms.openlocfilehash: 9ece624546cab1b8b6fab8c19f4401bd050f6267
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102179891"
---
# <a name="disaster-recovery-for-farmbeats"></a>Recuperación ante desastres de FarmBeats

La recuperación de datos le protege frente a la pérdida de datos en un evento como la contracción de la región de Azure. En tal caso, puede iniciar la conmutación por error y recuperar los datos almacenados en la implementación de FarmBeats.

La recuperación de datos no es una característica predeterminada de Azure FarmBeats. Puede configurar esta característica manualmente mediante la configuración de los recursos de Azure necesarios, que FarmBeats usa para almacenar los datos en una región emparejada de Azure. Use el enfoque activo - pasivo para habilitar la recuperación.

En las secciones siguientes se proporciona información sobre cómo configurar la recuperación de datos en Azure FarmBeats:

- [Habilitación de la redundancia de datos](#enable-data-redundancy)
- [Restauración del servicio desde Online Backup](#restore-service-from-online-backup)


## <a name="enable-data-redundancy"></a>Habilitación de la redundancia de datos

FarmBeats almacena los datos en tres servicios propios de Azure, que son **Azure Storage**, **Cosmos DB** y **Time Series Insights**. Siga estos pasos para habilitar la redundancia de datos para estos servicios en una región de Azure emparejada:

1.  **Azure Storage**: siga estas instrucciones para permitir la redundancia de datos para cada cuenta de almacenamiento de la implementación de FarmBeats.
2.  **Azure Cosmos DB**: siga estas instrucciones para habilitar la redundancia de datos para la cuenta de Cosmos DB de la implementación de FarmBeats.
3.  **Azure Time Series Insights (TSI)** : TSI no ofrece actualmente la redundancia de datos. Para recuperar datos de Time Series Insights, vaya a su partner de sensores o meteorológico e inserte los datos de nuevo en la implementación de FarmBeats.

## <a name="restore-service-from-online-backup"></a>Restauración del servicio desde Online Backup

Puede iniciar la conmutación por error y recuperar los datos almacenados para los que, cada uno de los servicios mencionados anteriormente almacena datos para la implementación de FarmBeats. Una vez que haya recuperado los datos para Azure Storage y Cosmos DB, cree otra implementación de FarmBeats en la región emparejada de Azure y, después, configure la nueva implementación para que use los datos de los almacenes de datos restaurados (es decir, Azure Storage y Cosmos DB) mediante los pasos siguientes:

1. [Configuración de Cosmos DB](#configure-cosmos-db)
2. [Configuración de la cuenta de almacenamiento](#configure-storage-account)


### <a name="configure-cosmos-db"></a>Configuración de Cosmos DB

Copie la clave de acceso de la instancia restaurada de Cosmos DB y actualice el nuevo almacén de claves de FarmBeats Datahub.


  ![Captura de pantalla en la que se destaca dónde obtener la copia de la clave de acceso.](./media/disaster-recovery-for-farmbeats/key-vault-secrets.png)

> [!NOTE]
> Copie la dirección URL de la instancia restaurada de Cosmos DB y actualícela en la nueva configuración de FarmBeats Datahub App Service. Ahora puede eliminar la cuenta de Cosmos DB en la nueva implementación de FarmBeats.

  ![Captura de pantalla que muestra dónde copiar la dirección URL de la instancia de Cosmos DB restaurada.](./media/disaster-recovery-for-farmbeats/configuration.png)

### <a name="configure-storage-account"></a>Configuración de la cuenta de almacenamiento

Copie la clave de acceso de la cuenta de almacenamiento restaurada y actualícela en el nuevo almacén de claves de FarmBeats Datahub.

![Captura de pantalla que muestra dónde copiar la clave de acceso de la cuenta de almacenamiento restaurada.](./media/disaster-recovery-for-farmbeats/key-vault-7-secrets.png)

>[!NOTE]
> Asegúrese de actualizar el nombre de la cuenta de almacenamiento en el nuevo archivo de configuración de la máquina virtual de FarmBeats Batch.

![Recuperación ante desastres](./media/disaster-recovery-for-farmbeats/batch-prep-files.png)

De forma similar, si ha habilitado la recuperación de datos para la cuenta de almacenamiento del acelerador, siga el paso 2 para actualizar la clave y el nombre de acceso de la cuenta de almacenamiento del acelerador en la nueva instancia de FarmBeats.
