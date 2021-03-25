---
title: 'Tutorial: Definición de un nuevo tipo de dispositivo de puerta de enlace en Azure IoT Central | Microsoft Docs'
description: Este tutorial le muestra, como desarrollador, cómo definir un nuevo tipo de dispositivo de puerta de enlace de IoT en la aplicación de Azure IoT Central.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: e42e1791693342592e391a14422b1441c9cbbd31
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99832393"
---
# <a name="tutorial---define-a-new-iot-gateway-device-type-in-your-azure-iot-central-application"></a>Tutorial: Definición de un nuevo tipo de dispositivo de puerta de enlace de IoT en la aplicación de Azure IoT Central

*Este artículo se aplica a los generadores de soluciones y a los desarrolladores de dispositivos.*

Como generador de soluciones, este tutorial le muestra cómo usar una plantilla de dispositivo de puerta de enlace para definir un dispositivo de puerta de enlace en la aplicación de IoT Central. A continuación, puede configurar varios dispositivos de nivel inferior que se conecten a la aplicación de IoT Central mediante el dispositivo de puerta de enlace. 

En este tutorial va a crear una plantilla de dispositivo de puerta de enlace **Smart Building**. Un dispositivo de puerta de enlace **Smart Building** tiene relaciones con otros dispositivos de nivel inferior.

![Diagrama de la relación entre el dispositivo de puerta de enlace y los dispositivos de nivel inferior](./media/tutorial-define-gateway-device-type/gatewaypattern.png)

Además de permitir que los dispositivos de nivel inferior se comuniquen con la aplicación de IoT Central, un dispositivo de puerta de enlace también puede:

* Enviar sus propios datos de telemetría como, por ejemplo, la temperatura.
* Responder a las actualizaciones de propiedades grabables que realiza un operador. Por ejemplo, un operador podría cambiar el intervalo de envío de datos de telemetría.
* Responder a comandos como, por ejemplo, el reinicio del dispositivo.

> [!div class="checklist"]
> Crear plantillas de dispositivo de bajada Crear una plantilla de dispositivo de puerta de enlace Publicar la plantilla de dispositivo Crear los dispositivos simulados

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesitará [crear una aplicación de Azure IoT Central](./quick-deploy-iot-central.md).

## <a name="create-downstream-device-templates"></a>Creación de plantillas de dispositivo de nivel inferior

En este tutorial se usan plantillas de dispositivo para un dispositivo con **sensor S1** y con **sensor de ocupación RS40** para generar dispositivos de nivel inferior simulados.

Para crear una plantilla para un dispositivo con **sensor S1**:

1. Seleccione **Device Templates** (Plantillas de dispositivo) en el panel izquierdo. A continuación, seleccione **+** para empezar a agregar la plantilla.

1. Desplácese hacia abajo hasta que pueda ver el icono del dispositivo con **sensor S1**. Seleccione el icono y, a continuación, seleccione **Siguiente: Customize** (Personalizar)

1. En la página **Revisar**, seleccione **Crear** para agregar la plantilla de dispositivo a la aplicación. 

Para crear una plantilla para un dispositivo con **sensor de ocupación RS40**:

1. Seleccione **Device Templates** (Plantillas de dispositivo) en el panel izquierdo. A continuación, seleccione **+** para empezar a agregar la plantilla.

1. Desplácese hacia abajo hasta que vea el icono del dispositivo con el ***sensor de ocupación RS40**. Seleccione el icono y, a continuación, seleccione **Siguiente: Customize** (Personalizar)

1. En la página **Revisar**, seleccione **Crear** para agregar la plantilla de dispositivo a la aplicación. 

Ahora tiene plantillas de dispositivo para los dos tipos de dispositivos de nivel inferior:

![Plantillas de los dispositivos de nivel inferior](./media/tutorial-define-gateway-device-type/downstream-device-types.png)


## <a name="create-a-gateway-device-template"></a>Creación de una plantilla de dispositivo de puerta de enlace

En este tutorial se va a crear una plantilla para un dispositivo de puerta de enlace desde el principio. Puede usar esta plantilla más adelante para crear un dispositivo simulado de puerta de enlace en la aplicación.

Para agregar una nueva plantilla de dispositivo de puerta de enlace a la aplicación:

1. Seleccione **Device Templates** (Plantillas de dispositivo) en el panel izquierdo. A continuación, seleccione **+** para empezar a agregar la plantilla.

1. En la página **Seleccionar tipo de plantilla**, seleccione el icono **Dispositivo IoT** y, después, seleccione **Siguiente: Customize** (Personalizar)

1. En la página **Personalizar dispositivo**, seleccione la casilla **Dispositivo de puerta de enlace**.

1. En la página **Revisar**, seleccione **Crear**. 

1. Escriba **Dispositivo de puerta de enlace de edificio inteligente** como nombre de la plantilla.

1. En la página **Crear un modelo de funcionalidad**, seleccione el icono **Personalizar**.

1. Seleccione **+** para agregar una interfaz.  Elija la interfaz estándar **Información del dispositivo**.

### <a name="add-relationships"></a>Adición de relaciones

A continuación, agregue relaciones a las plantillas para las plantillas de dispositivo de nivel inferior:

1. En la plantilla **Dispositivo de puerta de enlace de edificio inteligente** seleccione **Relaciones**.

1. Seleccione **+ Agregar relación**. Escriba **Sensor ambiental** como nombre para mostrar y seleccione **Sensor S1** como destino.

1. Seleccione **+ Agregar relación** de nuevo. Escriba **Sensor de ocupación** como nombre para mostrar y seleccione **Sensor de ocupación RS40** como destino.

1. Seleccione **Guardar**.

![Plantilla de dispositivo de puerta de enlace de edificio inteligente que muestra relaciones](./media/tutorial-define-gateway-device-type/relationships.png)

### <a name="add-cloud-properties"></a>Adición de propiedades de nube

Una plantilla de dispositivo de puerta de enlace puede incluir propiedades de la nube. Las propiedades de la nube solo existen en la aplicación IoT Central y nunca se envían a un dispositivo ni se reciben de él.

Para agregar propiedades de la nube a la plantilla **Dispositivo de puerta de enlace de edificio inteligente**:

1. En la plantilla **Dispositivo de puerta de enlace de edificio inteligente** seleccione **Propiedades de la nube**.

1.  Use la información de la tabla siguiente para agregar dos propiedades de la nube a la plantilla de dispositivo de puerta de enlace.

    | Nombre para mostrar      | Tipo semántico | Schema |
    | ----------------- | ------------- | ------ |
    | Fecha de la última revisión | None          | Date   |
    | Nombre del cliente     | None          | String |

2. Seleccione **Guardar**.

### <a name="create-views"></a>Creación de vistas

Como desarrollador, puede personalizar la aplicación para mostrar información pertinente sobre el dispositivo sensor ambiental a un operador. Las personalizaciones permiten al operador administrar estos dispositivo conectados a la aplicación. Puede crear dos tipos de vistas para que un operador interactúe con los dispositivos:

* Formularios para ver y editar las propiedades del dispositivo y la nube
* Paneles para visualizar los dispositivos

Para generar las vistas predeterminadas para la plantilla **Dispositivo de puerta de enlace de edificio inteligente**:

1. En la plantilla **Dispositivo de puerta de enlace de edificio inteligente** seleccione **Vistas**.

1. Seleccione el icono **Generar vistas predeterminadas** y asegúrese de que todas las opciones estén seleccionadas.

1. Seleccione **Generar vistas de panel predeterminadas**.

## <a name="publish-the-device-template"></a>Publicación de la plantilla de dispositivo

Para poder crear un dispositivo simulado de puerta de enlace, o conectar un dispositivo de puerta de enlace real, es preciso publicar la plantilla de dispositivo.

Para publicar la plantilla de dispositivo de puerta de enlace:

1. Seleccione la plantilla **Dispositivo de puerta de enlace de edificio inteligente** en la página **Plantillas de dispositivo**.

2. Seleccione **Publicar**.

3. En el cuadro de diálogo **Publish a Device Template** (Publicar una plantilla de dispositivo), elija **Publicar**.

Una vez publicada una plantilla de dispositivo, es visible en la página **Devices** (Dispositivos) y para el operador. En una plantilla de dispositivo publicada no se puede editar un modelo de dispositivo sin crear una versión. Sin embargo, puede hacer actualizaciones en las propiedades de la nube, las personalizaciones y las vistas en una plantilla de dispositivo publicada. Estas actualizaciones no hacen que se cree una nueva versión. Después de realizar los cambios, seleccione **Publish** (Publicar) para enviar esos cambios al operador.

