---
title: Evento de inicio de cambio de tamaño de un grupo de Azure Batch
description: Referencia del evento de inicio de cambio de tamaño de grupo de Batch. En el ejemplo se muestra el cuerpo de un evento de inicio de cambio de tamaño (manual) de un grupo de 0 a 2 nodos.
ms.topic: reference
ms.date: 12/28/2020
ms.openlocfilehash: be64a2ef30cbe3c404633b29202a4adf1e49ea9e
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803619"
---
# <a name="pool-resize-start-event"></a>Evento de inicio de cambio de tamaño de grupo

 Este evento se genera se inicia el cambio de tamaño de un grupo. Puesto que el cambio de tamaño de grupo es un evento asincrónico, puede esperar que se genere un evento completo de cambio de tamaño de grupo cuando se haya completado la operación de cambio de tamaño.

 En el ejemplo siguiente se muestra el cuerpo de un evento de inicio de cambio de tamaño de grupo de 0 a 2 nodos con un cambio de tamaño manual.

```
{
   "id": "myPool1",
   "nodeDeallocationOption": "Invalid",
   "currentDedicatedNodes": 0,
   "targetDedicatedNodes": 2,
   "currentLowPriorityNodes": 0,
   "targetLowPriorityNodes": 2,
   "enableAutoScale": false,
   "isAutoPool": false
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|`id`|String|El identificador del grupo.|
|`nodeDeallocationOption`|String|Especifica cuándo se pueden quitar los nodos del grupo, si disminuye el tamaño del grupo.<br /><br /> Los valores posibles son:<br /><br /> **requeue**: finalizar las tareas en ejecución y volver a ponerlas en cola. Las tareas volverán a ejecutarse cuando se habilite el trabajo. Elimine los nodos en cuanto finalicen las tareas.<br /><br /> **terminate**: finalizar las tareas en ejecución. Las tareas no se ejecutarán de nuevo. Elimine los nodos en cuanto finalicen las tareas.<br /><br /> **taskcompletion**: permita que finalicen las tareas actualmente en ejecución. No programe ninguna tarea nueva mientras espera. Elimine los nodos cuando se hayan completado todas las tareas.<br /><br /> **Retaineddata**: permite que finalicen las tareas actualmente en ejecución, luego espera que caduquen los períodos de retención de datos de todas las tareas. No programe ninguna tarea nueva mientras espera. Elimine los nodos cuando hayan caducado los períodos de retención de todas las tareas.<br /><br /> El valor predeterminado es requeue.<br /><br /> Si aumenta el tamaño del grupo, entonces el valor se establece en **invalid**.|
|`currentDedicatedNodes`|Int32|El número de nodos de proceso actualmente asignados al grupo.|
|`targetDedicatedNodes`|Int32|El número de nodos de proceso solicitados para el grupo.|
|`currentLowPriorityNodes`|Int32|El número de nodos de proceso actualmente asignados al grupo.|
|`targetLowPriorityNodes`|Int32|El número de nodos de proceso solicitados para el grupo.|
|`enableAutoScale`|Bool|Especifica si el tamaño del grupo se ajusta automáticamente con el tiempo.|
|`isAutoPool`|Bool|Especifica si se ha creado el grupo a través del mecanismo AutoPool de un trabajo.|
