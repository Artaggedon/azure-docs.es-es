---
title: 'Migración con tiempo de inactividad mínimo: Azure Database for MySQL'
description: En este artículo se describe cómo realizar una migración con tiempo de inactividad mínimo de una base de datos de MySQL a una base de datos de Azure Database for MySQL mediante Azure Database Migration Service.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: 75b548b53de4c0f4ea39959c3d70e9ddb361d64c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "94540338"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migración con tiempo de inactividad mínimo a Azure Database for MySQL
[!INCLUDE[applies-to-single-flexible-server](includes/applies-to-single-flexible-server.md)]

Puede realizar migraciones de MySQL a Azure Database for MySQL con un tiempo de inactividad mínimo mediante la **funcionalidad de sincronización continua** recién introducida para el [servicio Azure Database Migration Service](https://aka.ms/get-dms) (DMS). Esta funcionalidad limita la cantidad de tiempo de inactividad en el que incurre la aplicación.

También puede consultar la [Guía de migración de bases de datos](https://github.com/Azure/azure-mysql/tree/master/MigrationGuide), donde encontrará información detallada y casos de uso sobre la migración de bases de datos a Azure Database for MySQL. En esta guía se proporcionan instrucciones que conducen al planeamiento y la ejecución correctos de una migración de MySQL a Azure.

## <a name="overview"></a>Información general
Azure DMS realiza una carga inicial de la base de datos local en Azure Database for MySQL y luego realiza la sincronización continua de todas las transacciones nuevas de Azure mientras la aplicación se siga ejecutando. Una vez que los datos se ponen al día en el destino de Azure, puede detener la aplicación por un breve período de tiempo (tiempo de inactividad mínimo), esperar al último lote de datos (desde el momento en que detiene la aplicación hasta el momento en que la aplicación deja de estar disponible efectivamente para aceptar cualquier tráfico nuevo) para ponerse al día en el destino y, a continuación, actualizar la cadena de conexión para que apunte a Azure. Cuando haya terminado, la aplicación estará funcionando en Azure.

:::image type="content" source="./media/howto-migrate-online/ContinuousSync.png" alt-text="Sincronización continua con Azure Database Migration Service":::

## <a name="next-steps"></a>Pasos siguientes
- Para más información sobre cómo migrar bases de datos a Azure Database for MySQL, consulte la [Guía de migración de base de datos](https://github.com/Azure/azure-mysql/tree/master/MigrationGuide).
- Vea el vídeo sobre [migración fácil de aplicaciones de MySQL/PostgreSQL a Azure Managed Service](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), que contiene una demostración que muestra cómo migrar aplicaciones de MySQL a Azure Database for MySQL.
- Consulte el tutorial [Migración de MySQL a Azure Database for MySQL en línea mediante DMS](../dms/tutorial-mysql-azure-mysql-online.md).