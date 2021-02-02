---
title: Descripción de la facturación de Azure Files | Microsoft Docs
description: Aprenda a interpretar los modelos de facturación aprovisionado y de pago por uso para recursos compartidos de archivos de Azure.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 01/20/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 19ecbea70d9cb6b8cc31c72ed3c1294cd137ce93
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632485"
---
# <a name="understanding-azure-files-billing"></a>Descripción de la facturación de Azure Files
Azure Files proporciona dos modelos de facturación distintos: aprovisionado y pago por uso. El modelo aprovisionado solo está disponible para los recursos compartidos de archivos prémium, que son recursos compartidos de archivos implementados en el tipo de cuenta de almacenamiento **FileStorage**. El modelo de pago por uso solo está disponible para los recursos compartidos de archivos estándar, que son recursos compartidos de archivos implementados en el tipo de cuenta de almacenamiento de **uso general, versión 2 (GPv2)** . En este artículo se explica cómo funcionan ambos modelos con el fin de ayudarle a entender la factura mensual de Azure Files.

Los precios actuales de Azure Files pueden encontrarse en la [página de precios de Azure Files](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="provisioned-model"></a>Modelo aprovisionado
Azure Files usa un modelo aprovisionado para los recursos compartidos de archivos prémium. En un modelo de negocio aprovisionado, debe especificar de forma proactiva cuáles son los requisitos de almacenamiento del servicio Azure Files, en lugar de que se le aplique una factura basada en lo que usa. Esto es similar a la compra de hardware local, ya que cuando se aprovisiona un recurso compartido de archivos de Azure con una determinada cantidad de almacenamiento, se paga por ese almacenamiento independientemente de si se usa o no, de la misma manera que no se empiezan a pagar los costos de los soportes físicos locales al empezar a usar el espacio. A diferencia de la compra de soportes físicos locales, los recursos compartidos de archivos aprovisionados se pueden escalar o reducir verticalmente de forma dinámica en función de las características de rendimiento de almacenamiento y de E/S.

Al aprovisionar un recurso compartido de archivos prémium, se especifica la cantidad de GiB que requiere la carga de trabajo. Cada GiB aprovisionado permite beneficiarse de IOPS y rendimiento adicionales con una proporción fija. Además de la IOPS de línea de base garantizada, cada recurso compartido de archivos prémium admite ráfagas dentro de lo posible. Las fórmulas de IOPS y rendimiento son las siguientes:

- IOPS de línea base = 400+1 * GiB aprovisionados. (Hasta un máximo de 100 000 IOPS).
- Límite de ráfaga = MAX(4000, 3 * IOPS de línea de base).
- Velocidad de salida = 60 MiB/s + 0,06 * GiB aprovisionados.
- Velocidad de entrada = 40 MiB/s + 0,04 * GiB aprovisionados.

El tamaño aprovisionado del recurso compartido de archivos puede aumentar en cualquier momento, pero solo se puede reducir una vez transcurridas 24 horas desde el último aumento. Después de esperar 24 horas sin un aumento de cuota, puede reducir el recurso compartido tantas veces como quiera hasta que lo vuelva a aumentar. Los cambios de escala de IOPS/rendimiento se aplicarán minutos después del cambio de tamaño aprovisionado.

Es posible reducir el tamaño del recurso compartido aprovisionados por debajo de sus GiB usados. Si lo hace, no se perderán datos, pero se le seguirá facturando el tamaño usado y seguirá recibiendo el rendimiento (IOPS de línea de base, rendimiento e IOPS de ráfaga) del recurso compartido aprovisionado, no del tamaño utilizado.

En la tabla siguiente se ilustran algunos ejemplos de estas fórmulas para los tamaños de recursos compartidos aprovisionados:

|Capacidad (GiB) | IOPS base | IOPS de ráfaga | Salida (MiB/s) | Entrada (MiB/s) |
|---------|---------|---------|---------|---------|
|100         | 500     | Hasta 4000     | 66   | 44   |
|500         | 900     | Hasta 4000  | 90   | 60   |
|1024       | 1424   | Hasta 4000   | 122   | 81   |
|5120       | 5520   | Hasta 15 360  | 368   | 245   |
|10 240      | 10 640  | Hasta 30 720  | 675   | 450   |
|33 792      | 34 192  | Hasta 100 000 | 2088 | 1392   |
|51 200      | 51 600  | Hasta 100 000 | 3132 | 2088   |
|102 400     | 100 000 | Hasta 100 000 | 6204 | 4136   |

El rendimiento efectivo de los recursos compartidos de archivos está sujeto a los límites de red de la máquina, el ancho de banda de red disponible, los tamaños de E/S y el paralelismo, entre muchos otros factores. Por ejemplo, en función de las pruebas internas con tamaños de e/s de lectura/escritura de 8 KiB, una sola máquina virtual Windows sin SMB multicanal habilitado (*F16s_v2 estándar*) conectada al recurso compartido de archivos Premium a través de SMB podría alcanzar un valor de hasta 20 000 IOPS de lectura y 15 000 IOPS de escritura. Con tamaños de e/s de lectura/escritura de 512 MiB, la misma máquina virtual puede alcanzar un rendimiento de salida de 1,1 GiB/s y 370 MiB/s de entrada. El mismo cliente puede lograr un rendimiento hasta \~ tres veces superior si SMB multicanal está habilitado en los recursos compartidos Premium. Para lograr una escala de rendimiento máxima, [habilite SMB multicanal](storage-files-enable-smb-multichannel.md) y distribuya la carga entre varias máquinas virtuales. Consulte en el artículo sobre [rendimiento de SMB multicanal](storage-files-smb-multichannel-performance.md) y en la [ guía de solución de problemas](storage-troubleshooting-files-performance.md) algunos problemas de rendimiento comunes y sus soluciones alternativas.

### <a name="bursting"></a>Creación de ráfagas
Si la carga de trabajo necesita un rendimiento adicional para satisfacer los picos de demanda, el recurso compartido puede usar créditos de aumento para superar el límite de IOPS de la línea base del recurso compartido, con el fin de ofrecer el rendimiento de los recursos compartidos que necesita para cubrir la demanda. Los recursos compartidos de archivos Premium pueden aumentar su IOPS hasta 4000 o hasta un factor de tres, el valor que sea más alto. La creación de ráfagas está automatizada y funciona de acuerdo con un sistema de crédito. El aumento funciona en la medida de lo posible y su límite no es una garantía, los recursos compartidos de archivos pueden aumentar *hasta* el límite durante un máximo de 60 minutos.

Cada vez que el tráfico para el recurso compartido de archivos se encuentra por debajo del valor de IOPS de la línea de base, se acumulan créditos en un cubo de ráfagas. Por ejemplo, un recurso compartido de 100 GiB tiene un IOPS de línea base de 500. Si el tráfico real del recurso compartido era de 100 IOPS durante un intervalo específico de 1 segundo, el valor de 400 IOPS sin usar se agrega a un cubo de aumentos. Del mismo modo, un recurso compartido de 1 TiB inactivo acumula crédito de aumento en 1424 IOPS. Luego, estos créditos se usan más tarde si las operaciones superan el valor de IOPS de línea base.

Cada vez que un recurso compartido supera el valor de IOPS de línea base y tiene créditos en un cubo de aumentos, aumentará a la velocidad máxima permitida. Los recursos compartidos pueden seguir aumentando siempre y cuando queden créditos, hasta un máximo de 60 minutos, pero esto se basa en el número de créditos de aumento acumulados. Cada E/S que supere el valor de IOPS de línea base consume un crédito y, una vez que se consumen todos los créditos, el recurso compartido volvería al valor de IOPS de la línea base.

Los créditos de recursos compartidos tienen tres estados:

- Acumulado: cuando el recurso compartido de archivos usa un valor inferior al de IOPS de línea de base.
- Rechazado: cuando el recurso compartido de archivos usa más que el valor de IOPS de la línea base y en el modo de aumento.
- Constante: cuando el recurso compartido de archivos usa exactamente el valor de IOPS de línea de base, no hay créditos acumulados ni usados.

Los nuevos recursos compartidos de archivo empiezan con la cantidad total de créditos del cubo de ráfagas. Los créditos de ráfaga no se acumularán si el valor de IOPS del recurso compartido cae por debajo del valor de IOPS de la línea de base debido a una limitación del servidor.

## <a name="pay-as-you-go-model"></a>Modelo de pago por uso
Azure Files usa un modelo de negocio de pago por uso para los recursos compartidos de archivos estándar. En un modelo de negocio de pago por uso, la cantidad que se paga se determina según el volumen que se usa realmente, en lugar de basarse en una cantidad aprovisionada. En un nivel alto, se paga un costo por la cantidad de datos almacenados en el disco y, a continuación, un conjunto adicional de transacciones en función del uso de esos datos. Un modelo de pago por uso puede ser rentable, ya que no es necesario sobreaprovisionar para tener en cuenta los requisitos futuros de crecimiento o rendimiento ni el desaprovisionamiento si la carga de trabajo tiene una superficie de datos que varía con el tiempo. Por otro lado, un modelo de pago por uso también puede ser difícil de planear como parte de un proceso de presupuesto, porque este modelo se basa en el consumo del usuario final.

### <a name="differences-in-standard-tiers"></a>Diferencias en los niveles estándar
Al crear un recurso compartido de archivos estándar, puede elegir entre los niveles de transacción optimizada, acceso frecuente y acceso esporádico. Los tres niveles se almacenan en el mismo hardware de almacenamiento estándar. La principal diferencia de estos tres niveles son los precios de almacenamiento de datos en reposo, que son menores en los niveles de acceso esporádico, y los precios de las transacciones, que son más altos en estos mismos niveles. Esto significa lo siguiente:

- La transacción optimizada, como su nombre implica, optimiza el precio de las cargas de trabajo de mucha transacción. La transacción optimizada tiene el precio de almacenamiento de datos en reposo más alto, pero los precios de transacción más bajos.
- El acceso frecuente es para cargas de trabajo activas que no implican un gran número de transacciones, y tiene un precio de almacenamiento de datos en reposo ligeramente inferior, pero los precios de transacción son algo mayores, en comparación con la transacción optimizada. Considérelo como el punto medio entre los niveles de transacción optimizada y acceso esporádico.
- El acceso esporádico optimiza el precio de las cargas de trabajo que no tienen mucha actividad y ofrece el precio más bajo de datos en reposo, pero el más alto en las transacciones.

Si coloca una carga de trabajo a la que se accede con poca frecuencia en el nivel de transacción optimizada, no pagará casi nada por las pocas horas del mes en que realiza transacciones en el recurso compartido, pero pagará una cantidad elevada por los costos de almacenamiento de datos. Si tuviera que trasladar este mismo recurso compartido al nivel de acceso esporádico, tampoco pagaría casi nada por los costos de transacción, simplemente porque no realiza transacciones con mucha frecuencia en esta carga de trabajo, pero el nivel de acceso esporádico ofrece un precio de almacenamiento de datos mucho más barato. La selección del nivel adecuado para su caso de uso le permite reducir considerablemente los costos. La selección del nivel adecuado para su caso de uso le permite reducir considerablemente los costos.

Del mismo modo, si coloca en el nivel de acceso esporádico una carga de trabajo a la que accede con mucha frecuencia, incurrirá en muchos más costos por las transacciones, pero pagará menos por el almacenamiento de datos. Esto puede derivar en una situación en la que el aumento de los costos por los precios de las transacciones sobrepasan el ahorro obtenido por el precio más reducido del almacenamiento de datos, de tal forma que pagará más dinero en el nivel de acceso esporádico en comparación con el de transacción optimizada. Puede que, para algunos niveles de uso, mientras que el nivel de acceso frecuente será el nivel más rentable, el de acceso esporádico será más caro que el de transacción optimizada.

El nivel de la carga de trabajo y de la actividad determinarán el nivel más rentable del recurso compartido de archivos estándar. En la práctica, la mejor manera de elegir el nivel más rentable es examinar el consumo de recursos real del recurso compartido (datos almacenados, transacciones de escritura, etc.).

### <a name="what-are-transactions"></a>¿Qué son las transacciones?
Las transacciones son operaciones o solicitudes realizadas en Azure Files para cargar, descargar o manipular de otro modo el contenido del recurso compartido de archivos. Todas las acciones realizadas en un recurso compartido de archivos se traducen en una o varias transacciones, y en recursos compartidos estándar que usan el modelo de facturación de pago por uso, que genera costos por transacciones.

Hay cinco categorías de transacción básicas: escritura, lista, lectura, otras y eliminar. Todas las operaciones realizadas con la API de REST o SMB se agrupan en una de las cuatro categorías siguientes:

| Tipo de operación | Transacciones de escritura | Transacciones de lista | Transacciones de lectura | Otras transacciones | Transacciones de eliminación |
|-|-|-|-|-|-|
| Operaciones de administración | <ul><li>`CreateShare`</li><li>`SetFileServiceProperties`</li><li>`SetShareMetadata`</li><li>`SetShareProperties`</li></ul> | <ul><li>`ListShares`</li></ul> | <ul><li>`GetFileServiceProperties`</li><li>`GetShareAcl`</li><li>`GetShareMetadata`</li><li>`GetShareProperties`</li><li>`GetShareStats`</li></ul> | | <ul><li>`DeleteShare`</li></ul> |
| Operaciones de datos | <ul><li>`CopyFile`</li><li>`Create`</li><li>`CreateDirectory`</li><li>`CreateFile`</li><li>`PutRange`</li><li>`PutRangeFromURL`</li><li>`SetDirectoryMetadata`</li><li>`SetFileMetadata`</li><li>`SetFileProperties`</li><li>`SetInfo`</li><li>`SetShareACL`</li><li>`Write`</li><li>`PutFilePermission`</li></ul> | <ul><li>`ListFileRanges`</li><li>`ListFiles`</li><li>`ListHandles`</li></ul>  | <ul><li>`FilePreflightRequest`</li><li>`GetDirectoryMetadata`</li><li>`GetDirectoryProperties`</li><li>`GetFile`</li><li>`GetFileCopyInformation`</li><li>`GetFileMetadata`</li><li>`GetFileProperties`</li><li>`QueryDirectory`</li><li>`QueryInfo`</li><li>`Read`</li><li>`GetFilePermission`</li></ul> | <ul><li>`AbortCopyFile`</li><li>`Cancel`</li><li>`ChangeNotify`</li><li>`Close`</li><li>`Echo`</li><li>`Ioctl`</li><li>`Lock`</li><li>`Logoff`</li><li>`Negotiate`</li><li>`OplockBreak`</li><li>`SessionSetup`</li><li>`TreeConnect`</li><li>`TreeDisconnect`</li><li>`CloseHandles`</li><li>`AcquireFileLease`</li><li>`BreakFileLease`</li><li>`ChangeFileLease`</li><li>`ReleaseFileLease`</li></ul> | <ul><li>`ClearRange`</li><li>`DeleteDirectory`</li></li>`DeleteFile`</li></ul> |

> [!Note]  
> NFS 4.1 solo está disponible para los recursos compartidos de archivos prémium, que usan el modelo de facturación aprovisionado. Las transacciones no afectan a la facturación de los recursos compartidos de archivos prémium.

## <a name="see-also"></a>Consulte también
- [Página de precios de Azure Files](https://azure.microsoft.com/pricing/details/storage/files/)
- [Planeamiento de una implementación de Azure Files](./storage-files-planning.md) y [Planeamiento de una implementación de Azure File Sync](./storage-sync-files-planning.md)
- [Creación de un recurso compartido de archivos](./storage-how-to-create-file-share.md) e [Implementación de Azure File Sync](./storage-sync-files-deployment-guide.md)