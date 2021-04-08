---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 11/09/2018
ms.author: clemensv
ms.openlocfilehash: 500db9f6d299ea81b1f1b1be864df5d91ba1eae7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "76020841"
---
1. Inicie sesión en [Azure Portal][Azure portal].
1. Seleccione **Crear un recurso**. Seleccione **Integración** > **Retransmisión**. Si no ve **Retransmisión** en la lista, seleccione **Ver todo** en la esquina superior derecha.
1. Seleccione **Crear** y escriba un nombre de espacio de nombres en el campo **Nombre**. Azure Portal comprueba si el nombre está disponible.
1. Elija la suscripción de Azure en la que se va a crear el espacio de nombres.
1. En [Grupo de recursos](../articles/azure-resource-manager/management/manage-resource-groups-portal.md), elija un grupo de recursos existente en el que se colocará el espacio de nombres o cree uno.  
1. Seleccione el país o región donde se debe hospedar el espacio de nombres.

    ![Crear un espacio de nombres][create-namespace]

1. Seleccione **Crear**. Azure Portal crea ahora el espacio de nombres del servicio y lo habilita. Tras unos minutos, el sistema realiza el aprovisionamiento de los recursos para la cuenta.

### <a name="get-management-credentials"></a>Obtención de las credenciales de administración

1. Seleccione **Todos los recursos** y, después, seleccione el nombre del espacio de nombres recién creado.
1. Seleccione **Directivas de acceso compartido**.  
1. En **Directivas de acceso compartido**, seleccione **RootManageSharedAccessKey**.
1. En **Directiva SAS: RootManageSharedAccessKey**, haga clic en el botón **Copiar** situado junto a **Cadena de conexión principal**. Esta acción copia la cadena de conexión en el Portapapeles para su uso posterior. Pegue este valor en el Bloc de notas o cualquier otra ubicación temporal.
1. Repita el paso anterior para copiar y pegar el valor de **Clave principal** en una ubicación temporal para su uso posterior.  

    ![connection-string][connection-string]

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace-vs2019.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string-vs2019.png
[Azure portal]: https://portal.azure.com
