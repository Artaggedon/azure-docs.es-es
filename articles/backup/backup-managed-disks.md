---
title: Copia de seguridad de Azure Managed Disks
description: Obtenga información sobre cómo realizar una copia de seguridad de Azure Managed Disks desde Azure Portal.
ms.topic: conceptual
ms.date: 01/07/2021
ms.openlocfilehash: e234495eb483d6d0cc6ca556ca418138c61a99f5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105110634"
---
# <a name="back-up-azure-managed-disks"></a>Copia de seguridad de Azure Managed Disks

En este artículo se explica cómo realizar copias de seguridad de [Azure Managed Disks](../virtual-machines/managed-disks-overview.md) desde Azure Portal.

En este artículo, aprenderá a:

- Creación de un almacén de Backup

- Crear una directiva de copia de seguridad

- Configurar una copia de seguridad de Azure Disk

- Ejecutar un trabajo de copia de seguridad a petición.

Para obtener información sobre la disponibilidad de la región de la copia de seguridad de Azure Disk, los escenarios admitidos y las limitaciones, consulte la [matriz de compatibilidad](disk-backup-support-matrix.md).

## <a name="create-a-backup-vault"></a>Creación de un almacén de Backup

Un almacén de Backup es una entidad de almacenamiento de Azure que contiene los datos de las copias de seguridad de varias cargas de trabajo recientes que admite Azure Backup, como los servidores de Azure Database for PostgreSQL y los discos de Azure Disk. Los almacenes de Backup facilitan la tarea de organizar los datos de copia de seguridad, al mismo tiempo que reducen al mínimo la sobrecarga administrativa. Los almacenes de copias de seguridad se basan en el modelo de Azure Resource Manager de Azure, que proporciona características para proteger los datos de las copias de seguridad.

1. Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com/).
1. Escriba **Backup center** (Centro de copias de seguridad) en el cuadro de búsqueda.
1. En **Servicios**, seleccione **Centro de copias de seguridad**.
1. En la página del **Centro de copias de seguridad**, seleccione **Almacén**.

   ![Seleccionar Almacén en el Centro de copias de seguridad](./media/backup-managed-disks/backup-center.png)

1. En la pantalla **Initiate: Create Vault** (Iniciar: crear almacén), seleccione **Almacén de Backup** y **Continuar**.

   ![Iniciar: Crear almacén](./media/backup-managed-disks/initiate-create-vault.png)

