---
title: Solicitud de datos de transporte público en tiempo real con los servicios de Mobility de Microsoft Azure Maps (versión preliminar)
description: Obtenga información acerca de cómo solicitar datos de tránsito público en tiempo real, como las llegadas en una parada de transporte público. Consulte cómo usar los servicios de Mobility de Azure Maps (versión preliminar) para este fin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: d3e3dc4b0e3bc64a38856da8344583b744ea62b6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96906053"
---
# <a name="request-real-time-public-transit-data-using-the-azure-maps-mobility-services-preview"></a>Solicitud de datos de transporte público en tiempo real mediante los servicios de Mobility de Azure Maps (versión preliminar) 

> [!IMPORTANT]
> Los servicios de Mobility de Azure Maps se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


En este artículo se muestra cómo usar los [servicios de Mobility](/rest/api/maps/mobility) de Azure Maps para solicitar datos de transporte público en tiempo real.

En este artículo, aprenderá a solicitar las siguientes llegadas en tiempo real de todas las líneas que llegan a una parada determinada.

## <a name="prerequisites"></a>Prerrequisitos

En primer lugar, debe tener una cuenta de Azure Maps y una clave de suscripción para realizar cualquier llamada a las API de transporte público de Azure Maps. Para más información, siga las instrucciones de [Crear una cuenta](quick-demo-map-app.md#create-an-azure-maps-account) para crear una cuenta de Azure Maps. Siga los pasos descritos en la sección de [obtención de la clave principal](quick-demo-map-app.md#get-the-primary-key-for-your-account) para obtener la clave principal de la cuenta. Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](./how-to-manage-authentication.md).

Este artículo utiliza la [aplicación Postman](https://www.getpostman.com/apps) para generar llamadas REST. Puede usar cualquier entorno de desarrollo de API que prefiera.

## <a name="request-real-time-arrivals-for-a-stop"></a>Solicitud de llegadas en tiempo real en una parada

Para solicitar datos en tiempo real de las llegadas de una parada de transporte público determinada, deberá realizar una solicitud a [Real-time Arrivals API](/rest/api/maps/mobility/getrealtimearrivalspreview) de los [servicios de Mobility (versión preliminar)](/rest/api/maps/mobility) de Azure Maps. Necesitará los valores de **metroID** y **stopID** para completar la solicitud. Para más información sobre cómo solicitar estos parámetros, consulte nuestra guía para [solicitar rutas de transporte público](./how-to-request-transit-data.md).

Vamos a usar "522" como metroID, que es el identificador de metro del área "Seattle – Tacoma – Bellevue, WA". Use "522---2060603" como stopID, esta parada de autobús está en "Ne 24th St & 162nd Ave Ne, Bellevue WA". Para solicitar los datos de las siguientes cinco llegadas en tiempo real de esta parada, complete los pasos siguientes:

1. Abra la aplicación Postman y cree una colección para almacenar las solicitudes. Cerca de la parte superior de la aplicación Postman, seleccione **New** (Nuevo). En la ventana **Create New** (Crear nuevo), seleccione **Collection** (Colección).  Asigne un nombre a la colección y seleccione el botón **Create** (Crear).

2. Para crear la solicitud, seleccione **New** (Nuevo) otra vez. En la ventana **Create New** (Crear nuevo), seleccione **Request** (Solicitud). Escriba un valor de **Request name** (Nombre de solicitud) para la solicitud. Seleccione la colección que creó en el paso anterior, como la ubicación en la que se va a guardar la solicitud. Después, seleccione **Guardar**.

    ![Creación de una solicitud en Postman](./media/how-to-request-transit-data/postman-new.png)

3. Seleccione el método HTTP **GET** en la pestaña del generador y escriba la siguiente dirección URL para realizar una solicitud GET. Reemplace `{subscription-key}` por la clave principal de Azure Maps.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

4. Si la solicitud es correcta, recibirá la respuesta siguiente.  Observe que el parámetro "scheduleType" define si la hora de llegada estimada se basa en datos en tiempo real o en datos estáticos.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```

## <a name="next-steps"></a>Pasos siguientes

Aprenda a solicitar datos de transporte público mediante los servicios de Mobility (versión preliminar):

> [!div class="nextstepaction"]
> [Cómo solicitar datos de tránsito](how-to-request-transit-data.md)

Explore la documentación de las API de los servicios de Mobility de Azure Maps (versión preliminar):

> [!div class="nextstepaction"]
> [Documentación de las API de los servicios de Mobility](/rest/api/maps/mobility)