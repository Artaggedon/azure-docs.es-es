---
title: Creación de una cuenta de Video Indexer conectada a Azure
titleSuffix: Azure Media Services
description: Obtenga información sobre cómo crear una cuenta de Video Indexer conectada a Azure.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 10/21/2020
ms.author: juliako
ms.openlocfilehash: 82dc9aa9615ef86c878fb75df6650dcc1f904a8f
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2020
ms.locfileid: "97702630"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Creación de una cuenta de Video Indexer conectada a Azure

Al crear una cuenta de Video Indexer, puede optar entre una cuenta de evaluación gratuita (donde obtiene un número determinado de minutos gratuitos de indexación) o una opción de pago (donde no está limitado por la cuota). Con una versión de evaluación gratuita, Video Indexer proporciona hasta 600 horas de indexación gratuita a los usuarios de sitios web y hasta 2400 minutos de indexación gratuita a los usuarios de API. Con la opción de pago, se crea una cuenta de Video Indexer que está conectada a la suscripción de Azure. Se paga por minutos de indexación. Para más información, consulte [Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/).

En este artículo se muestra cómo crear una cuenta de Video Indexer que está vinculada a su suscripción de Azure y a una cuenta de Azure Media Services. Se proporcionan los pasos para conectarse a Azure mediante el flujo automático (predeterminado). También se muestra cómo conectarse a Azure manualmente (avanzado).

