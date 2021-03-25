---
title: Configuración de Apple Push Notification Service en Azure Notification Hubs | Microsoft Docs
description: Aprenda a definir un centro de notificaciones de Azure con la configuración de Apple Push Notification Service (APNS).
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 06/22/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 63c7e0c9569428b55420911f253deee52ce440cb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "85255405"
---
# <a name="configure-apple-push-notification-service-settings-for-a-notification-hub-in-the-azure-portal"></a>Configuración de Apple Push Notification Service para un centro de notificaciones en Azure Portal

En este artículo podrá ver cómo establecer la configuración de Apple Push Notification Service (APNS) para un centro de notificaciones de Azure mediante Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md).

## <a name="configure-apple-push-notification-service"></a>Configuración de Apple Push Notification Service

El siguiente procedimiento le proporciona los pasos para establecer la configuración de Apple Push Notification Service (APNS) para un centro de notificaciones:

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Apple (APNS)** en el menú izquierdo.

1. Para **Modo de autenticación**, seleccione **Certificado** o **Token**.

   - Si ha seleccionado **Certificado**:
      - Seleccione el icono de archivo, y luego el archivo *.p12* que desea cargar.
      - Escriba una contraseña.
      - Seleccione el modo **Espacio aislado**. O para enviar notificaciones push a los usuarios que compraron la aplicación en la tienda, seleccione el modo **Producción**.

     ![Captura de pantalla de una configuración del certificado APNS en Azure Portal](./media/configure-apple-push-notification-service/notification-hubs-apple-config-cert.png)

   - Si selecciona **Token**:
      - Especifique los valores **Id. de clave**, **Id. de agrupación**, **Id. de equipo** y **Token**.
      - Seleccione el modo **Espacio aislado**. O para enviar notificaciones push a los usuarios que compraron la aplicación en la tienda, seleccione el modo **Producción**.

     ![Captura de pantalla de una configuración del token APNS en Azure Portal](./media/configure-apple-push-notification-service/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Pasos siguientes

Para ver un tutorial con instrucciones paso a paso para enviar notificaciones en dispositivos iOS, vea el artículo siguiente: [Envío de notificaciones push a aplicaciones iOS mediante Azure Notification Hubs](ios-sdk-get-started.md).
