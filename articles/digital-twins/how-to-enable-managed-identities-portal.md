---
title: Habilitación de una identidad administrada para el enrutamiento de eventos (versión preliminar) mediante Azure Portal
titleSuffix: Azure Digital Twins
description: Vea cómo habilitar una identidad asignada por el sistema para Azure Digital Twins y úsela para reenviar eventos mediante Azure Portal.
author: baanders
ms.author: baanders
ms.date: 1/21/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 5c216661e45e5ed34a95e8a56002f3ad175b089a
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100545925"
---
# <a name="enable-a-managed-identity-for-routing-azure-digital-twins-events-preview-azure-portal"></a>Habilitación de una identidad administrada para el enrutamiento de eventos de Azure Digital Twins (versión preliminar): Azure portal

[!INCLUDE [digital-twins-managed-service-identity-selector.md](../../includes/digital-twins-managed-service-identity-selector.md)]

En este artículo se describe cómo habilitar una [identidad asignada por el sistema para una instancia de Azure Digital Twins](concepts-security.md#managed-identity-for-accessing-other-resources-preview) (actualmente en versión preliminar) y usar la identidad al reenviar eventos a destinos admitidos como [centro de eventos ](../event-hubs/event-hubs-about.md), [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)  y [Azure Storage Container](../storage/blobs/storage-blobs-introduction.md).

En este artículo se describe el proceso que utiliza [**Azure Portal**](https://portal.azure.com).

Estos son los pasos que se describen en este artículo: 

1. Creación una instancia de Azure Digital Twins con una identidad asignada por el sistema o habilitación de la identidad asignada por el sistema en una instancia existente de Azure Digital Twins. 
1. Adición de un rol adecuado a la identidad. Por ejemplo, asigne el rol **Emisor de datos de Centro de eventos de Azure** a la identidad si el punto de conexión es Centro de eventos, o bien el **rol Emisor de datos de Azure Service Bus** si es Service Bus.
1. Cree un punto de conexión en Azure Digital Twins que pueda usar identidades asignadas por el sistema para la autenticación.

## <a name="enable-system-managed-identities-for-an-instance"></a>Habilitación de identidades administradas por el sistema para una instancia 

Cuando se habilita una identidad asignada por el sistema en la instancia de Azure Digital Twins, Azure crea automáticamente una identidad para ella en [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md). Esa identidad se puede usar para autenticarse en los puntos de conexión de Azure Digital Twins para el reenvío de eventos.

Puede activar identidades administradas por el sistema para una instancia de Azure Digital Twins **como parte de la configuración inicial de la instancia**, o bien **habilitarlas más adelante en una instancia que ya exista**.

Cualquiera de estos métodos de creación proporcionará las mismas opciones de configuración y el mismo resultado final para la instancia. En esta sección se describe cómo realizar ambos.

### <a name="add-a-system-managed-identity-during-instance-creation"></a>Adición de una identidad administrada por el sistema durante la creación de la instancia

En esta sección, aprenderá a habilitar una identidad administrada por el sistema en una instancia de Azure Digital Twins que se está creando actualmente. Esta sección se centra en el paso de la identidad administrada del proceso de creación; para obtener un tutorial completo sobre la creación de una instancia de Azure Digital Twins, vea [*Procedimiento: Configuración de una instancia y autenticación*](how-to-set-up-instance-portal.md).

La opción de identidad administrada por el sistema se encuentra en la pestaña **Avanzado** de configuración de la instancia.

En esta pestaña, seleccione la opción **Activar** para **Identidad administrada por el sistema** a fin de activar esta característica.

:::image type="content" source="media/how-to-enable-managed-identities/create-instance-advanced.png" alt-text="Captura de pantalla de Azure Portal en la que se muestra la pestaña Avanzado del cuadro de diálogo Crear recurso para Azure Digital Twins. Hay un resaltado alrededor del nombre de la pestaña, la opción Activar para Identidad administrada por el sistema y los botones de navegación (Revisar y crear, Anterior, Siguiente: Avanzado).":::

Después, puede usar los botones de navegación inferiores para continuar con el resto de la configuración de la instancia.

### <a name="add-a-system-managed-identity-to-an-existing-instance"></a>Adición de una identidad administrada por el sistema a una instancia existente

En esta sección, agregará una identidad administrada por el sistema a una instancia de Azure Digital Twins que ya existe.

1. En primer lugar, navegue a [Azure Portal](https://portal.azure.com) en un explorador.

1. Busque el nombre de la instancia en la barra de búsqueda del portal y selecciónela para ver sus detalles.

1. Seleccione **Identidad (versión preliminar)** en el menú de la izquierda.

1. En esta página, seleccione la opción **Activar** para activar esta característica.

1. Presione el botón **Guardar** y **Sí** para confirmar.

    :::image type="content" source="media/how-to-enable-managed-identities/identity-digital-twins.png" alt-text="Captura de pantalla de Azure Portal en la que se muestra la página Identidad (versión preliminar) de una instancia de Azure Digital Twins. Hay un resaltado alrededor del nombre de la página en el menú de la instancia de Azure Digital Twins, la opción Activar para Estado, el botón Guardar y el botón de confirmación Sí.":::

Una vez que se haya guardado el cambio, aparecerán más campos en esta página para el **Id. de objeto** y los **Permisos** de la nueva identidad.

Puede copiar el **Id. de objeto** desde aquí si es necesario y usar el botón **Permisos** para ver los roles de Azure que están asignados a la identidad. Para configurar algunos roles, vaya a la sección siguiente.

## <a name="assign-azure-roles-to-the-identity"></a>Asignación de roles de Azure a la identidad 

Una vez que se haya creado una identidad asignada por el sistema para la instancia de Azure Digital Twins, tendrá que asignarle los roles adecuados para que se autentique con distintos tipos de [puntos de conexión ](concepts-route-events.md) a fin de reenviar eventos a los destinos admitidos. En esta sección se describen las opciones de rol y cómo asignarlos a la identidad asignada por el sistema.

>[!NOTE]
> Este es un paso importante; sin él, la identidad no podrá acceder a los puntos de conexión y los eventos no se entregarán.

### <a name="supported-destinations-and-azure-roles"></a>Destinos admitidos y roles de Azure 

Estos son los roles mínimos que necesita una identidad para acceder a un punto de conexión, en función del tipo de destino. Los roles con permisos más altos (como los de propietario de datos) también funcionarán.

| Destination | Rol de Azure |
| --- | --- |
| Azure Event Hubs | Emisor de datos de Azure Event Hubs |
| Azure Service Bus | Emisor de datos de Azure Service Bus |
| Contenedor de almacenamiento de Windows Azure | Colaborador de datos de blobs de almacenamiento |

Para obtener más información sobre los puntos de conexión, las rutas y los tipos de destinos que se admiten para el enrutamiento en Azure Digital Twins, vea [*Conceptos: rutas de eventos*](concepts-route-events.md).

### <a name="assign-the-role"></a>Asignación del rol

>[!NOTE]
> Esta sección la debe completar un usuario de Azure con permisos para administrar el acceso de los usuarios a los recursos de Azure (incluidas la concesión y delegación de permisos). Los roles comunes que cumplen este requisito son *propietario*, *administrador de cuentas* o la combinación de *administrador de acceso de usuarios* y *colaborador*. Para obtener más información sobre los requisitos de permisos para los roles de Azure Digital Twins, vea [*Procedimiento para configuración de una instancia de Azure Digital Twins y autenticación*](how-to-set-up-instance-portal.md#prerequisites-permission-requirements).

Para asignar un rol a la identidad, abra primero [Azure Portal](https://portal.azure.com).

1. Navegue hasta el recurso de punto de conexión (el centro de eventos, el tema de Service Bus o el contenedor de almacenamiento); para ello, busque su nombre en la barra de búsqueda del portal. 
1. Seleccione **Control de acceso (IAM)** en el panel de la izquierda.
1. Seleccione el botón **+ Agregar** para agregar una nueva asignación de roles.

    :::image type="content" source="media/how-to-enable-managed-identities/add-role-assignment-1.png" alt-text="Captura de pantalla de Azure Portal en la que se muestra la página Control de acceso (IAM) de un centro de eventos. El botón + Agregar está resaltado." lightbox="media/how-to-enable-managed-identities/add-role-assignment-1.png":::

1. En la página **Agregar asignación de roles** siguiente, rellene los valores:
    * **Rol**: seleccione el rol deseado en el menú desplegable.
    * **Asignar acceso a**: En **Identidad administrada asignada por el sistema**, seleccione **Digital Twins**.
    * **Suscripción**: seleccione su suscripción. Se mostrarán todas las identidades administradas de Azure Digital Twins en la suscripción seleccionada.
    * **Select**: aquí, seleccionará la identidad administrada de la instancia de Azure Digital Twins a la que se asigna el rol. El nombre de la identidad administrada coincide con el de la instancia, por lo que debe elegir el nombre de la instancia de Azure Digital Twins. Cuando lo seleccione, la identidad de la instancia aparecerá en la sección **Miembros seleccionados** en la parte inferior del panel.

    :::row:::
        :::column:::
            :::image type="content" source="media/how-to-enable-managed-identities/add-role-assignment-2.png" alt-text="Relleno de los campos indicados en el diálogo Agregar una asignación de roles":::
        :::column-end:::
        :::column:::
        :::column-end:::
    :::row-end:::

Cuando acabe de escribir los detalles, seleccione **Guardar**.

## <a name="create-an-endpoint-with-identity-based-authorization"></a>Creación de un punto de conexión con autorización basada en identidades

Después de configurar una identidad administrada por el sistema para la instancia de Azure Digital Twins y asignarle los roles adecuados, puede crear [puntos de conexión](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins) de Azure Digital Twins que sean capaces de usar la identidad para la autenticación. Esta opción solo está disponible para los puntos de conexión de centro de eventos y de tipo Service Bus (no se admite para Event Grid).

>[!NOTE]
> No se puede editar un punto de conexión que ya se haya creado con la identidad basada en claves para cambiar a la autenticación basada en identidades. Debe elegir el tipo de autenticación al crear el punto de conexión por primera vez.

Siga las [instrucciones para crear un punto de conexión de Azure Digital Twins](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins).

Cuando llegue al paso de completar los detalles necesarios para el tipo de punto de conexión, asegúrese de seleccionar **Basado en identidades** para el tipo de autenticación.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-event-hub-authentication.png" alt-text="Captura de pantalla de la creación de un punto de conexión de tipo de centro de eventos." lightbox="media/how-to-manage-routes-portal/create-endpoint-event-hub-authentication.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Termine de configurar el punto de conexión y seleccione **Guardar**.

## <a name="considerations-for-disabling-system-managed-identities"></a>Consideraciones para deshabilitar las identidades administradas por el sistema

Como una identidad se administra de forma independiente a los puntos de conexión que la usan, es importante tener en cuenta los efectos que cualquier cambio en la identidad o sus roles puede tener en los puntos de conexión de la instancia de Azure Digital Twins. Si la identidad está deshabilitada o se quita un rol necesario para un punto de conexión, este puede dejar de estar accesible y se interrumpirá el flujo de eventos.

Para seguir usando un punto de conexión que se ha configurado con una identidad administrada que ahora se ha deshabilitado, tendrá que eliminar el punto de conexión y [volver a crearlo](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins) con otro tipo de autenticación. Los eventos pueden tardar hasta una hora en reanudar la entrega al punto de conexión después de este cambio.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las identidades administradas en Azure AD: 
* [*Identidades administradas de recursos de Azure*](../active-directory/managed-identities-azure-resources/overview.md)