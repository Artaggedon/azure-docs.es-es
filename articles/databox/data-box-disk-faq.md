---
title: Preguntas frecuentes de Microsoft Azure Data Box Disk | Microsoft Docs
description: Contiene preguntas frecuentes acerca de Azure Data Box Disk, una solución en la nube que permite transferir grandes cantidades de datos en Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: conceptual
ms.date: 03/02/2021
ms.author: alkohli
ms.custom: references_regions
ms.openlocfilehash: 88aedb7daa375ae6b4a9107dceed1d25ed72ed92
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102039111"
---
# <a name="azure-data-box-disk-frequently-asked-questions"></a>Azure Data Box Disk: Preguntas más frecuentes

La solución en la nube Microsoft Azure Data Box Disk permite enviar terabytes de datos a Azure de forma rápida, económica y confiable. Este documento contiene preguntas que pueden surgirle al usar Data Box Disks en Azure Portal y respuestas a dichas preguntas. 

Las preguntas y respuestas se organizan en las categorías siguientes:

- Acerca del servicio
- Configuración y conexión 
- Seguimiento del estado
- Migración de los datos 
- Comprobación y carga de datos 


## <a name="about-the-service"></a>Acerca del servicio

### <a name="q-what-is-azure-data-box-service"></a>Q. ¿Qué es el servicio Azure Data Box? 
A.  El servicio Azure Data Box está diseñado para la ingesta de datos sin conexión. Este servicio administra una matriz de productos adaptados para el transporte de datos para diferentes capacidades de almacenamiento. 

### <a name="q-what-are-azure-data-box-disks"></a>Q. ¿Qué son los discos de Azure Data Box?
A. Los discos de Azure Data Box permiten transferir terabytes de datos de forma rápida, barata y segura a y desde Azure. Microsoft envía entre 1 y 5, con una capacidad de almacenamiento máxima de 35 TB. Dichos discos se pueden configurar, conectar y desbloquear fácilmente con el servicio Data Box en Azure Portal.  

Los discos cuentan con cifrado de unidad BitLocker de Microsoft, mientras que las claves de cifrado se administran desde Azure Portal. Luego se copian los datos de los servidores del cliente. En el centro de datos, Microsoft migra los datos de la unidad a la nube con un vínculo de carga en la red rápido y privado, y los carga en Azure.

### <a name="q-when-should-i-use-data-box-disks"></a>Q. ¿Cuándo deben usarse los discos de Data Box?
A. Si desea transferir 40 TB de datos (o menos) a Azure, es aconsejable usar de los discos de Data Box.

