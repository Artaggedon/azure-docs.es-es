---
title: 'Actualización de la entidad compuesta: LUIS'
description: Actualice la entidad compuesta en la entidad de aprendizaje automático con el proceso de actualización en el portal de LUIS.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/04/2020
ms.openlocfilehash: 46e9ece70d9f980065c719ee1205eb46591b45c0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "95025249"
---
# <a name="upgrade-composite-entity-to-machine-learning-entity"></a>Actualización de la entidad compuesta en la entidad de aprendizaje automático

Actualice la entidad compuesta en la entidad de aprendizaje automático para crear una entidad que reciba predicciones más completas con una capacidad de descomposición mejorada a fin de depurar la entidad.

## <a name="current-version-model-restrictions"></a>Restricciones del modelo de versión actual

El proceso de actualización crea entidades de aprendizaje automático, en función de las entidades compuestas existentes que se encuentran en la aplicación, en una nueva versión de la aplicación. Esto incluye los roles y los elementos secundarios de la entidad compuesta. El proceso también cambia las etiquetas en las expresiones de ejemplo para usar la nueva entidad.

## <a name="upgrade-process"></a>Proceso de actualización

El proceso de actualización consiste en lo siguiente:
* Crea una nueva entidad de aprendizaje automático para cada entidad compuesta.
* Entidades secundarias:
    * Si la entidad secundaria solo se usa en la entidad compuesta, solo se agregará a la entidad de aprendizaje automático.
    * Si la entidad secundaria se usa en la compuesta _y_ como una entidad independiente (etiquetada en las expresiones de ejemplo), se agregará a la versión como una entidad y a la nueva entidad de aprendizaje automático como una subentidad.
    * Si la entidad secundaria usa un rol, cada rol se convertirá en una subentidad con el mismo nombre.
    * Si la entidad secundaria es una entidad que no es de aprendizaje automático (expresión regular, entidad de lista o entidad precompilada), se crea una nueva subentidad con el mismo nombre, y la nueva subentidad tiene una característica que usa la entidad que no es de aprendizaje automático con la característica necesaria agregada.
* Los nombres se conservan, pero deben ser únicos en el mismo nivel o en la misma subentidad. Consulte [Límites de los nombres únicos](./luis-limits.md#name-uniqueness).
* Las etiquetas de las expresiones de ejemplo se cambian a la nueva entidad de aprendizaje automático con subentidades.

Use el gráfico siguiente para comprender cómo cambia el modelo:

|Objeto anterior|Nuevo objeto|Notas|
|--|--|--|
|Entidad compuesta|Entidad de aprendizaje automático con estructura|Ambos objetos son principales.|
|La entidad secundaria compuesta es una **entidad simple**.|Subentidad|Ambos objetos son secundarios.|
|La entidad secundaria compuesta es una **entidad precompilada** como Number.|Subentidad con el nombre de la entidad precompilada, como Number, y la subentidad tiene la _característica_ de la entidad precompilada Number con la opción de restricción establecida en _true_.|La subentidad contiene una característica con restricción a nivel de subentidad.|
|La entidad secundaria compuesta es una **entidad precompilada** como Number, y la entidad precompilada tiene un **rol**.|Subentidad con el nombre de rol, y la subentidad tiene la característica de la entidad precompilada Number con la opción de restricción establecida en "true".|La subentidad contiene una característica con restricción a nivel de subentidad.|
|Role|Subentidad|El nombre del rol se convierte en el nombre de la subentidad. La subentidad es un descendiente directo de la entidad de aprendizaje automático.|

## <a name="begin-upgrade-process"></a>Inicio del proceso de actualización

Antes de realizar la actualización, asegúrese de lo siguiente:

* Cambiar versiones si no tiene la versión correcta que se debe actualizar


1. Iniciar el proceso de actualización desde la notificación, o bien puede esperar hasta el siguiente aviso programado.

    > [!div class="mx-imgBorder"]
    > ![Inicio de la actualización desde las notificaciones](./media/update-composite-entity/notification-begin-update.png)

1. En el menú emergente, seleccione **Actualizar ahora**.

1. Revise la información **What happens when you upgrade** (Qué sucede al realizar la actualización) y seleccione **Continuar**.

1. Seleccione las entidades compuestas de la lista que quiera actualizar y, a continuación, seleccione **Continuar**.

1. Para mover los cambios no entrenados de la versión actual a la actualizada, puede activar la casilla.

1. Seleccione **Continuar** para iniciar el proceso de actualización.

1. La barra de progreso indica el estado del proceso de actualización.

1. Una vez finalizado el proceso, se encontrará en una nueva versión entrenada con las nuevas entidades de aprendizaje automático. Seleccione **Try your new entities** (Probar las nuevas entidades) para ver la nueva entidad.

    Si se produjo un error en la actualización o el entrenamiento de la nueva entidad, hay una notificación que proporciona más información.

1. En la página Lista de entidades, las nuevas entidades se marcan con **NUEVO** junto al nombre de tipo.

## <a name="next-steps"></a>Pasos siguientes

* [Creadores y colaboradores](luis-how-to-collaborate.md)