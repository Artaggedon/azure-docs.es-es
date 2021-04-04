---
title: Tutorial de copia de datos en Azure Data Box mediante NFS | Microsoft Docs
description: Aprenda a copiar datos de Azure Data Box mediante NFS
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 12/18/2020
ms.author: alkohli
ms.openlocfilehash: 64bb5e94c4b18626d1f85d7e61252aae74202eb9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97680612"
---
# <a name="tutorial-copy-data-from-azure-data-box-via-nfs"></a>Tutorial: Copia de datos de Azure Data Box a través de NFS

En este tutorial se describe cómo conectar y copiar datos de la interfaz de usuario Web local de Data Box a un servidor de datos local a través de NFS. Los datos de Data Box se exportan desde la cuenta de Azure Storage.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Requisitos previos
> * Conexión a un dispositivo Data Box
> * Copiar datos de Data Box

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que:

1. Ha realizado el pedido de Azure Data Box.
    - Si necesita un pedido de importación, consulte el [Tutorial: Realización de pedidos de Azure Data Box](data-box-deploy-ordered.md).
    - En caso de un pedido de exportación, consulte el [Tutorial: Realización de pedidos de Azure Data Box](data-box-deploy-export-ordered.md).
2. Ha recibido su dispositivo Data Box y el estado del pedido en el portal se actualiza a **Delivered** (Entregado).
3. Tiene un equipo host en el que quiere copiar los datos de Data Box. El equipo host debe:
   * Ejecutar un [sistema operativo admitido](data-box-system-requirements.md).
   * Estar conectado a una red de alta velocidad. Es muy recomendable tener una conexión de 10 GbE como mínimo. Si no hay disponible una conexión 10 GbE, use un vínculo de datos de 1 GbE, pero las velocidades de copia se verán afectadas.

## <a name="connect-to-data-box"></a>Conexión a un dispositivo Data Box

[!INCLUDE [data-box-shares](../../includes/data-box-shares.md)]

Si usa un equipo host Linux, realice los pasos siguientes para configurar un dispositivo Data Box para que pueda acceder a los clientes NFS. Data Box puede conectar hasta cinco clientes NFS al mismo tiempo.

1. Proporcione las direcciones IP de los clientes que pueden acceder al recurso compartido:

    1.  En la interfaz de usuario web local, vaya a la página **Connect and copy** (Conectar y copiar). En **NFS settings** (Configuración de NFS), haga clic en **NFS client access** (Acceso de cliente NFS). 

        ![Permitir el acceso de clientes NFS](media/data-box-deploy-export-copy-data/nfs-client-access-1.png)

    1. Para agregar un cliente NFS, proporcione la dirección IP del cliente y haga clic en **Add** (Agregar). Data Box puede conectar hasta cinco clientes NFS al mismo tiempo. Cuando termine, haga clic en **OK** (Aceptar).

         ![Agregar un cliente NFS](media/data-box-deploy-export-copy-data/nfs-client-access-2.png)

2. Asegúrese de que el equipo host de Linux tiene instalada una [versión admitida](data-box-system-requirements.md) del cliente NFS. Use la versión específica para su distribución de Linux. 

3. Una vez instalado el cliente NFS, use el siguiente comando para montar el recurso compartido NFS en el dispositivo Data Box:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    En el ejemplo siguiente se muestra cómo conectarse mediante NFS a un recurso compartido de Data Box. La dirección IP del dispositivo Data Box es `10.161.23.130`; el recurso compartido `Mystoracct_Blob` se monta en la máquina virtual ubuntuVM y el punto de montaje es `/home/databoxubuntuhost/databox`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`
    
    Para los clientes de Mac, debe agregar una opción adicional como sigue: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`

    **Cree siempre una carpeta para los archivos que se va a copiar en el recurso compartido y, después, copie los archivos en ella**. La carpeta que se creó en los recursos compartidos de blob en bloques y blob en páginas representa un contenedor en el que los datos se cargan como blobs. No se pueden copiar los archivos directamente en la carpeta *root* de la cuenta de almacenamiento.

## <a name="copy-data-from-data-box"></a>Copia de datos de Data Box

Una vez que esté conectado a los recursos compartidos de Data Box, el siguiente paso es copiar los datos.

[!INCLUDE [data-box-export-review-logs](../../includes/data-box-export-review-logs.md)]

 Ahora puede comenzar la copia de datos. Si su equipo es un host Linux, use una utilidad de copia similar a Robocopy. Algunas de las alternativas disponibles en Linux son [`rsync`](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) o [Ultracopier](https://ultracopier.first-world.info/).  

El comando `cp` es una de las mejores opciones para copiar un directorio. Para más información sobre cómo usarlo, consulte las [páginas sobre cp](http://man7.org/linux/man-pages/man1/cp.1.html).

Si usa la opción `rsync` para una copia multiproceso, siga estas directrices:

* Instale el paquete **CIFS Utils** o **NFS Utils** según el sistema de archivos que use el cliente Linux.

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

* Instale `rsync` y **Parallel** (varía según la versión de la distribución de Linux).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

* Cree un punto de montaje.

    `sudo mkdir /mnt/databox`

* Monte el volumen.

    `sudo mount -t NFS4  //Databox IP Address/share_name /mnt/databox` 

* Refleje la estructura de directorios de carpetas.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databox`

* Copie los archivos.

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databox/{}`

     donde j especifica el número de la paralelización, X = número de copias en paralelo

     Se recomienda empezar con 16 copias en paralelo y aumentar el número de subprocesos según los recursos disponibles.

> [!IMPORTANT]
> No se admiten los siguientes tipos de archivos de Linux: vínculos simbólicos, archivos de caracteres, archivos de bloqueo, sockets y canalizaciones. Estos tipos de archivo generarán errores durante el paso **Preparación para el envío**.

Una vez completada la copia, vaya a **Panel** y compruebe el espacio utilizado y el espacio disponible en el dispositivo.

Ahora puede proceder al envío del dispositivo Data Box a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Data Box, como:

> [!div class="checklist"]
>
> * Requisitos previos
> * Conexión a un dispositivo Data Box
> * Copiar datos de Data Box

En el siguiente tutorial aprenderá a enviar su dispositivo Data Box a Microsoft.

> [!div class="nextstepaction"]
> [Envío de un dispositivo Data Box a Microsoft](./data-box-deploy-export-picked-up.md)
