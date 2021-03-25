---
title: 'Restauración de Azure Database for MySQL con Azure Portal: servidor flexible'
description: En este artículo se explica cómo realizar operaciones en Azure Database for MySQL con Azure Portal.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: 062d53fcb122ebacd004d7dca5e11f5a883354cd
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93241963"
---
# <a name="point-in-time-restore-of-a-azure-database-for-mysql---flexible-server-preview"></a>Recuperación a un momento dado de Azure Database for MySQL: servidor flexible (versión preliminar)


> [!IMPORTANT]
> Azure Database for MySQL: servidor flexible está actualmente en versión preliminar pública.

En este artículo se proporciona un procedimiento detallado para llevar a cabo recuperaciones a un momento dado en servidores flexibles mediante copias de seguridad.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía, necesita:

-   Debe tener Azure Database for MySQL: servidor flexible.

## <a name="restore-to-the-latest-restore-point"></a>Recuperación al punto de restauración más reciente

Siga estos pasos para restaurar un servidor flexible utilizando su copia de seguridad más antigua.

1.  En [Azure Portal](https://portal.azure.com/), seleccione el servidor flexible cuya copia de seguridad quiera restaurar.

2.  En el panel izquierdo, haga clic en **Información general**.

3.  En la página Información general, haga clic en **Restaurar**.

    [Marcador de posición]

4.  Se mostrará la página de restauración con una opción para elegir entre el **punto de restauración más reciente** y uno personalizado.

5.  Seleccione el **punto de restauración más reciente**.


6.  Indique un nombre de servidor nuevo en el campo **Restaurar en el servidor nuevo**.

    :::image type="content" source="./media/concept-backup-restore/restore-blade-latest.png" alt-text="Punto de restauración más antiguo":::

8.  Haga clic en **Aceptar**.

9.  Se mostrará una notificación en la que se indica que se ha iniciado la operación de restauración.

## <a name="restoring-to-a-custom-restore-point"></a>Restauración a un punto de restauración personalizado

Siga estos pasos para restaurar un servidor flexible mediante la copia de seguridad más antigua.

1.  En [Azure Portal](https://portal.azure.com/), seleccione el servidor flexible cuya copia de seguridad quiera restaurar.

2.  En la página Información general, haga clic en **Restaurar**.

    [Marcador de posición]

3.  Se mostrará la página de restauración con una opción para elegir entre el primer punto de restauración y uno personalizado.

4.  Elija un **punto de restauración personalizado**.

5.  Seleccione la fecha y la hora.

6.  Indique un nombre de servidor nuevo en el campo **Restaurar en el servidor nuevo**.

6.  Indique un nombre de servidor nuevo en el campo **Restaurar en el servidor nuevo**. 
   
    :::image type="content" source="./media/concept-backup-restore/restore-blade-custom.png" alt-text="ver información general":::
 
7.  Haga clic en **Aceptar**.

8.  Se mostrará una notificación en la que se indica que se ha iniciado la operación de restauración.

## <a name="next-steps"></a>Pasos siguientes

Marcador de posición
