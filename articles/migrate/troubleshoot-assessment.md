---
title: Solución de problemas de evaluación y visualización de dependencias en Azure Migrate
description: Obtenga ayuda para solucionar problemas de evaluación y visualización de dependencias en Azure Migrate.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: troubleshooting
ms.date: 01/02/2020
ms.openlocfilehash: 78e54543c94cd6e8434023b61516242c2491f353
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104863605"
---
# <a name="troubleshoot-assessmentdependency-visualization"></a>Solución de problemas de evaluación o visualización de dependencias

Este artículo incluye ayuda para la solución de problemas de evaluación y visualización de dependencias con [Azure Migrate: Detección y evaluación](migrate-services-overview.md#azure-migrate-discovery-and-assessment-tool).


## <a name="assessment-readiness-issues"></a>Problemas de preparación de la evaluación

Revise los problemas de preparación de la evaluación como se indica a continuación:

**Problema** | **Revisión**
--- | ---
Tipo de arranque no compatible | Azure no admite máquinas virtuales con el tipo de arranque EFI. Se recomienda convertir el tipo de arranque a BIOS antes de ejecutar una migración. <br/><br/>Puede usar Azure Migrate Server Migration para controlar la migración de estas máquinas virtuales. Durante la migración, el tipo de arranque de la máquina virtual se convertirá a BIOS.
Sistemas operativos Windows condicionalmente compatibles | El sistema operativo alcanzó la fecha de finalización del soporte técnico y necesita un contrato de soporte técnico personalizado (CSA) para [recibir soporte técnico en Azure](/troubleshoot/azure/virtual-machines/server-software-support). Considere la posibilidad de actualizar antes de migrar a Azure. Revise la información sobre la [preparación de los servidores que ejecutan Windows Server 2003](prepare-windows-server-2003-migration.md) para la migración a Azure.
Sistemas operativos Windows no compatibles | Azure solo admite [versiones del sistema operativo Windows seleccionadas](/troubleshoot/azure/virtual-machines/server-software-support). Considere la posibilidad de actualizar el servidor antes de migrar a Azure.
Sistema operativo Linux aprobado con condiciones | Azure aprueba solo [versiones del sistema operativo Linux seleccionadas](../virtual-machines/linux/endorsed-distros.md). Considere la posibilidad de actualizar el servidor antes de migrar a Azure. Haga clic [aquí](#linux-vms-are-conditionally-ready-in-an-azure-vm-assessment) para consultar más detalles.
Sistema operativo Linux no aprobado | El servidor puede iniciarse en Azure, pero Azure no proporciona compatibilidad con el sistema operativo. Considere la actualización a una [versión de Linux aprobada](../virtual-machines/linux/endorsed-distros.md) antes de migrar a Azure.
Sistema operativo desconocido | El sistema operativo de la máquina virtual se especificó como "Otros" en vCenter Server. Este comportamiento impide que Azure Migrate compruebe la preparación para Azure de la máquina virtual. Asegúrese de que Azure [admite](./migrate-support-matrix-vmware-migration.md#azure-vm-requirements) el sistema operativo antes de migrar el servidor.
Versión de bits no admitida | Las máquinas virtuales con un sistema operativo de 32 bits pueden arrancar en Azure, pero se recomienda que actualice a 64 bits antes de migrar a Azure.
Requiere una suscripción de Microsoft Visual Studio | En el servidor se ejecuta un sistema operativo cliente Windows, que solo se admite mediante una suscripción de Visual Studio.
No se encontró ninguna máquina virtual para el rendimiento de almacenamiento requerido | El rendimiento del almacenamiento (operaciones de entrada/salida por segundo [IOPS] y rendimiento) necesario para el servidor supera la compatibilidad con máquinas virtuales de Azure. Reduzca los requisitos de almacenamiento del servidor antes de realizar la migración.
No se encontró ninguna máquina virtual para el rendimiento de red requerido | El rendimiento de red (entrada/salida) requerido para el servidor excede el soporte técnico de máquina virtual de Azure. Reduzca los requisitos de red del servidor.
No se encontró ninguna máquina virtual en la ubicación especificada | Utilice una ubicación de destino diferente antes de la migración.
Uno o varios discos no son adecuados | Uno o varios discos conectados a la máquina virtual no cumplen los requisitos de Azure.<br/><br/> Azure Migrate: Detección y evaluación no admite actualmente discos Ultra SSD y evalúa los discos en función de los límites de disco de los discos administrados Premium (32 TB).<br/><br/> Para cada disco conectado a la máquina virtual, asegúrese de que el tamaño del disco es menor que 64 TB (compatible con discos SSD Ultra).<br/><br/> Si no lo es, reduzca el tamaño de los discos antes de la migración o bien use varios discos en Azure y [colóquelos juntos](../virtual-machines/premium-storage-performance.md#disk-striping) para obtener mayores límites de almacenamiento. Asegúrese de que el rendimiento (IOPS y rendimiento) necesario para cada disco sea compatible con los [discos de máquinas virtuales administradas](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits) de Azure.
Uno o varios adaptadores de red no son adecuados | Quite los adaptadores de red no utilizados del servidor antes de realizar la migración.
El recuento del disco supera el límite | Quite los discos no utilizados del servidor antes de realizar la migración.
El tamaño del disco supera el límite | Azure Migrate: Detección y evaluación no admite actualmente discos Ultra SSD y evalúa los discos en función de los límites de los discos Premium (32 TB).<br/><br/> Sin embargo, Azure admite discos con un tamaño de hasta 64 TB (compatible con discos SSD Ultra). Reduzca los discos a menos de 64 TB antes de la migración o use varios discos en Azure y [colóquelos juntos](../virtual-machines/premium-storage-performance.md#disk-striping) para obtener mayores límites de almacenamiento.
El disco no está disponible en la ubicación especificada | Asegúrese de que el disco está en la ubicación de destino antes de realizar la migración.
El disco no está disponible para la redundancia especificada | El disco debe usar el tipo de almacenamiento de redundancia definido en la configuración de evaluación (LRS de forma predeterminada).
No se pudo determinar la idoneidad del disco debido a un error interno | Intente crear una evaluación para el grupo.
No se encontró ninguna máquina virtual con los núcleos y memoria requeridos | Azure no pudo encontrar un tipo de máquina virtual adecuado. Reduzca la memoria y el número de núcleos del servidor local antes de realizar la migración.
No se pudo determinar la idoneidad de la máquina virtual debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios discos debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios adaptadores de red debido a un error interno | Intente crear una evaluación para el grupo.
No se encontró ningún tamaño de máquina virtual para la instancia reservada de moneda de oferta. | Servidor marcado como no válido porque no se encontró el tamaño de la máquina virtual para la combinación de RI, oferta y moneda seleccionada. Edite las propiedades de valoración para elegir las combinaciones válidas y recalcular la valoración. 
Protocolo de Internet preparado condicionalmente | Solo se aplica a las valoraciones de Azure VMware Solution (AVS). AVS no es compatible con el factor de direcciones IPv6 de Internet. Póngase en contacto con el equipo de AVS para obtener instrucciones de corrección el servidor se detecta con IPv6.

## <a name="suggested-migration-tool-in-import-based-avs-assessment-marked-as-unknown"></a>La herramienta de migración sugerida en la evaluación de AVS basada en la importación está marcada como desconocida.

En el caso de los servidores importados mediante un archivo CSV, se desconoce la herramienta de migración predeterminada en una evaluación de AVS. Sin embargo, para los servidores del entorno de VMware, se recomienda usar la solución Hybrid Cloud Extension (HCX) de VMware. [Más información](../azure-vmware/tutorial-deploy-vmware-hcx.md).

## <a name="linux-vms-are-conditionally-ready-in-an-azure-vm-assessment"></a>Las máquinas virtuales Linux están "condicionalmente preparadas" en una valoración de máquinas virtuales de Azure.

En el caso de las máquinas virtuales de VMware e Hyper-V, la evaluación de máquinas virtuales de Azure marca las máquinas virtuales Linux como "condicionalmente preparadas" debido a una brecha conocida. 

- La brecha evita que detecte la versión secundaria del sistema operativo Linux instalada en las máquinas virtuales locales.
- Por ejemplo, para RHEL 6.10, la evaluación de máquinas virtuales de Azure actualmente solo detecta RHEL 6 como la versión del sistema operativo. Esto se debe a que el vCenter Server y el host de Hyper-V no proporcionan la versión de kernel para los sistemas operativos de máquinas virtuales Linux.
-  Puesto que Azure solo aprueba versiones específicas de Linux, las máquinas virtuales Linux actualmente se marcan como listas con condiciones en la evaluación de máquinas virtuales de Azure.
- Puede determinar si el sistema operativo Linux en ejecución en la máquina virtual local está aprobado en Azure; para ello, consulte [Soporte técnico de Linux en Azure](../virtual-machines/linux/endorsed-distros.md).
-  Una vez haya comprobado que la distribución está aprobada, puede omitir esta advertencia.

Este intervalo se puede solucionar habilitando [la detección de aplicaciones](./how-to-discover-applications.md) en las máquinas virtuales de VMware. La evaluación de máquinas virtuales de Azure usa el sistema operativo detectado de la máquina virtual con las credenciales de invitado proporcionadas. Estos datos del sistema operativo identifican la información del sistema operativo adecuada en el caso de las máquinas virtuales Windows y Linux.

## <a name="operating-system-version-not-available"></a>La versión del sistema operativo no está disponible

En el caso de los servidores físicos, la información de la versión secundaria del sistema operativo debe estar disponible. Si no es así, póngase en contacto con el soporte técnico de Microsoft. En el caso de servidores de entornos de VMware, Azure Migrate usa la información del sistema operativo especificada para la máquina virtual en vCenter Server. Sin embargo, vCenter Server no proporciona la versión secundaria para los sistemas operativos. Para detectar la versión secundaria, debe configurar la [detección de aplicaciones](./how-to-discover-applications.md). En el caso de las máquinas virtuales de Hyper-V, no se admite la detección de versiones secundarias del sistema operativo. 

## <a name="azure-skus-bigger-than-on-premises-in-an-azure-vm-assessment"></a>Las SKU de Azure son más grandes que las locales en una valoración de máquinas virtuales de Azure.

La evaluación de máquinas virtuales de Azure podría recomendar las SKU de máquinas virtuales de Azure con más núcleos y memoria que la asignación local actual según el tipo de evaluación:

- La recomendación de las SKU de máquinas virtuales depende de las propiedades de la evaluación.
- Esto se ve afectado por el tipo de evaluación que realiza en la evaluación de máquinas virtuales de Azure: *Basado en el rendimiento* y *Como local*.
- Para obtener evaluaciones "en función del rendimiento", la evaluación de máquinas virtuales de Azure tiene en cuenta los datos de uso de las máquinas virtuales en el entorno local (uso de CPU, memoria, disco y red) para determinar la SKU de máquina virtual de destino correcta para las máquinas virtuales en el entorno local. También agrega un factor de confort al determinar el uso efectivo.
- Para el ajuste de tamaño en la opción como en el entorno local, no se tienen en cuenta los datos de rendimiento y se recomienda una SKU de destino en función de la asignación local.

Para mostrar cómo esto puede afectar a las recomendaciones, veamos un ejemplo:

Tenemos una máquina virtual local con cuatro núcleos y 8 GB de memoria, con un uso de CPU del 50 % y un uso de memoria del 50 % y un factor de confort específico de 1,3.

-  Si la evaluación es **Como local**, se recomienda una SKU de VM de Azure con 4 núcleos y 8 GB de memoria.
- Si la evaluación está basada en el rendimiento, en función de la utilización de memoria y CPU efectiva (el 50 % de 4 núcleos * 1,3 = 2,6 núcleos y el 50 % de 8 GB de memoria * 1,3 = 5,3 GB de memoria), se recomienda la SKU de máquina virtual más barata de cuatro núcleos (número de núcleos admitidos más cercano) y 8 GB de memoria (tamaño de memoria admitido más cercano).
- [Más información](concepts-assessment-calculation.md#types-of-assessments) sobre el ajuste de tamaño de evaluación.

## <a name="why-is-the-recommended-azure-disk-skus-bigger-than-on-premises-in-an-azure-vm-assessment"></a>¿Por qué las SKU de discos de Azure recomendadas son más grandes que las locales en una evaluación de máquinas virtuales de Azure?

La evaluación de máquinas virtuales de Azure podría recomendar un disco más grande según el tipo de evaluación.
- El ajuste de tamaño de disco depende de dos propiedades de evaluación: criterio de ajuste de tamaño y tipo de almacenamiento.
- Si el criterio de ajuste de tamaño es **Basado en el rendimiento** y el tipo de almacenamiento es **Automático**, se tendrán en cuenta los valores de rendimiento y de IOPS del disco para identificar el tipo de disco de destino (discos HDD estándar, SSD estándar o Premium). Se recomienda una SKU de disco del tipo de disco y esta recomendación tiene en cuenta los requisitos de tamaño del disco en el entorno local.
- Si el criterio de ajuste de tamaño es **En función del rendimiento** y el tipo de almacenamiento es **Premium**, se recomienda una SKU de disco Premium de Azure en función de los requisitos de IOPS, rendimiento y tamaño del disco en el entorno local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es **Como local** y el tipo de almacenamiento es **HDD estándar**, **SSD estándar** o **Premium**.

Por ejemplo, si tiene un disco en el entorno local con 32 GB de memoria, pero las IOPS de lectura y escritura agregadas son 800 IOPS, la evaluación de máquinas virtuales de Azure recomienda un tipo de disco Premium (debido a los mayores requisitos de IOPS) y, a continuación, una SKU de disco que pueda admitir el tamaño y las IOPS necesarias. La coincidencia más cercana en este ejemplo sería P15 (256 GB, 1100 IOPS). Aunque el tamaño requerido por el disco en el entorno local fue de 32 GB, la evaluación de máquinas virtuales de Azure recomienda un disco con un tamaño mayor debido a los altos requisitos de IOPS del disco en el entorno local.

## <a name="why-is-performance-data-missing-for-someall-vms-in-my-assessment-report"></a>¿Por qué faltan datos de rendimiento de algunas o todas las máquinas virtuales en el informe de evaluación?

En el caso de la evaluación "en función del rendimiento", la exportación del informe de valoración indica "PercentageOfCoresUtilizedMissing" o "PercentageOfMemoryUtilizedMissing" cuando el dispositivo de Azure Migrate no puede recopilar datos de rendimiento de las máquinas virtuales locales pertinentes. Compruebe:

- Si las máquinas virtuales están encendidas durante el tiempo que ha estado creando la evaluación
- Si solo faltan contadores de memoria y está intentando evaluar las máquinas virtuales de Hyper-V, compruebe si tiene habilitada la memoria dinámica en estas máquinas virtuales. Existe un problema conocido debido a que el dispositivo de Azure Migrate no puede recopilar el uso de memoria de dichas máquinas virtuales.
- Si faltan todos los contadores de rendimiento, asegúrese de que se cumplen los requisitos de acceso a puertos para la evaluación. Obtenga más información sobre los requisitos de acceso a puertos para la evaluación de [VMware](./migrate-support-matrix-vmware.md#port-access-requirements), [Hyper-V](./migrate-support-matrix-hyper-v.md#port-access) y los servidores [físicos](./migrate-support-matrix-physical.md#port-access).
Nota: Si falta alguno de los contadores de rendimiento, Azure Migrate: Detección y evaluación vuelve a la memoria y los núcleos locales asignados, y recomienda un tamaño de máquina virtual acorde.

## <a name="why-is-performance-data-missing-for-someall-servers-in-my-azure-vm-andor-avs-assessment-report"></a>¿Por qué faltan los datos de rendimiento de algunos o todos los servidores de mi máquina virtual de Azure o el informe de evaluación de AVS?

En el caso de la evaluación "en función del rendimiento", la exportación del informe de evaluación indica "PercentageOfCoresUtilizedMissing" o "PercentageOfMemoryUtilizedMissing" cuando el dispositivo de Azure Migrate no puede recopilar datos de rendimiento de los servidores locales pertinentes. Compruebe:

- Si los servidores están encendidos durante el tiempo en que crea la evaluación
- Si solo faltan contadores de memoria y está intentando evaluar servidores en un entorno Hyper-V. En este caso, habilite la memoria dinámica en los servidores y "vuelva a calcular" la evaluación para reflejar los últimos cambios. El dispositivo solo puede recopilar los valores de uso para servidores en un entorno Hyper-V si el servidor tiene habilitada la memoria dinámica.

- Si faltan todos los contadores de rendimiento, asegúrese de que se permiten las conexiones de salida en los puertos 443 (HTTPS).

    > [!Note]
    > Si falta alguno de los contadores de rendimiento, Azure Migrate: Detección y evaluación vuelve a la memoria y los núcleos locales asignados, y recomienda un tamaño de máquina virtual acorde.


## <a name="why-is-performance-data-missing-for-someall-sql-instancesdatabases-in-my-azure-sql-assessment"></a>¿Por qué faltan datos de rendimiento en algunas o todas las instancias o bases de datos de SQL en la evaluación de Azure SQL?

Para asegurarse de que se recopilen los datos de rendimiento, compruebe lo siguiente:

- Si las instancias de SQL Server están encendidas durante el tiempo en que crea la evaluación
- Si el estado de la conexión del Agente SQL en Azure Migrate es "Conectado" y compruebe el último latido 
- Si el estado de conexión de Azure Migrate para todas las instancias de SQL es "Conectado" en la hoja de la instancia de SQL detectada
- Si faltan todos los contadores de rendimiento, asegúrese de que se permiten las conexiones de salida en los puertos 443 (HTTPS).

Si falta alguno de los contadores de rendimiento, la evaluación de Azure SQL recomienda la configuración de Azure SQL más pequeña para esa instancia o base de datos.

## <a name="why-is-the-confidence-rating-of-my-assessment-low"></a>¿Por qué la clasificación de confianza de mi valoración es baja?

La clasificación de confianza se calcula para las evaluaciones "en función del rendimiento" en función del porcentaje de [puntos de datos disponibles](./concepts-assessment-calculation.md#ratings) necesarios para calcular la evaluación. Estos son los motivos por los que una valoración puede obtener una clasificación de confianza baja:

- No generó un perfil de su entorno durante el tiempo que está creando la evaluación. Por ejemplo, si está creando una evaluación con la duración de rendimiento establecida en una semana, debe esperar al menos una semana después de iniciar la detección para que se recopilen todos los puntos de datos. Si no puede esperar el período de duración, puede cambiar la duración del rendimiento a un período más pequeño y **Recalcular** la evaluación.
 
- La evaluación no puede recopilar los datos de rendimiento de algunos o de todos los servidores en el período de evaluación. Para obtener una clasificación de confianza alta, asegúrese de que: 
    - Los servidores estén activados mientras dure la evaluación.
    - Se permiten las conexiones salientes en los puertos 443.
    - La memoria dinámica está habilitada en los servidores de Hyper-V. 
    - Si el estado de la conexión de los agentes en Azure Migrate es "Conectado" y compruebe el último latido.
    - Para las evaluaciones de Azure SQL, el estado de conexión de Azure Migrate para todas las instancias de SQL es "Conectado" en la hoja de la instancia de SQL detectada.

    **Recalcule** la evaluación para reflejar los cambios más recientes en la clasificación de confianza.

- En el caso de las evaluaciones de máquinas virtuales de Azure y AVS, se crearon pocos servidores después de iniciar la detección. Por ejemplo, si va a crear una evaluación para el historial de rendimiento del último mes, pero se crearon algunos servidores en el entorno hace solo una semana. En este caso, los datos de rendimiento de los nuevos servidores no estarán disponibles para todo el período y la clasificación de confianza será baja. [Más información](./concepts-assessment-calculation.md#confidence-ratings-performance-based)

- En el caso de las evaluaciones de Azure SQL, se crearon algunas instancias de SQL o bases de datos después de que se iniciara la detección. Por ejemplo, si va a crear una evaluación para el historial de rendimiento del último mes, pero se crearon algunas instancias o bases de datos de SQL en el entorno hace solo una semana. En este caso, los datos de rendimiento de los nuevos servidores no estarán disponibles para todo el período y la clasificación de confianza será baja. [Más información](./concepts-azure-sql-assessment-calculation.md#confidence-ratings)

## <a name="is-the-operating-system-license-included-in-an-azure-vm-assessment"></a>¿Se incluye la licencia del sistema operativo en una valoración de máquinas virtuales de Azure?

La evaluación de máquinas virtuales de Azure solo tiene en cuenta el costo de la licencia del sistema operativo para los servidores Windows. Los costos de las licencias para los servidores Linux no se tienen en cuenta actualmente.

## <a name="how-does-performance-based-sizing-work-in-an-azure-vm-assessment"></a>¿Cómo funciona el dimensionamiento basado en el rendimiento en una valoración de máquinas virtuales de Azure?

La evaluación de máquinas virtuales de Azure recopila continuamente datos de rendimiento de los servidores en el entorno local y los usa para recomendar la SKU de máquina virtual y la SKU de disco de Azure. [Obtenga información](concepts-assessment-calculation.md#calculate-sizing-performance-based) sobre cómo se recopilan datos basados en el rendimiento.

## <a name="why-is-my-assessment-showing-a-warning-that-it-was-created-with-an-invalid-combination-of-reserved-instances-vm-uptime-and-discount-"></a>¿Por qué mi evaluación muestra una advertencia de que se creó con una combinación no válida de instancias reservadas, tiempo de actividad de la máquina virtual y descuento (%)?
Al seleccionar ' instancias reservadas ', el ' descuento (%) ' y las propiedades de "tiempo de actividad de la máquina virtual" no son aplicables. A medida que la evaluación se creó con una combinación no válida de estas propiedades, se deshabilitan los botones editar y volver a calcular. Cree una nueva valoración. [Más información](./concepts-assessment-calculation.md#whats-an-assessment).

## <a name="i-do-not-see-performance-data-for-some-network-adapters-on-my-physical-servers"></a>No veo los datos de rendimiento de algunos adaptadores de red en los servidores físicos.

Esto puede ocurrir si el servidor físico tiene habilitada la virtualización de Hyper-V. En estos servidores, debido a una brecha en el producto, Azure Migrate detecta actualmente los adaptadores de red físicos y virtuales. El rendimiento de la red solo se captura en los adaptadores de red virtuales descubiertos.

## <a name="recommended-azure-vm-sku-for-my-physical-server-is-oversized"></a>La SKU de máquina virtual de Azure recomendada para el servidor físico tiene un gran tamaño.

Esto puede ocurrir si el servidor físico tiene habilitada la virtualización de Hyper-V. En estos servidores, Azure Migrate detecta actualmente los adaptadores de red físicos y virtuales. Por lo tanto, el número de adaptadores de red detectados es mayor que el valor real. Como la evaluación de máquinas virtuales de Azure elige una máquina virtual de Azure que puede admitir el número requerido de adaptadores de red, esto puede dar lugar a una máquina virtual de gran tamaño. [Obtenga más información](./concepts-assessment-calculation.md#calculating-sizing) sobre el impacto del número de adaptadores de red para dimensionamiento. Se trata de una brecha del producto que se solucionará más adelante.

## <a name="readiness-category-not-ready-for-my-physical-server"></a>Categoría de preparación "No preparado" para mi servidor físico

La categoría de preparación puede estar marcada incorrectamente como "No preparado" en el caso de un servidor físico que tenga habilitada la virtualización de Hyper-V. En estos servidores, debido a una brecha en el producto, Azure Migrate detecta actualmente los adaptadores físicos y virtuales. Por lo tanto, el número de adaptadores de red detectados es mayor que el valor real. En las evaluaciones tanto locales como basadas en el rendimiento, la evaluación de máquinas virtuales de Azure elige una máquina virtual de Azure que puede admitir el número requerido de adaptadores de red. Si se detecta que el número de adaptadores de red es mayor que 32, el número máximo de NIC admitidas en las máquinas virtuales de Azure, el servidor se marcará como "No preparado".  [Obtenga más información](./concepts-assessment-calculation.md#calculating-sizing) sobre el impacto del número de NIC en el dimensionamiento.


## <a name="number-of-discovered-nics-higher-than-actual-for-physical-servers"></a>El número de NIC detectado es mayor que el valor real para servidores físicos.

Esto puede ocurrir si el servidor físico tiene habilitada la virtualización de Hyper-V. Actualmente, Azure Migrate detecta los adaptadores físicos y virtuales en estos servidores. Por lo tanto, el número de NIC detectadas es mayor que el valor real.

## <a name="dependency-visualization-in-azure-government"></a>Visualización de dependencias en Azure Government

El análisis de dependencias basado en agente no está disponible en Azure Government. Use el análisis de dependencias sin agente.


## <a name="dependencies-dont-show-after-agent-install"></a>Las dependencias no se muestran después de la instalación del agente

Después de instalar los agentes de visualización de dependencias en las máquinas virtuales locales, Azure Migrate suele tardar entre 15 y 30 minutos en mostrar las dependencias en el portal. Si ha esperado durante más de 30 minutos, asegúrese de que Microsoft Monitoring Agent (MMA) puede conectarse al área de trabajo de Log Analytics.

Para las máquinas virtuales de Windows:
1. En el Panel de control, inicie MMA.
2. En **Propiedades de Microsoft Monitoring Agent** > **Azure Log Analytics (OMS)** , asegúrese de que el **estado** del área de trabajo esté en verde.
3. Si el estado no está en verde, intente quitar el área de trabajo y vuelva a agregarla a MMA.

    ![Estado de MMA](./media/troubleshoot-assessment/mma-properties.png)

Para las máquinas virtuales Linux, asegúrese de que se hayan completado correctamente los comandos de instalación de MMA y el agente de dependencias. Consulte la guía de solución de problemas [aquí](../azure-monitor/vm/service-map.md#post-installation-issues).

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

- **Agente de MMS**: Revise los sistemas operativos [Windows](../azure-monitor/agents/agents-overview.md#supported-operating-systems) y [Linux](../azure-monitor/agents/agents-overview.md#supported-operating-systems) admitidos.
- **Agente de dependencias**: sistemas operativos [Windows y Linux](../azure-monitor/vm/vminsights-enable-overview.md#supported-operating-systems) admitidos.

## <a name="visualize-dependencies-for--1-hour"></a>Visualización de dependencias de > 1 hora

Con el análisis de dependencia sin agente, puede visualizar dependencias o exportarlas en un mapa para una duración de hasta 30 días.

Con el análisis de dependencias basado en agentes, aunque Azure Migrate le permita volver atrás a una fecha determinada en el último mes, el tiempo máximo durante el que se pueden visualizar las dependencias es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Sin embargo, puede usar los registros de Azure Monitor para [consultar los datos de dependencia](./how-to-create-group-machine-dependencies.md) durante un período más largo.

## <a name="visualized-dependencies-for--10-servers"></a>Visualización de dependencias de > 10 horas

En Azure Migrate, puede [visualizar las dependencias para grupos](./how-to-create-a-group.md#refine-a-group-with-dependency-mapping) que tengan un máximo de 10 máquinas virtuales con un análisis de dependencia basado en agentes. En el caso de los grupos más grandes, se recomienda dividir las máquinas virtuales en grupos más pequeños para visualizar las dependencias.


## <a name="servers-show-install-agent"></a>Los servidores muestran "Instalar agente"

Después de migrar los servidores con la visualización de dependencias habilitada en Azure, es posible que los servidores muestren la acción "Instalar agente" en lugar de "Ver dependencias" debido al comportamiento siguiente:

- Después de la migración a Azure, los servidores locales están desconectados y las máquinas virtuales equivalentes trabajan en Azure. Estos servidores adquieren una dirección MAC diferente.
- Los servidores también pueden adquirir una dirección IP distinta en función de si se elige conservar o no la dirección IP local.
- Si las direcciones IP y MAC difieren de las locales, Azure Migrate no asocia los servidores locales con ningún dato de dependencias de Service Map. En este caso, se mostrará la opción para instalar el agente en lugar de ver las dependencias.
- Después de una migración de prueba a Azure, los servidores locales permanecen encendidos según lo previsto. Los servidores equivalentes que se activan en Azure adquieren una dirección MAC distinta y pueden adquirir una dirección IP diferente. A menos que bloquee el tráfico saliente del registro de Azure Monitor procedente de estos servidores, Azure Migrate no asociará los servidores locales con ningún dato de dependencias de Service Map y, por tanto, mostrará la opción de instalar los agentes en lugar de visualizar las dependencias.

## <a name="dependencies-export-csv-shows-unknown-process"></a>Dependencias de exportación de CSV muestra "Proceso desconocido".
En el análisis de dependencias sin agente, los nombres de los procesos se capturan de la mejor manera posible. En ciertos escenarios, aunque los nombres de los servidores de origen y destino y el puerto de destino se capturen, no es factible determinar los nombres de los procesos en ambos extremos de la dependencia. En tales casos, el proceso se marca como "Proceso desconocido".

## <a name="my-log-analytics-workspace-is-not-listed-when-trying-to-configure-the-workspace-in-azure-migrate"></a>Mi área de trabajo de Log Analytics no aparece al intentar configurar el área de trabajo en Azure Migrate
Actualmente, Azure Migrate admite la creación de un área de trabajo de OMS en las regiones Este de EE. UU., Sudeste de Asia y Oeste de Europa. Si el área de trabajo se crea fuera de Azure Migrate en cualquier otra región, actualmente no se puede asociar a un proyecto.


## <a name="capture-network-traffic"></a>Captura del tráfico de red

Recopile registros de tráfico de red como se indica a continuación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Presione F12 para iniciar las Herramientas de desarrollo. Si es necesario, desactive la opción **Borrar entradas de navegación**.
3. Seleccione la pestaña **Red** para empezar a capturar el tráfico de red:
   - En Chrome, seleccione **Preserve log** (Conservar registro). La grabación debe iniciarse automáticamente. Un círculo rojo indica que la captura del tráfico está en curso. Si no aparece el círculo rojo, seleccione el círculo negro para comenzar.
   - En Microsoft Edge e Internet Explorer, la grabación debe iniciarse automáticamente. Si no es así, seleccione el botón de reproducción de color verde.
4. Pruebe a reproducir el error.
5. Una vez haya encontrado el error durante la grabación, detenga la grabación y guarde una copia de la actividad grabada:
   - En Chrome, haga clic con el botón derecho y seleccione **Guardar como HAR con contenido**. Esta acción comprime y exporta los registros como un archivo .har.
   - En Microsoft Edge o Internet Explorer, seleccione la opción **Exportar tráfico capturado**. Esta acción comprime y exporta el registro.
6. Seleccione la pestaña **Consola** para ver si hay alguna advertencia o error. Para guardar el registro de la consola:
   - En Chrome, haga clic con el botón derecho en cualquier lugar en el registro de la consola. Seleccione **Save as** (Guardar como) para exportar y comprimir el registro.
   - En Microsoft Edge o Internet Explorer, haga clic con el botón derecho sobre los errores y seleccione **Copiar todo**.
7. Cierre las Herramientas de desarrollo.


## <a name="where-is-the-operating-system-data-in-my-assessment-discovered-from"></a>¿Dónde se detectan los datos del sistema operativo en mi valoración?

- En el caso de las máquinas virtuales de VMware, de forma predeterminada, son los datos del sistema operativo que proporciona vCenter. 
   - En el caso de las máquinas virtuales Linux de VMware, si está habilitada la detección de aplicaciones, los detalles del sistema operativo se capturan desde la máquina virtual invitada. Para comprobar los detalles del sistema operativo de la valoración, vaya a la vista de Servidores detectados y ponga el mouse sobre el valor de la columna "Sistema operativo". En el texto que aparece, podrá ver si los datos del sistema operativo que ve se recopilan de vCenter Server o de la máquina virtual invitada con las credenciales de la máquina virtual. 
   - En el caso de las máquinas virtuales Windows, los detalles del sistema operativo siempre se capturan desde el vCenter Server.
- En el caso de las máquinas virtuales de Hyper-V, los datos del sistema operativo se recopilan desde el host de Hyper-V.
- En el caso de los servidores físicos, se capturan desde el servidor.

## <a name="next-steps"></a>Pasos siguientes

[Crear](how-to-create-assessment.md) o [personalizar](how-to-modify-assessment.md) una evaluación.