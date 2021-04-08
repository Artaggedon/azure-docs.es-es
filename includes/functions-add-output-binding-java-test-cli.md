---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 6a359cdd44cc0c0cfbd93bd23b69a67a641c7fbb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "80673268"
---
## <a name="update-the-tests"></a>Actualización de las pruebas

Dado que el arquetipo también crea un conjunto de pruebas, debe actualizar estas pruebas para controlar el nuevo parámetro `msg` en la signatura del método `run`.  

Vaya a la ubicación del código de prueba en _src/test/java_, abra el archivo de proyecto *Function.Java* y reemplace la línea de código debajo de `//Invoke` por el código siguiente.

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/test/java/com/function/FunctionTest.java" range="48-50":::