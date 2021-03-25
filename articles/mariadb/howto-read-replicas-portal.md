---
title: Administración de réplicas de lectura de Azure Database for MariaDB mediante Azure Portal
description: En este artículo se describe cómo configurar y administrar réplicas de lectura en Azure Database for MariaDB mediante el portal.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: how-to
ms.date: 6/10/2020
ms.openlocfilehash: 3ca6ef3c368a5f578cc90fae3923caa89f3b076a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98665011"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mariadb-using-the-azure-portal"></a>Procedimientos para crear y administrar réplicas de lectura en Azure Database for MariaDB mediante Azure Portal

En este artículo, obtendrá información sobre cómo crear y administrar réplicas de lectura en el servicio Azure Database for MariaDB mediante Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

- Un [servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md) que se usará como servidor de origen.

> [!IMPORTANT]
> La característica de réplica de lectura solo está disponible para servidores de Azure Database for MariaDB en los planes de tarifa De uso general u Optimizado para memoria. Asegúrese de que el servidor de origen esté en uno de estos planes de tarifa.

## <a name="create-a-read-replica"></a>Creación de una réplica de lectura

> [!IMPORTANT]
> Cuando se crea una réplica para un origen que no tiene réplicas existentes, el origen se reiniciará primero a fin de prepararse para la replicación. Téngalo en cuenta y realice estas operaciones durante un período de poca actividad.

Para crear un servidor de réplica de lectura, puede seguir estos siguientes pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).

2. Seleccione el servidor de Azure Database for MariaDB existente que quiera usar como servidor maestro. Esta acción abre la página **Información general**.

3. Seleccione **Replicación** en el menú, en **CONFIGURACIÓN**.

4. Seleccione **Agregar réplica**.

   ![Azure Database for MariaDB: replicación](./media/howto-read-replica-portal/add-replica.png)

5. Escriba un nombre para el servidor de réplica.

    ![Azure Database for MariaDB: nombre de réplica](./media/howto-read-replica-portal/replica-name.png)

6. Seleccione la ubicación del servidor de réplica. La ubicación predeterminada es la misma que la del servidor de origen.

    ![Azure Database for MariaDB: ubicación de la réplica](./media/howto-read-replica-portal/replica-location.png)

7. Seleccione **Aceptar** para confirmar la creación de la réplica.

> [!NOTE]
> Las réplicas de lectura se crean con la misma configuración de servidor que el servidor maestro. Una vez creado, se puede cambiar la configuración del servidor de réplica. Se recomienda mantener la configuración del servidor de réplica con valores iguales o mayores que el origen para asegurarse de que la réplica trabaja al mismo nivel que el servidor maestro.

Una vez creado el servidor de réplica, puede verlo en la hoja **Replicación**.

   ![Azure Database for MariaDB: lista de réplicas](./media/howto-read-replica-portal/list-replica.png)

## <a name="stop-replication-to-a-replica-server"></a>Detención de la replicación en un servidor de réplica

> [!IMPORTANT]
> La detención la replicación en un servidor es irreversible. Una vez detenida la replicación entre un origen y una réplica, la operación no se puede deshacer. Después, el servidor de réplica se convierte en un servidor independiente que admite operaciones de lectura y escritura. Este servidor no puede volver a convertirse en una réplica.

Para detener la replicación entre un servidor de origen y un servidor de réplicas desde Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione el servidor de Azure Database for MariaDB como origen. 

2. Seleccione **Replicación** en el menú, en **CONFIGURACIÓN**.

3. Seleccione el servidor de réplica para el que desea detener la replicación.

   ![Azure Database for MariaDB: selección del servidor para detener la replicación](./media/howto-read-replica-portal/stop-replication-select.png)

4. Seleccione **Detener replicación**.

   ![Azure Database for MariaDB: detención de la replicación](./media/howto-read-replica-portal/stop-replication.png)

5. Para confirmar que desea detener la replicación, haga clic en **Aceptar**.

   ![Azure Database for MariaDB: confirmación de la detención de la replicación](./media/howto-read-replica-portal/stop-replication-confirm.png)

## <a name="delete-a-replica-server"></a>Eliminación de un servidor de réplica

Para eliminar un servidor de réplica de lectura en Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione el servidor de Azure Database for MariaDB como origen.

2. Seleccione **Replicación** en el menú, en **CONFIGURACIÓN**.

3. Seleccione el servidor de réplica que desea eliminar.

   ![Azure Database for MariaDB: selección del servidor para eliminar las réplicas](./media/howto-read-replica-portal/delete-replica-select.png)

4. Seleccione **Eliminar réplica**.

   ![Azure Database for MariaDB: eliminación de la réplica](./media/howto-read-replica-portal/delete-replica.png)

5. Escriba el nombre de la réplica y haga clic en **Eliminar** para confirmar la eliminación.  

   ![Azure Database for MariaDB: confirmación de la eliminación de la réplica](./media/howto-read-replica-portal/delete-replica-confirm.png)

## <a name="delete-a-source-server"></a>Eliminación de un servidor de origen

> [!IMPORTANT]
> Al eliminar un servidor de origen, se detiene la replicación en todos los servidores de réplica y se elimina el propio servidor de origen. Los servidores de réplica se convierten en servidores independientes que ahora admiten tanto lectura como escritura.

Para eliminar un servidor de origen en Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione el servidor de Azure Database for MariaDB como origen.

2. En la página **Información general**, seleccione **Eliminar**.

   ![Azure Database for MariaDB: eliminación del servidor maestro](./media/howto-read-replica-portal/delete-master-overview.png)

3. Escriba el nombre del servidor de origen y haga clic en **Eliminar** para confirmar la eliminación.  

   ![Azure Database for MariaDB: confirmación de la eliminación del servidor maestro](./media/howto-read-replica-portal/delete-master-confirm.png)

## <a name="monitor-replication"></a>Supervisión de la replicación

1. En [Azure Portal](https://portal.azure.com/), seleccione el servidor de réplica de Azure Database for MariaDB que quiera supervisar.

2. En la sección **Supervisión** de la barra lateral, seleccione **Métricas**:

3. Seleccione **Intervalo de replicación en segundos** en la lista desplegable de métricas disponibles.

   ![Seleccionar el intervalo de replicación](./media/howto-read-replica-portal/monitor-select-replication-lag.png)

4. Seleccione el intervalo de tiempo que desea ver. En la imagen siguiente se ha seleccionado un intervalo de tiempo de 30 minutos.

   ![Seleccionar intervalo de tiempo](./media/howto-read-replica-portal/monitor-replication-lag-time-range.png)

5. Ver el intervalo de replicación para el intervalo de tiempo seleccionado. En la imagen siguiente se muestran los últimos 30 minutos para una carga de trabajo grande.

   ![Seleccionar un intervalo de tiempo de 30 minutos](./media/howto-read-replica-portal/monitor-replication-lag-time-range-thirty-mins.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [las réplicas de lectura](concepts-read-replicas.md)
