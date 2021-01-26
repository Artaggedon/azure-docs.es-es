---
title: Novedades de Azure Backup
description: Obtenga información acerca de las nuevas características de Azure Backup.
ms.topic: conceptual
ms.date: 11/11/2020
ms.openlocfilehash: 62a6146990863c339917777b2624fee76ebe60d8
ms.sourcegitcommit: 9d9221ba4bfdf8d8294cf56e12344ed05be82843
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2021
ms.locfileid: "98569426"
---
# <a name="whats-new-in-azure-backup"></a>Novedades de Azure Backup

Azure Backup constantemente recibe mejoras y nuevas características que mejoran la protección de los datos en Azure. Estas nuevas características amplían la protección de datos a nuevos tipos de cargas de trabajo, mejoran la seguridad y también la disponibilidad de los datos de copia de seguridad. Además, agregan nuevas funcionalidades de administración, supervisión y automatización.

Para obtener más información acerca de las nuevas versiones, puede marcar esta página o [suscribirse a las actualizaciones con este vínculo](https://azure.microsoft.com/updates/?query=backup).

## <a name="updates-summary"></a>Resumen de actualizaciones

- Enero de 2021
  - [Azure Disk Backup (en versión preliminar)](disk-backup-overview.md)
  - [El cifrado en reposo mediante claves administradas por el cliente ahora está disponible con carácter general](encryption-at-rest-with-cmk.md)
- Noviembre de 2020
  - [Plantilla de Azure Resource Manager para la copia de seguridad de recursos compartido de archivos de Azure (AFS)](#azure-resource-manager-template-for-afs-backup)
  - [Copias de seguridad incrementales para bases de datos de SAP HANA en máquinas virtuales de Azure](#incremental-backups-for-sap-hana-databases)
- Septiembre de 2020
  - [Centro de copia de seguridad](#backup-center)
  - [Copia de seguridad de Azure Database for PostgreSQL](#backup-azure-database-for-postgresql)
  - [Copia de seguridad y restauración selectivas de discos](#selective-disk-backup-and-restore)
  - [Restauración entre regiones de bases de datos SQL Server y SAP HANA en máquinas virtuales de Azure](#cross-region-restore-for-sql-server-and-sap-hana)
  - [Compatibilidad de la copia de seguridad de máquinas virtuales con hasta 32 discos](#support-for-backup-of-vms-with-up-to-32-disks)
  - [Experiencia de configuración de copia de seguridad simplificada para SQL en las máquinas virtuales de Azure](#simpler-backup-configuration-for-sql-in-azure-vms)
  - [Copia de seguridad de SAP HANA en máquinas virtuales de Azure de RHEL](#backup-sap-hana-in-rhel-azure-virtual-machines)
  - [Almacenamiento con redundancia de zona para datos de copia de seguridad](#zone-redundant-storage-zrs-for-backup-data)
  - [Eliminación temporal de las cargas de trabajo de SQL Server y SAP HANA en máquinas virtuales de Azure](#soft-delete-for-sql-server-and-sap-hana-workloads)

## <a name="azure-disk-backup-in-preview"></a>Azure Disk Backup (en versión preliminar)

Azure Disk Backup ofrece una solución inmediata que le proporciona la oportunidad de administrar el ciclo de vida de las instantáneas para [Azure Managed Disks](https://docs.microsoft.com/azure/virtual-machines/managed-disks-overview); para ello, solo debe automatizar la creación periódica de instantáneas y guardarlas durante la duración configurada mediante la directiva de copia de seguridad. Puede administrar las instantáneas de disco sin costos de infraestructura, sin necesidad de realizar un scripting personalizado y sin sufrir ninguna sobrecarga de administración. Esta solución de copia de seguridad coherente con los bloqueos realiza en un momento dado una copia de seguridad de un disco administrado mediante [instantáneas incrementales](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots), y admite la realización de varias copias de seguridad al día. También es una solución sin agente que no afecta al rendimiento de las aplicaciones de producción. Igualmente, admite las copias de seguridad y la restauración de los discos de datos y del sistema operativo (incluidos los discos compartidos), tanto si están conectados actualmente a una máquina virtual de Azure en ejecución como si no.

Para más información, consulte [Azure Disk Backup (en versión preliminar)](disk-backup-overview.md).

## <a name="encryption-at-rest-using-customer-managed-keys"></a>Cifrado en reposo con claves administradas por el cliente

La compatibilidad con el cifrado en reposo mediante claves administradas por el cliente ahora está disponible con carácter general. Esto le ofrece la capacidad de cifrar los datos de copia de seguridad en los almacenes de Recovery Services con sus propias claves almacenadas en Azure Key Vault. La clave de cifrado utilizada para cifrar las copias de seguridad en el almacén de Recovery Services puede ser diferente de las que se usan para cifrar el origen. Los datos se protegen mediante una clave de cifrado de datos (DEK) basada en AES 256, que, a su vez, está protegida con las claves del usuario almacenadas en Key Vault. En comparación con el cifrado mediante claves administradas por la plataforma (que está disponible de manera predeterminada), esto le proporciona más control sobre las claves y puede ayudarlo a satisfacer mejor sus necesidades de cumplimiento.

Para más información, consulte [Cifrado de datos de copia de seguridad mediante claves administradas por el cliente](encryption-at-rest-with-cmk.md).

## <a name="azure-resource-manager-template-for-afs-backup"></a>Plantilla de Azure Resource Manager para la copia de seguridad de AFS

Azure Backup permite ahora configurar la copia de seguridad de recursos compartidos de archivos de Azure existentes mediante una plantilla de Azure Resource Manager (ARM). La plantilla configura la protección para un recurso compartido de archivos de Azure existente al especificar los detalles correspondientes para la directiva de copia de seguridad y el almacén de Recovery Services. Opcionalmente, crea una directiva de copia de seguridad y un almacén de Recovery Services nuevos, y registra la cuenta de almacenamiento que contiene el recurso compartido de archivos en el almacén de Recovery Services.

Para más información, consulte [Plantillas de Azure Resource Manager para Azure Backup](backup-rm-template-samples.md).

## <a name="incremental-backups-for-sap-hana-databases"></a>Copias de seguridad incrementales para bases de datos SAP HANA

Azure Backup ahora admite las copias de seguridad incrementales para las bases de datos SAP HANA hospedadas en máquinas virtuales de Azure. Esto permite realizar copias de seguridad más rápidas y rentables de sus datos de SAP HANA.

Para obtener más información, consulte las [distintas opciones disponibles durante la creación de una directiva de copia de seguridad](sap-hana-faq-backup-azure-vm.md#policy) y [cómo crear una directiva de copia de seguridad para las bases de datos SAP HANA](tutorial-backup-sap-hana-db.md#creating-a-backup-policy).

## <a name="backup-center"></a>Centro de copia de seguridad

Azure Backup ha habilitado una nueva funcionalidad de administración nativa para todo el estado de la copia de seguridad desde una consola central. El centro de copias de seguridad proporciona la capacidad de supervisar, operar, administrar y optimizar la protección de datos a escala de manera unificada con las experiencias de administración nativas de Azure.

Con el centro de copias de seguridad, obtiene una vista agregada del inventario entre suscripciones, ubicaciones, grupos de recursos, almacenes e incluso inquilinos con Azure Lighthouse. Además, dicho centro es también un centro de actividades desde el que puede desencadenar actividades relacionadas con la copia de seguridad, como la configuración y restauración de copias de seguridad y la creación de directivas o almacenes, todo desde un único lugar. Asimismo, con la integración sin problemas con Azure Policy, ahora puede controlar el entorno y realizar un seguimiento del cumplimiento desde la perspectiva de una copia de seguridad. Las directivas integradas de Azure específicas para Azure Backup también permiten configurar las copias de seguridad a escala.

Para obtener más información, consulte [Información general sobre el centro de copias de seguridad](backup-center-overview.md).

## <a name="backup-azure-database-for-postgresql"></a>Copia de seguridad de Azure Database for PostgreSQL

Azure Backup y los servicios de base de datos de Azure se han unido para crear una solución de copia de seguridad de clase empresarial para Azure PostgreSQL (actualmente en versión preliminar). Ahora puede satisfacer sus necesidades de protección de datos y cumplimiento con una directiva de copia de seguridad controlada por el cliente que permita la retención de copias de seguridad durante un máximo de 10 años. Con ella, tiene un control detallado para administrar las operaciones de creación y restauración de copias de seguridad en el nivel de base de datos individual. Del mismo modo, puede realizar restauraciones a través de las versiones de PostgreSQL o Blob Storage fácilmente.

Para más información, consulte el artículo [Copia de seguridad de Azure Database for PostgreSQL](backup-azure-database-postgresql.md).

## <a name="selective-disk-backup-and-restore"></a>Copia de seguridad y restauración selectivas de discos

Azure Backup admite la copia de seguridad de todos los discos (sistema operativo y datos) en una máquina virtual junto con la solución de copia de seguridad de máquinas virtuales. Ahora, con la funcionalidad de copia de seguridad y restauración selectivas de discos, puede realizar una copia de seguridad de un subconjunto de los discos de datos de una máquina virtual. Esto proporciona una solución eficaz y rentable para sus necesidades de copia de seguridad y restauración. Cada punto de recuperación contiene solo los discos que se incluyen en la operación de copia de seguridad.

Para obtener más información, consulte [Copias de seguridad y restauración selectivas de discos para máquinas virtuales de Azure](selective-disk-backup-restore.md).

## <a name="cross-region-restore-for-sql-server-and-sap-hana"></a>Restauración entre regiones para SQL Server y SAP HANA

Con la introducción de la restauración entre regiones, ahora puede iniciar las restauraciones en una región secundaria a fin de mitigar los problemas de tiempo de inactividad reales en una región primaria de su entorno. Esto hace que las restauraciones de la región secundaria las controle por completo el cliente. Azure Backup usa los datos de copia de seguridad replicados en la región secundaria para dichas restauraciones.

Ahora, además de admitir la restauración entre regiones para las máquinas virtuales de Azure, la característica se ha ampliado a la restauración de bases de datos SQL y SAP HANA también en máquinas virtuales de Azure.

Para obtener más información, consulte [Restauración entre regiones para bases de datos SQL](restore-sql-database-azure-vm.md#cross-region-restore) y [Restauración entre regiones para bases de datos SAP HANA](sap-hana-db-restore.md#cross-region-restore).

## <a name="support-for-backup-of-vms-with-up-to-32-disks"></a>Compatibilidad de la copia de seguridad de máquinas virtuales con hasta 32 discos

Hasta ahora, Azure Backup ha admitido 16 discos administrados por cada máquina virtual. Ahora, Azure Backup admite la copia de seguridad de hasta 32 de discos administrados por cada máquina virtual.

Para obtener más información, consulte la matriz de [compatibilidad con almacenamiento de máquina virtual](backup-support-matrix-iaas.md#vm-storage-support).

## <a name="simpler-backup-configuration-for-sql-in-azure-vms"></a>Configuración de copia de seguridad más sencilla para SQL en las máquinas virtuales de Azure

La configuración de las copias de seguridad para SQL Server en las máquinas virtuales de Azure es aún más fácil gracias a la configuración de copia de seguridad en línea integrada en el panel de la máquina virtual de Azure Portal. En unos pocos pasos, puede habilitar la copia de seguridad de SQL Server para proteger todas las bases de datos actuales, así como las que se agregarán en el futuro.

Para obtener más información, consulte [Copia de seguridad de una instancia de SQL Server desde el panel de máquina virtual](backup-sql-server-vm-from-vm-pane.md).

## <a name="backup-sap-hana-in-rhel-azure-virtual-machines"></a>Copia de seguridad de SAP HANA en máquinas virtuales de Azure de RHEL

Azure Backup es la solución de copias de seguridad nativa de Azure y tiene la certificación BackInt de SAP. Azure Backup ha agregado compatibilidad con Red Hat Enterprise Linux (RHEL), uno de los sistemas operativos Linux más usados que ejecutan SAP HANA.

Para obtener más información, consulte la matriz de [compatibilidad de escenarios para copia de seguridad de bases de datos SAP HANA](sap-hana-backup-support-matrix.md#scenario-support).

## <a name="zone-redundant-storage-zrs-for-backup-data"></a>Almacenamiento con redundancia de zona para datos de copia de seguridad

Azure Storage proporciona un equilibrio fantástico entre alto rendimiento, alta disponibilidad y alta resistencia de datos con sus variadas opciones de redundancia. Azure Backup le permite ampliar también estas ventajas a los datos de copias de seguridad, con opciones para almacenar las copias de seguridad en almacenamiento con redundancia local (LRS) y almacenamiento con redundancia geográfica (GRS). Ahora, hay opciones de durabilidad adicionales con la compatibilidad agregada para el almacenamiento con redundancia de zona (ZRS).

Para obtener más información, consulte [Establecimiento de la redundancia de almacenamiento para el almacén de Recovery Services](backup-create-rs-vault.md#set-storage-redundancy).

## <a name="soft-delete-for-sql-server-and-sap-hana-workloads"></a>Eliminación temporal de las cargas de trabajo de SQL Server y SAP HANA

Cada vez es mayor la preocupación que generan problemas de seguridad como malware, ransomware e intrusión. Estos problemas de seguridad pueden ser costosos, en términos de dinero y datos. Para protegerse contra dichos ataques, Azure Backup proporciona características de seguridad que protegen los datos de las copias de seguridad incluso después de su eliminación.

Una de estas características es la eliminación temporal. Con la eliminación temporal, aunque un actor malintencionado elimine una copia de seguridad (o se eliminen por accidente datos de copia de seguridad), los datos de copia de seguridad se conservan durante 14 días adicionales, lo que permite la recuperación de ese elemento de copia de seguridad sin pérdida de datos. La retención adicional de 14 días de los datos de copia de seguridad con el estado de "eliminación temporal" no tiene costo alguno para el cliente.

Ahora, además de la compatibilidad con la eliminación temporal para las máquinas virtuales de Azure, las cargas de trabajo de SQL Server y SAP HANA en las máquinas virtuales de Azure también están protegidas a través de la eliminación temporal.

Para obtener más información, consulte [Eliminación temporal de servidores SQL Server en máquinas virtuales de Azure e instancias de SAP HANA en cargas de trabajo de máquinas virtuales de Azure](soft-delete-sql-saphana-in-azure-vm.md).

## <a name="next-steps"></a>Pasos siguientes

- [Guía y procedimientos recomendados de Azure Backup](guidance-best-practices.md)
