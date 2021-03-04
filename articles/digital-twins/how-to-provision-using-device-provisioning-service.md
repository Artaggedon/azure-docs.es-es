---
title: Administración automática de dispositivos con DPS
titleSuffix: Azure Digital Twins
description: Consulte cómo configurar un proceso automatizado para aprovisionar y retirar dispositivos IoT en Azure Digital Twins mediante Device Provisioning Service (DPS).
author: baanders
ms.author: baanders
ms.date: 9/1/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 24dac044982d59e93da17ee75190f378d5e3cdea
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102050926"
---
# <a name="auto-manage-devices-in-azure-digital-twins-using-device-provisioning-service-dps"></a>Administración automática de dispositivos en Azure Digital Twins mediante Device Provisioning Service (DPS)

En este artículo, aprenderá a integrar Azure Digital Twins con [Device Provisioning Service (DPS)](../iot-dps/about-iot-dps.md).

La solución que se describe en este artículo le permitirá automatizar el proceso de **_aprovisionar_** y **_retirar_** dispositivos IoT Hub de Azure Digital Twins mediante Device Provisioning Service. 

Para más información sobre las etapas de _aprovisionamiento_ y _retirada_ y para comprender mejor el conjunto de etapas generales de administración de dispositivos que son comunes a todos los proyectos de IoT empresarial, consulte la sección [*Ciclo de vida de dispositivo*](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle) de la documentación de administración de dispositivos IoT Hub.

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar el aprovisionamiento, debe tener una **instancia de Azure Digital Twins** que contenga modelos y dispositivos gemelos. Esta instancia también debe estar configurada con la posibilidad de actualizar la información de los gemelos digitales según los datos. 

Si todavía no tiene esta configuración, puede crearla si sigue las instrucciones sobre Azure Digital Twins que aparecen en [*Tutorial: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md). El tutorial le guiará a través de la configuración de una instancia de Azure Digital Twins con modelos y dispositivos gemelos, una instancia conectada de [Azure IoT Hub](../iot-hub/about-iot-hub.md) y varias [funciones de Azure](../azure-functions/functions-overview.md) para propagar el flujo de datos.

