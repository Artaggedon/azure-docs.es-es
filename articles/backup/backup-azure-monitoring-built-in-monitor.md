---
title: Supervisión de cargas de trabajo protegidas de Azure Backup
description: En este artículo se obtiene información sobre las funcionalidades de supervisión y notificación de las cargas de trabajo de Azure Backup mediante Azure Portal.
ms.topic: conceptual
ms.date: 03/05/2019
ms.assetid: 86ebeb03-f5fa-4794-8a5f-aa5cbbf68a81
ms.openlocfilehash: 83ed5af00bb61d7a8929e710b52e60c33c0f479b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105559220"
---
# <a name="monitoring-azure-backup-workloads"></a>Supervisión de cargas de trabajo de Azure Backup

Azure Backup proporciona varias soluciones de copia de seguridad según los requisitos de copia de seguridad y la topología de la infraestructura (local o en Azure). Cualquier usuario o administrador de copias de seguridad debería poder ver lo que está ocurriendo en todas las soluciones y esperar recibir notificaciones en los escenarios importantes. En este artículo se detallan las capacidades de supervisión y notificación que presta el servicio de Azure Backup.

[!INCLUDE [backup-center.md](../../includes/backup-center.md)]

## <a name="backup-items-in-recovery-services-vault"></a>Elementos de copia de seguridad en el almacén de Recovery Services

Puede supervisar todos los elementos de copia de seguridad por medio de un almacén de Recovery Services. Al ir a la sección **Elementos de copia de seguridad** del almacén se abre una vista que proporciona el número de elementos de copia de seguridad de cada tipo de carga de trabajo asociado al almacén. Al hacer clic en cualquier fila se abre una vista detallada que enumera todos los elementos de copia de seguridad del tipo de carga de trabajo dado, con información sobre el último estado de copia de seguridad de cada elemento, el último punto de restauración disponible, etc.

![Elementos de copia de seguridad del almacén de Recovery Services](media/backup-azure-monitoring-laworkspace/backup-items-view.png)

> [!NOTE]
> En el caso de los elementos cuya copia de seguridad en Azure se ha efectuado mediante DPM, la lista muestra todos los orígenes de datos protegidos (tanto en disco como en línea) mediante el servidor de DPM. Si se detiene la protección del origen de datos con datos de copia de seguridad conservados, el origen de datos sigue apareciendo en el portal. Puede ir a los detalles del origen de datos para ver si los puntos de recuperación están presentes en disco, en línea o ambos. Además, en los orígenes de datos cuya protección en línea se ha detenido pero cuyos datos se conservan, la facturación de los puntos de recuperación en línea continúa hasta que los datos se eliminan por completo.
>
> La versión de DPM debe ser DPM 1807 (5.1.378.0) o DPM 2019 (versión 10.19.58.0 o superior) para que los elementos de copia de seguridad sean visibles en el portal del almacén de Recovery Services.

## <a name="backup-jobs-in-recovery-services-vault"></a>Trabajos de copia de seguridad en el almacén de Recovery Services

Azure Backup proporciona capacidades de supervisión y notificación relativas a las cargas de trabajo bajo protección de Azure Backup. En la sección **Supervisión** de la configuración del almacén de Recovery Services encontrará trabajos y alertas integrados.

![Almacén de Recovery Services frente a supervisión integrada](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltmonitoring.png)

Los trabajos se generan cuando se realizan operaciones como configurar copias de seguridad, hacer copias de seguridad, restaurar copias de seguridad, eliminar copias de seguridad, etc.

Aquí se muestran los trabajos de las siguientes soluciones de Azure Backup:

- Copia de seguridad de máquina virtual de Azure
- Copia de seguridad de archivos de Azure
- Copia de seguridad de cargas de trabajo de Azure, como SQL y SAP HANA
- Agente de Microsoft Azure Recovery Services (MARS)

Los trabajos procedentes de System Center Data Protection Manager (SC-DPM) y Microsoft Azure Backup Server (MABS) no se muestran.

> [!NOTE]
> Las cargas de trabajo de Azure, como las copias de seguridad de SQL y SAP HANA en VM de Azure, tienen gran número de trabajos de copia de seguridad. Por ejemplo, se pueden ejecutar copias de seguridad de los registros cada 15 minutos. Por lo tanto, en el caso de estas cargas de trabajo de base de datos, solamente se muestran las operaciones desencadenadas por el usuario. No se muestran las operaciones de copia de seguridad programadas.

