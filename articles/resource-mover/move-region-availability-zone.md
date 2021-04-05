---
title: Traslado de máquinas virtuales de Azure a las zonas de disponibilidad de otra región mediante Azure Resource Mover
description: Aprenda a trasladar máquinas virtuales de Azure a otras zonas de disponibilidad mediante Azure Resource Mover.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 88006fb354af2673496c6476090d7f73c8a005e6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "95543007"
---
# <a name="move-azure-vms-to-an-availability-zone-in-another-region"></a>Traslado de máquinas virtuales de Azure a una zona de disponibilidad de otra región

En este artículo se ofrece información sobre el traslado de las máquinas virtuales de Azure, junto con los recursos de red y almacenamiento relacionados, a otra región de Azure distinta mediante [Azure Resource Mover](overview.md).

Las [zonas de disponibilidad de Azure](../availability-zones/az-overview.md#availability-zones) ayudan a proteger la implementación de Azure ante errores del centro de datos. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. Para garantizar la resistencia, hay un mínimo de tres zonas independientes en todas las [regiones habilitadas](../availability-zones/az-region.md). Con Resource Mover, se puede trasladar:

- Una máquina virtual de instancia única a una zona de disponibilidad o conjunto de disponibilidad de la región de destino.
- Una máquina virtual de un conjunto de disponibilidad a una zona de disponibilidad o conjunto de disponibilidad de la región de destino.
- Una máquina virtual de una zona de disponibilidad de la región de origen a una zona de disponibilidad de la región de destino.


> [!IMPORTANT]
> Azure Resource Mover se encuentra actualmente en versión preliminar pública.

Si desea trasladar las máquinas virtuales a una zona de disponibilidad distinta de la misma región, [revise este artículo](../site-recovery/azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery.md).

## <a name="prerequisites"></a>Requisitos previos

- Acceso de *propietario* en la suscripción en la que se encuentran los recursos que se van a trasladar.
    - La primera vez que agregue un recurso de una asignación de origen y destino específicas a una suscripción de Azure, Azure Resource Mover creará una [identidad administrada asignada por el sistema](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types), anteriormente llamada identidad de servicio administrado (MSI), en la que confiará la suscripción.
    - Para crear la identidad y asignarle el rol requerido (Colaborador o Administrador de acceso de usuario en la suscripción de origen), la cuenta que utilice para agregar recursos necesita permisos de *propietario* para la suscripción. [Obtenga más información](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) sobre los roles de Azure.
- La suscripción necesita tener cuota suficiente para crear los recursos de origen de la región de destino. De lo contrario, hay que solicitar límites adicionales. [Más información](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Compruebe los precios y los cargos asociados con la región de destino a la que va a trasladar las máquinas virtuales. El uso de la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) le resultará útil.
    


## <a name="check-vm-requirements"></a>Comprobación de los requisitos de máquina virtual

1. Asegúrese de que las máquinas virtuales que desea trasladar están admitidas.

    - [Compruebe](support-matrix-move-region-azure-vm.md#windows-vm-support) las máquinas virtuales Windows admitidas.
    - [Compruebe](support-matrix-move-region-azure-vm.md#linux-vm-support) las máquinas virtuales Linux y las versiones de kernel admitidas.
    - Compruebe la configuración admitida de [proceso](support-matrix-move-region-azure-vm.md#supported-vm-compute-settings), [almacenamiento](support-matrix-move-region-azure-vm.md#supported-vm-storage-settings) y [redes](support-matrix-move-region-azure-vm.md#supported-vm-networking-settings).
2. Compruebe que las máquinas virtuales que desea trasladar están encendidas.
3. Asegúrese de que las máquinas virtuales dispongan de los certificados raíz de confianza más recientes, así como de una lista actualizada de revocación de certificados (CRL). 
    - En las máquinas virtuales de Azure que ejecuten Windows, instale las actualizaciones de Windows más recientes.
    - En las máquinas virtuales que ejecuten Linux, siga las instrucciones del distribuidor de Linux para asegurarse de que la máquina tenga los certificados y la CRL más recientes. 
4. Permita la conectividad saliente desde las máquinas virtuales:
    - Si utiliza un servidor proxy de firewall basado en direcciones URL para controlar la conectividad de salida, debe facilitar el acceso a estas [URL](support-matrix-move-region-azure-vm.md#url-access).
    - Si utiliza reglas de grupo de seguridad de red para determinar la conectividad saliente, cree estas [reglas de etiquetas de servicio](support-matrix-move-region-azure-vm.md#nsg-rules).

## <a name="select-resources-to-move"></a>Selección de los recursos que se van a trasladar

Seleccione los recursos que desea trasladar.

- Puede seleccionar cualquier tipo de recurso admitido de los grupos de recursos de la región de origen seleccionada.
- Los recursos se trasladarán a una región de destino de la suscripción de la región de origen. Si desea cambiar la suscripción, puede hacerlo después de trasladar los recursos.

1. En Azure Portal, busque resource mover. A continuación, en **Servicios**, seleccione **Azure Resource Mover**.

    ![Búsqueda de Resource Mover](./media/move-region-availability-zone/search.png)

2. En **Información general**, seleccione **Introducción**.

    ![Botón para comenzar](./media/move-region-availability-zone/get-started.png)

3. En **Mover recursos** > **Origen + destino**, seleccione la suscripción y la región de origen.
4. En **Destino**, seleccione la región a la que desea trasladar las máquinas virtuales. A continuación, haga clic en **Siguiente**.

     ![Página para rellenar la suscripción o región de origen y destino](./media/move-region-availability-zone/source-target.png)

6. En **Recursos que se van a mover** , haga clic en **Seleccionar recursos**.
7. En **Seleccionar recursos**, seleccione la máquina virtual. Solo es posible agregar recursos que se puedan mover. A continuación, haga clic en **Hecho**. En **Recursos que se van a mover**, haga clic en **Siguiente**.

    ![Página para seleccionar las máquinas virtuales que se van a trasladar](./media/move-region-availability-zone/select-vm.png)
8. En **Revisar + agregar**, compruebe la configuración de origen y de destino.

    ![Página para revisar la configuración y realizar el traslado de los recursos](./media/move-region-availability-zone/review.png)

9. Haga clic en **Continuar** para empezar a agregar recursos.
10. Una vez que haya terminado de agregarlos correctamente, haga clic en **Adding resources for move** (Agregar recursos para mover) en el icono de notificación.

    ![Mensaje en notificaciones](./media/move-region-availability-zone/notification.png)

Después de hacer clic en la notificación, los recursos aparecen en la página **Across regions** (Entre regiones).

> [!NOTE]
> Después de hacer clic en la notificación, los recursos aparecen en la página **Across regions** (Entre regiones), en un estado *Prepare pending* (Preparación pendiente).
> - Si desea quitar un recurso de una colección de traslado, el método para hacerlo dependerá del punto en el que se encuentre en dicha operación. [Más información](remove-move-resources.md).

## <a name="resolve-dependencies"></a>Resolución de dependencias

1. Si los recursos muestran el mensaje *Validar dependencias* en la columna **Problemas**, haga clic en el botón **Validar dependencias**. Comienza el proceso de validación.
2. Si se encuentran dependencias, haga clic en **Agregar dependencias**. 
3. En **Agregar dependencias**, seleccione los recursos dependientes > **Agregar dependencias**. Compruebe el progreso en el área de notificaciones.

    ![Botón para agregar dependencias](./media/move-region-availability-zone/add-dependencies.png)

3. Agregue más dependencias si es necesario y vuelva a validarlas. 

    ![Página para agregar otras dependencias](./media/move-region-availability-zone/add-additional-dependencies.png)

4. En la página **Entre regiones**, compruebe que los recursos tienen ahora el estado *Prepare pending* (Preparación pendiente) y que no presentan problemas.

    ![Página que muestra los recursos que tienen el estado de preparación pendiente](./media/move-region-availability-zone/prepare-pending.png)

## <a name="move-the-source-resource-group"></a>Traslado del grupo de recursos de origen 

Antes de poder preparar y trasladar las máquinas virtuales, el grupo de recursos de origen debe existir en la región de destino. 

### <a name="prepare-to-move-the-source-resource-group"></a>Preparación del grupo de recursos de origen para el traslado

Lleve a cabo los siguientes preparativos:

1. En **Entre regiones**, seleccione el grupo de recursos de origen > **Preparar**.
2. En **Preparar recursos**, haga clic en **Preparar**.

    ![Botón para preparar el grupo de recursos de origen](./media/move-region-availability-zone/prepare-resource-group.png)

    Durante el proceso de preparación, Resource Mover genera plantillas de Azure Resource Manager mediante la configuración del grupo de recursos. Los recursos que pertenecen al grupo no se ven afectados.

> [!NOTE]
>  Una vez preparado el grupo de recursos, tendrá el estado *Initiate move pending* (Iniciar movimiento pendiente). 

![Estado que muestra el estado de inicio pendiente](./media/move-region-availability-zone/initiate-resource-group-pending.png)

### <a name="move-the-source-resource-group"></a>Traslado del grupo de recursos de origen

Empiece a trasladar el grupo de recursos como se indica a continuación:

1. En **Entre regiones**, seleccione el grupo de recursos > **Iniciar movimiento**.
2. En **Mover recursos**, haga clic en **Iniciar movimiento**. El grupo de recursos pasará al estado *Initiate move in progress* (Iniciar movimiento en curso).
3. Una vez iniciado el traslado, se creará el grupo de recursos de destino, basado en la plantilla de Resource Manager generada. El grupo de recursos de origen pasa al estado *Commit move Pending* (Confirmar movimiento pendiente).

![Estado que muestra la confirmación de movimiento](./media/move-region-availability-zone/commit-move-pending.png)

Para confirmar y finalizar el proceso de traslado:

1. En **Across regions** (Entre regiones), seleccione el grupo de recursos > **Commit move** (Confirmar movimiento).
2. En **Mover recursos**, haga clic en **Confirmar**.

> [!NOTE]
> Una vez confirmado el traslado, el grupo de recursos de origen tendrá el estado *Delete source pending* (Eliminar origen pendiente).


## <a name="add-a-target-availability-zone"></a>Adición de una zona de disponibilidad de destino

Antes de trasladar el resto de los recursos, hay que establecer una zona de disponibilidad de destino para la máquina virtual.

1. En la página **Accross regions** (Entre regiones), haga clic en el vínculo de la columna **Destination configuration** (Configuración de destino) de la máquina virtual que va a trasladar.

    ![Propiedades de máquina virtual](./media/move-region-availability-zone/select-vm-settings.png)

3. En **Opciones de configuración**, especifique la configuración de la máquina virtual de destino. Se puede configurar una máquina virtual en la región de destino de la siguiente forma:
    -  Cree un recurso equivalente en la región de destino. A excepción de la configuración que especifique, el recurso de destino se crea con la misma configuración que el origen.
    - Seleccione una máquina virtual existente en la región de destino. 

4. En **Zonas**, seleccione la zona en la que desea colocar la máquina virtual cuando se traslade.

    Solo se realizan cambios para el recurso que se edita. Los recursos dependientes deben actualizarse por separado.

5. En **SKU**, especifique el [nivel de Azure](..//virtual-machines/sizes.md) que desea asignar a la máquina virtual de destino.
6. En **Conjunto de disponibilidad**, seleccione un conjunto de disponibilidad si desea que la máquina virtual de destino se ejecute dentro de un conjunto de disponibilidad de la zona de disponibilidad.
7. Seleccione **Save changes** (Guardar los cambios).

    ![Configuración de máquina virtual](./media/move-region-availability-zone/vm-settings.png)


## <a name="prepare-resources-to-move"></a>Preparación de los recursos que se van a trasladar

Ahora que el grupo de recursos de origen se ha trasladado, puede preparar el traslado de los demás recursos.

1. En **Entre regiones**, seleccione los recursos que desea preparar. 

    ![Página para seleccionar la preparación de otros recursos](./media/move-region-availability-zone/prepare-other.png)

2. Seleccione **Preparar**. 

> [!NOTE]
> - Durante el proceso de preparación, el agente de Azure Site Recovery Mobility se instala en las máquinas virtuales para la replicación.
> - Los datos de las máquinas virtuales se replican periódicamente en la región de destino. Esto no afecta a la máquina virtual de origen.
> - Resource Mover genera plantillas de Resource Manager para los demás recursos de origen.
> - Una vez preparados los recursos, pasarán al estado *Initiate move pending* (Iniciar movimiento pendiente).

![Página que muestra los recursos que tienen el estado de iniciar movimiento pendiente](./media/move-region-availability-zone/initiate-move-pending.png)

## <a name="initiate-the-move"></a>Inicio de la migración

Con los recursos ya preparados, puede iniciar el traslado. 

1. En **Entre regiones**, seleccione los recursos que tengan el estado *Initiate move pending* (Iniciar movimiento pendiente). A continuación, haga clic en **Initiate move** (Iniciar movimiento).
2. En **Mover recursos**, haga clic en **Iniciar movimiento**.

    ![Página para iniciar el movimiento de los recursos](./media/move-region-availability-zone/initiate-move.png)

3. Haga un seguimiento del progreso del traslado en la barra de notificaciones.


> [!NOTE]
> - En el caso de las máquinas virtuales, se crearán réplicas en la región de destino. La máquina virtual de origen se apagará y se producirá cierto tiempo de inactividad (normalmente es cuestión de minutos).
> - Resource Mover recreará otros recursos mediante las plantillas de Resource Manager preparadas. Normalmente, no se produce tiempo de inactividad.
> - Después de preparar los recursos, tendrán el estado *Commit move pending* (Confirmar movimiento pendiente).


![Página para mostrar los recursos en un estado de confirmación de movimiento pendiente](./media/move-region-availability-zone/resources-commit-move-pending.png)

## <a name="discard-or-commit"></a>¿Descartar o confirmar?

Tras la operación inicial de traslado, puede decidir si desea confirmarla o descartarla. 

- **Descartar**: puede descartar un traslado cuando esté haciendo pruebas y no quiera trasladar verdaderamente el recurso de origen. Si el traslado se descarta, el recurso volverá a tener el estado *Initiate move pending* (Iniciar movimiento pendiente).
- **Confirmación**: la confirmación completa la operación de traslado a la región de destino. Tras la confirmación, un recurso de origen tendrá el estado *Delete source pending* (Eliminar origen pendiente) y el usuario puede decidir si desea eliminarlo.

## <a name="discard-the-move"></a>Descartar la operación de traslado 

Para descartar la operación de traslado, siga estos pasos:

1. En **Entre regiones**, seleccione los recursos que tengan el estado *Commit move pending* (Confirmar movimiento pendiente) y haga clic en **Descartar movimiento**.
2. En **Descartar movimiento**, haga clic en **Descartar**.
3. Haga un seguimiento del progreso del traslado en la barra de notificaciones.
 

> [!NOTE]
> Después de haber descartado recursos, las máquinas virtuales tendrán el estado *Initiate move pending* (Iniciar movimiento pendiente).

## <a name="commit-the-move"></a>Confirmación de la operación de traslado

Si desea completar la operación de traslado, confírmela. 

1. En **Entre regiones**, seleccione los recursos que tengan el estado *Commit move pending* (Confirmar movimiento pendiente) y haga clic en **Confirmar movimiento**.
2. En **Confirmar recursos**, haga clic en **Confirmar**.

    ![Página para confirmar los recursos y completar la operación de traslado](./media/move-region-availability-zone/commit-resources.png)

3. Haga un seguimiento del progreso de la operación de confirmación en la barra de notificaciones.

> [!NOTE]
> - Tras confirmar el traslado, las máquinas virtuales dejarán de replicarse. La confirmación no afecta a la máquina virtual de origen.
> - La confirmación tampoco afecta a los recursos de redes de origen.
> - Una vez confirmado el traslado, los recursos tendrán el estado *Delete source pending* (Eliminar origen pendiente).

![Página que muestra los recursos que tienen el estado de "eliminar origen pendiente"](./media/move-region-availability-zone/delete-source-pending.png)

## <a name="configure-settings-after-the-move"></a>Configuración de parámetros tras el traslado

El servicio Mobility no se desinstala automáticamente de las máquinas virtuales. Puede desinstalarlo manualmente, o bien mantenerlo si tiene previsto trasladar el servidor de nuevo.


## <a name="delete-source-resources-after-commit"></a>Eliminación de los recursos de origen después de la confirmación

Después del traslado, puede eliminar los recursos de la región de origen si lo desea.

1. En **Entre regiones**, haga clic en el nombre de los recursos de origen que desee eliminar.
2. En la página Propiedades de cada recurso, seleccione **Eliminar**.

## <a name="delete-additional-resources-created-for-move"></a>Eliminación de recursos adicionales creados para el traslado

Después del traslado, puede eliminar manualmente la colección de traslado, junto con los recursos de Site Recovery que se crearon.

- La colección de traslado está oculta de forma predeterminada. Para verla, debe activar los recursos ocultos.
- El almacenamiento en caché tiene un bloqueo que debe eliminarse antes de que se pueda eliminar la colección.

Realice la eliminación de la siguiente manera: 

1. Identifique los recursos pertenecientes al grupo ```RegionMoveRG-<sourceregion>-<target-region>```.
2. Compruebe que todas las máquinas virtuales y los demás recursos de la región de origen se hayan trasladado o eliminado. De este modo, se garantiza que no haya recursos pendientes que los utilicen.
2. Eliminación de los recursos:

    - El nombre de la colección de traslado es ```movecollection-<sourceregion>-<target-region>```.
    - El nombre de la cuenta de almacenamiento en caché es ```resmovecache<guid>```.
    - El nombre del almacén es ```ResourceMove-<sourceregion>-<target-region>-GUID```.

## <a name="next-steps"></a>Pasos siguientes

[Más información](about-move-process.md) acerca del proceso de traslado.