Si va a pasar de una *cuenta de prueba* a una *cuenta de pago* de Video Indexer, puede copiar todos los vídeos y la personalización del modelo en la nueva cuenta, como se describe en la sección [Importación del contenido de la cuenta de prueba](#import-your-content-from-the-trial-account).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure.

    Si aún no tiene una suscripción de Azure, [regístrese para obtener una cuenta de evaluación gratuita de Azure](https://azure.microsoft.com/free/).
* Un dominio de Azure Active Directory (Azure AD).

    Si no tiene un dominio de Azure AD, créelo con su suscripción de Azure. Para más información, vea [Administración de nombres de dominio personalizados en Azure AD](../../active-directory/enterprise-users/domains-manage.md).
* Un usuario en el dominio de Azure AD con un rol **Administrador de aplicaciones**. Usará a este miembro para conectar su cuenta de Video Indexer a Azure.

    Este usuario debe ser un usuario de Azure AD con una cuenta profesional o educativa. No use una cuenta personal, como outlook.com, live.com o hotmail.com.

    ![todos los usuarios de AAD](./media/create-account/all-aad-users.png)

### <a name="additional-prerequisites-for-automatic-flow"></a>Requisitos previos adicionales para el flujo automático

* Un usuario y un miembro del dominio de Azure AD.

    Usará a este miembro para conectar su cuenta de Video Indexer a Azure.

    Este usuario debe ser miembro de la suscripción de Azure con un rol de **Propietario**, o con los roles de **Colaborador** y **Administrador de acceso de usuario**. Un usuario se puede agregar dos veces, con dos roles. Una vez como Colaborador y otra como Administrador de acceso de usuario. Para más información, consulte [Visualización del acceso de un usuario a los recursos de Azure](../../role-based-access-control/check-access.md).

    ![control de acceso](./media/create-account/access-control-iam.png)

### <a name="additional-prerequisites-for-manual-flow"></a>Requisitos previos adicionales para el flujo manual

* Registrar el proveedor de recursos EventGrid mediante Azure Portal.

    En [Azure Portal](https://portal.azure.com/), vaya a **Suscripciones**->[suscripción]->**ResourceProviders**.

    Busque **Microsoft.Media** y **Microsoft.EventGrid**. Si no se encuentra en el estado "Registrado", haga clic en **Registrarse**. Tarda unos minutos en registrarse.

    ![EventGrid](./media/create-account/event-grid.png)

## <a name="create-a-new-account"></a>Creación de una nueva cuenta

> [!NOTE]
> Si su suscripción de Azure usa la autenticación multifactor basada en certificados, es fundamental que siga estos pasos en un dispositivo que tenga instalados los certificados necesarios.

1. Vaya al sitio web de [Video Indexer](https://www.videoindexer.ai/) e inicie sesión.
1. Seleccione el botón **Crear cuenta ilimitada**:

    ![Creación de una cuenta de Video Indexer](./media/create-account/create-unlimited-account.png)
1. Cuando aparezca la lista de suscripciones, seleccione la suscripción que desea usar.

    ![Conexión de Video Indexer a Azure](./media/create-account/new-account-on-azure-subscription.png)
1. Seleccione una región de Azure de las ubicaciones admitidas: Oeste de EE. UU. 2, Norte de Europa o Este de Asia.
1. En **Cuenta de Azure Media Services**, elija una de estas opciones:

    * Para crear una cuenta de Media Services nueva, seleccione **Crear nuevo grupo de recursos**. Escriba un nombre para el grupo de recursos.

        Azure creará la nueva cuenta en su suscripción, incluida una nueva cuenta de Azure Storage.  
    * Para usar una cuenta de Media Services existente, seleccione **Usar el recurso existente**. En la lista de cuentas, seleccione su cuenta.

        La cuenta de Media Services debe tener la misma región que la cuenta de Video Indexer.

        > [!NOTE]
        > Para reducir la duración de la indexación y el bajo rendimiento, se recomienda encarecidamente ajustar el tipo y el número de [unidades reservadas](../previous/media-services-scale-media-processing-overview.md ) a **10 unidades reservadas S3** en la cuenta de Media Services. Vea [Uso del portal para cambiar las unidades reservadas](../previous/media-services-portal-scale-media-processing.md). Las unidades reservadas se cobran a su cuenta, consulte los [detalles de precios](https://azure.microsoft.com/pricing/details/media-services/#analytics).
    * Para configurar manualmente la conexión, seleccione el vínculo **Switch to manual configuration** (Cambiar a configuración manual).

        Para más información, consulte la sección [Conexión manual de Azure](#connect-to-azure-manually-advanced-option) (opción avanzada) que viene a continuación.
1. Cuando termine, seleccione **Crear**. Esta operación puede tardar unos minutos.

    Después de conectarse a Azure, la nueva cuenta de Video Indexer aparece en la lista de cuentas:

    ![nueva cuenta](./media/create-account/new-account.png)
1. Asegúrese de que el punto de conexión de streaming de la cuenta de Media Services esté en ejecución antes de poder reproducir los vídeos en la aplicación web Video Indexer (presione Iniciar si es estado detenido).

> [!TIP]
> Para proporcionar un nombre descriptivo a su cuenta, vaya a **Configuración**.

## <a name="connect-to-azure-manually-advanced-option"></a>Conexión manual a Azure (opción avanzada)

Si se produjo un error en la conexión a Azure, puede intentar solucionar el problema mediante la conexión manual.

> [!NOTE]
> Se recomienda encarecidamente tener las siguientes tres cuentas en la misma región: la cuenta de Video Indexer a la que se conecta con la cuenta de Media Services, así como la cuenta de almacenamiento de Azure conectada a la misma cuenta de Media Services.

### <a name="create-and-configure-a-media-services-account"></a>Creación y configuración de una cuenta de Media Services

1. Use [Azure Portal](https://portal.azure.com/) para crear una cuenta de Azure Media Services, como se describe en [Creación de una cuenta](../previous/media-services-portal-create-account.md).

     Asegúrese de que la cuenta de Media Services se creó con las API clásicas. 
 
    ![API clásica de Media Services](./media/create-account/enable-classic-api.png)


    Al crear una cuenta de almacenamiento para la cuenta de Media Services, seleccione **StorageV2** como tipo de cuenta y **Almacenamiento con redundancia geográfica (GRS)** en los campos de replicación.

    ![Nueva cuenta de ASM](./media/create-account/create-new-ams-account.png)

    > [!NOTE]
    > Asegúrese de anotar los nombres de recurso y cuenta de Media Services. Los necesitará para los pasos de la siguiente sección.
1. Ajuste el tipo y número de [unidades reservadas](../previous/media-services-scale-media-processing-overview.md ) a **10 unidades reservadas S3** en la cuenta de Media Services que ha creado. Vea [Uso del portal para cambiar las unidades reservadas](../previous/media-services-portal-scale-media-processing.md).

    Las unidades reservadas se cobran a su cuenta, consulte los [detalles de precios](https://azure.microsoft.com/pricing/details/media-services/#analytics).
1. Para poder reproducir los vídeos en la aplicación web de Video Indexer, debe iniciar el **punto de conexión de streaming** predeterminado de la nueva cuenta de Media Services.

    En la nueva cuenta de Media Services, seleccione **Puntos de conexión de streaming**. Luego, seleccione el punto de conexión de streaming y presione Iniciar.

    ![Puntos de conexión de streaming](./media/create-account/create-ams-account-se.png)
4. Para que Video Indexer se autentique con la API de Media Services, es necesario crear una aplicación de AD. Los pasos siguientes le guían por el proceso de autenticación de Azure AD que se describe en [Introducción a la autenticación de Azure AD mediante Azure Portal](../previous/media-services-portal-get-started-with-aad.md):

    1. En la nueva cuenta de Media Services, seleccione **Acceso de API**.
    2. Seleccione [Service principal authentication method](../previous/media-services-portal-get-started-with-aad.md) (Método de autenticación de la entidad de servicio).
    3. Obtención del identificador de cliente y del secreto de cliente

        Después de seleccionar **Configuración**->**Claves**, agregue una **Descripción**, presione **Guardar** y el valor de la clave se rellenará.

        Si la clave expira, el propietario de la cuenta tendrá que ponerse en contacto con el soporte técnico de Video Indexer para renovar la clave.

        > [!NOTE]
        > Asegúrese de anotar el valor de clave y el identificador de aplicación. Los necesitará para los pasos de la siguiente sección.

### <a name="connect-manually"></a>Conexión manual

En el cuadro de diálogo **Crear una cuenta nueva en una suscripción a Azure** de su página [Video Indexer](https://www.videoindexer.ai/), seleccione el vínculo **Cambiar a configuración manual**.

En el cuadro de diálogo, proporcione la siguiente información:

|Configuración|Descripción|
|---|---|
|Región de la cuenta de Video Indexer|El nombre de la región de la cuenta de Video Indexer. Para lograr un mejor rendimiento y reducir los costos, se recomienda encarecidamente especificar el nombre de la región donde se encuentran el recurso de Azure Media Services y la cuenta de Azure Storage. |
|Inquilino de Azure AD|El nombre del inquilino de Azure AD, por ejemplo, "contoso.onmicrosoft.com". La información del inquilino se puede recuperar desde Azure Portal. Coloque el cursor sobre el nombre del usuario con sesión iniciada en la esquina superior derecha. Busque el nombre a la derecha de **Domain** (Dominio).|
|Id. de suscripción|La suscripción de Azure en la que debe crearse esta conexión. El id. de suscripción del inquilino se puede recuperar de Azure Portal. Seleccione **Todos los servicios** en el panel izquierdo y busque "suscripciones". Seleccione **Subscriptions** (Suscripciones) y elija el identificador deseado de la lista de las suscripciones.|
|Nombre del grupo de recursos de Azure Media Services|El nombre del grupo de recursos en el que creó la cuenta de Media Services.|
|Nombre del recurso de Media Services|El nombre de la cuenta de Azure Media Services que creó en la sección anterior.|
|Identificador de aplicación|El identificador de aplicación de Azure AD (con permisos para la cuenta de Media Services especificada) que creó en la sección anterior.|
|Clave de la aplicación|La clave de aplicación de Azure AD que creó en la sección anterior. |

## <a name="import-your-content-from-the-trial-account"></a>Importación del contenido de la cuenta de *prueba*

Al crear una cuenta, tiene la opción de importar el contenido de la cuenta de *prueba* a la nueva cuenta. Si activa la opción *Importar* en el cuadro de diálogo **Crear una cuenta nueva en una suscripción a Azure**, todas las personalizaciones de modelos de contenido y medios se copiarán de la cuenta de *prueba* a la nueva cuenta.

La capacidad de importar el contenido es válida para los enfoques automatizados y manuales descritos anteriormente.

> [!NOTE]
> El contenido solo se puede importar una vez desde cada cuenta.

## <a name="delete-the-account"></a>Eliminación de la cuenta

Si más adelante quiere eliminar la cuenta, puede eliminarla desde el sitio web de Video Indexer. Para eliminar la cuenta, debe ser el propietario.

Seleccione la cuenta de -> **Settings** -> **Delete this account** (Configuración > Eliminar esta cuenta). 

La cuenta se eliminará de manera permanente en 90 días.

## <a name="considerations"></a>Consideraciones

Tenga en cuenta las siguientes consideraciones con relación a Azure Media Services:

* Si tiene previsto conectarse a una cuenta de Media Services, asegúrese de que la cuenta de Media Services se haya creado con las API clásicas. 
 
    ![API clásica de Media Services](./media/create-account/enable-classic-api.png)
* Si se conecta a una cuenta de Media Services existente, Video Indexer no cambia la configuración existente de **unidades reservadas**.

   Es posible que tenga que ajustar el tipo y número de unidades reservadas, según la carga planeada. Tenga en cuenta que si la carga es alta y no tiene suficientes unidades o velocidad, el procesamiento de los vídeos pueden producir errores de tiempo de espera.
* Si se conecta a una nueva cuenta de Media Services, Video Indexer inicia automáticamente en ella el **punto de conexión de streaming** predeterminado:

    ![Punto de conexión de streaming de Media Services](./media/create-account/ams-streaming-endpoint.png)

    Los puntos de conexión de streaming tienen un tiempo de inicio considerable. Por lo tanto, pueden transcurrir varios minutos desde que conecta su cuenta a Azure hasta que los vídeos se puedan transmitir y ver en la aplicación web de Video Indexer.
* Si se conecta a una cuenta de Media Services existente, Video Indexer no cambia la configuración del punto de conexión de streaming predeterminado. Si no hay ningún **punto de conexión de streaming** en ejecución, no podrá ver vídeos desde esta cuenta de Media Services ni en Video Indexer.
* Si se conecta automáticamente, Video Indexer establece las **unidades reservadas** en 10 unidades de S3:

    ![Unidades reservadas de Media Services](./media/create-account/ams-reserved-units.png)
    
## <a name="automate-creation-of-the-video-indexer-account"></a>Automatización de la creación de la cuenta de Video Indexer

Para automatizar la creación de la cuenta hay un proceso de dos pasos:
 
1. Use Azure Resource Manager para crear una cuenta de Azure Media Services y una aplicación de Azure AD.

    Consulte un ejemplo de la [plantilla de creación de cuentas de Media Services](https://github.com/Azure-Samples/media-services-v3-arm-templates).
1. Llame a [Create-Account con la aplicación de Media Services y Azure AD](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Paid-Account).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado este tutorial, elimine los recursos que no planea usar.

## <a name="next-steps"></a>Pasos siguientes

Para interactuar mediante programación con su cuenta de evaluación gratuita o con sus cuentas de Video Indexer conectadas a Azure, siga las instrucciones descritas en: [Uso de las API](video-indexer-use-apis.md).

Debe utilizar el mismo usuario de Azure AD que usa al conectarse a Azure.
