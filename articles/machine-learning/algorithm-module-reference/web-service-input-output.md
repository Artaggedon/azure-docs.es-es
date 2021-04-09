---
title: 'Entrada/salida del servicio web: Referencia del módulo'
description: Aprenda a usar los módulos de servicio web en el diseñador de Azure Machine Learning para administrar entradas y salidas.
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/13/2020
ms.openlocfilehash: 475ad3de8e0a7636a14949d4fcd8a5ec2812ad5e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "93421131"
---
# <a name="web-service-input-and-web-service-output-modules"></a>Módulos Web Service Input y Web Service Output

En este artículo se describen los módulos Web Service Input (Entrada del servicio web) y Web Service Output (Salida del servicio web) en el diseñador de Azure Machine Learning.

El módulo Web Service Input (Entrada de servicio web) solo puede conectarse al puerto del entrada con el tipo **DataFrameDirectory**. El módulo Web Service Output (Salida del servicio web) solo puede conectarse desde el puerto de salida con el tipo **DataFrameDirectory**. Puede encontrar los dos módulos en el árbol de módulos, en la categoría de **servicio web**. 

El módulo Web Service Input (Entrada del servicio web) indica en dónde entran los datos de usuario en la canalización. El módulo Web Service Output (Salida del servicio web) indica a dónde se devuelven los datos de usuario en una canalización de inferencia en tiempo real.

## <a name="how-to-use-web-service-input-and-output"></a>Procedimientos para el uso de los módulos de entrada y salida del servicio web

Al [crear una canalización de inferencia en tiempo real](../tutorial-designer-automobile-price-deploy.md#create-a-real-time-inference-pipeline) a partir de la canalización de entrenamiento, los módulos Web Service Input (Entrada de servicio web) y Web Service Output (Salida de servicio web) se agregarán automáticamente para mostrar el lugar en donde los datos del usuario entran en la canalización y aquel a donde se devuelven. 

> [!NOTE]
> La generación automática de una canalización de inferencia en tiempo real es un proceso de la mejor opción basado en reglas. No hay ninguna garantía de corrección. 

Puede agregar o quitar manualmente los módulos Web Service Input (Entrada de servicio web) y Web Service Output (Salida de servicio web) para satisfacer sus requisitos. Asegúrese de que la canalización de inferencia en tiempo real tenga por lo menos un módulo Web Service Input (Entrada de servicio web) y un módulo Web Service Output (Salida de servicio web). Si tiene varios módulos Web Service Input (Entrada de servicio web) y Web Service Output (Salida de servicio web), asegúrese de que tienen nombres únicos. Puede escribir el nombre en el panel derecho del módulo.

También puede crear manualmente una canalización de inferencia en tiempo real agregando los módulos Web Service Input (Entrada de servicio web) y Web Service Output (Salida de servicio web) a la canalización sin enviar.

> [!NOTE]
> El tipo de canalización se establecerá la primera vez que la envíe. Asegúrese de agregar los módulos Web Service Input (Entrada de servicio web) y Web Service Output (Salida de servicio web) antes de enviarlos por primera vez.

En el ejemplo siguiente se muestra cómo crear manualmente una canalización de inferencia en tiempo real desde el módulo Execute Python Script (Ejecutar el script de Python). 

![Ejemplo](media/module/web-service-input-output-example.png)
   
Después de enviar la canalización y de que la ejecución se complete correctamente, puede implementar el punto de conexión en tiempo real.
   
> [!NOTE]
>  En el ejemplo anterior, el módulo **Enter Data Manually** (Introducción manual de datos) proporciona el esquema de datos para la entrada del servicio web y es necesario para implementar el punto de conexión en tiempo real. En general, debe conectar siempre un módulo o un conjunto de datos al puerto al que está conectado **Web Service Input** (Entrada de servicio web) para proporcionar el esquema de datos.
   
## <a name="next-steps"></a>Pasos siguientes
Más información sobre la [implementación del punto de conexión en tiempo real](../tutorial-designer-automobile-price-deploy.md#deploy-the-real-time-endpoint).

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning.