---
title: Envío autoadministrado de Microsoft Azure Data Box Disk | Microsoft Docs en datos
description: Describe el flujo de trabajo de envío autoadministrado para dispositivos Azure Data Box Disk.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 02/02/2021
ms.author: alkohli
ms.openlocfilehash: f512b4415f4a83e779a8f9bf790ba2806e3b05c5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "99526337"
---
# <a name="use-self-managed-shipping-for-azure-data-box-disk-in-the-azure-portal"></a>Uso del envío autoadministrado para Azure Data Box Disk en Azure Portal

En este artículo se describen las tareas de envío autoadministradas para pedir, recoger y entregar un dispositivo Azure Data Box Disk. Puede administrar Data Box Disk mediante Azure Portal.

## <a name="prerequisites"></a>Prerrequisitos

El envío autoadministrado está disponible como opción cuando [realiza un pedido de Azure Data Box Disk](data-box-disk-deploy-ordered.md). Solo está disponible en las siguientes regiones:

* US Gov
* Reino Unido
* Europa Occidental
* Australia
* Japón
* Singapur
* Corea del Sur
* Sudáfrica
* India (versión preliminar)

## <a name="use-self-managed-shipping"></a>Uso del envío autoadministrado

Al realizar un pedido de Data Box Disk, puede elegir la opción de envío autoadministrado.

1. En el pedido de Azure Data Box Disk, en **Detalles del contacto**, seleccione **+ Agregar dirección de envío**.

   ![Captura de pantalla del asistente para pedidos que muestra el paso Detalles de contacto, con la opción Agregar dirección de envío seleccionada.](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-1.png)

2. Al elegir el tipo de envío, seleccione la opción **Envío autoadministrado**. Esta opción solo está disponible si se encuentra en una región admitida, tal como se describe en los requisitos previos.

3. Una vez proporcionada la dirección de envío, tendrá que validarla y completar el pedido.

   ![Captura de pantalla del cuadro de diálogo Agregar dirección de envío, con las opciones Enviar con y la opción Agregar dirección de envío seleccionadas.](media\data-box-portal-customer-managed-shipping\choose-self-managed-shipping-2.png)

4. Cuando el dispositivo esté preparado y haya recibido una notificación por correo electrónico, podrá programar una recogida. En el pedido de Azure Data Box Disk, vaya a **Información general** y seleccione **Programar recogida**.

   ![Pedir un dispositivo Data Box para su recogida](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-01b.png)

5. Siga las instrucciones indicadas en **Programar recogida para Azure**. Antes de poder obtener el código de autorización, debe enviar un correo electrónico a [adbops@microsoft.com](mailto:adbops@microsoft.com) para programar la recogida del dispositivo en el centro de datos de su región.

   ![Programación de la recogida](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-user-pickup-02c.png)

6. Una vez que haya programado la recogida del dispositivo, puede ver el código de autorización en **Programar recogida para Azure**.

   ![Captura de pantalla del cuadro de diálogo Programar recogida para Azure con el código de autorización del cuadro de texto Recogida seleccionado.](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-01b.png)

   Tome nota de este código de autorización.

   Según los requisitos de seguridad, en el momento de programar la recogida es necesario presentar el nombre de la persona que se encargará de hacerlo.

   También debe proporcionar detalles de quién acudirá al centro de datos a recoger el dispositivo. Usted o el punto de contacto deben llevar un identificador con una foto aprobado por el gobierno, que se validará en el centro de datos.

   La persona que recoge el dispositivo también debe tener el código de autorización. El código de autorización es exclusivo de una recogida o una entrega y se valida en el centro de datos.

7. El pedido cambia automáticamente al estado **Recogido** una vez que el dispositivo se ha recogido en el centro de datos.

   ![Picked up (Recogido)](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-ready-disk-01b.png)

8. Después de recogerlo, podrá copiar los datos en Data Box Disk en su sitio. Una vez completada la copia, prepare para enviar el dispositivo Data Box Disk.

   Una vez finalizada la copia de los datos, póngase en contacto con el departamento de operaciones para programar una cita para la entrega. Tendrá que compartir los detalles de la persona que acudirá al centro de datos para entregar el disco. El centro de recursos también tendrá que verificar el código de autorización en el momento de la entrega. Encontrará el código de autorización para la entrega en Azure Portal en la sección **Schedule drop off** (Programar entrega).

   > [!NOTE]
   > No comparta el código de autorización por correo electrónico. Este solo debe verificarse en el centro de datos durante la entrega.

9. Una vez recibida la cita para una entrega, el pedido debe encontrarse en el estado **Listo para recibir el paquete en el centro de datos de Azure** en Azure Portal.

   ![Captura de pantalla del cuadro de diálogo Agregar dirección de envío, con las opciones Enviar con y la opción Agregar dirección de envío seleccionadas.](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-authcode-dropoff-02b.png)

10. Después de verificar el identificador y el código de autorización y de entregar el dispositivo en el centro de datos, el estado del pedido debe ser **Recibido**.

    ![Recepción completada](media\data-box-disk-portal-customer-managed-shipping\data-box-disk-received-01a.png)

11. Una vez que se recibe el dispositivo, la copia de datos continuará. El pedido se completará cuando finalice la copia.

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Implementación de Azure Data Box Disk mediante Azure Portal](data-box-disk-quickstart-portal.md)
