---
title: Conexión de una aplicación de Azure IoT Central con los servicios de campo de Dynamics 365 | Microsoft Docs
description: Aprenda a crear una solución de un extremo a otro con Azure IoT Central y Dynamics 365 for Field Service
author: miriambrus
ms.author: miriamb
ms.date: 12/11/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 1d098f56bbadfe115620580c8d93fb6dd021550d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "97586681"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>Creación de una solución de un extremo a otro con Azure IoT Central y Dynamics 365 for Field Service 
Como generador, puede habilitar la integración de una aplicación de Azure IoT Central en otros sistemas empresariales. 

Por ejemplo, en una solución de administración de residuos conectada, puede optimizar el envío de camiones de recogida de basura. La optimización se puede realizar en función de los datos de los sensores de IoT de los cubos de basura conectados. En la [aplicación de administración de residuos conectada de IoT Central](./tutorial-connected-waste-management.md) puede configurar reglas y acciones, y establecer que desencadene la creación de alertas en Dynamics 365 for Field Service. Este escenario se consigue mediante el uso de Power Automate, que se configurará directamente en IoT Central para automatizar los flujos de trabajo entre aplicaciones y servicios. Además, en función de las actividades de los servicios de Field Service, la información se puede devolver a Azure IoT Central. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Conexión de una aplicación de Azure IoT Central con los servicios de campo de Dynamics 365 

Los siguientes procesos de integración se pueden implementar fácilmente mediante una experiencia de configuración pura:
* Azure IoT Central puede enviar información sobre las anomalías del dispositivo al servicio de campo conectado (como una alerta de IoT) para el diagnóstico.
* El servicio de campo conectado puede crear casos o pedidos desencadenados a partir de las anomalías del dispositivo.
* El servicio de campo conectado puede programar técnicos para que realicen las comprobaciones pertinentes, con el fin de evitar incidentes que provoquen que haya un tiempo de inactividad.
* El panel de dispositivos de Azure IoT Central se puede actualizar con información relevante acerca de la programación y los servicios.


## <a name="next-steps"></a>Pasos siguientes
* Más información acerca de las [plantillas de IoT Central Government](./overview-iot-central-government.md)
* Más información acerca de [IoT Central](../core/overview-iot-central.md)
* Más información acerca de los [servicios de campo de Dynamics 365](/dynamics365/field-service/cfs-iot-overview)