## <a name="backup-alerts-in-recovery-services-vault"></a>Supervisión de alertas de copia de seguridad en el almacén de Recovery Services

> [!NOTE]
> Actualmente no se admite la visualización de alertas en almacenes en el Centro de copia de seguridad. Debe ir a un almacén individual para ver las alertas de ese almacén.

Las alertas son, fundamentalmente, los escenarios donde los usuarios reciben notificaciones para que puedan tomar las medidas oportunas. La sección **Alertas de copia de seguridad** muestra las alertas generadas por el servicio de Azure Backup. Estas alertas las define el servicio; es decir, el usuario no puede crear ninguna alerta personalizada.

### <a name="alert-scenarios"></a>Escenarios de alerta

El servicio define los siguientes escenarios como escenarios susceptibles de generar alertas.

- Errores de copia de seguridad y restauración
- Copia de seguridad correcta con advertencias para el agente de Microsoft Azure Recovery Services (MARS)
- Detener la protección con conservación de datos/Detener la protección con eliminación de datos

### <a name="alerts-from-the-following-azure-backup-solutions-are-shown-here"></a>Aquí se muestran las alertas de las siguientes soluciones de Azure Backup

- Copias de seguridad de máquinas virtuales de Azure
- Copias de seguridad de archivos de Azure
- Copias de seguridad de cargas de trabajo de Azure, como SQL y SAP HANA
- Agente de Microsoft Azure Recovery Services (MARS)

> [!NOTE]
> Las alertas procedentes de System Center Data Protection Manager (SC-DPM) y Microsoft Azure Backup Server (MABS) no se muestran aquí.

### <a name="consolidated-alerts"></a>Alertas consolidadas

En el caso de soluciones de copia de seguridad de la carga de trabajo de Azure, como SQL y SAP HANA, las copias de seguridad de registros se pueden generar con mucha frecuencia (hasta cada 15 minutos, según la directiva). Por lo tanto, también es posible que los errores de copia de seguridad de registros sean muy frecuentes (hasta cada 15 minutos). En este escenario, el usuario final puede verse abrumado si se genera una alerta para cada caso de error. Así, se envía una alerta para el primer caso y, si los errores posteriores se deben a la misma causa principal, no se generan más alertas. La primera alerta se actualiza con el recuento de errores. Pero, si el usuario la desactiva, el siguiente caso desencadenará otra alerta y se tratará como la primera alerta de ese caso. Así es cómo Azure Backup realiza la consolidación de alertas para las copias de seguridad de SQL y SAP HANA.

### <a name="exceptions-when-an-alert-is-not-raised"></a>Excepciones cuando una alerta no se genera

Hay algunas excepciones en las que no se genera una alerta en caso de error. Son las siguientes:

