---
title: Arquitectura de referencia de la solución de administración de residuos conectada creada con Azure IoT Central | Microsoft Docs
description: Conozca los conceptos de una solución de administración de residuos conectada creada con Azure IoT Central.
author: miriambrus
ms.author: miriamb
ms.date: 12/11/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 2d6c8c693db42789a965ee896a6d913d6ee20ae0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99831678"
---
# <a name="connected-waste-monitoring-reference-architecture"></a>Arquitectura de referencia de supervisión de residuos conectada 

Una solución de administración de residuos conectada se puede crear con la **plantilla de aplicación de Azure IoT Central** como una aplicación de IoT de inicio rápido. En este artículo se proporciona una guía de arquitectura de referencia de alto nivel sobre la creación de una solución de un extremo a otro. 

![Arquitectura de la administración de residuos conectada](./media/concepts-connectedwastemanagement-architecture/concepts-connectedwastemanagement-architecture1.png)


Conceptos:

1. Dispositivos y conectividad  
1. IoT Central 
1. Extensibilidad e integraciones
1. Aplicaciones empresariales

Echemos un vistazo a los componentes clave que generalmente desempeñan un papel en una solución de supervisión del consumo de agua.

## <a name="devices-and-connectivity"></a>Dispositivos y conectividad 
Los dispositivos que se usan en entornos abiertos, como los contenedores de residuos, normalmente se conectan mediante redes de área extensa de bajo consumo (LPWAN) a través de un operador de red de terceros. Para estos tipos de dispositivos, puede usar el [Puente de dispositivos de Azure IoT Central](../core/howto-build-iotc-device-bridge.md) para enviar los datos del dispositivo a la aplicación de IoT en Azure IoT Central. Como alternativa, puede tener puertas de enlace de dispositivo que sean compatibles con IP y puedan conectarse directamente a IoT Central.

## <a name="iot-central"></a>IoT Central 
Azure IoT Central es una plataforma de aplicaciones de IoT que le ayuda a empezar a trabajar con su solución IoT rápidamente. Puede utilizar su marca, personalizar e integrar la solución con servicios de terceros.
Después de conectar los dispositivos de agua inteligentes a IoT Central, obtendrá el control, la supervisión y las alertas de los dispositivos, la interfaz de usuario con RBAC integrado, paneles de información configurables y opciones de extensibilidad. 

## <a name="extensibility-and-integrations"></a>Extensibilidad e integraciones
Puede ampliar su aplicación de IoT en IoT Central y, si lo desea:
* transformar e integrar los datos de IoT para el análisis avanzado, como el entrenamiento de modelos de aprendizaje automático, mediante la exportación continua de datos de la aplicación de IoT Central.
* Automatizar flujos de trabajo en otros sistemas al desencadenar acciones mediante Power Automate o webhooks desde la aplicación de IoT Central.
* acceder mediante programación a la aplicación IoT de IoT Central mediante las API de IoT Central.

## <a name="business-applications"></a>Aplicaciones empresariales 
Los datos de IoT se pueden usar para potenciar una variedad de aplicaciones empresariales en una instalación de tratamiento de residuos. Para obtener información sobre cómo conectar la aplicación de administración de residuos conectada de IoT Central con los servicios de campo, siga el artículo sobre [cómo integrar con los servicios de campo de Dynamics 365](./how-to-configure-connected-field-services.md). 

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre cómo [crear una aplicación de administración de residuos conectada](./tutorial-connected-waste-management.md) de IoT Central.
* Más información acerca de las [plantillas de IoT Central Government](./overview-iot-central-government.md)
* Para más información acerca de IoT Central, consulte [Introducción a IoT Central](../core/overview-iot-central.md).
