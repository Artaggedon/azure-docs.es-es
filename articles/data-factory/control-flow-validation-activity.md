---
title: Actividad de validación en Azure Data Factory
description: La actividad de validación no continúa la ejecución de la canalización hasta que valida el conjunto de datos adjunto con determinados criterios que el usuario especifica.
author: dcstwh
ms.author: weetok
ms.reviewer: maghan
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: e00090fc31c129f35632d30b9a5ed6c39c13a414
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100385258"
---
# <a name="validation-activity-in-azure-data-factory"></a>Actividad de validación en Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Puede usar una validación en una canalización para asegurarse de que la canalización solo continúe la ejecución una vez que haya validado la existencia de la referencia del conjunto de datos adjunto, que cumple con los criterios especificados o que se haya alcanzado el tiempo de expiración.


## <a name="syntax"></a>Sintaxis

```json

{
    "name": "Validation_Activity",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_File",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "minimumSize": 20
    }
},
{
    "name": "Validation_Activity_Folder",
    "type": "Validation",
    "typeProperties": {
        "dataset": {
            "referenceName": "Storage_Folder",
            "type": "DatasetReference"
        },
        "timeout": "7.00:00:00",
        "sleep": 10,
        "childItems": true
    }
}

```


## <a name="type-properties"></a>Propiedades de tipo

Propiedad | Descripción | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
name | Nombre de la actividad de validación | String | Sí |
type | Debe establecerse en **Validación**. | String | Sí |
dataset | La actividad bloqueará la ejecución hasta que haya validado la existencia de esta referencia del conjunto de datos y que cumple con los criterios especificados, o se haya alcanzado el tiempo de expiración. El conjunto de datos proporcionado debe admitir la propiedad "MinimumSize" o "ChildItems". | Referencia del conjunto de datos | Sí |
timeout | Especifica el tiempo de espera para que se ejecute la actividad. Si no se especifica ningún valor, el valor predeterminado es 7 días ("7.00:00:00"). El formato es d.hh:mm:ss | String | No |
en reposo | Un retardo en segundos entre los intentos de validación. Si no se especifica ningún valor, el valor predeterminado es 10 segundos. | Entero | No |
childItems | Comprueba si la carpeta tiene elementos secundarios. Se puede establecer en -true: valida que la carpeta existe y que tiene elementos. Se bloquea hasta que al menos un elemento está presente en la carpeta o se alcanza el valor del tiempo de expiración. -false: valida que la carpeta existe y está vacía. Se bloquea hasta que la carpeta está vacía o hasta que se alcanza el valor del tiempo de expiración. Si no se especifica ningún valor, la actividad se bloqueará hasta que exista la carpeta o hasta que se alcance el tiempo de espera. | Boolean | No |
minimumSize | Tamaño mínimo de un archivo en bytes. Si no se especifica ningún valor, el valor predeterminado es 0 bytes. | Entero | No |


## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory:

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
