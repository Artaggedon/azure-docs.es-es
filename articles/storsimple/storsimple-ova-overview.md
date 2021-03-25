---
title: Introducción sobre la matriz virtual de Microsoft Azure StorSimple
description: Describe la matriz virtual de StorSimple, una solución de almacenamiento integrada que administra las tareas de almacenamiento de información entre una matriz virtual local y el almacenamiento de nube de Microsoft Azure.
author: alkohli
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.topic: conceptual
ms.date: 03/08/2021
ms.author: alkohli
ms.openlocfilehash: 6d7d8c37d0e7ffacb3221a4f30a96ef6cccda819
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "104674451"
---
# <a name="introduction-to-the-storsimple-virtual-array"></a>Introducción a la matriz virtual de StorSimple

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

## <a name="overview"></a>Información general

La matriz virtual de Microsoft Azure StorSimple es una solución de almacenamiento integrada que administra las tareas de almacenamiento de información entre una matriz virtual local que se ejecuta en un hipervisor y el almacenamiento de nube de Microsoft Azure. La matriz virtual es una solución de servidor iSCSI o servidor de archivos eficaz, rentable y fácilmente administrable que elimina muchos de los problemas y gastos asociados con el almacenamiento empresarial y la protección de datos. La matriz virtual está especialmente indicada para el almacenamiento de datos de archivo a los que se accede con poca frecuencia.

En este artículo se proporciona una descripción general de la matriz virtual. Aquí hay algunos otros recursos:

* Para obtener prácticas recomendadas, consulte [Prácticas recomendadas de la matriz virtual de StorSimple](storsimple-ova-best-practices.md).
* Para obtener información general acerca de los dispositivos de la serie 8000 de StorSimple, consulte el artículo [Serie StorSimple 8000: una solución de almacenamiento en la nube híbrida](storsimple-overview.md).
* Para más información acerca de los dispositivos de la serie 5000/7000 de StorSimple, consulte la [ayuda en línea de StorSimple](http://onlinehelp.storsimple.com/).

La matriz virtual es compatible con el protocolo de bloque de mensajes del servidor (SMB) o el protocolo iSCSI. Se ejecuta en la infraestructura existente de hipervisor y proporciona una organización en niveles en la nube, copias de seguridad de nube, restauración rápida, recuperación a nivel de elemento y características de recuperación ante desastres.

La tabla siguiente resume las características más importantes de la matriz virtual de StorSimple.

| Característica | StorSimple Virtual Array |
| --- | --- |
| Requisitos de instalación |Usa una infraestructura de virtualización (Hyper-V o VMware) |
| Disponibilidad |Nodo único |
| Capacidad total (incluida la nube) |Hasta 64 TB de capacidad útil por matriz virtual |
| Capacidad local |De 390 GB a 6,4 TB de capacidad útil por matriz virtual (necesaria para aprovisionar de 500 GB a 8 TB de espacio en disco) |
| Protocolos nativos |iSCSI o SMB |
| Objetivo de tiempo de recuperación (RTO) |iSCSI: menos de 2 minutos, independientemente del tamaño |
| Objetivo de punto de recuperación (RPO) |Copias de seguridad diarias y a petición |
| Organización en niveles del almacenamiento |Usa mapas de calor para determinar qué datos deben organizarse en niveles en la nube o de forma local |
| Soporte técnico |Infraestructura de virtualización admitida por el proveedor |
| Rendimiento |Varía en función de la infraestructura subyacente |
| Movilidad de datos |Puede realizar la restauración en el mismo dispositivo o realizar la recuperación a nivel de elemento (servidor de archivos) |
| Niveles de almacenamiento |Almacenamiento de hipervisor local y de nube |
| Tamaño del recurso compartido |Organizado en niveles: hasta 20 TB. Anclado localmente: hasta 2 TB |
| Tamaño del volumen |En capas: 500 GB a 5 TB; anclado localmente: 50 GB a 200 GB <br> La reserva local máxima para los volúmenes en capas es de 200 GB. |
| Instantáneas |Coherencia frente a bloqueos |
| Recuperación a nivel de elemento |Sí; los usuarios pueden realizar restauraciones desde recursos compartidos |

## <a name="why-use-storsimple"></a>¿Por qué usar StorSimple?

StorSimple conecta tanto a usuarios como a los servidores al almacenamiento de Azure en minutos, sin tener que modificar la aplicación.

En la tabla siguiente se describen algunas de las ventajas principales que proporciona la solución de la matriz virtual de StorSimple.

| Característica | Ventaja |
| --- | --- |
| Integración transparente |La matriz virtual es compatible con el protocolo SMB o el protocolo iSCSI. El movimiento de datos entre el nivel local y el nivel de nube se realiza sin problemas y de forma transparente para el usuario. |
| Costos de almacenamiento reducidos |Gracias a StorSimple, puede aprovisionar el almacenamiento local necesario para satisfacer las demandas actuales de la mayoría de los datos activos más usados. A medida que aumenten sus necesidades de almacenamiento, StorSimple puede organizar en niveles los datos inactivos en un almacenamiento en la nube que le resulte rentable. Los datos se desduplican y comprimen antes de enviarse a la nube para reducir aún más los gastos y los requisitos de almacenamiento. |
| Administración simplificada del almacenamiento |StorSimple proporciona administración centralizada en la nube con el administrador de dispositivos de StorSimple para administrar varios dispositivos. |
| Mejor recuperación ante desastres y cumplimiento normativo |StorSimple facilita una recuperación ante desastres más rápida al restaurar los metadatos inmediatamente y restaurar los datos según sea necesario. Las operaciones normales pueden seguir con una interrupción mínima. |
| Movilidad de datos |Se puede tener acceso a los datos organizados en niveles en la nube desde otros sitios, para que así pueda recuperar y migrar los datos que desee. Solo puede restaurar datos en la matriz virtual original. Sin embargo, debe usar características de recuperación ante desastres para restaurar toda la matriz virtual en otra matriz virtual. |



## <a name="storsimple-workload-summary"></a>Resumen de la carga de trabajo de StorSimple

A continuación, presentamos un resumen de las cargas de trabajo de StorSimple.

|Escenario     |Carga de trabajo     |Compatible      |Restricciones               | Versiones aplicables|
|-------------|-------------|---------------|---------------------------|--------------------|
|Oficina remota/sucursal (ROBO)  |Uso compartido de archivos     |Sí      |Consulte los [límites máximos del servidor de archivos](storsimple-ova-limits.md).<br></br>Consulte los [requisitos del sistema para versiones de SMB compatibles](storsimple-ova-system-requirements.md).| Todas las versiones     |
|Archivado en la nube  |Uso compartido de archivos     |Sí      |Consulte los [límites máximos del servidor de archivos](storsimple-ova-limits.md).<br></br>Consulte los [requisitos del sistema para versiones de SMB compatibles](storsimple-ova-system-requirements.md).| Todas las versiones     |

StorSimple Virtual Array es ideal para los datos a los que se accede con poca frecuencia. Mientras que la matriz virtual tiene una memoria caché local para mejorar el rendimiento, los usuarios deben suponer que los archivos de los servicios de dispositivo están en el nivel más bajo de almacenamiento (la nube). Cada matriz virtual puede escribir y leer en Azure Storage a una velocidad aproximada de 100 Mbps. Ese vínculo se comparte entre todas las solicitudes que entran en el dispositivo y puede convertirse en un cuello de botella, tal como se muestra en el diagrama siguiente.

![Archivado en la nube](./media/storsimple-ova-overview/cloud-archiving.png)

Cuando varios usuarios simultáneos acceden a la matriz virtual, todos comparten la conexión a Azure, por lo que se reduce el rendimiento. No hay ningún rendimiento garantizado por usuario, y el dispositivo procesa las solicitudes individuales a medida que llegan.

StorSimple Virtual Array no es adecuado para cargas de trabajo que necesitan una alta disponibilidad. La matriz virtual es un dispositivo de nodo único que experimenta un tiempo de inactividad cuando se instalan las actualizaciones de software. Los administradores deben planear una ventana de mantenimiento de 30 minutos tres o cuatro veces al año.

## <a name="workflows"></a>Workflows

La matriz virtual de StorSimple es especialmente adecuada para los siguientes flujos de trabajo:

* [Administración del almacenamiento basado en la nube](#cloud-based-storage-management)
* [Copias de seguridad independientes de la ubicación](#location-independent-backup)
* [Recuperación ante desastres y protección de datos](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Administración del almacenamiento basado en la nube
Puede usar el servicio de administrador de dispositivos de StorSimple que se ejecuta en Azure Portal para administrar los datos almacenados en varios dispositivos y en varias ubicaciones. Esto es especialmente útil en escenarios de sucursales distribuidas. Debe crear instancias independientes del servicio Administrador de dispositivos de StorSimple para administrar matrices virtuales y dispositivos físicos StorSimple. La matriz virtual ahora usa la nueva instancia de Azure Portal en lugar del portal clásico de Azure.<!--Is the "now" element still in date? Could it go at this point? Just checking.-->

![Administración del almacenamiento basado en la nube](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Copias de seguridad independientes de la ubicación
Con la matriz virtual, las instantáneas en la nube proporcionan una copia de punto en el tiempo independiente de la ubicación de un volumen o recurso compartido. Las instantáneas de nube están habilitadas de forma predeterminada y no se pueden deshabilitar. Se realiza una copia de seguridad simultánea de todos los volúmenes y recursos compartidos mediante una directiva de copia de seguridad diaria única, y se pueden realizar copias de seguridad ad-hoc adicionales cuando sea necesario.

### <a name="data-protection-and-disaster-recovery"></a>Recuperación ante desastres y protección de datos
La matriz virtual admite la siguiente protección de datos y escenarios de recuperación ante desastres:

* **Restaurar volumen o recurso compartido** : use la restauración como nuevo flujo de trabajo para recuperar un volumen o un recurso compartido. Puede usar este método para recuperar el recurso compartido o el volumen al completo.
* **Recuperación a nivel de elemento** : los recursos compartidos le permiten obtener acceso simplificado a copias de seguridad recientes. Si obtiene acceso en la nube a una carpeta *.backup* especial con formato .backup, podrá recuperar fácilmente el archivo individual que desee. Esta capacidad de restauración está controlada por el usuario y no es necesario que intervenga el administrador.
* **Recuperación ante desastres**: use la funcionalidad de conmutación por error para recuperar todos los volúmenes o recursos compartidos en una nueva matriz virtual. Puede crear la nueva matriz virtual y registrarla con el servicio StorSimple Device; a continuación, podrá conmutar por error la matriz virtual original. Entonces, la nueva matriz virtual se hará con los recursos aprovisionados.

## <a name="storsimple-virtual-array-components"></a>Componentes de la matriz virtual de StorSimple

La matriz virtual incluye los siguientes componentes:

* [Matriz virtual](#virtual-array): es un dispositivo de almacenamiento de nube híbrida basado en una máquina virtual aprovisionada en un entorno virtualizado o hipervisor.
* [Servicio StorSimple Device Manager](#storsimple-device-manager-service): es una extensión de Azure Portal que le permite administrar uno o varios dispositivos StorSimple desde una única interfaz web a la cual puede acceder desde diferentes ubicaciones geográficas. Asimismo, puede usar el servicio de administrador de dispositivos de StorSimple para crear y administrar servicios, ver y administrar dispositivos y alertas y administrar los volúmenes, los recursos compartidos y las instantáneas ya existentes.
* [Interfaz de usuario web local](#local-web-user-interface): es una interfaz de usuario basada en web que se usa para configurar el dispositivo de modo que se pueda conectar a la red local y luego registrarse en el servicio Administrador de dispositivos de StorSimple. 
* [Interfaz de la línea de comandos](#command-line-interface): es una interfaz de Windows PowerShell que puede usar para iniciar una sesión de soporte en la matriz virtual.
  En las secciones siguientes se describen los componentes más detalladamente y se explica la manera en que la solución ordena los datos, asigna el almacenamiento y facilita la administración del almacenamiento y la protección de los datos.

### <a name="virtual-array"></a>Matriz virtual

La matriz virtual es una solución de almacenamiento de nodo único que proporciona un espacio de almacenamiento principal, administra la comunicación con el almacenamiento en la nube y ayuda a garantizar la seguridad y confidencialidad de todos los datos almacenados en el dispositivo.

Tiene un modelo de la matriz virtual disponible para su descarga. La matriz virtual tiene una capacidad máxima de 6,4 TB en el dispositivo (con un requisito de almacenamiento subyacente de 8 TB) y 64 TB si se incluye el almacenamiento en la nube.

La matriz virtual tiene las siguientes características:

* Es rentable. Usa la infraestructura de virtualización y se puede implementar en el hipervisor de Hyper-V o VMware existente.
* Se encuentra en el centro de datos y puede configurarse como servidor iSCSI o como servidor de archivos.
* Está integrada en la nube.
* Las copias de seguridad se almacenan en la nube, a la cual puede tener acceso fácilmente para recuperar datos ante desastres y simplificar la recuperación a nivel de elemento (ILR).
* Puede aplicar actualizaciones en la matriz virtual, tal como haría con un dispositivo físico.

> [!NOTE]
> Una matriz virtual no puede ampliarse. Por lo tanto, es importante aprovisionar un espacio de almacenamiento adecuado al crear la matriz virtual.

### <a name="storsimple-device-manager-service"></a>Servicio de administrador de dispositivos de StorSimple

Microsoft Azure StorSimple proporciona una interfaz de usuario basada en web, el servicio Administrador de dispositivos de StorSimple, que permite administrar de manera centralizada el almacén de StorSimple. Puede usar el servicio de administrador de dispositivos de StorSimple para realizar las siguientes tareas:

* Administrar varias matrices virtuales de StorSimple desde un único servicio.
* Configurar y administrar ajustes de seguridad para matrices virtuales de StorSimple. (El cifrado en la nube depende de las API de Microsoft Azure).
* Configurar las propiedades y las credenciales de la cuenta de almacenamiento.
* Configurar y administrar los volúmenes o recursos compartidos.
* Realizar copias de seguridad y restaurar datos en volúmenes o recursos compartidos.
* Supervisar el rendimiento.
* Revisar la configuración del sistema e identificar posibles problemas.

Puede usar el servicio Administrador de dispositivos de StorSimple para realizar la administración diaria de la matriz virtual.

Para obtener más información, consulte [Uso del servicio de administrador de dispositivos de StorSimple para administrar su dispositivo StorSimple](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Interfaz de usuario web local

La matriz virtual incluye una interfaz de usuario basada en la web que se usa para realizar una única configuración y para registrar el dispositivo con el servicio de administrador de dispositivos de StorSimple. Puede usarla para apagar y reiniciar la matriz virtual, ejecutar pruebas de diagnóstico, actualizar el software, cambiar la contraseña del administrador de los dispositivos, ver los registros del sistema y ponerse en contacto con el servicio de soporte técnico de Microsoft para realizar una solicitud de servicio.

Para más información acerca de cómo usar la interfaz de usuario basada en web, consulte [Usar la interfaz de usuario web para administrar la matriz virtual de StorSimple](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Interfaz de la línea de comandos

La interfaz de Windows PowerShell que se incluye permite iniciar una sesión de soporte con Soporte técnico de Microsoft, que puede ayudarle a solucionar cualquier problema que pudiera surgir en la matriz virtual.

## <a name="storage-management-technologies"></a>Tecnologías de administración de almacenamiento

Además de la matriz virtual y otros componentes, la solución StorSimple usa las siguientes tecnologías de software para proporcionar acceso rápido a datos importantes, reducir el consumo de almacenamiento y proteger los datos almacenados en la matriz virtual:

* [Organización automática del almacenamiento en niveles](#automatic-storage-tiering) 
* [Volúmenes y recursos compartidos anclados localmente](#locally-pinned-shares-and-volumes)
* Desduplicación y compresión de datos organizados en niveles o que tienen una copia de seguridad en la nube 
* [Copias de seguridad a petición y programadas](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Organización automática del almacenamiento en niveles
La matriz virtual usa un nuevo mecanismo de organización en niveles para administrar los datos almacenados en la matriz virtual y en la nube. Solo existen dos niveles: la matriz virtual y el almacenamiento en nube de Azure. La matriz virtual de StorSimple ordena automáticamente los datos en los niveles según un mapa de calor, el cual realiza un seguimiento del uso actual, la edad y las relaciones que pudiera haber con otros datos. Los datos más activos se almacenan de manera local, mientras que los datos menos activos y los datos inactivos migran automáticamente a la nube. (Todas las copias de seguridad se almacenan en la nube). StorSimple ajusta y reordena las asignaciones de datos y almacenamiento a medida que cambian los patrones de uso. Por ejemplo, cierta información puede volverse menos activo con el tiempo. A medida que se vuelve menos activa, se organiza en niveles en la nube. Si esos mismos datos vuelven a activarse, se vuelven a organizar en niveles en la matriz de almacenamiento.

Los datos de un volumen o recurso compartido determinado organizado en capas tienen garantizado su propio espacio de capa local (aproximadamente el 10 % del espacio total aprovisionado para ese recurso compartido o volumen). Si bien esto reduce el almacenamiento disponible en la matriz virtual para ese recurso compartido o volumen, garantiza que la organización en capas de un recurso compartido o volumen no se vea afectada por las necesidades de organización en capas de otros recursos compartidos o volúmenes. Así, una carga de trabajo muy ocupada de un recurso compartido o volumen no puede forzar a las demás cargas de trabajo a la nube.

Los volúmenes en capas creados para iSCSI tienen una reserva local máxima de 200 GB independientemente del tamaño del volumen.

![Organización automática del almacenamiento en niveles](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> Puede especificar que un volumen esté anclado localmente, en cuyo caso los datos permanecen en la matriz virtual y no organizados en niveles en la nube. Para más información, consulte la sección [Volúmenes y recursos compartidos anclados localmente](#locally-pinned-shares-and-volumes).

> [!IMPORTANT]
> Al usar StorSimple, no convierta los blobs a archivado, aunque el dispositivo esté en proceso de eliminación gradual. Para recuperar datos del dispositivo, tiene que rehidratar los blobs de archivado a nivel de acceso frecuente o esporádico, lo que supone un costo considerable.


### <a name="locally-pinned-shares-and-volumes"></a>Volúmenes y recursos compartidos anclados localmente

Puede crear útiles recursos compartidos y volúmenes que estén anclados localmente. Esta funcionalidad le garantiza que los datos que necesitan las aplicaciones críticas no se almacenarán en niveles en la nube, sino que permanecerán en la matriz virtual. Los recursos compartidos y los volúmenes anclados localmente tienen las siguientes características:

* No dependen de las latencias de la nube ni sufren problemas de conectividad.
* Siguen beneficiándose de las características de copia de seguridad en la nube y de la recuperación ante desastres de StorSimple.

Puede restaurar un recurso compartido o volumen anclado localmente para que esté organizado en niveles o viceversa. 

Para más información sobre los volúmenes anclados localmente, consulte [Usar el servicio de administrador de dispositivos de StorSimple para administrar volúmenes](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Desduplicación y compresión de datos organizados en niveles o que tienen una copia de seguridad en la nube

StorSimple usa la desduplicación y la compresión de datos para reducir aún más los requisitos de almacenamiento en la nube. La desduplicación disminuye la cantidad general de datos almacenados al eliminar la redundancia en el conjunto de datos almacenados. A medida que cambia la información, StorSimple omite los datos no modificados y solo captura los cambios. Es más, StorSimple reduce la cantidad de datos almacenados al identificar y eliminar la información duplicada.

> [!NOTE]
> Los datos almacenados en la matriz virtual no se desduplican ni comprimen. Todas las operaciones de desduplicación y compresión se llevan a cabo justo antes de que los datos se envíen a la nube.


### <a name="scheduled-and-on-demand-backups"></a>Copias de seguridad a petición y programadas

Las características de protección de datos de StorSimple le permiten crear copias de seguridad a petición. Asimismo, si programa una copia de seguridad de forma predeterminada, se asegurará de que se copien los datos a diario. Las copias de seguridad se realizan en forma de instantáneas incrementales, las cuales se almacenan en la nube. Las instantáneas, que solo registran los cambios desde que se realizó la última copia de seguridad, se pueden crear y restaurar rápidamente. Estas instantáneas pueden serle de gran utilidad en escenarios de recuperación ante desastres, ya que reemplazan a los sistemas de almacenamiento secundarios (por ejemplo, las copias de seguridad en cinta) y le permiten restaurar los datos en el centro de datos o en sitios alternativos si fuera necesario.

## <a name="managing-personal-information"></a>Administración de información personal

StorSimple Device Manager para la serie virtual recopila información personal en dos instancias clave:
 - La configuración del usuario de alerta en la que se configuran las direcciones de correo electrónico de los usuarios. El administrador puede borrar esta información. 
 - Los usuarios que pueden acceder a los datos de los recursos compartidos. Se muestra y se puede exportar una lista de los usuarios que pueden acceder a los datos del recurso compartido. Esta lista se elimina al eliminar el recurso compartido.

Para obtener más información, revise la [directiva de privacidad de Microsoft en el Centro de confianza](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [preparar el portal de la matriz virtual](storsimple-virtual-array-deploy1-portal-prep.md).
