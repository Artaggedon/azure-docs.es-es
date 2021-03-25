---
title: 'Empleo de un espacio aislado o emulador de Apache Hadoop: Azure HDInsight'
description: 'Para empezar a obtener información sobre el ecosistema de Apache Hadoop, puede configurar un espacio aislado de Hadoop desde Hortonworks en una máquina virtual de Azure. '
keywords: emulador de hadoop, espacio aislado de hadoop
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: how-to
ms.date: 05/29/2019
ms.openlocfilehash: 60f4e4998e1077ca373f47a0cdf35c9d7a064605
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104866682"
---
# <a name="get-started-with-an-apache-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a>Introducción a un espacio aislado de Apache Hadoop, un emulador en una máquina virtual

Aprenda a instalar el espacio aislado de Apache Hadoop desde Hortonworks en una máquina virtual para obtener información sobre el ecosistema de Hadoop. El espacio aislado proporciona un entorno de desarrollo local para comprender Hadoop, el sistema de archivos distribuido de Hadoop (HDFS) y el envío de trabajos. Cuando se haya familiarizado con Hadoop, puede empezar a usarlo en Azure mediante la creación de un clúster de HDInsight. Para obtener más información sobre cómo empezar, consulte [Tutorial de Hadoop: Introducción al uso de Hadoop en HDInsight basado en Linux](apache-hadoop-linux-tutorial-get-started.md).

## <a name="prerequisites"></a>Requisitos previos

* [Oracle VirtualBox](https://www.virtualbox.org/). Descárguelo e instálelo [aquí](https://www.virtualbox.org/wiki/Downloads).

## <a name="download-and-install-the-virtual-machine"></a>Descarga e instalación de la máquina virtual

1. Vaya a las [descargas de Cloudera](https://www.cloudera.com/downloads/hortonworks-sandbox/hdp.html).

1. Haga clic en **VIRTUALBOX** en **Elegir el tipo de instalación** para descargar el espacio aislado de Hortonworks más reciente en una máquina virtual. Inicie sesión o complete el formulario de interés del producto.

1. Haga clic en el botón **HDP SANDBOX (LATEST)** (Espacio aislado de HDP más reciente) para comenzar la descarga.

Para obtener instrucciones sobre cómo configurar el espacio aislado, consulte [Sandbox Deployment and Install Guide](https://hortonworks.com/tutorial/sandbox-deployment-and-install-guide/section/1/) (Guía de instalación e implementación del espacio aislado).

Para descargar un espacio aislado anterior de la versión de HDP, consulte los vínculos en **Versiones anteriores**.

## <a name="start-the-virtual-machine"></a>Inicio de la máquina virtual

1. Abra Oracle VM VirtualBox.
1. En el menú **File** (Archivo), haga clic en **Import Appliance** (Importar aplicación) y luego especifique la imagen de Hortonworks Sandbox.
1. Seleccione el espacio aislado Hortonworks Sandbox, haga clic en **Start** (Iniciar) y luego en **Normal Start** (Inicio normal). Una vez que la máquina virtual ha terminado el proceso de arranque, muestra instrucciones de inicio de sesión.

    :::image type="content" source="./media/apache-hadoop-emulator-get-started/virtualbox-normal-start.png" alt-text="Inicio normal del administrador de VirtualBox" border="true":::

1. Abra un explorador web y vaya a la dirección URL que se muestra (normalmente `http://127.0.0.1:8888`).

## <a name="set-sandbox-passwords"></a>Establecimiento de las contraseñas del espacio aislado

1. En el paso **get started** (introducción) de la página Sandbox de Hortonworks, seleccione **View Advanced Options** (Ver opciones avanzadas). Use la información de esta página para iniciar sesión en el espacio aislado mediante SSH. Utilice el nombre y la contraseña proporcionada.

   > [!NOTE]
   > Si no tiene instalado un cliente SSH, puede usar el SSH web que proporciona la máquina virtual en **http://localhost:4200/** .

    La primera vez que se conecte mediante SSH, se le pedirá que cambie la contraseña de la cuenta raíz. Escriba una contraseña nueva, que se usará al iniciar sesión mediante SSH.

2. Una vez que haya iniciado sesión, escriba el comando siguiente:

    ```bash
    ambari-admin-password-reset
    ```

    Cuando se le solicite, proporcione una contraseña para la cuenta de administración de Ambari. Esta se usará para tener acceso a la interfaz de usuario web de Ambari.

## <a name="use-hive-commands"></a>Uso de comandos de Hive

1. Desde una conexión SSH en el espacio aislado, utilice el siguiente comando para iniciar el shell de Hive:

    ```bash
    hive
    ```

2. Una vez que se inició el shell, use lo siguiente para ver las tablas que se proporcionan con el espacio aislado:

    ```hiveql
    show tables;
    ```

3. Utilice lo siguiente para recuperar 10 filas de la tabla `sample_07` :

    ```hiveql
    select * from sample_07 limit 10;
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a usar Visual Studio con Sandbox de Hortonworks](./apache-hadoop-visual-studio-tools-get-started.md)

* [Learning the ropes of the Hortonworks Sandbox](https://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)

* [Hadoop tutorial - Getting started with HDP](https://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)