Más adelante en este artículo, necesitará los valores siguientes que obtuvo al configurar la instancia. Si necesita volver a recopilar estos valores, use los vínculos siguientes para obtener instrucciones.
* El **_nombre de host_** de la instancia de Azure Digital Twins ([que se encuentra en el portal](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values))
* La cadena de conexión de Azure Event Hubs **_cadena de conexión_** ([que se encuentra en el portal](../event-hubs/event-hubs-get-connection-string.md#get-connection-string-from-the-portal))

En este ejemplo también se usa un **simulador de dispositivos** que incluye el aprovisionamiento mediante Device Provisioning Service. El simulador de dispositivos se encuentra aquí: [Ejemplo de integración de Azure Digital Twins e IoT Hub](/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/). Para obtener el proyecto de ejemplo en la máquina, vaya al vínculo del ejemplo y seleccione el botón *Descargar ZIP* situado debajo del título. Descomprima la carpeta descargada.

El simulador de dispositivos se basa en **Node.js**, versión 10.0.x u otra posterior. En [*Preparación del entorno de desarrollo*](https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md) se describe cómo instalar Node.js para este tutorial en Windows o Linux.

## <a name="solution-architecture"></a>Arquitectura de la solución

En la imagen siguiente se muestra la arquitectura de esta solución mediante Azure Digital Twins con Device Provisioning Service. Muestra el flujo de aprovisionamiento y retirada de dispositivos.

:::image type="content" source="media/how-to-provision-using-dps/flows.png" alt-text="Una vista de un dispositivo y varios servicios de Azure en un escenario completo. Los datos fluyen en ambos sentidos entre un dispositivo termostato y DPS. Los datos también fluyen desde DPS a IoT Hub y a Azure Digital Twins a través de una función de Azure con la etiqueta &quot;Asignación&quot;. Los datos de una acción manual de &quot;Eliminar dispositivo&quot; fluyen a través de IoT Hub > Event Hubs > Azure Functions > Azure Digital Twins.":::

Este artículo se divide en dos secciones:
* [*Aprovisionamiento automático de un dispositivo mediante Device Provisioning Service*](#auto-provision-device-using-device-provisioning-service)
* [*Retirada automática del dispositivo mediante eventos de ciclo de vida de IoT Hub*](#auto-retire-device-using-iot-hub-lifecycle-events)

Para obtener una explicación más detallada de cada paso de la arquitectura, consulte sus secciones individuales más adelante en el artículo.

## <a name="auto-provision-device-using-device-provisioning-service"></a>Aprovisionamiento automático de un dispositivo mediante Device Provisioning Service

En esta sección, va a conectar Device Provisioning Service a Azure Digital Twins para aprovisionar automáticamente los dispositivos a través de la ruta de acceso siguiente. Este es un extracto de la arquitectura completa que se ha mostrado [anteriormente](#solution-architecture).

:::image type="content" source="media/how-to-provision-using-dps/provision.png" alt-text="Flujo de aprovisionamiento: un extracto del diagrama de la arquitectura de la solución, con números que etiquetan las secciones del flujo. Los datos fluyen en ambos sentidos entre un dispositivo de termostato y DPS. (1 para dispositivo > DPS y 5 para DPS > dispositivo). Los datos también fluyen desde DPS a IoT Hub (4) y a Azure Digital Twins (3) a través de una función de Azure con la etiqueta &quot;Asignación&quot;.":::

Esta es una descripción del flujo del proceso:
1. El dispositivo se pone en contacto con el punto de conexión de DPS y pasa la información de identificación para demostrar su identidad.
2. DPS valida la identidad del dispositivo mediante la validación del identificador y la clave de registro en comparación con la lista de inscripción y llama a una [función de Azure](../azure-functions/functions-overview.md) para realizar la asignación.
3. La función de Azure crea un nuevo [gemelo](concepts-twins-graph.md) en Azure Digital Twins para el dispositivo.
4. DPS registra el dispositivo con un centro de IoT y rellena el estado deseado de los dispositivos gemelos.
5. El centro de IoT devuelve la información sobre la identidad del dispositivo y la conexión del centro de IoT al dispositivo. Ahora el dispositivo puede conectarse al centro de IoT.

En las siguientes secciones se describen los pasos necesarios para configurar este flujo de aprovisionamiento automático del dispositivo.

### <a name="create-a-device-provisioning-service"></a>Creación de una instancia de Device Provisioning Service

Cuando se aprovisiona un nuevo dispositivo mediante Device Provisioning Service, se puede crear un nuevo dispositivo gemelo para ese dispositivo en Azure Digital Twins.

Cree una instancia de Device Provisioning Service que se usará para aprovisionar dispositivos IoT. Puede usar las instrucciones de la CLI de Azure que aparecen a continuación o usar Azure Portal: [*Guía de inicio rápido: Configuración de Azure IoT Hub Device Provisioning Service con Azure Portal*](../iot-dps/quick-setup-auto-provision.md)

El siguiente comando de la CLI de Azure creará una instancia de Device Provisioning Service. Tendrá que especificar un nombre, un grupo de recursos y una región. El comando se puede ejecutar en [Cloud Shell](https://shell.azure.com), o localmente si tiene la [CLI de Azure instalada en la máquina](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true).

```azurecli-interactive
az iot dps create --name <Device Provisioning Service name> --resource-group <resource group name> --location <region; for example, eastus>
```

### <a name="create-an-azure-function"></a>Creación de una función de Azure

A continuación, creará una función desencadenada mediante una solicitud HTTP dentro de una aplicación de funciones. Puede usar la aplicación de funciones creada en el tutorial integral ([*Tutorial: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md)) o una propia.

Device Provisioning Service usará esta función en una [directiva de asignación personalizada](../iot-dps/how-to-use-custom-allocation-policies.md) para aprovisionar un nuevo dispositivo. Para más información sobre cómo usar solicitudes HTTP con funciones de Azure, consulte [*Desencadenador de solicitudes HTTP para Azure Functions*](../azure-functions/functions-bindings-http-webhook-trigger.md).

En el proyecto de la aplicación de funciones, agregue una nueva función. Agregue también un nuevo paquete NuGet al proyecto: `Microsoft.Azure.Devices.Provisioning.Service`.

En el archivo de código de la función recién creada, pegue el código siguiente.

:::code language="csharp" source="~/digital-twins-docs-samples-dps/functions/DpsAdtAllocationFunc.cs":::

Guarde el archivo y, a continuación, vuelva a publicar la aplicación de funciones. Para obtener instrucciones sobre la publicación de la aplicación de funciones, consulte la sección [*Publicación de la aplicación*](tutorial-end-to-end.md#publish-the-app) del tutorial general.

### <a name="configure-your-function"></a>Configuración de la función

A continuación, tendrá que establecer variables de entorno en la aplicación de funciones anterior que contiene la referencia a la instancia de Azure Digital Twins que ha creado. Si ha empleado el tutorial general ([*Tutorial: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md)), la configuración ya estará establecida.

Agregue la configuración con este comando de la CLI de Azure:

```azurecli-interactive
az functionapp config appsettings set --settings "ADT_SERVICE_URL=https://<Azure Digital Twins instance _host name_>" -g <resource group> -n <your App Service (function app) name>
```

Asegúrese de que los permisos y la asignación de roles de identidad administrada están configurados correctamente para la aplicación de funciones, como se indica en la sección [*Asignación de permisos a la aplicación de funciones*](tutorial-end-to-end.md#assign-permissions-to-the-function-app) en el tutorial general.

### <a name="create-device-provisioning-enrollment"></a>Creación de una inscripción de Device Provisioning Service

A continuación, deberá crear una inscripción en Device Provisioning Service mediante una **función de asignación personalizada**. Siga las instrucciones para realizar esta tarea que se encuentran en las secciones [*Creación de la inscripción*](../iot-dps/how-to-use-custom-allocation-policies.md#create-the-enrollment) y [*Derivación de las claves de dispositivo únicas*](../iot-dps/how-to-use-custom-allocation-policies.md#derive-unique-device-keys) del artículo sobre Device Provisioning Service acerca de las directivas de asignación personalizadas.

Mientras pasa por ese flujo, vinculará la inscripción a la función que acaba de crear seleccionando la función durante el paso para **seleccionar cómo desea asignar los dispositivos a los centros**. Después de crear la inscripción, el nombre de la inscripción y la clave SAS principal o secundaria se usarán más adelante para configurar el simulador de dispositivos de este artículo.

### <a name="set-up-the-device-simulator"></a>Configuración del simulador de dispositivos

En este ejemplo se usa un simulador de dispositivos que incluye el aprovisionamiento mediante Device Provisioning Service. El simulador de dispositivos se encuentra aquí: [Ejemplo de integración de Azure Digital Twins e IoT Hub](/samples/azure-samples/digital-twins-iothub-integration/adt-iothub-provision-sample/). Si no ha descargado aún el ejemplo, hágalo ahora. Para ello, vaya al vínculo de ejemplo y seleccione el botón *Descargar archivo ZIP* que aparece debajo del título. Descomprima la carpeta descargada.

Abra una ventana de comandos y vaya a la carpeta descargada y, después, al directorio *device-simulator*. Instale las dependencias del proyecto mediante el siguiente comando:

```cmd
npm install
```

A continuación, copie el archivo *.env.template* en un nuevo archivo denominado *.env* y rellene estos valores:

```cmd
PROVISIONING_HOST = "global.azure-devices-provisioning.net"
PROVISIONING_IDSCOPE = "<Device Provisioning Service Scope ID>"
PROVISIONING_REGISTRATION_ID = "<Device Registration ID>"
ADT_MODEL_ID = "dtmi:contosocom:DigitalTwins:Thermostat;1"
PROVISIONING_SYMMETRIC_KEY = "<Device Provisioning Service enrollment primary or secondary SAS key>"
```

Guarde y cierre el archivo.

### <a name="start-running-the-device-simulator"></a>Iniciar ejecución del simulador de dispositivos

Todavía en el directorio *device-simulator* de la ventana de comandos, inicie el simulador de dispositivos mediante el comando siguiente:

```cmd
node .\adt_custom_register.js
```

Debería ver que el dispositivo se registra y se conecta a IoT Hub y, a continuación, comienza a enviar mensajes.
:::image type="content" source="media/how-to-provision-using-dps/output.png" alt-text="Ventana Comandos que muestra el registro de dispositivos y el envío de mensajes":::

### <a name="validate"></a>Validación

Como resultado del flujo que ha configurado en este artículo, el dispositivo se registrará automáticamente en Azure Digital Twins. Use el siguiente comando de la [CLI de Azure Digital Twins](how-to-use-cli.md) para buscar el gemelo del dispositivo de la instancia de Azure Digital Twins que ha creado.

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id <Device Registration ID>"
```

Debería ver el gemelo del dispositivo que se encuentra en la instancia de Azure Digital Twins.
:::image type="content" source="media/how-to-provision-using-dps/show-provisioned-twin.png" alt-text="Ventana Comandos que muestra el gemelo recién creado":::

## <a name="auto-retire-device-using-iot-hub-lifecycle-events"></a>Retirada automática del dispositivo mediante eventos de ciclo de vida de IoT Hub

En esta sección, va a asociar los eventos de ciclo de vida de IoT Hub a Azure Digital Twins para retirar automáticamente los dispositivos a través de la ruta de acceso siguiente. Este es un extracto de la arquitectura completa que se ha mostrado [anteriormente](#solution-architecture).

:::image type="content" source="media/how-to-provision-using-dps/retire.png" alt-text="Flujo de retirada de dispositivo: un extracto del diagrama de la arquitectura de la solución, con números que etiquetan las secciones del flujo. El dispositivo de termostato aparece sin conexiones con los servicios de Azure en el diagrama. Los datos de una acción manual de &quot;Eliminar dispositivo&quot; fluyen a través de IoT Hub (1) > Event Hubs (2) > Azure Functions > Azure Digital Twins (3).":::

Esta es una descripción del flujo del proceso:
1. Un proceso externo o manual desencadena la eliminación de un dispositivo en IoT Hub.
2. IoT Hub elimina el dispositivo y genera un evento de [ciclo de vida de dispositivo](../iot-hub/iot-hub-device-management-overview.md#device-lifecycle) que se va a enrutar a un [centro de eventos](../event-hubs/event-hubs-about.md).
3. Una función de Azure elimina el gemelo del dispositivo en Azure Digital Twins.

En las siguientes secciones se describen los pasos necesarios para configurar este flujo de retirada automática del dispositivo.

### <a name="create-an-event-hub"></a>Creación de un centro de eventos

Ahora debe crear un [centro de eventos](../event-hubs/event-hubs-about.md) de Azure que se usará para recibir los eventos de ciclo de vida de IoT Hub. 

Siga los pasos descritos en el inicio rápido [*Creación de un centro de eventos*](../event-hubs/event-hubs-create.md) mediante la siguiente información:
* Si usa el tutorial general ([*Tutorial: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md)), puede volver a usar el grupo de recursos que creó para dicho tutorial.
* Asigne el nombre *lifecycleevents* a su centro de eventos, o cualquier otro de su elección, y recuerde el espacio de nombres que ha creado. Los usará cuando configure la función de ciclo de vida y la ruta de IoT Hub en las secciones siguientes.

### <a name="create-an-azure-function"></a>Creación de una función de Azure

A continuación, creará una función desencadenada por Event Hubs dentro de una aplicación de funciones. Puede usar la aplicación de funciones creada en el tutorial integral ([*Tutorial: Conexión de una solución de un extremo a otro*](tutorial-end-to-end.md)) o una propia. 

Asigne el nombre *lifecycleevents* al desencadenador del centro de eventos y conéctelo al centro de eventos que creó en el paso anterior. Si ha usado un nombre de centro de eventos diferente, cámbielo para que coincida con el nombre del desencadenador que aparece a continuación.

Esta función usará el evento del ciclo de vida del dispositivo IoT Hub para retirar un dispositivo existente. Para más información sobre los eventos de ciclo de vida, consulte [*Eventos que no son de telemetría de IoT Hub*](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events). Para más información sobre cómo usar Event Hubs con funciones de Azure, consulte [*Desencadenador de Azure Event Hubs para Azure Functions*](../azure-functions/functions-bindings-event-hubs-trigger.md).

Dentro de la aplicación de funciones publicada, agregue una nueva clase de función del tipo *Desencadenador de centro de eventos* y pegue el código siguiente.

:::code language="csharp" source="~/digital-twins-docs-samples-dps/functions/DeleteDeviceInTwinFunc.cs":::

Guarde el proyecto y, a continuación, vuelva a publicar la aplicación de funciones. Para obtener instrucciones sobre la publicación de la aplicación de funciones, consulte la sección [*Publicación de la aplicación*](tutorial-end-to-end.md#publish-the-app) del tutorial general.

### <a name="configure-your-function"></a>Configuración de la función

A continuación, tendrá que establecer variables de entorno en la aplicación de funciones anterior que contiene la referencia a la instancia de Azure Digital Twins que ha creado y al centro de eventos. Si ha empleado el tutorial general ([*Tutorial: Conexión de una solución de un extremo a otro*](./tutorial-end-to-end.md)), el primer valor ya estará configurado.

Agregue la configuración con este comando de la CLI de Azure. El comando se puede ejecutar en [Cloud Shell](https://shell.azure.com), o localmente si tiene la [CLI de Azure instalada en la máquina](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true).

```azurecli-interactive
az functionapp config appsettings set --settings "ADT_SERVICE_URL=https://<Azure Digital Twins instance _host name_>" -g <resource group> -n <your App Service (function app) name>
```

A continuación, tendrá que configurar la variable de entorno de la función para conectarse al centro de eventos recién creado.

```azurecli-interactive
az functionapp config appsettings set --settings "EVENTHUB_CONNECTIONSTRING=<Event Hubs SAS connection string Listen>" -g <resource group> -n <your App Service (function app) name>
```

Asegúrese de que los permisos y la asignación de roles de identidad administrada están configurados correctamente para la aplicación de funciones, como se indica en la sección [*Asignación de permisos a la aplicación de funciones*](tutorial-end-to-end.md#assign-permissions-to-the-function-app) en el tutorial general.

### <a name="create-an-iot-hub-route-for-lifecycle-events"></a>Creación de una ruta de IoT Hub para eventos de ciclo de vida

Ahora debe configurar una ruta de IoT Hub para enrutar los eventos de ciclo de vida del dispositivo. En este caso, escuchará específicamente los eventos de eliminación de dispositivos, que se identifican mediante `if (opType == "deleteDeviceIdentity")`. Esto desencadenará la eliminación del elemento gemelo digital, finalizando la retirada del dispositivo y de su gemelo digital.

Las instrucciones para crear una ruta de IoT Hub se detallan en este artículo: [*Uso del enrutamiento de mensajes de IoT Hub para enviar mensajes del dispositivo a la nube a distintos puntos de conexión*](../iot-hub/iot-hub-devguide-messages-d2c.md). La sección *Eventos que no son de telemetría* indica que puede usar **eventos de ciclo de vida del dispositivo** como origen de datos de la ruta.

Los pasos que debe seguir para esta configuración son:
1. Creación de un punto de conexión personalizado del centro de eventos de IoT Hub. Este punto de conexión debe tener como destino el centro de eventos que creó en la sección [*Creación de un centro de eventos*](#create-an-event-hub).
2. Agregue una ruta a *Eventos de ciclo de vida del dispositivo*. Use el punto de conexión que creó en el paso anterior. Puede limitar los eventos de ciclo de vida del dispositivo para que envíe solo los eventos de eliminación agregando la consulta de enrutamiento `opType='deleteDeviceIdentity'`.
    :::image type="content" source="media/how-to-provision-using-dps/lifecycle-route.png" alt-text="Agregar una ruta":::

Una vez que haya pasado por este flujo, todo está listo para la completa retirada de los dispositivos.

### <a name="validate"></a>Validación

Para desencadenar el proceso de retirada, debe eliminar manualmente el dispositivo de IoT Hub.

En la [primera mitad de este artículo](#auto-provision-device-using-device-provisioning-service), creó un dispositivo en IoT Hub y un dispositivo gemelo digital correspondiente. 

Ahora, vaya a IoT Hub y elimine ese dispositivo (puede hacerlo con un [comando de la CLI de Azure](/cli/azure/ext/azure-iot/iot/hub/module-identity?view=azure-cli-latest&preserve-view=true#ext_azure_iot_az_iot_hub_module_identity_delete) o en [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Devices%2FIotHubs)). 

El dispositivo se eliminará automáticamente de Azure Digital Twins. 

Use el siguiente comando de la [CLI de Azure Digital Twins](how-to-use-cli.md) para comprobar el gemelo del dispositivo de la instancia de Azure Digital Twins que ha eliminado.

```azurecli-interactive
az dt twin show -n <Digital Twins instance name> --twin-id <Device Registration ID>"
```

Debería ver que el gemelo del dispositivo ya no se encuentra en la instancia de Azure Digital Twins.
:::image type="content" source="media/how-to-provision-using-dps/show-retired-twin.png" alt-text="Ventana de comandos que muestra que no se ha encontrado el dispositivo gemelo":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos creados en este artículo, siga estos pasos para eliminarlos.

Con Azure Cloud Shell o la CLI de Azure local, puede eliminar todos los recursos de Azure de un grupo de recursos mediante el comando [az group delete](/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete). Esto permite eliminar el grupo de recursos, la instancia de Azure Digital Twins, el centro de IoT y el registro del dispositivo del centro, el tema de Event Grid y las suscripciones asociadas, así como el espacio de nombres de los centros de eventos y ambas aplicaciones de Azure Functions, incluidos los recursos asociados, como el almacenamiento.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. 

```azurecli-interactive
az group delete --name <your-resource-group>
```

Por último, elimine la carpeta de ejemplo del proyecto que descargó de la máquina local.

## <a name="next-steps"></a>Pasos siguientes

Los gemelos digitales que se han creado para los dispositivos se almacenan como una jerarquía plana en Azure Digital Twins, pero se pueden enriquecer con la información del modelo y una jerarquía de varios niveles para la organización. Para más información sobre este concepto, consulte:

* [*Conceptos: Gemelos digitales y grafo de gemelos*](concepts-twins-graph.md)

Puede escribir la lógica personalizada para proporcionar automáticamente esta información con los datos del modelo y del gráfico ya almacenados en Azure Digital Twins. Para más información sobre cómo administrar, actualizar y recuperar información del grafo de gemelos, consulte lo siguiente:

* [*Procedimiento: Administración de un gemelo digital*](how-to-manage-twin.md)
* [*Procedimiento: Consulta del grafo gemelo*](how-to-query-graph.md)