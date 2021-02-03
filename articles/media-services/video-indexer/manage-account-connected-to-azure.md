---
title: Administración de una cuenta de Video Indexer
titleSuffix: Azure Media Services
description: Obtenga información sobre cómo administrar una cuenta de Video Indexer conectada a Azure.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 01/14/2021
ms.author: juliako
ms.openlocfilehash: 25025efe5254442c4862563ce329cfae69e7d033
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98898450"
---
# <a name="manage-a-video-indexer-account-connected-to-azure"></a>Administración de una cuenta de Video Indexer conectada a Azure

En este artículo se muestra cómo administrar una cuenta de Video Indexer conectada a su suscripción de Azure y a una cuenta de Azure Media Services.

> [!NOTE]
> Debe ser propietario de la cuenta de Video Indexer para realizar los ajustes de configuración descritos en este tema.

## <a name="prerequisites"></a>Prerrequisitos

Conecte su cuenta de Video Indexer a Azure, como se describe en [Conexión a Azure](connect-to-azure.md).

Asegúrese de tener en cuenta los [requisitos previos](connect-to-azure.md#prerequisites-for-connecting-to-azure) y de revisar las [consideraciones](connect-to-azure.md#azure-media-services-considerations) del artículo.

## <a name="examine-account-settings"></a>Comprobación de la configuración de la cuenta

En esta sección se comprueba la configuración de la cuenta de Video Indexer.

Para ver la configuración:

1. Haga clic en el icono de usuario en la esquina superior derecha y seleccione **Configuración**.

    ![Configuración de Video Indexer](./media/manage-account-connected-to-azure/select-settings.png)

2. En la página **Settings** (Configuración), seleccione la pestaña **Account** (Cuenta).

Si la cuenta de Video Indexer está conectada a Azure, verá los elementos siguientes:

* El nombre de la cuenta subyacente de Azure Media Services.
* El número de trabajos de indexación en ejecución y en cola.
* El número y el tipo de unidades reservadas asignadas.

Si la cuenta precisa algunos ajustes, verá errores y advertencias pertinentes sobre la configuración de la cuenta en la página **Configuración**. Los mensajes incluyen vínculos a las ubicaciones exactas de Azure Portal donde debe realizar los cambios. Para más información, vea la sección [Errores y advertencias](#errors-and-warnings) a continuación.

## <a name="repair-the-connection-to-azure"></a>Reparación de la conexión a Azure

En el cuadro de diálogo **Update connection to Azure Media Services** (Actualizar conexión a Azure Media Services) de la página [Video Indexer](https://www.videoindexer.ai/), se le pide que proporcione valores para la siguiente configuración:

|Configuración|Descripción|
|---|---|
|Identificador de suscripción de Azure|El id. de suscripción del inquilino se puede recuperar de Azure Portal. Haga clic en **Todos los servicios** en el panel izquierdo y busque "suscripciones". Seleccione **Subscriptions** (Suscripciones) y elija el identificador deseado de la lista de las suscripciones.|
|Nombre del grupo de recursos de Azure Media Services|El nombre del grupo de recursos en el que creó la cuenta de Media Services.|
|Identificador de aplicación|El identificador de aplicación de Azure AD (con permisos para la cuenta de Media Services especificada) que creó para esta cuenta de Video Indexer. <br/><br/>Para obtener el id. de la aplicación, vaya a Azure Portal. En la cuenta de Media Services, elija su cuenta y vaya a **Acceso de API**. Haga clic en **Conectarse a la API de Media Services con la entidad de servicio** -> **Aplicación de Azure AD**. Copie los parámetros pertinentes.|
|Clave de la aplicación|La clave de aplicación de Azure AD asociada a la cuenta de Media Services que especificó anteriormente. <br/><br/>Para obtener la clave de aplicación, vaya a Azure Portal. En la cuenta de Media Services, elija su cuenta y vaya a **Acceso de API**. Haga clic en **Conectarse a la API de Media Services con la entidad de servicio** -> **Administrar aplicación** -> **Certificados y secretos**. Copie los parámetros pertinentes.|

## <a name="autoscale-reserved-units"></a>Unidades reservadas de escalado automático

En la página **Configuración** puede establecer el escalado automático de unidades reservadas de multimedia (RU). Si la opción es **On** (Activado), puede asignar el número máximo de unidades reservadas y asegurarse de que Video Indexer detiene o inicia las unidades reservadas automáticamente. Con esta opción, no tiene que pagar ningún costo adicional por el tiempo de inactividad, pero tampoco tiene que esperar mucho tiempo a que los trabajos de indexación se completen cuando la carga de indexación es elevada.

El escalado automático no escalará por debajo de una unidad reservada ni por encima del límite predeterminado de la cuenta de Media Services. Con el fin de aumentar el límite, cree una solicitud de servicio. Para obtener información sobre las cuotas y limitaciones y sobre cómo abrir una incidencia de soporte técnico, consulte [Cuotas y limitaciones](../../media-services/previous/media-services-quotas-and-limitations.md).

![Unidades reservadas de escalado automático en Video Indexer](./media/manage-account-connected-to-azure/autoscale-reserved-units.png)

## <a name="errors-and-warnings"></a>Errores y advertencias

Si la cuenta precisa de algunos ajustes, verá errores y advertencias pertinentes sobre la configuración de la cuenta en la página **Settings** (Configuración). Los mensajes incluyen vínculos a las ubicaciones exactas de Azure Portal donde debe realizar los cambios. En esta sección se proporcionan más detalles sobre los errores y mensajes de advertencia.

* EventGrid

    Debe registrar el proveedor de recursos de EventGrid mediante Azure Portal. En [Azure Portal](https://portal.azure.com/), vaya a **Suscripciones** > [suscripción] > **Proveedores de recursos** > **Microsoft.EventGrid**. Si no se encuentra en el estado **Registrado**, haga clic en **Registrarse**. Tarda unos minutos en registrarse.

* extremo de streaming

    Asegúrese de que la cuenta de Media Services subyacente tiene el valor predeterminado del **Punto de conexión de streaming** en un estado iniciado. De lo contrario, no podrá ver vídeos desde esta cuenta de Media Services ni en Video Indexer.

* Unidades reservadas de multimedia

    Debe asignar unidades reservadas de multimedia al recurso de Media Services para poder indexar vídeos. Para optimizar el rendimiento de indexación, se recomienda asignar al menos diez unidades reservadas S3. Para obtener información sobre precios, vea la sección de preguntas más frecuentes de la página de [precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="next-steps"></a>Pasos siguientes

Para interactuar mediante programación con su cuenta de evaluación gratuita o con sus cuentas de Video Indexer conectadas a Azure, siga las instrucciones descritas en: [Uso de las API](video-indexer-use-apis.md).

Utilice el mismo usuario de Azure AD que usa al conectarse a Azure.