- Un usuario ha cancelado expresamente el trabajo en ejecución.
- Se ha producido un error en el trabajo porque otro trabajo de copia de seguridad está en curso (no hay nada que se pueda hacer, solo hay que esperar a que el trabajo anterior finalice).
- Se ha producido un error en el trabajo de copia de seguridad de máquina virtual porque la máquina virtual de Azure de la que se está haciendo la copia de seguridad ya no existe.
- [Alertas consolidadas](#consolidated-alerts)

Las excepciones están diseñadas bajo el supuesto de que el resultado de estas operaciones (desencadenadas principalmente por el usuario) se muestra inmediatamente en los clientes de Portal/PowerShell/CLI. Por lo tanto, el usuario es consciente de ello de inmediato y no necesita que se le notifique.

### <a name="alert-types"></a>Tipos de alerta

Se pueden definir tres tipos alertas, según la gravedad:

- **Crítico**: en principio, cualquier error en una copia de seguridad o una recuperación (sea programada o desencadenada por el usuario) daría lugar a la generación de una alerta, que se mostraría como una alerta crítica junto con operaciones de destrucción como eliminar la copia de seguridad.
- **Advertencia**: si la operación de copia de seguridad se realiza correctamente, pero con algunas advertencias, estas se muestran como alertas de advertencia. Las alertas de advertencia solo están disponibles actualmente para las copias de seguridad del agente de Azure Backup.
- **Informativo**: actualmente, el servicio de Azure Backup aún no ha generado ninguna alerta informativa.

## <a name="notification-for-backup-alerts"></a>Notificación de alertas de copia de seguridad

> [!NOTE]
> La configuración de notificaciones solo se puede realizar a través de Azure Portal. No se admite PowerShell, la CLI, la API de REST ni la plantilla de Azure Resource Manager.

Después de que se genere una alerta, se notifica a los usuarios. Azure Backup proporciona un mecanismo de notificación integrado por correo electrónico. Se pueden especificar direcciones de correo electrónico individuales o listas de distribución para recibir una notificación cuando una alerta se genere. También puede optar por recibir una notificación con cada alerta individual, o bien agruparlas en un resumen cada hora y, luego, recibir la notificación correspondiente.

![Notificación por correo electrónico integrada del almacén de Recovery Services](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltnotification.png)

Cuando configure una notificación, recibirá un correo electrónico de bienvenida o de introducción. De esta forma, se confirma que Azure Backup puede enviar mensajes de correo electrónico a estas direcciones cuando se genere una alerta.<br>

Si la frecuencia se ha establecido en un resumen cada hora y una alerta se genera y resuelve en el plazo de una hora, dicha alerta no formará parte del próximo resumen de cada hora.

> [!NOTE]
>
> - Si se lleva a cabo una operación de destrucción, como la **detención de la protección con eliminación de datos**, se generará una alerta y se enviará un correo electrónico a los propietarios de la suscripción, los administradores y los coadministradores, aun cuando las notificaciones no estén configuradas en el almacén de Recovery Services.
> - Para configurar notificaciones que informen de los trabajos realizados correctamente, use [Log Analytics](backup-azure-monitoring-use-azuremonitor.md#using-log-analytics-workspace).

## <a name="inactivating-alerts"></a>Desactivación de alertas

Para desactivar o resolver una alerta activa, puede seleccionar el elemento de lista correspondiente a la alerta que desea desactivar. Se abre una pantalla en la que se muestra información detallada sobre la alerta, con un botón **Desactivar** en la parte superior. Al seleccionar este botón, el estado de la alerta cambiará a **Inactivo**. También puede desactivar una alerta si hace clic con el botón derecho en el elemento de lista correspondiente a esa alerta y selecciona **Desactivar**.

![Inactivación de alertas de un depósito de RS](media/backup-azure-monitoring-laworkspace/vault-alert-inactivation.png)

## <a name="azure-monitor-alerts-for-azure-backup-preview"></a>Alertas de Azure Monitor para Azure Backup (versión preliminar)

Azure Backup también proporciona alertas a través de Azure Monitor, a fin de permitir que los usuarios tengan una experiencia coherente de administración de alertas en distintos servicios de Azure, incluida la copia de seguridad. Con las alertas de Azure Monitor, puede enrutar las alertas a cualquier canal de notificación que Azure Backup admita, como el correo electrónico, ITSM, un webhook, una aplicación lógica, etc.

Actualmente, esta característica está disponible para las instancias de servidor de Azure Database for PostgreSQL, blobs de Azure y Azure Managed Disks. Las alertas se generan para los siguientes escenarios y, para acceder a estas, puede navegar a un almacén de Backup y hacer clic en el elemento de menú **Alertas**:

- Eliminación de datos de copia de seguridad
- Error de Backup (para obtener alertas de error de Backup, debe registrar la marca AFEC denominada **EnableAzureBackupJobFailureAlertsToAzureMonitor** a través del Portal de vista previa).
- Error de restauración (para obtener alertas de error de restauración, debe registrar la marca AFEC denominada **EnableAzureBackupJobFailureAlertsToAzureMonitor** a través del Portal de vista previa).

Para más información sobre las alertas de Azure Monitor, consulte [Información general sobre las alertas en Microsoft Azure](../azure-monitor/alerts/alerts-overview.md).

## <a name="next-steps"></a>Pasos siguientes

[Supervisión de cargas de trabajo de Azure Backup mediante Azure Monitor](backup-azure-monitoring-use-azuremonitor.md)