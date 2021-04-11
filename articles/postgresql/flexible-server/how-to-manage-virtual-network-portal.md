---
title: 'Administración de redes virtuales - Azure Portal: Servidor flexible de Azure Database for PostgreSQL'
description: Creación y administración de redes virtuales en Servidor flexible de Azure Database for PostgreSQL mediante Azure Portal
author: rothja
ms.author: jroth
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 8a3c983a60dc542cf83f9e818b7f9c1f20265b49
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2021
ms.locfileid: "106552366"
---
# <a name="create-and-manage-virtual-networks-for-azure-database-for-postgresql---flexible-server-using-the-azure-portal"></a>Creación y administración de redes virtuales en Servidor flexible de Azure Database for PostgreSQL mediante Azure Portal

> [!IMPORTANT]
> Servidor flexible de Azure Database for PostgreSQL está en versión preliminar

Azure Database for PostgreSQL: Servidor flexible admite dos tipos de métodos de conectividad de red mutuamente excluyentes para conectarse al servidor flexible. Las dos opciones son las siguientes:

* Acceso público (direcciones IP permitidas)
* Acceso privado (integración con red virtual)

Este artículo se centrará en la creación de un servidor PostgreSQL con **acceso privado (integración con red virtual)** mediante Azure Portal. Con Acceso privado (integración con red virtual), puede implementar el servidor flexible en la propia instancia de [Azure Virtual Network](../../virtual-network/virtual-networks-overview.md). Las redes virtuales de Azure proporcionan una comunicación de red privada y segura. Con el acceso privado, las conexiones con el servidor PostgreSQL están restringidas a la red virtual. Para obtener más información al respecto, consulte [Acceso privado (Integración con red virtual)](./concepts-networking.md#private-access-vnet-integration).

Puede implementar el servidor flexible en una red virtual y una subred durante la creación del servidor. Una vez que se haya implementado el servidor flexible, no se puede trasladar a otra red virtual, subred o a *Acceso público (direcciones IP permitidas)* .

## <a name="prerequisites"></a>Requisitos previos
Para crear un servidor flexible en una red virtual, necesitará lo siguiente:
- Una [red virtual](../../virtual-network/quick-create-portal.md#create-a-virtual-network)
    > [!Note]
    > La red virtual y la subred deben estar en la misma región y suscripción que el servidor flexible.

-  [Delegar una subred](../../virtual-network/manage-subnet-delegation.md#delegate-a-subnet-to-an-azure-service) a **Microsoft.DBforPostgreSQL/flexibleServers**. Esta delegación significa que solo los servidores flexibles de Azure Database for PostgreSQL pueden usar esa subred. No puede haber otros tipos de recursos de Azure en la subred delegada.
-  Agregue `Microsoft.Storage` al punto de conexión de servicio de la subred delegada en los servidores flexibles. Para ello, siga estos pasos:
     1. Vaya a la página de la red virtual.
     2. Seleccione la red virtual en la que planea implementar el servidor flexible.
     3. Elija la subred delegada para el servidor flexible.
     4. En la pantalla de extracción, en **Punto de conexión de servicio**, elija `Microsoft.storage` de la lista desplegable.
     5. Guarde los cambios.


## <a name="create-azure-database-for-postgresql---flexible-server-in-an-already-existing-virtual-network"></a>Creación de Servidor flexible de Azure Database for PostgreSQL en una red virtual existente

1. En la esquina superior izquierda del portal, seleccione **Crear un recurso** (+).
2. Seleccione **Bases de datos** > **Azure Database for PostgreSQL**. También puede escribir **PostgreSQL** en el cuadro de búsqueda para encontrar el servicio.
3. Seleccione **Servidor flexible** como opción de implementación.
4. Rellene el formulario **Datos básicos**.
5. Vaya a la pestaña **Redes** para configurar cómo se quiere conectar al servidor.
6. En **Método de conectividad**, seleccione **Acceso privado (integración con red virtual)** . Vaya a **Red virtual** y seleccione la *red virtual* y la *subred* ya existentes creadas como parte de los requisitos previos anteriores.
7. Seleccione **Revisar y crear** para revisar la configuración del servidor flexible.
8. Seleccione **Crear** para realizar el aprovisionamiento del servidor. El aprovisionamiento puede tardar unos minutos.

>[!Note]
> Una vez que se haya implementado el servidor flexible en una red virtual y una subred, no se puede trasladar a Acceso público (direcciones IP permitidas).
## <a name="next-steps"></a>Pasos siguientes
- [Creación y administración de redes virtuales en Servidor flexible de Azure Database for PostgreSQL mediante la CLI de Azure](./how-to-manage-virtual-network-cli.md).
- Más información sobre [Redes en Servidor flexible de Azure Database for PostgreSQL](./concepts-networking.md)
- Información más detallada sobre [Red virtual de Servidor flexible de Azure Database for PostgreSQL](./concepts-networking.md#private-access-vnet-integration).