### <a name="q-what-is-the-price-of-data-box-disks"></a>Q. ¿Cuánto cuestan los discos de Data Box?
A. Para más información sobre el precio de Data Box Disks, vaya a la [página de precios](https://azure.microsoft.com/pricing/details/databox/disk/).

### <a name="q-how-do-i-get-data-box-disks"></a>Q. ¿Cómo se obtienen los discos de Data Box? 
A.  Para obtener Azure Data Box Disk, inicie sesión en Azure Portal y cree un pedido de discos de Data Box. Proporcione su información de contacto y los datos de notificación. Una vez que realiza el pedido, y en función de la disponibilidad, los discos se envían en un plazo máximo de 10 días.

### <a name="q-what-is-the-maximum-amount-of-data-i-can-transfer-with-data-box-disks-in-one-instance"></a>Q. ¿Cuál es la cantidad máxima de datos que se puede transferir con los discos de Data Box en una instancia?
A. Para cinco discos de 8 TB cada uno (7 TB de capacidad usable), la capacidad máxima usable es 35 TB. Por tanto, puede transferir 35 TB de datos en una instancia. Para transferir más datos, es preciso solicitar más discos.

### <a name="q-how-can-i-check-if-data-box-disks-are-available-in-my-region"></a>Q. ¿Cómo se puede comprobar si los discos de Data Box están disponibles en una región concreta? 
A.  Para ver dónde están disponibles actualmente las instancias de Data Box Disk, vaya a [Disponibilidad en regiones](data-box-disk-overview.md#region-availability).  

### <a name="q-which-regions-can-i-store-data-in-with-data-box-disks"></a>Q. ¿En qué regiones puedo almacenar datos con los discos de Data Box?
A. Data Box Disk se admite en todas las regiones de EE. UU., Canadá, Europa, Reino Unido, Australia, Singapur, India, China, Hong Kong, Japón, Corea y Sudáfrica. Solo se admiten las regiones con nube pública de Azure. Azure Government u otras nubes soberanas no se admiten.

### <a name="q-how-can-i-import-source-data-present-at-my-location-in-one-countryregion-to-an-azure-region-in-a-different-country"></a>Q. ¿Cómo puedo importar datos de origen presentes en mi ubicación de un país o región a una región de Azure en un país diferente?
A. Data Box Disk es compatible con la ingesta de datos solo dentro del mismo país o región de destino y no cruzará ninguna frontera internacional. La única excepción es en el caso de los pedidos dentro de la Unión Europea (UE), donde los discos Data Box Disk se pueden enviar desde y hasta cualquier país o región de la UE.

Por ejemplo, si quiere mover datos de su ubicación en Canadá a una cuenta de almacenamiento del Oeste de EE. UU. de Azure, podría conseguirlo de la siguiente manera:

#### <a name="option-1"></a>Opción 1: 

Envíe un [disco compatible](../import-export/storage-import-export-requirements.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#supported-disks) que contenga datos mediante el [servicio Azure Import/Export](../import-export/storage-import-export-service.md) desde la ubicación de origen en Canadá hasta el centro de datos del Oeste de EE. UU. de Azure.

#### <a name="option-2"></a>Opción 2:

1. Pida un dispositivo Data Box Disk en Canadá al seleccionar una cuenta de almacenamiento del Centro de Canadá. Los discos SSD se envían desde un centro de datos de Azure en Centro de Canadá a la dirección de envío (en Canadá) proporcionada durante la creación del pedido.

2. Después de copiar los datos del servidor local en los discos, devuélvalos al centro de datos de Azure en Canadá con las etiquetas de devolución proporcionadas por Microsoft. Los datos presentes en los discos Data Box Disk se cargarán en la cuenta de almacenamiento de destino en la región de Azure canadiense elegida durante la creación del pedido.

3. A continuación, puede usar una herramienta como AzCopy para copiar los datos en una cuenta de almacenamiento en la región del Oeste de EE. UU. Este paso incurre en [almacenamiento estándar](https://azure.microsoft.com/pricing/details/storage/) y [cargos de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/) que no se incluyen en la facturación de Data Box Disk.

#### <a name="q-does-data-box-disk-store-any-customer-data-outside-of-the-service-region"></a>Q. ¿Data Box Disk almacena los datos de clientes fuera de la región de servicio?

A. No. Data Box Disk no almacena los datos de clientes fuera de la región de servicio. El cliente tiene la propiedad total de sus datos y puede guardarlos en una ubicación determinada en función de la cuenta de almacenamiento que seleccione durante la creación del pedido.  

Además de los datos del cliente, hay datos de Data Box Disk que incluyen los metadatos y registros de supervisión. En todas las regiones (excepto el Sur de Brasil y el Sudeste Asiático), los datos de Data Box Disk se almacenan y replican en una [región emparejada](../best-practices-availability-paired-regions.md) a través de una cuenta de almacenamiento con redundancia geográfica como protección contra la pérdida de datos.  

Debido a los [requisitos de residencia de datos](https://azure.microsoft.com/global-infrastructure/data-residency/#more-information) del Sur de Brasil y el Sudeste Asiático, los datos de Data Box Disk se almacenan en una cuenta de almacenamiento con redundancia de zona (ZRS) a fin de contenerlos en una sola región. En el caso del Sudeste Asiático, todos los datos de Data Box Disk se almacenan en Singapur. En el caso del Sur de Brasil, se almacenan en Brasil. 

Si ocurre una interrupción del servicio en el Sur de Brasil y el Sudeste Asiático, los clientes pueden crear pedidos nuevos desde otra región. Los pedidos nuevos se atenderán desde la región en la que se crearon y los clientes serán responsables del envío desde y hasta Data Box Disk.



### <a name="q-how-can-i-recover-my-data-if-an-entire-region-fails"></a>Q. ¿Cómo puedo recuperar mis datos si se produce un error en toda una región?

A. En casos extremos en los que se pierde una región debido a un desastre importante, Microsoft puede iniciar una conmutación por error regional. En este caso, no se requieren acciones por su parte. El pedido se realizará a través de la región de conmutación por error si está dentro del mismo país o límite de comercio. Sin embargo, algunas regiones de Azure no tienen una región emparejada en el mismo límite geográfico o de comercio. Si se produce un desastre en cualquiera de esas regiones, tendrá que volver a crear el pedido de Data Box desde otra región que esté disponible y copiar los datos en Azure en la región nueva. Para más información, consulte [Continuidad empresarial y recuperación ante desastres (BCDR): Regiones emparejadas de Azure](../best-practices-availability-paired-regions.md).

### <a name="q-whom-should-i-contact-if-i-encounter-any-issues--with-data-box-disks"></a>Q. ¿Con quién debo ponerme en contacto si surge algún problemas con los discos de Data Box?
A. Si surge algún problema con los dispositivos de Data Box Disk, [póngase en contacto con el Soporte técnico de Microsoft](./data-box-disk-contact-microsoft-support.md).

## <a name="order-device"></a>Pedir el dispositivo

### <a name="q-how-do-i-get-data-box-disk"></a>Q. ¿Cómo se obtiene un dispositivo Data Box Disk? 
A.  Para obtener un dispositivo de Azure Data Box Disk, inicie sesión en Azure Portal y cree un pedido de Data Box Disk. Proporcione su información de contacto y los datos de notificación. Una vez realizado el pedido, y en función de la disponibilidad, Data Box Disk se envía en un plazo máximo de 10 días. Para obtener más información, vaya a [Order a Data Box](data-box-disk-deploy-ordered.md) (Pedir Data Box).

### <a name="q-i-couldnt-create-a-data-box-disk-order-in-the-azure-portal-why"></a>Q. No pude hacer un pedido de Data Box Disk en Azure Portal. ¿Por qué?
A. Si no pudo pedir un dispositivo Data Box Disk, es que hay un problema con su tipo de suscripción o con el acceso.

Compruebe su suscripción. Data Box Disk solo está disponible para las ofertas de suscripción de Contratos Enterprise (EA) y del Proveedor de soluciones en la nube (CSP). Si su suscripción no se encuentra entre ninguno de los tipos anteriores, póngase en contacto con el Soporte técnico de Microsoft para actualizar la suscripción.

Si tiene un tipo de oferta compatible para la suscripción, compruebe el nivel de acceso de la suscripción. Debe ser un colaborador o propietario de la suscripción para poder realizar un pedido.

### <a name="q-how-long-will-my-order-take-from-order-creation-to-data-uploaded-to-azure"></a>Q. ¿Cuánto tardará mi pedido desde su creación hasta que se carguen los datos en Azure?

A. Los siguientes plazos estimados para cada fase del procesamiento de pedidos le proporcionarán una idea precisa de lo que puede esperar.  

Recuerde que estos plazos son *estimaciones*. El tiempo de cada fase de procesamiento de pedidos se ve afectado por la carga del centro de recursos, los pedidos simultáneos y otras condiciones del entorno.

**Plazos estimados para una solicitud de Data Box Disk:**

1. Solicitud de Data Box Disk: Unos minutos, desde el portal.
2. Preparación y asignación del disco: Hasta 5 días laborables, en función de la disponibilidad de inventario y el número de pedidos pendientes que se van a procesar
3. Envío: 2 a 3 días laborables
4. Copia de datos en el sitio del cliente: Depende de la naturaleza de los datos, su tamaño y el número de archivos.
5. Envío de devolución: 2 a 3 días laborables
6. Procesamiento en el centro de datos y carga en Azure: La carga de datos comienza en el centro de datos en cuanto se completa el procesamiento operativo y se conecta el disco. El tiempo de carga depende de la naturaleza de los datos, su tamaño y el número de archivos.

## <a name="configure-and-connect"></a>Configuración y conexión
 
### <a name="q-can-i-specify-the-number-of-data-box-disks-in-the-order"></a>Q. ¿Se puede especificar el número de discos de Data Box en el pedido?
A.  No. Obtiene discos de 8 TB (un máximo de cinco discos) en función del tamaño de los datos y de la disponibilidad de los discos.  

### <a name="q-how-do-i-unlock-the-data-box-disks"></a>Q. ¿Cómo se desbloquean los discos de Data Box? 
A.  En Azure Portal, vaya a su pedido de discos de Data Box y, después, a **Detalles del dispositivo**. Copie la clave de paso. Descargue y extraiga la herramienta de desbloqueo Data Box Disk para su sistema operativo desde Azure Portal. Ejecute la herramienta en el equipo que contiene los datos que desea copiar en los discos. Escriba la clave de paso para desbloquear los discos. La misma clave desbloquea todos los discos. 

Para obtener instrucciones detalladas, consulte los temas donde se explica [cómo desbloquear discos en un cliente de Windows](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) o [cómo desbloquear discos en un cliente de Linux](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client).

### <a name="q-can-i-use-a-linux-host-computer-to-connect-and-copy-the-data-on-to-the-data-box-disks"></a>Q. ¿Se puede usar un equipo host Linux para conectarse y copiar los datos de los discos de Data Box?
A.  Sí. Tanto los clientes de Linux como los de Windows pueden utilizarse para conectar y copiar datos en los discos de Data Box Disk. Para más información, vaya a la lista de [sistemas operativos compatibles](data-box-disk-system-requirements.md) del equipo host.

### <a name="q-my-disks-are-dispatched-but-now-i-want-to-cancel-this-order-why-is-the-cancel-button-not-available"></a>Q. Quiero cancelar un pedido una vez que los discos se han enviado. ¿Por qué no está disponible el botón de cancelación?
A.  El pedido solo se puede cancelar durante el periodo que va entre la realización del pedido y el momento en que se realiza el envío. Una vez que los discos se han enviado, no de puede cancelar el pedido. Sin embargo, puede devolver los discos a un precio. 

### <a name="q-can-i-connect-multiple-data-box-disks-at-the-same-to-the-host-computer-to-transfer-data"></a>Q. ¿Se pueden conectar varios discos de Data Box al mismo al equipo host para transferir datos?
A. Sí. Se pueden conectar varios discos de Data Box al mismo equipo host para transferir datos y se pueden ejecutar varios trabajos de copia en paralelo.

## <a name="track-status"></a>Seguimiento del estado

### <a name="q-how-do-i-track-the-disks-from-when-i-placed-the-order-to-shipping-the-disks-back"></a>Q. ¿Cómo se realiza el seguimiento de los discos desde el momento en que se realizó el pedido a la devolución de los discos? 
A.  El seguimiento del estado del pedido de Data Box Disk se puede realizar desde Azure Portal. Al crear el pedido, también se le solicita que indique una dirección de correo electrónico para cualquier notificación. Si ya la ha especificado, los cambios de estado del pedido se le notificarán por correo electrónico. Para más información acerca de cómo [configurar correos electrónicos de notificación](data-box-portal-ui-admin.md#edit-notification-details).

### <a name="q-how-do-i-return-the-disks"></a>Q. ¿Cómo se devuelven los discos? 
A.  Microsoft incluye una etiqueta de envío con los discos de Data Box en el paquete en que estos se envían. Pegue dicha etiqueta en la caja de envío y lleve el paquete precintado a las instalaciones de la empresa que va a realizar el envío. Si la etiqueta se daña o se pierde, vaya a **Información general > Descargar la etiqueta de envío** y descargue una nueva etiqueta de envío de devolución.

### <a name="can-i-pick-up-my-data-box-disk-order-myself-can-i-return-the-disks-via-a-carrier-that-i-choose"></a>¿Puedo seleccionar mi pedido de Data Box Disk? ¿Puedo usar el transportista que prefiera para devolver los discos?
A. Sí. Microsoft también ofrece el envío autoadministrado, pero solo en la región US Gov. Al realizar el pedido de Data Box Disk, puede elegir la opción de envío autoadministrado. Para recoger el pedido de Data Box Disk, siga estos pasos:
    
1. Después de haber realizado el pedido, este se procesa y se preparan los discos. Se le enviará por correo electrónico una notificación cuando el pedido se pueda recoger. 
2. Una vez que el pedido esté listo para su recogida, vaya a él en Azure Portal y. después, a la hoja **Información general**. 
3. Verá una notificación con un código en Azure Portal. Envíe un correo electrónico al [equipo de operaciones de Azure Data Box](mailto:adbops@microsoft.com), en el que les indicará el código. El equipo proporcionará la ubicación y programará una fecha y hora de recogida. Debe llamar al equipo en un plazo máximo de cinco días laborables después de recibir la notificación por correo electrónico.

Una vez que completan la copia y la validación de los datos, siga estos pasos siguientes para devolver el disco:

1. Una vez completada la validación de datos, desconecte los discos. Quite los cables de conexión.
2. Envuelva todos los discos y los cables de conexión en un envoltorio de burbujas y colóquelos en la caja de envío. La falta de accesorios puede acarrear un costo.

    - Utilice el empaquetado que se usó en el envío inicial. Es aconsejable usar un envoltorio de burbujas bien protegido para empaquetar los discos.
    - Asegúrese de que todo encaja perfectamente para reducir los movimientos dentro de la caja.
3. Vaya a la **hoja Información general** del pedido en Azure Portal. Debería ver una notificación con un código.
4. Use ese código y envíe al [equipo de operaciones de Azure Data Box](mailto:adbops@microsoft.com) un correo electrónico con el código. El equipo le indicará dónde y cuándo puede dejar los discos.


## <a name="migrate-data"></a>Migración de los datos

### <a name="q-what-is-the-maximum-data-size-that-can-be-used-with-data-box-disks"></a>Q. ¿Cuál es el tamaño máximo de los datos que se puede usar con los discos de Data Box?  
A.  La solución Data Box Disks puede tener hasta cinco discos, con una capacidad máxima utilizable de 35 TB. Los discos en sí tienen 8 TB (7 TB utilizables).

### <a name="q-what-are-the-maximum-block-blob-and-page-blob-sizes-supported-by-data-box-disks"></a>Q. ¿Qué tamaño máximo de blobs en bloques y blobs en páginas admiten los discos de Data Box? 
A.  El tamaño máximo lo determinan los límites de Azure Storage. El blob en bloques máximo es, aproximadamente, de 4,768 TiB, mientras que el tamaño de blob en páginas máximo es de 8 TiB. Para más información, consulte [Objetivos de escalabilidad y rendimiento de Blob Storage](../storage/blobs/scalability-targets.md).

### <a name="q-what-is-the-data-transfer-speed-for-data-box-disks"></a>Q. ¿Cuál es la velocidad de transferencia de datos de los discos de Data Box?
A. Cuando se prueba con discos conectados mediante USB 3.0, el rendimiento del disco era de hasta 430 MB/s. Los números reales varían según el tamaño de archivo usado. Con archivos más pequeños, es posible que observe un rendimiento menor.

### <a name="q-how-do-i-know-that-my-data-is-secure-during-transit"></a>Q. ¿Cómo se sabe si los datos están protegidos durante el tránsito? 
A.  Con los discos de Data Box se emplea el cifrado BitLocker AES de 128 bits y la clave de paso solo está disponible en Azure Portal. Para obtenerla, debe iniciar sesión en Azure Portal con las credenciales de su cuenta. Escriba dicha clave al ejecutar la herramienta de desbloqueo de Data Box Disk.

### <a name="q-how-do-i-copy-the-data-to-the-data-box-disks"></a>Q. ¿Cómo se copian los datos a los discos de Data Box? 
A.  Use una herramienta de copia de SMB, como `Robocopy`, `Diskboss` o incluso el Explorador de archivos de Windows, para arrastrar y colocar los datos en los discos, y así copiarlos.

### <a name="q-are-there-any-tips-to-speed-up-the-data-copy"></a>Q. ¿Algunas sugerencia para agilizar la copia de datos?
A.  Para acelerar el proceso de copia:

- Use varias secuencias de copia de datos. Por ejemplo, si usa `Robocopy`, emplee la opción de varios procesos. Para más información sobre el comando exacto utilizado, vea [Tutorial: Copia de datos a Azure Data Box Disk y comprobación de los mismos](data-box-disk-deploy-copy-data.md#copy-data-to-disks).
- Utilice varias sesiones.
- En lugar de realizar la copia a través de un recurso compartido de red (en el que puede tener la limitación de la velocidad de la red), asegúrese de que los datos se encuentran localmente en el equipo al que se conectan los discos.
- Asegúrese de que usa USB 3.0 o una versión posterior durante todo el proceso de copia. Descargue la herramienta [`USBView`](/windows-hardware/drivers/debugger/usbview) y úsela para identificar los controladores y dispositivos USB conectados al equipo.
- Evalúe el rendimiento del equipo usado para copiar los datos. Descargue la [herramienta `Bluestop` `FIO`](https://ci.appveyor.com/project/axboe/fio) y úsela para evaluar el rendimiento del hardware del servidor. Seleccione la compilación x86 o x64 más reciente, seleccione la pestaña **Artefactos** y descargue el MSI.

### <a name="q-how-to-speed-up-the-data-if-the-source-data-has-small-files-kbs-or-few-mbs"></a>Q. ¿Cómo acelerar los datos si los datos de origen tienen archivos pequeños (de KB o pocos MB)?
A.  Para acelerar el proceso de copia:

- Cree un VHDx local en el almacenamiento rápido, o bien cree disco duro virtual vacío en la HDD o SSD (más lenta).
- Móntelo en una máquina virtual.
- Copie los archivos en el disco de la máquina virtual.

### <a name="q-can-i-use-multiple-storage-accounts-with-data-box-disks"></a>Q. ¿Se pueden usar varias cuentas de almacenamiento con los discos de Data Box?
A.  No. Actualmente, los discos de Data Box solo admiten una cuenta de almacenamiento, general o clásica. Se admiten blobs frecuentes y esporádicos.

### <a name="q-what-is-the-toolset-available-for-my-data-with-data-box-disks"></a>Q. ¿Qué contiene el conjunto de herramientas disponible para mis datos con Data Box Disk?
A. El conjunto de herramientas disponible con Data Box Disk contiene tres herramientas:
 - **Herramienta de desbloqueo de Data Box Disk**: Use esta herramienta para desbloquear los discos cifrados que se envían desde Microsoft. Cuando desbloquee los discos mediante la herramienta, deberá proporcionar una clave de paso disponible en el pedido de Data Box Disk en Azure Portal. 
 - **Herramienta de validación de Data Box Disk**: Use esta herramienta para validar el tamaño, el formato y los nombres de blob según las convenciones de nomenclatura de Azure. La herramienta también genera sumas de comprobación para los datos copiados que, posteriormente, se usan para comprobar los datos cargados en Azure.
 - **Herramienta de copia y división de Data Box Disk**: Use esta herramienta cuando utilice varios discos y tenga un conjunto de datos grande que es necesario dividir y copiar entre todos los discos. Esta herramienta está actualmente disponible para Windows. Esta herramienta no es compatible con los discos administrados. Esta herramienta valida los datos a medida que los copia, de modo que puede omitir el paso de validación al usarla.

El conjunto de herramientas está disponible para Windows y Linux. Puede descargar el conjunto de herramientas desde aquí:
- [Descargar conjunto de herramientas de Data Box Disk para Windows](https://aka.ms/databoxdisktoolswin) 
- [Descargar conjunto de herramientas de Data Box Disk para Linux](https://aka.ms/databoxdisktoolslinux)
 
### <a name="q-can-i-use-data-box-disk-to-transfer-data-to-azure-files-and-then-use-the-data-with-azure-file-sync"></a>Q. ¿Puedo usar Data Box Disk para transferir datos a Azure Files y, a continuación, usar los datos con Azure File Sync? 
A. Azure Files es compatible con Data Box Disk, pero no funcionará bien con Azure File Sync. Los metadatos no se conservan si los datos del archivo se utilizan con Azure File Sync.


## <a name="verify-and-upload"></a>Comprobación y carga

### <a name="q-how-soon-can-i-access-my-data-in-azure-once-ive-shipped-the-disks-back"></a>Q. ¿Cuándo se puede acceder a los datos en Azure una vez que se han devuelto los discos? 
A.  Una vez que el estado del pedido de Copia de datos indica que se ha completado, debería poder acceder a los datos de inmediato.

### <a name="q-where-is-my-data-located-in-azure-after-the-upload"></a>Q. ¿Dónde se encuentran los datos en Azure después de la carga?
A.  Cuando se copian los datos de las carpetas *BlockBlob* y *PageBlob* del disco, se crea un contenedor en la cuenta de Azure Storage para cada una de las subcarpetas de las carpetas *BlockBlob* y *PageBlob*. Si ha copiado los archivos de las carpetas *BlockBlob* y *PageBlob* directamente, estos se encontrarán en el contenedor predeterminado *$root* de la cuenta de Azure Storage. Al copiar los datos en una carpeta situada bajo la carpeta *AzureFile*, se crea un recurso compartido de archivos.

### <a name="q-i-just-noticed-that-i-didnt-follow-the-azure-naming-requirements-for-my-containers-will-my-data-fail-to-upload-to-azure"></a>Q. Acabo de darme cuenta de que no seguí los requisitos de nomenclatura de Azure en los contenedores. ¿Pueden aparecer errores al cargar los datos en Azure?
A. Las letras mayúsculas de los nombres de los contenedores se convierten automáticamente a minúsculas. Si los nombres no son compatibles por algún otro motivo (por ejemplo, porque tienen caracteres especiales, usan otros idiomas, etc.), se producirá un error en la carga. Para más información, vaya a [Convenciones de nomenclatura de Azure](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions).

### <a name="q-how-do-i-verify-the-data-i-copied-onto-multiple-data-box-disks"></a>Q. ¿Cómo se comprueban los datos que se han copiado en varios discos de Data Box?
A.  Una vez completada la copia de datos, puede ejecutar `DataBoxDiskValidation.cmd`, que se encuentra en la carpeta *DataBoxDiskImport*, para generar sumas de comprobación de validación. Si tiene varios discos, es preciso que abra una ventana Comandos por disco y que ejecute este comando. Tenga en cuenta que esta operación puede tardar mucho tiempo (horas) en función del tamaño de los datos.

### <a name="q-what-happens-to-my-data-after-i-have-returned-the-disks"></a>Q. ¿Qué ocurre con los datos después de que se devuelven los discos?
A.  Una vez que se completa la copia de datos en Azure, los datos de los discos se borran de forma segura según las directrices de la revisión 1 de NIST SP 800-88.  

### <a name="q-how-is-my-data-protected-during-transit"></a>Q. ¿Cómo se protegen los datos durante el tránsito? 
A.  Los dispositivos Data Box Disk se cifran con el cifrado de Microsoft BitLocker AES-128 y se requiere una sola clave de paso para desbloquear todos los discos y obtener acceso a los datos.

### <a name="q-do-i-need-to-rerun-checksum-validation-if-i-add-more-data-to-the-data-box-disks"></a>Q. ¿Es preciso volver a ejecutar la validación de suma de comprobación si se agregan más datos a los discos de Data Box?
A. Sí. Si decide validar los datos (es aconsejable que lo haga), tendrá que volver a ejecutar la validación si agrega más datos a los discos.

### <a name="q-i-used-all-my-disks-to-transfer-data-and-need-to-order-more-disks-is-there-a-way-to-quickly-place-the-order"></a>Q. He usado todos los discos para transferir datos y necesito pedir más discos. ¿Hay alguna manera rápida de realizar el pedido?
A. Puede clonar el pedido anterior. La clonación crea el mismo pedido que antes y permite editar los detalles del pedido solo sin necesidad de escribir los datos de dirección, contacto y notificación.

### <a name="q-i-copied-data-to-the-manageddisk-folder-i-dont-see-any-managed-disks-with-the-resource-group-specified-for-managed-disks-was-my-data-uploaded-to-azure-how-can-i-locate-it"></a>Q. He copiado datos en la carpeta ManagedDisk. No veo ningún disco administrado con el grupo de recursos especificado para los discos administrados. ¿Los datos se cargaron en Azure? ¿Cómo puedo encontrarlos?
A. Sí. Sus datos se cargaron en Azure, pero si no ve ningún disco administrado con los grupos de recursos especificados, probablemente se deba a que los datos no eran válidos. Si los blobs en páginas, blobs en bloques, Azure Files o los discos administrados no son válidos, estos irán a las siguientes carpetas:
 - Los blobs en páginas irán a un contenedor de blobs en bloques que empieza por *databoxdisk-invalid-pb-* .
 - Azure Files irá a un contenedor de blobs en bloques que empieza por *databoxdisk-invalid-af-* .
 - Los discos administrados irán a un contenedor de blob en bloques que empieza por *databoxdisk-invalid-md-* .

## <a name="next-steps"></a>Pasos siguientes

- Consulte los [requisitos del sistema de Data Box Edge](data-box-disk-system-requirements.md).
- Información acerca de los [límites de Data Box Disk](data-box-disk-limits.md).
- Implemente rápidamente [Azure Data Box Disk](data-box-disk-quickstart-portal.md) en Azure Portal.