## <a name="create-the-simulated-devices"></a>Creación de los dispositivos simulados

En este tutorial se usan dispositivos simulados de nivel inferior y un dispositivo simulado de puerta de enlace.

Para crear un dispositivo simulado de puerta de enlace:

1. En la página **Dispositivos**, seleccione **Dispositivo de puerta de enlace de edificio inteligente** en la lista de plantillas de dispositivo.

1. Seleccione **+** para empezar a agregar un nuevo dispositivo.

1. Conserve el **Id. de dispositivo** y el **Nombre de dispositivo** que se han generado. Asegúrese de que el conmutador **Simulado** esté **Activado**. Seleccione **Crear**.

Para crear dispositivos simulados de nivel inferior:

1. En la página **Dispositivos**, seleccione **Sensor de ocupación RS40** en la lista de plantillas de dispositivo.

1. Seleccione **+** para empezar a agregar un nuevo dispositivo.

1. Conserve el **Id. de dispositivo** y el **Nombre de dispositivo** que se han generado. Asegúrese de que el conmutador **Simulado** esté **Activado**. Seleccione **Crear**.

1. En la página **Dispositivos**, seleccione **Sensor S1** en la lista de plantillas de dispositivo.

1. Seleccione **+** para empezar a agregar un nuevo dispositivo.

1. Conserve el **Id. de dispositivo** y el **Nombre de dispositivo** que se han generado. Asegúrese de que el conmutador **Simulado** esté **Activado**. Seleccione **Crear**.

![Dispositivos simulados en la aplicación](./media/tutorial-define-gateway-device-type/simulated-devices.png)

### <a name="add-downstream-device-relationships-to-a-gateway-device"></a>Adición de relaciones de dispositivos de nivel inferior a un dispositivo de puerta de enlace

Ahora que tiene los dispositivos simulados en la aplicación, puede crear las relaciones entre los dispositivos de nivel inferior y el dispositivo de puerta de enlace:

1. En la página **Dispositivos**, seleccione **Sensor S1** en la lista de plantillas de dispositivo y, a continuación, seleccione el dispositivo simulado **Sensor S1**.

1. Seleccione **Conectar con la puerta de enlace**.

1. En el cuadro de diálogo **Conectar con la puerta de enlace**, seleccione la plantilla **Dispositivo de puerta de enlace de edificio inteligente** y, a continuación, seleccione la instancia simulada que creó anteriormente.

1. Seleccione **Combinar**.

1. En la página **Dispositivos**, seleccione **Sensor de ocupación RS40** en la lista de plantillas de dispositivo y, a continuación, seleccione el dispositivo simulado **Sensor de ocupación RS40**.

1. Seleccione **Conectar con la puerta de enlace**.

1. En el cuadro de diálogo **Conectar con la puerta de enlace**, seleccione la plantilla **Dispositivo de puerta de enlace de edificio inteligente** y, a continuación, seleccione la instancia simulada que creó anteriormente.

1. Seleccione **Combinar**.

Los dispositivos simulados de nivel inferior ahora están ya conectados al dispositivo simulado de puerta de enlace. Si va a la vista **Dispositivos de bajada** del dispositivo de puerta de enlace, podrá ver los dispositivos de nivel inferior relacionados:

![Vista Dispositivos de bajada](./media/tutorial-define-gateway-device-type/downstream-device-view.png)

Seleccione una plantilla de dispositivo de puerta de enlace y la instancia del dispositivo de puerta de enlace y haga clic en **Unir**.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

* Crear una puerta de enlace de IoT como plantilla de dispositivo.
* Crear propiedades de la nube
* Crear personalizaciones
* Definir una visualización para la telemetría del dispositivo
* Agregar relaciones.
* Publicar la plantilla de dispositivo

A continuación, como desarrollador de dispositivos, puede obtener información sobre:

> [!div class="nextstepaction"]
> [Incorporación de un dispositivo Azure IoT Edge a la aplicación Azure IoT Central](tutorial-add-edge-as-leaf-device.md)
