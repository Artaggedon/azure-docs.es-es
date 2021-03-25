---
title: Creación de una función en Azure que se ejecuta según una programación
description: Obtenga información sobre cómo usar Azure Portal para crear una función que se ejecuta según la programación que usted defina.
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.topic: how-to
ms.date: 04/16/2020
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 14d918cc41f49b954f5cabf48572db5df829fd10
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98035196"
---
# <a name="create-a-function-in-the-azure-portal-that-runs-on-a-schedule"></a>Creación de una función en Azure Portal que se ejecuta según una programación

Obtenga información sobre cómo usar Azure Portal para crear una función que se ejecuta [sin servidor](https://azure.microsoft.com/solutions/serverless/) en Azure según la programación que defina.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

+ Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-function-app"></a>Creación de una aplicación de función

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

La nueva aplicación de funciones está lista para usarse. A continuación, creará una función en la nueva aplicación de funciones.

:::image type="content" source="./media/functions-create-scheduled-function/function-app-create-success.png" alt-text="La aplicación de funciones se creó correctamente." border="true":::

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Creación de una función desencadenada por un temporizador

1. En la aplicación de funciones, seleccione **Funciones** y, a continuación, seleccione **+ Agregar**. 

   :::image type="content" source="./media/functions-create-scheduled-function/function-add-function.png" alt-text="Agregue una función en Azure Portal" border="true":::.

1. Seleccione la plantilla **Desencadenador de temporizador**. 

    :::image type="content" source="./media/functions-create-scheduled-function/function-select-timer-trigger.png" alt-text="Seleccione el desencadenador de temporizador en Azure Portal." border="true":::

1. Configure el nuevo desencadenador según la configuración especificada en la tabla que aparece debajo de la imagen y, a continuación, seleccione **Crear función**.

    :::image type="content" source="./media/functions-create-scheduled-function/function-configure-timer-trigger.png" alt-text="Captura de pantalla que muestra la página Nueva función con la plantilla Desencadenador de temporizador seleccionada." border="true":::
    
    | Configuración | Valor sugerido | Descripción |
    |---|---|---|
    | **Nombre** | Valor predeterminado | Define el nombre de la función desencadenada por el temporizador. |
    | **Programación** | 0 \*/1 \* \* \* \* | [Expresión CRON](functions-bindings-timer.md#ncrontab-expressions) de seis campos que programa la función para que se ejecute cada minuto. |

## <a name="test-the-function"></a>Prueba de la función

1. En la función, seleccione **Código y prueba** y expanda los registros.

    :::image type="content" source="./media/functions-create-scheduled-function/function-test-timer-trigger.png" alt-text="Pruebe el desencadenador de temporizador en Azure Portal." border="true":::

1. Vea la información escrita en los registros para verificar la ejecución.

    :::image type="content" source="./media/functions-create-scheduled-function/function-view-timer-logs.png" alt-text="Vea el desencadenador de temporizador en Azure Portal." border="true":::

Ahora puede cambiar la programación de la función para que se ejecute una vez cada hora, en lugar de cada minuto.

## <a name="update-the-timer-schedule"></a>Actualizar la programación del temporizador

1. En la función, seleccione **Integración**. Aquí es donde puede definir los enlaces de entrada y salida de la función, así como establecer la programación. 

1. Seleccione **Timer (myTimer)** (Temporizador [myTimer]).

    :::image type="content" source="./media/functions-create-scheduled-function/function-update-timer-schedule.png" alt-text="Actualice la programación del temporizador en Azure Portal." border="true":::

1. Actualice el valor **Programación**  a `0 0 */1 * * *` y, luego, seleccione **Guardar**.  

    :::image type="content" source="./media/functions-create-scheduled-function/function-edit-timer-schedule.png" alt-text="Actualización de la programación del temporizador de funciones en Azure Portal." border="true":::

Ahora tiene una función que se ejecuta una vez cada hora.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha creado una función que se ejecuta según una programación. Para más información sobre los desencadenadores del temporizador, consulte [Programación de la ejecución de código con Azure Functions](functions-bindings-timer.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
