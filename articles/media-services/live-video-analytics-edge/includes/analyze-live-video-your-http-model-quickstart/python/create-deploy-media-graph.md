---
ms.openlocfilehash: 7b9ed5dda925793eff4f85408c5eb494f5ac0925
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99569431"
---
### <a name="examine-and-edit-the-sample-files"></a>Examen y edición de los archivos de ejemplo

Como parte de los requisitos previos, ha descargado el código de ejemplo en una carpeta. Siga estos pasos para examinar y editar los archivos de ejemplo.

1. En Visual Studio Code, vaya a *src/edge*. Puede ver el archivo *.env* y algunos archivos de plantilla de implementación.

    La plantilla de implementación hace referencia al manifiesto de implementación del dispositivo perimetral. Incluye algunos valores de marcador de posición. El archivo *.env* incluye los valores de esas variables.

1. Vaya a la carpeta *src/cloud-to-device-console-app*. Aquí verá tanto su archivo *appsettings.json* como otros archivos:

    * operations.json: una lista de las operaciones que desea que ejecute el programa.
    * main.py: código del programa de ejemplo. Este código:

        * Carga la configuración de la aplicación.
        * Invoca los métodos directos que expone el módulo Live Video Analytics en IoT Edge. Puede usar el módulo para analizar secuencias de vídeo en directo mediante la invocación de sus métodos directos.
        * Se pone en pausa para que pueda examinar la salida del programa en la ventana **TERMINAL** y los eventos generados por el módulo en la ventana **SALIDA**.
        * Invoca los métodos directos para limpiar los recursos.  
1. Edite el archivo *operations.json*:
    * Cambie el vínculo a la topología del grafo:

        `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/httpExtension/2.0/topology.json"`

    * En `GraphInstanceSet`, edite el nombre de la topología del grafo para que coincida con el valor del vínculo anterior:

      `"topologyName" : "InferencingWithHttpExtension"`

    * En `GraphTopologyDelete`, edite el nombre:

      `"name": "InferencingWithHttpExtension"`

### <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>Generación e implementación del manifiesto de implementación de IoT Edge

1. Haga clic con el botón derecho en el archivo *src/edge/ deployment.yolov3.template.json* y seleccione **Generate IoT Edge Deployment Manifest** (Generar manifiesto de implementación de IoT Edge).

    ![Generación de un manifiesto de implementación de IoT Edge](../../../media/quickstarts/generate-iot-edge-deployment-manifest-yolov3.png)  

    El archivo de manifiesto *deployment.yolov3.amd64.json* se crea en la carpeta *src/edge/config*.

1. Si ha completado el inicio rápido [Detección de movimiento y emisión de eventos](../../../detect-motion-emit-events-quickstart.md), omita este paso. 

    Si no lo ha hecho, cerca del panel **AZURE IOT HUB**, en la esquina inferior izquierda, seleccione el icono **Más acciones** y, después, seleccione **Set IoT Hub Connection String** (Establecer cadena de conexión de IoT Hub). La cadena se puede copiar del archivo *appsettings.json*. O bien, para asegurarse de que ha configurado el centro de IoT adecuado en Visual Studio Code, use el [comando de selección de IoT Hub](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Select-IoT-Hub).
    
    ![Establecimiento de la cadena de conexión de IoT Hub](../../../media/quickstarts/set-iotconnection-string.png)

    > [!NOTE]
    > Es posible que se le pida que proporcione información del punto de conexión integrado del centro de IoT. Para obtener esa información, en Azure Portal, vaya a su centro de IoT y busque la opción **Puntos de conexión integrados** en el panel de navegación izquierdo. Haga clic ahí y busque el **punto de conexión compatible con el centro de eventos** en la sección **Punto de conexión compatible con el centro de eventos**. Copie y use el texto del cuadro. El punto de conexión será similar a este:  
        ```
        Endpoint=sb://iothub-ns-xxx.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX;EntityPath=<IoT Hub name>
        ```

1. Haga clic con el botón derecho en *src/edge/config/deployment.yolov3.amd64.json* y seleccione **Create Deployment for Single Device** (Crear una implementación para un dispositivo individual). 

    ![Crear una implementación para un dispositivo individual](../../../media/quickstarts/create-deployment-single-device.png)

1. Cuando se le pida que seleccione un dispositivo IoT Hub, seleccione **lva-sample-device**.
1. Tras aproximadamente 30 segundos, en la esquina inferior izquierda de la ventana, actualice Azure IoT Hub. El dispositivo perimetral ahora muestra los siguientes módulos implementados:

    * El módulo de Live Video Analytics, denominado **lvaEdge**.
    * El módulo **rtspsim**, que simula un servidor RTSP y actúa como el origen de una fuente de vídeo en directo.
        > [!NOTE]
        > En los pasos anteriores se supone que usa la máquina virtual creada mediante el script de configuración. Si usa su propio dispositivo perimetral, acceda a él y ejecute los siguientes comandos con **derechos de administrador** a fin de extraer y almacenar el archivo de vídeo de ejemplo que se usa en este inicio rápido:  
        
        ```
        mkdir /home/lvaadmin/samples
        mkdir /home/lvaadmin/samples/input    
        curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
        chown -R lvaadmin /home/lvaadmin/samples/  
        ```
    * El módulo **yolov3**, que es el modelo de detección de objetos YOLOv3 que aplica Computer Vision a las imágenes y devuelve varias clases de tipos de objeto
 
      ![Módulos que se implementan en el dispositivo perimetral](../../../media/quickstarts/yolov3.png)

