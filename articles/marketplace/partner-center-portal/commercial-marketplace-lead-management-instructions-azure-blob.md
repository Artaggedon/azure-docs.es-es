---
title: 'Administración de clientes potenciales con Azure Blob Storage: Marketplace comercial de Microsoft'
description: Aprenda a usar Azure Blob para configurar clientes potenciales de Microsoft AppSource y Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: trkeya
ms.author: trkeya
ms.date: 05/01/2020
ms.openlocfilehash: bd2f3d40b1aea1c0133f95e069ebfd527d30bd59
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "94491110"
---
# <a name="use-azure-blob-storage-to-manage-commercial-marketplace-leads"></a>Uso de Azure Blob Storage para administrar clientes potenciales de Marketplace comercial

>[!Caution]
>La compatibilidad de Marketplace comercial con Azure Blob Storage ha quedado en desuso y ya no es una opción para procesar a los clientes potenciales de la oferta. Si actualmente dispone de una oferta de Marketplace comercial con la administración de clientes potenciales configurada para Azure Blob, ya no recibirá clientes potenciales. Actualice la configuración de administración de clientes potenciales a cualquiera de las otras opciones de administración de clientes potenciales. Obtenga información sobre las demás opciones en la [página de aterrizaje de administración de clientes potenciales](./commercial-marketplace-get-customer-leads.md).

 Si el sistema de administración de las relaciones con clientes (CRM) no se admite explícitamente en el Centro de partners para recibir clientes potenciales de Microsoft AppSource y Azure Marketplace, puede usar Azure Blob Storage. Después, puede exportar los datos e importarlos en el sistema CRM. Las instrucciones de este artículo le guiarán por el proceso de creación de una cuenta de Azure Storage y un blob en esa cuenta. Además, puede crear un flujo mediante Power Automate para enviar una notificación por correo electrónico cuando su oferta reciba un cliente potencial.

>[!NOTE]
>El conector de Power Automate usado en estas instrucciones requiere una suscripción de pago de Power Automate. Tenga esto en cuenta antes de seguir las instrucciones de este artículo.

## <a name="configure-azure-blob-storage"></a>Configuración de Azure Blob Storage

1. Si no tiene una cuenta de Azure, puede [crear una cuenta de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

2. Cuando la cuenta de Azure esté activa, inicie sesión en [Azure Portal](https://portal.azure.com).

3. En Azure Portal, cree una cuenta de almacenamiento mediante el siguiente procedimiento.  
    1. En la barra de menús de la izquierda, seleccione **+ Crear un recurso**.  El panel **Nuevo** (hoja) se mostrará a la derecha.
    2. Seleccione **Almacenamiento** en el panel **Nuevo**.  En la derecha se mostrará una lista de **Destacados**.
    3. Seleccione **Cuenta de almacenamiento** para iniciar la creación de la cuenta.  Siga las instrucciones que aparecen en el artículo [Crear una cuenta de almacenamiento](../../storage/common/storage-account-create.md?tabs=azure-portal).

    ![Pasos para crear una cuenta de Azure Storage](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-create.png)

    Para más información sobre las cuentas de almacenamiento, consulte este [Inicio rápido](../../storage/blobs/storage-quickstart-blobs-portal.md).  Para más información sobre los precios de almacenamiento, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

4. Espere hasta que se aprovisione la cuenta de almacenamiento, proceso que normalmente tarda unos minutos.  Luego, acceda a la cuenta de almacenamiento desde la página de **Inicio** de Azure Portal mediante la selección de **Ver todos los recursos** o **Todos los recursos** en la barra de menú de navegación de la izquierda.

    ![Acceso a la cuenta de almacenamiento de Azure](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-access.png)

5. En el panel de la cuenta de almacenamiento, seleccione **Claves de acceso** y copie el valor de la *Cadena de conexión* de la clave. Guarde este valor, ya que es el valor de la *cadena de conexión de cuenta de almacenamiento* que tendrá que proporcionar en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace.

     Este es un ejemplo de una cadena de conexión:

     ```sql
     DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
     ```

    ![Captura de pantalla que muestra la página "Claves de acceso" con el cuadro de texto "Cadena de conexión" resaltado.](./media/commercial-marketplace-lead-management-instructions-azure-blob/azure-storage-keys-2.png)

6. En la página de la cuenta de almacenamiento, seleccione **Blobs**.

   ![Captura de pantalla de la página de la cuenta de Azure Storage con Blobs seleccionados](./media/commercial-marketplace-lead-management-instructions-azure-blob/select-blobs.png)

7. Cuando esté en la página de blobs, seleccione el botón **+ Contenedor**.

8. Escriba un **nombre** para el nuevo contenedor. El nombre del contenedor debe escribirse en minúsculas, comenzar por una letra o un número, y solo puede incluir letras, números y el carácter de guión (-). Para más información sobre la nomenclatura de contenedores y blobs, consulte [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Asignación de nombres y referencia a contenedores, blobs y metadatos).

    Guarde este valor, ya que es el valor de *Nombre del contenedor* que tendrá que proporcionar en el portal de publicación para recibir clientes potenciales para su oferta de Marketplace.

9. Establezca el nivel de acceso público al contenedor como **Privado (sin acceso anónimo)** .

10. Seleccione **Aceptar** para crear el contenedor.

    ![Nuevo contenedor](./media/commercial-marketplace-lead-management-instructions-azure-blob/new-container.png)

## <a name="configure-your-offer-to-send-leads-to-azure-blob-storage"></a>Configuración de la oferta para enviar clientes potenciales Azure Blob Storage

Cuando esté listo para configurar la información de administración de clientes potenciales de la oferta en el portal de publicación, siga estos pasos:

1. Vaya a la página **Configuración de la oferta** de su oferta.
2. En la sección **Clientes potenciales**, seleccione **Conectar**.

    :::image type="content" source="./media/commercial-marketplace-lead-management-instructions-azure-blob/customer-leads.png" alt-text="Clientes potenciales":::

3. En la ventana emergente Detalles de conexión, seleccione **Blob de Azure** como destino de clientes potenciales.

    ![Detalles de conexión](./media/commercial-marketplace-lead-management-instructions-azure-blob/connect-details.png) 

4. Proporcione el **Nombre del contenedor** y la **Cadena de conexión de cuenta de almacenamiento** que ha recibido según las instrucciones a continuación.

    * Ejemplo de nombre de contenedor: `marketplaceleadcontainer`
    * Ejemplo de Cadena de conexión de cuenta de almacenamiento: `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net` ![Detalles de conexión](./media/commercial-marketplace-lead-management-instructions-azure-blob/connection-details.png) 

5. Seleccione **Guardar**.

    > [!NOTE]
    > Debe terminar de configurar el resto de las partes de la oferta y publicarla para poder recibir clientes potenciales para la oferta.