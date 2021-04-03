---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/09/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 999f3fb054eedab64a1f7bcebd9788e1edbf29f9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "86220623"
---
En este paso, creará la conexión entre una red virtual y el centro de conectividad. Repita estos pasos para cada red virtual que desee conectar.

1. En la página de la red WAN virtual, seleccione **Conexiones de red virtual**.
1. En la página de conexión de red virtual, seleccione **+Agregar conexión**.
1. En la página **Agregar conexión**, rellene los campos siguientes:

    * **Nombre de la conexión**: asigne un nombre a la conexión.
    * **Centros**: seleccione el concentrador que desea asociar a esta conexión.
    * **Suscripción**: compruebe la suscripción.
    * **Red virtual**: seleccione la red virtual que quiere conectar con este concentrador. La red virtual no puede tener una puerta de enlace de red virtual ya existente.
1. Seleccione **Aceptar** para crear la conexión.