### <a name="prepare-to-monitor-events"></a>Preparación para supervisar eventos

1. En Visual Studio Code, abra la pestaña **Extensiones** (o presione Ctrl + Mayús + X) y busque Azure IoT Hub.
1. Haga clic con el botón derecho y seleccione la **Configuración de la extensión**.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="../../../media/run-program/extensions-tab.png" alt-text="Configuración de la extensión":::
1. Busque y habilite "Show Verbose Message" (Mostrar mensaje detallado).

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="../../../media/run-program/show-verbose-message.png" alt-text="Show Verbose Message"::: (Mostrar mensaje detallado)
1. Haga clic con el botón derecho en el dispositivo de Live Video Analytics y seleccione **Start Monitoring Built-in Event Endpoint** (Iniciar supervisión del punto de conexión de eventos integrado). Este paso es necesario para supervisar los eventos de IoT Hub en la ventana **SALIDA** de Visual Studio Code. 

   ![Iniciar supervisión](../../../media/quickstarts/start-monitoring-iothub-events.png) 

> [!NOTE]
> Es posible que se le pida que proporcione información del punto de conexión integrado del centro de IoT. Para obtener esa información, en Azure Portal, vaya a su centro de IoT y busque la opción **Puntos de conexión integrados** en el panel de navegación izquierdo. Haga clic ahí y busque el **punto de conexión compatible con el centro de eventos** en la sección **Punto de conexión compatible con el centro de eventos**. Copie y use el texto del cuadro. El punto de conexión será similar a este:  
    ```
    Endpoint=sb://iothub-ns-xxx.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX;EntityPath=<IoT Hub name>
    ```
### <a name="run-the-sample-program"></a>Ejecución del programa de ejemplo

1. Para iniciar una sesión de depuración, seleccione la tecla F5. Verá mensajes impresos en la ventana **TERMINAL**.
1. El código de *operations.json* comienza con llamadas a los métodos directos `GraphTopologyList` y `GraphInstanceList`. Si ha limpiado los recursos tras haber completado los inicios rápidos anteriores, este proceso devolverá listas vacías y, después, se pausará. Para continuar, seleccione la tecla Entrar.

   ```
   --------------------------------------------------------------------------
   Executing operation GraphTopologyList
   -----------------------  Request: GraphTopologyList  --------------------------------------------------
   {
   "@apiVersion": "2.0"
   }
   ---------------  Response: GraphTopologyList - Status: 200  ---------------
   {
   "value": []
   }
   --------------------------------------------------------------------------
   Executing operation WaitForInput
   Press Enter to continue
   ```

    La ventana **TERMINAL** muestra el siguiente conjunto de llamadas al método directo:

     * Una llamada a `GraphTopologySet` que utiliza la instancia anterior de `topologyUrl`.
     * Una llamada a `GraphInstanceSet` que usa el cuerpo siguiente:

         ```
         {
           "@apiVersion": "2.0",
           "name": "Sample-Graph-1",
           "properties": {
             "topologyName": "InferencingWithHttpExtension",
             "description": "Sample graph description",
             "parameters": [
               {
                 "name": "rtspUrl",
                 "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
               },
               {
                 "name": "rtspUserName",
                 "value": "testuser"
               },
               {
                 "name": "rtspPassword",
                 "value": "testpassword"
               }
             ]
           }
         }
         ```

     * Una llamada a `GraphInstanceActivate` que inicia la instancia del grafo y el flujo de vídeo.
     * Una segunda llamada a `GraphInstanceList` que muestra que la instancia del grafo está en ejecución.
1. La salida de la ventana **TERMINAL** se pone en pausa tras el mensaje `Press Enter to continue`. No seleccione Entrar todavía. Desplácese hacia arriba para ver las cargas de la respuesta JSON para los métodos directos que ha invocado.
1. Cambie a la ventana **SALIDA** de Visual Studio Code. Verá los mensajes que el módulo Live Video Analytics en IoT Edge envía al centro de IoT. En la siguiente sección de este inicio rápido se analizan estos mensajes.
1. El grafo multimedia continúa ejecutándose e imprimiendo los resultados. El simulador RTSP sigue recorriendo el vídeo de origen. Para detener el grafo multimedia, vuelva a la ventana **TERMINAL** y seleccione Entrar. 

    La siguiente serie de llamadas limpia los recursos:
      * Una llamada a `GraphInstanceDeactivate` desactiva la instancia del grafo.
      * Una llamada a `GraphInstanceDelete` elimina la instancia.
      * Una llamada a `GraphTopologyDelete` elimina la topología.
      * Una llamada final a `GraphTopologyList` muestra que la lista está vacía.
