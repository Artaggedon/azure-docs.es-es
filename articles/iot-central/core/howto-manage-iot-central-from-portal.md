---
title: Administración de IoT Central desde Azure Portal | Microsoft Docs
description: En este artículo se describe cómo crear y administrar aplicaciones de IoT Central desde el Azure Portal.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/11/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: 2af97206db00d683ab409710bc71a3b5048bf6ae
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104658472"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Administración de IoT Central desde Azure Portal

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

En lugar de crear y administrar aplicaciones de IoT Central desde el sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral), puede usar [Azure Portal](https://portal.azure.com) para administrar las aplicaciones.

## <a name="create-iot-central-applications"></a>Creación de aplicaciones de IoT Central

[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]


Para crear una aplicación, vaya a [Azure Portal](https://ms.portal.azure.com) y seleccione **Crear un recurso**.

En la barra **Buscar en Marketplace**, escriba *IoT Central*:

![Portal de administración: buscar](media/howto-manage-iot-central-from-portal/image0a1.png)

Seleccione el icono **Aplicación de IoT Central** en los resultados de búsqueda:

![Portal de administración: resultados de la búsqueda](media/howto-manage-iot-central-from-portal/image0b1.png)

Ahora, seleccione **Crear**:

![Portal de administración: Recurso de IoT Central](media/howto-manage-iot-central-from-portal/image0c1.png)

Rellene todos los campos en el formulario. Este formulario es similar al que se debe rellenar para crear aplicaciones en el sitio web del [administrador de aplicaciones de Azure IoT Central](https://aka.ms/iotcentral). Para más información, consulte la guía de inicio rápido para [Crear una aplicación de IoT Central](quick-deploy-iot-central.md).

![Creación de formularios de IoT Central](media/howto-manage-iot-central-from-portal/image6a.png)

**Ubicación** es la [zona geográfica](https://azure.microsoft.com/global-infrastructure/geographies/) en la que desea crear la aplicación. Normalmente, se debe elegir la ubicación más cercana físicamente a los dispositivos para un rendimiento óptimo. Actualmente, Azure IoT Central está disponible en las geografías siguientes: **Australia**, **Asia Pacífico**, **Europa**, **Estados Unidos**, **Reino Unido** y **Japón**. Una vez elegida una ubicación, no podrá mover la aplicación a otra más adelante.

Después de rellenar todos los campos, seleccione **Crear**.

## <a name="manage-existing-iot-central-applications"></a>Administrar las aplicaciones existentes de IoT Central

Si ya tiene una aplicación de Azure IoT Central, puede eliminarla o moverla a un grupo de recursos o suscripción diferente en Azure Portal.

> [!NOTE]
> Las aplicaciones creadas con el plan *gratuito* no requieren suscripciones de Azure y, por lo tanto, no aparecerán en la suscripción de Azure en Azure Portal. Solo puede ver y administrar las aplicaciones gratuitas del portal de IoT Central.

Para empezar, seleccione **Todos los recursos** en el portal. Seleccione **Mostrar tipos ocultos** y comience a escribir el nombre de la aplicación en **Filtrar por nombre** para encontrarla. Después, seleccione la aplicación de IoT Central que quiera administrar.

Para ir a la aplicación, seleccione la **URL de la aplicación de IoT Central**:

![Captura de pantalla que muestra la página "Información general" con la opción "Dirección URL de la aplicación de IoT Central" resaltada.](media/howto-manage-iot-central-from-portal/image3.png)

Para mover la aplicación a otro grupo de recursos, seleccione **Cambiar** al lado del grupo de recursos. En la página **Mover recursos**, seleccione el grupo de recursos al que le gustaría mover esta aplicación:

![Captura de pantalla que muestra la página "Información general" con la opción "Grupo de recursos (cambiar)" resaltada.](media/howto-manage-iot-central-from-portal/image4a.png)

Para mover la aplicación a otra suscripción, seleccione **Cambiar** al lado de la suscripción. En la página **Mover recursos**, elija la suscripción a la que le gustaría mover esta aplicación:

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar aplicaciones de Azure IoT Central desde Azure Portal, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [Administrar su aplicación](howto-administer.md)