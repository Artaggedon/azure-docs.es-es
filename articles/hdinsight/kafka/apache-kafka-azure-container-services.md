---
title: Uso de Azure Kubernetes Service con Kafka en HDInsight
description: Aprenda a usar Kafka en HDInsight desde imágenes de contenedor hospedadas en Azure Kubernetes Service (AKS).
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/04/2019
ms.openlocfilehash: d807b591229644984f6658cdacd0bf447759f292
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98933030"
---
# <a name="use-azure-kubernetes-service-with-apache-kafka-on-hdinsight"></a>Usar Azure Kubernetes Service con Apache Kafka en HDInsight

Aprenda a usar Azure Kubernetes Service (AKS) con [Apache Kafka](https://kafka.apache.org/) en un clúster de HDInsight. Los pasos descritos en este documento usan una aplicación de Node.js hospedada en AKS para comprobar la conectividad con Kafka. Esta aplicación usa el paquete [kafka-node](https://www.npmjs.com/package/kafka-node) para comunicarse con Kafka. Usa [Socket.io](https://socket.io/) para la mensajería basada en eventos entre el cliente del explorador y el servidor back-end hospedado en AKS.

[Apache Kafka](https://kafka.apache.org) es una plataforma de streaming distribuido de código abierto que se puede utilizar para compilar canalizaciones de datos de streaming en tiempo real y aplicaciones. Azure Kubernetes Service administra el entorno de Kubernetes hospedado y permite que sea rápido y fácil implementar en él aplicaciones en contenedor. Mediante una red virtual de Azure, puede conectar los dos servicios.

> [!NOTE]  
> Este documento se centra en los pasos necesarios para permitir que Azure Kubernetes Service se comunique con Kafka en HDInsight. El ejemplo en sí es simplemente un cliente de Kafka básico para demostrar que la configuración funciona.

## <a name="prerequisites"></a>Requisitos previos

* [CLI de Azure](/cli/azure/install-azure-cli)
* Una suscripción de Azure

En este documento se supone que está familiarizado con la creación y el uso de los siguientes servicios de Azure:

* Kafka en HDInsight
* Azure Kubernetes Service
* Instancias de Azure Virtual Network

También se supone que ha examinado el [tutorial de Azure Kubernetes Service](../../aks/tutorial-kubernetes-prepare-app.md). En este artículo se crea un servicio de contenedor, un clúster de Kubernetes y un registro de contenedor, y se configura la utilidad `kubectl`.

## <a name="architecture"></a>Architecture

### <a name="network-topology"></a>Topología de red

Tanto HDInsight como AKS usan una red virtual de Azure como contenedor para los recursos de proceso. Para permitir la comunicación entre HDInsight y AKS, debe habilitar la comunicación entre sus redes. En los pasos descritos en este documento se usa emparejamiento de red virtual a las redes. También deberían funcionar otras conexiones, por ejemplo, VPN. Para más información sobre el emparejamiento, consulte el documento [Emparejamiento de redes virtuales](../../virtual-network/virtual-network-peering-overview.md).

El diagrama siguiente ilustra la topología de red usada en este documento:

![HDInsight en una red virtual, AKS en otra, con emparejamiento](./media/apache-kafka-azure-container-services/kafka-aks-architecture.png)

> [!IMPORTANT]  
> No está habilitada la resolución de nombres entre las redes emparejadas, por lo que se usa el direccionamiento IP. De forma predeterminada, Kafka en HDInsight está configurado para devolver nombres de host en lugar de direcciones IP cuando se conectan los clientes. Los pasos descritos en este documento modifican Kafka para usar en su lugar el anuncio de direcciones IP.

## <a name="create-an-azure-kubernetes-service-aks"></a>Creación de Azure Kubernetes Service (AKS)

Si aún no dispone de un clúster AKS, use uno de los siguientes documentos para saber cómo crear uno:

* [Implementación de un clúster de Azure Kubernetes Service (AKS): Portal](../../aks/kubernetes-walkthrough-portal.md)
* [Implementación de un clúster de Azure Kubernetes Service (AKS): CLI](../../aks/kubernetes-walkthrough.md)

> [!IMPORTANT]  
> AKS crea una red virtual durante la instalación en un grupo de recursos **adicional**. El grupo de recursos adicional sigue la convención de nomenclatura de **MC_grupoDeRecursos_nombreDeClusterDeAKS_ubicación**.  
> Esta red se empareja con la creada para HDInsight en la sección siguiente.

## <a name="configure-virtual-network-peering"></a>Configuración del emparejamiento de red virtual

### <a name="identify-preliminary-information"></a>Identificación de la información preliminar

1. En [Azure Portal](https://portal.azure.com), ubique el **Grupos de recursos** adicional que contiene la red virtual del clúster de AKS.

2. En el grupo de recursos, seleccione el recurso __Red virtual__. Anote el nombre para usarlo más adelante.

3. En __Configuración__, seleccione **Espacio de direcciones**. Observe el espacio de direcciones que se muestra.

### <a name="create-virtual-network"></a>Creación de una red virtual

1. Para crear una red virtual para HDInsight, vaya a __+ Crear un recurso__ > __Redes__ > __Red virtual__.

1. Cree la red siguiendo las directrices a continuación para ciertas propiedades:

    |Propiedad | Value |
    |---|---|
    |Espacio de direcciones|Debe usar un espacio de direcciones que no se superponga con el usado por la red del clúster de AKS.|
    |Location|Use la misma __ubicación__ para la red virtual que la que usó para el clúster de AKS.|

1. Espere a que se cree la red virtual antes de pasar al siguiente paso.

### <a name="configure-peering"></a>Configuración del emparejamiento

1. Para configurar el emparejamiento entre la red de HDInsight y la red del clúster de AKS, seleccione la red virtual y, a continuación, seleccione __Emparejamientos__.

1. Seleccione __+ Agregar__ y use los siguientes valores para rellenar el formulario:

    |Propiedad |Value |
    |---|---|
    |Nombre del emparejamiento de \<this VN> con la red virtual remota|escriba un nombre único para esta configuración de emparejamiento.|
    |Virtual network|Seleccione la red virtual para el **clúster de AKS**.|
    |Nombre del emparejamiento de \<AKS VN> con \<this VN>|Escriba un nombre único.|

    Deje todos los demás campos en el valor predeterminado y, luego, seleccione __Aceptar__ para configurar el emparejamiento.

## <a name="create-apache-kafka-cluster-on-hdinsight"></a>Creación de un clúster de Apache Kafka en HDInsight

Al crear el clúster de Kafka en HDInsight, debe unir la red virtual que creó anteriormente para HDInsight. Para más información sobre cómo crear un clúster de Kafka, consulte el documento [Creación de un clúster de Apache Kafka](apache-kafka-get-started.md).

## <a name="configure-apache-kafka-ip-advertising"></a>Configurar el anuncio de direcciones IP de Apache Kafka

Use los pasos siguientes para configurar Kafka para anunciar direcciones IP en lugar de nombres de dominio:

1. Vaya a `https://CLUSTERNAME.azurehdinsight.net` desde un explorador web. Reemplace CLUSTERNAME por el nombre del clúster de Kafka en HDInsight.

    Cuando se le solicite, use el nombre de usuario y la contraseña HTTPS para el clúster. Aparece la interfaz de usuario web de Ambari para el clúster.

2. Para ver información sobre Kafka, seleccione __Kafka__ en la lista de la izquierda.

    ![Lista de servicios donde Kafka aparece resaltado](./media/apache-kafka-azure-container-services/select-kafka-service.png)

3. Para ver la configuración de Kafka, seleccione __Configs__ (Configuraciones) en la parte superior central.

    ![Configuración de servicios de Apache Ambari](./media/apache-kafka-azure-container-services/select-kafka-config1.png)

4. Para encontrar la configuración __kafka-env__, escriba `kafka-env` en el campo __Filtrar__ que se encuentra en la esquina superior derecha.

    ![Configuración de Kafka para kafka-env](./media/apache-kafka-azure-container-services/search-for-kafka-env.png)

5. Para configurar Kafka y anunciar direcciones IP, agregue el texto siguiente en la parte inferior del campo __kafka-env-template__:

    ```bash
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Para configurar la interfaz en que escucha Kafka, escriba `listeners` en el campo __Filtrar__ que se encuentra en la esquina superior derecha.

7. Para configurar Kafka para que escuche en todas las interfaces de red, cambie el valor del campo __listeners__ (agentes de escucha) a `PLAINTEXT://0.0.0.0:9092`.

8. Use el botón __Guardar__ para guardar los cambios en la configuración. Escriba un mensaje de texto para describir los cambios. Seleccione __Aceptar__ una vez que se guarden los cambios.

    ![Apache Ambari: guardar configuración](./media/apache-kafka-azure-container-services/save-configuration-button.png)

9. Para evitar errores al reiniciar Kafka, use el botón __Acciones de servicio__ y seleccione __Activar el modo de mantenimiento__. Seleccione Aceptar para completar esta operación.

    ![Acciones de servicio, en que Activar mantenimiento aparece resaltado](./media/apache-kafka-azure-container-services/turn-on-maintenance-mode.png)

10. Para reiniciar Kafka, use el botón __Reiniciar__ y seleccione __Restart All Affected__ (Reiniciar todos los elementos afectados). Confirme el reinicio y use el botón __Aceptar__ una vez que se complete la operación.

    ![Botón Reiniciar con la opción Restart All Affected resaltada](./media/apache-kafka-azure-container-services/restart-required-button.png)

11. Para deshabilitar el modo de mantenimiento, use el botón __Acciones de servicio__ y seleccione __Desactivar el modo de mantenimiento__. Seleccione **Aceptar** para completar esta operación.

## <a name="test-the-configuration"></a>Pruebe la configuración.

En este momento, Kafka y Azure Kubernetes Service se están comunicando a través de las redes virtuales emparejadas. Para probar esta conexión, siga estos pasos:

1. Cree un tema de Kafka que lo use la aplicación de prueba. Para obtener más información sobre cómo crear temas de Kafka, consulte el documento [Create an Apache Kafka cluster](apache-kafka-get-started.md) (Crear un clúster de Apache Kafka).

2. Descargue la aplicación de ejemplo de [https://github.com/Blackmist/Kafka-AKS-Test](https://github.com/Blackmist/Kafka-AKS-Test).

3. Edite el archivo `index.js` y cambie las líneas siguientes:

    * `var topic = 'mytopic'`: reemplace `mytopic` por el nombre del tema de Kafka usado por esta aplicación.
    * `var brokerHost = '176.16.0.13:9092`: reemplace `176.16.0.13` por la dirección IP interna de uno de los hosts agentes del clúster.

        Para encontrar la dirección IP interna de los host agentes (nodos de trabajo) en el clúster, consulte el documento sobre la [API REST de Apache Ambari](../hdinsight-hadoop-manage-ambari-rest-api.md#get-the-internal-ip-address-of-cluster-nodes). Seleccione la dirección IP de una de las entradas donde el nombre de dominio comience con `wn`.

4. Desde una línea de comandos del directorio `src`, instale dependencias y use Docker para crear una imagen para la implementación:

    ```bash
    docker build -t kafka-aks-test .
    ```

    > [!NOTE]  
    > Los paquetes que necesita esta aplicación se insertan en el repositorio, así que no tiene que usar la utilidad `npm` para instalarlos.

5. Inicie sesión en su instancia de Azure Container Registry (ACR) y busque el nombre de loginServer:

    ```azurecli
    az acr login --name <acrName>
    az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
    ```

    > [!NOTE]  
    > Si no conoce el nombre de su instancia de Azure Container Registry, o no sabe cómo usar la CLI de Azure para que funcione con Azure Kubernetes Service, consulte los [tutoriales de AKS](../../aks/tutorial-kubernetes-prepare-app.md).

6. Etiquete la imagen `kafka-aks-test` local con el nombre de loginServer de su ACR. Agregue `:v1` al final para indicar la versión de la imagen:

    ```bash
    docker tag kafka-aks-test <acrLoginServer>/kafka-aks-test:v1
    ```

7. Inserte la imagen en el registro:

    ```bash
    docker push <acrLoginServer>/kafka-aks-test:v1
    ```

    La operación tarda varios minutos en completarse.

8. Edite el archivo de manifiesto de Kubernetes (`kafka-aks-test.yaml`) y reemplace `microsoft` por el nombre de loginServer de ACR recuperado en el paso 4.

9. Use el comando siguiente para implementar la configuración de la aplicación del manifiesto:

    ```bash
    kubectl create -f kafka-aks-test.yaml
    ```

10. Use el siguiente comando para inspeccionar el valor de `EXTERNAL-IP` de la aplicación:

    ```bash
    kubectl get service kafka-aks-test --watch
    ```

    Cuando haya asignado una dirección IP externa, use __CTRL + C__ para salir de la inspección.

11. Abra un explorador web y escriba la dirección IP externa del servicio. Llegará a una página similar a la siguiente imagen:

    ![Apache Kafka: imagen de página web de prueba](./media/apache-kafka-azure-container-services/test-web-page-image1.png)

12. Escriba texto en el campo y, a continuación, seleccione el botón __Enviar__. Los datos se envían a Kafka. A continuación, el consumidor de Kafka en la aplicación lee el mensaje y lo agrega a la sección __Messages from Kafka__ (Mensajes de Kafka).

    > [!WARNING]  
    > Puede que reciba varias copias de un mensaje. Normalmente, este problema se produce al actualizar el explorador después de conectarse o al abrir varias conexiones de explorador a la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Use los vínculos siguientes para aprender a usar a Apache Kafka en HDInsight:

* [Introducción a Apache Kafka en HDInsight](apache-kafka-get-started.md)

* [Uso de MirrorMaker para crear una réplica de Apache Kafka en HDInsight](apache-kafka-mirroring.md)

* [Uso de Apache Storm con Apache Kafka en HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Uso de Apache Spark con Apache Kafka en HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Conexión a Apache Kafka a través de una instancia de Azure Virtual Network](apache-kafka-connect-vpn-gateway.md)