1. En la pestaña **Datos básicos**, proporcione la suscripción, el grupo de recursos, el nombre del almacén de Backup, la región y la redundancia del almacenamiento de copias de seguridad. A continuación, seleccione **Revisar y crear**. Obtenga más información sobre cómo [crear un almacén de Backup](./backup-vault-overview.md#create-a-backup-vault).

   ![Revisar y crear almacén](./media/backup-managed-disks/review-and-create.png)

## <a name="create-backup-policy"></a>Creación de una directiva de copia de seguridad

1. En el **almacén de Backup** denominado *DemoVault* que creó en el paso anterior, vaya a **directivas de copia de seguridad** y seleccione **Agregar**.

   ![Agregar una directiva de copia de seguridad](./media/backup-managed-disks/backup-policies.png)

1. En la pestaña **Datos básicos**, proporcione el nombre de la directiva y seleccione un **tipo de origen de datos** como **Azure Disk**. El almacén ya se habrá rellenado previamente, y se mostrarán las propiedades seleccionadas del almacén.

   >[!NOTE]
   > Aunque el almacén seleccionado puede tener la configuración de redundancia global, Azure Disk Backup solo admite el almacén de datos de la instantánea. Todas las copias de seguridad se almacenan en un grupo de recursos de la suscripción y no se copian en el almacén de copias de seguridad.

   ![Selección del tipo de origen de datos](./media/backup-managed-disks/datasource-type.png)

1. En la pestaña **Directiva de copia de seguridad**, seleccione la frecuencia de programación de la copia de seguridad.

   ![Selección de la frecuencia de programación de la copia de seguridad](./media/backup-managed-disks/backup-schedule-frequency.png)

   Azure Disk Backup le ofrece varias copias de seguridad al día. Si necesita realizar copias de seguridad con más frecuencia, elija la frecuencia de copia de seguridad **Cada hora**, ya que le ofrece la posibilidad de realizar copias de seguridad a intervalos de 4, 6, 8 o 12 horas. Las copias de seguridad se programan en función del intervalo de **tiempo** seleccionado. Por ejemplo, si selecciona **Cada 4 horas**, las copias de seguridad se realizan aproximadamente cada 4 horas, por lo que las mismas se distribuyen equitativamente a lo largo del día. Si realizar una copia de seguridad al día es suficiente, elija la frecuencia **Diaria**. En la frecuencia de copia de seguridad diaria, puede especificar la hora del día en la que se realizarán las copias de seguridad. Es importante tener en cuenta que la hora del día indica la hora de inicio de la copia de seguridad y no la hora en que se completa la misma. El tiempo necesario para completar la operación de copia de seguridad depende de varios factores, como el tamaño del disco y la tasa de abandono entre copias de seguridad consecutivas. No obstante, Azure Disk Backup es una opción de copias de seguridad sin agente que usa [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md), lo que no afecta al rendimiento de la aplicación de producción.

1. En la pestaña **Directiva de copia de seguridad**, seleccione la configuración de retención que cumpla el requisito del objetivo de punto de recuperación (RPO).

   La regla de retención predeterminada se aplica si no se especifica ninguna otra regla de retención. Asimismo, la regla de retención predeterminada se puede modificar para cambiar la duración de la retención, pero no se puede eliminar. Puede agregar una nueva regla de retención seleccionando **Add retention rule** (Agregar regla de retención).

   ![Agregar una regla de retención](./media/backup-managed-disks/add-retention-rule.png)

   Puede elegir la **primera copia de seguridad correcta** realizada diariamente o semanalmente, y proporcionar la duración de la retención, para indicar el tiempo que se conservarán las copias de seguridad específicas antes de que se eliminen. Esta opción es útil para conservar copias de seguridad específicas del día o de la semana durante un período de tiempo más largo. Todas las demás copias de seguridad frecuentes se pueden conservar durante menos tiempo.

   ![Configuración de retención](./media/backup-managed-disks/retention-settings.png)

   >[!NOTE]
   >Azure Backup para Managed Disks usa instantáneas incrementales que se limitan a 200 instantáneas por disco. Para que pueda realizar copias de seguridad a petición además de las copias de seguridad programadas, la directiva de copia de seguridad limita el número total de copias de seguridad a 180. Obtenga más información sobre las [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md#restrictions) del disco administrado.

1. Para completar la creación de la directiva de copia de seguridad, seleccione **Revisar y crear**.

## <a name="configure-backup"></a>Configuración de la copia de seguridad

El almacén de Backup usa la identidad administrada para obtener acceso a otros recursos de Azure. Para configurar la copia de seguridad de los discos administrados, la identidad administrada del almacén de Backup requiere un conjunto de permisos en los discos de origen y los grupos de recursos donde se crean y administran las instantáneas.

Cada recurso solo puede tener una identidad administrada asignada por el sistema, y cada una de ellas está asociada al ciclo de vida del recurso. Puede conceder permisos a la identidad administrada mediante el control de acceso basado en roles de Azure (Azure RBAC). Tenga en cuenta que una identidad administrada es una entidad de servicio de un tipo especial que solo se puede usar con recursos de Azure. Obtenga más información sobre las [identidades administradas](../active-directory/managed-identities-azure-resources/overview.md).

Los siguientes requisitos previos son necesarios para configurar la copia de seguridad de los discos administrados:

1. Asigne el rol **Lector de copias de seguridad de discos** a la identidad administrada del almacén de Backup en el disco de origen del que se va a realizar la copia de seguridad.

   1. Vaya al disco del que se debe hacer una copia de seguridad.

   1. Vaya al **Control de acceso (IAM)** y seleccione **Add role assignments** (Agregar asignaciones de roles).

   1. En el panel de contexto derecho, seleccione **Disk Backup Reader** (Lector de copias de seguridad de discos) en la lista desplegable **Rol**. Seleccione la identidad administrada del almacén de Backup y haga clic en **Guardar**.

   >[!TIP]
   >Escriba el nombre del almacén de Backup para seleccionar la identidad administrada del mismo.

   ![Agregar el rol de lector de copias de seguridad de discos](./media/backup-managed-disks/disk-backup-reader-role.png)

1. Asigne el rol de **Colaborador de instantáneas de discos** a la identidad administrada del almacén de Backup que se encuentra en el grupo de recursos donde el servicio Azure Backup crea y administra las copias de seguridad. Las instantáneas de disco se almacenan en un grupo de recursos de la suscripción. Para permitir que el servicio Azure Backup cree, almacene y administre instantáneas, debe proporcionar permisos al almacén de Backup.

   **Elección del grupo de recursos para almacenar y administrar instantáneas:**

   - No seleccione el mismo grupo de recursos que el del disco de origen.

   - Se recomienda crear un grupo de recursos dedicado como un almacén de datos de instantáneas que usará el servicio Azure Backup. Si tiene un grupo de recursos dedicado, podrá restringir los permisos de acceso en el grupo de recursos para asegurar y administrar fácilmente los datos de copia de seguridad.

   - Puede usar este grupo de recursos para almacenar instantáneas en varios discos que se vayan a incluir (o que tenga planeado incluir) en la copia de seguridad.  

   - No puede crear una instantánea incremental de un disco determinado fuera de la suscripción de ese disco. Por consiguiente, elija el grupo de recursos en la misma suscripción que el disco del que se va a realizar la copia de seguridad. Obtenga más información sobre las [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md#restrictions) de discos administrados.

   Siga estos pasos para asignar un rol:

   1. Vaya al grupo de recursos. Por ejemplo, el grupo de recursos *SnapshotRG* se encuentra en la misma suscripción que el disco del que se va a realizar la copia de seguridad.

   1. Vaya al **Control de acceso (IAM)** y seleccione **Add role assignments** (Agregar asignaciones de roles).

   1. En el panel de contexto derecho, seleccione **Disk Snapshot Contributor** (Colaborador de instantáneas de discos) en la lista desplegable **Rol**. Seleccione la identidad administrada del almacén de Backup y haga clic en **Guardar**.

   >[!TIP]
   >Escriba el nombre del almacén de Backup para seleccionar la identidad administrada del mismo.

   ![Agregar el rol de colaborador de instantáneas de discos](./media/backup-managed-disks/disk-snapshot-contributor-role.png)

1. Compruebe que la identidad administrada del almacén de Backup tiene el conjunto adecuado de asignaciones de roles en el disco de origen y el grupo de recursos que servirán de almacén de datos de instantáneas.

   1. Vaya a **Almacén de Backup-> Identidad** y seleccione **Asignaciones de roles de Azure**.

      ![Selección de la opción Asignaciones de roles de Azure](./media/backup-managed-disks/azure-role-assignments.png)

   1. Compruebe que el rol, el nombre del recurso y el tipo de recurso aparecen correctamente.

      ![Comprobación del rol, el nombre del recurso y el tipo de recurso](./media/backup-managed-disks/verify-role.png)

   >[!NOTE]
   >Aunque las asignaciones de roles se reflejan correctamente en el portal, el permiso puede tardar aproximadamente 15 minutos en aplicarse en la identidad administrada del almacén de Backup.

1. Una vez cumplidos los requisitos previos, vaya a **Almacén de Backup -> Información general** y seleccione **Copia de seguridad** para comenzar a configurar la copia de seguridad de los discos.

   ![Selección de la copia de seguridad](./media/backup-managed-disks/select-backup.png)

1. En la pestaña **Datos básicos**, seleccione **Azure Disk** como el tipo de origen de datos.

   ![Selección de Azure Disk](./media/backup-managed-disks/select-azure-disk.png)

   >[!NOTE]
   >Azure Backup usa [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md#restrictions) de discos administrados, que solo almacenan los cambios diferenciales en el disco desde la última instantánea en almacenamiento HDD estándar, independientemente del tipo de almacenamiento del disco principal. Para aumentar la confiabilidad, las instantáneas incrementales se guardan de forma predeterminada en el almacenamiento con redundancia de zona (ZRS) en aquellas regiones que admiten ZRS. Actualmente, Azure Disk Backup admite la copia de seguridad operativa de discos administrados que no copie las copias de seguridad en el almacenamiento del almacén de Backup. Por lo tanto, la configuración de redundancia del almacenamiento de Backup no se aplica a los puntos de recuperación.

1. En la pestaña **Directivas de copia de seguridad**, elija una directiva de copia de seguridad.

   ![Elegir directiva de copia de seguridad](./media/backup-managed-disks/choose-backup-policy.png)

1. En la pestaña **Recursos**, haga clic en **Seleccionar Azure Disk**. En el panel de contexto derecho, seleccione los discos de los que se va a hacer una copia de seguridad.

   ![Selección de los discos de los que se van a realizar copias de seguridad](./media/backup-managed-disks/select-disks-to-backup.png)

   >[!NOTE]
   >Aunque el portal le permite seleccionar varios discos y configurar la copia de seguridad, tenga en cuenta que cada disco es una instancia de copia de seguridad individual. Actualmente Azure Disk Backup solo admite la copia de seguridad de discos individuales. No se pueden realizar copias de seguridad en un momento dado de varios discos que estén conectados a un disco virtual.
   >
   >Al usar el portal, solo debe seleccionar los discos que estén en la misma suscripción. Si tiene varios discos a los que va a hacer una copia de seguridad o si los discos están en una suscripción diferente, puede usar scripts para automatizar este proceso.
   >
   >Para obtener más información sobre la disponibilidad de la región de Azure Disk Backup, los escenarios admitidos y las limitaciones, consulte la [matriz de compatibilidad](disk-backup-support-matrix.md).

1. Seleccione un **grupo de recursos de instantánea** y haga clic en **Validar**. Este es el grupo de recursos donde se creó el servicio Azure Backup, y se encarga de administrar las instantáneas incrementales del almacén de Backup. La identidad administrada se asigna con los permisos de rol necesarios como parte de los requisitos previos.

   ![Selección de un grupo de recursos de instantáneas](./media/backup-managed-disks/select-snapshot-resource-group.png)

   >[!NOTE]
   >La validación puede tardar unos minutos en completarse y que así pueda completar la configuración del flujo de trabajo de copias de seguridad. Puede producirse un error de validación si:
   >
   > - no se admite un disco. Consulte la [Matriz de compatibilidad](disk-backup-support-matrix.md) para ver los escenarios que no se admiten.
   > - la identidad administrada del almacén de Backup no tiene asignaciones de roles válidas en el **disco** del que se va a realizar una copia de seguridad o en el **grupo de recursos de instantáneas** donde se almacenan las instantáneas incrementales.

1. Después de realizar una validación correcta, seleccione **Revisar y configurar** para configurar la copia de seguridad de los discos seleccionados.

## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición

1. En el **almacén de Backup** denominado *DemoVault* que creó en el paso anterior, vaya a **Instancias de copia de seguridad** y seleccione una instancia de copia de seguridad.

   ![Selección de una instancia de copia de seguridad](./media/backup-managed-disks/select-backup-instance.png)

1. En la pantalla **Instancias de copia de seguridad**, encontrará:

   - **Información esencial**, incluido el nombre del disco de origen, el grupo de recursos de instantáneas donde se almacenan las instantáneas incrementales, el almacén de Backup y la directiva de copia de seguridad.
   - El **estado del trabajo** que muestra un resumen de las operaciones de copia de seguridad y restauración, así como de su estado en los últimos siete días.
   - Una lista de **puntos de restauración** del período de tiempo seleccionado.

1. Seleccione **Realizar copia de seguridad** para realizar el proceso de copias de seguridad a petición.

   ![Seleccionar Realizar copia de seguridad ahora](./media/backup-managed-disks/backup-now.png)

1. Seleccione una de las reglas de retención asociadas a la directiva de copia de seguridad. Esta regla de retención determinará la duración de la retención de la copia de seguridad a petición. Seleccione **Realizar copia de seguridad ahora** para iniciar la copia de seguridad.

   ![Iniciar una copia de seguridad](./media/backup-managed-disks/initiate-backup.png)

## <a name="track-a-backup-operation"></a>Seguimiento de una operación de copia de seguridad

El servicio Azure Backup crea un trabajo para las copias de seguridad programadas o si desencadena una operación de copia de seguridad a petición para su seguimiento. Para ver el estado del trabajo de copia de seguridad:

1. Vaya a la pantalla **Instancia de copia de seguridad**. En ella se muestra el panel de trabajos que cuenta con la operación y el estado de los últimos siete días.

   ![Panel de trabajos](./media/backup-managed-disks/jobs-dashboard.png)

1. Para ver el estado de la operación de copia de seguridad, seleccione **Ver todos** para mostrar los trabajos en curso y pasados de esta instancia de copia de seguridad.

   ![Selección de la opción Ver todo](./media/backup-managed-disks/view-all.png)

1. Revise la lista de trabajos de copia de seguridad y restauración, así como su estado. Seleccione un trabajo de la lista de trabajos para ver los detalles del mismo.

   ![Selección del trabajo para ver los detalles](./media/backup-managed-disks/select-job.png)

## <a name="next-steps"></a>Pasos siguientes

- [Restauración de Azure Managed Disks](restore-managed-disks.md)