---
title: Depuración de código de Azure Data Lake Analytics de forma local
description: Aprenda a usar las Herramientas de Azure Data Lake para Visual Studio para depurar los trabajos de U-SQL en la estación de trabajo local.
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: how-to
ms.date: 07/03/2018
ms.openlocfilehash: 1c7218deac9efba6df6c1284f2578a744e768284
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "92221033"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Depuración de código de Azure Data Lake Analytics de forma local

Puede usar las Herramientas de Azure Data Lake para Visual Studio para ejecutar y depurar código de Azure Data Lake Analytics en su estación de trabajo local, de la misma forma que en el servicio Azure Data Lake Analytics.

Obtenga información sobre [cómo ejecutar el script U-SQL en la máquina local](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="debug-scripts-and-c-assemblies-locally"></a>Depurar scripts y ensamblados de C# localmente

Puede depurar ensamblados de C# sin enviarlos y registrarlos en el servicio Azure Data Lake Analytics. Puede establecer puntos de interrupción en el código subyacente del archivo y en un proyecto de C# al que se hace referencia.

### <a name="debug-local-code-in-a-code-behind-file"></a>Depuración del código local en el código subyacente del archivo

1. Establezca puntos de interrupción en el archivo de código subyacente.
2. Presione **F5** para depurar el script localmente.

> [!NOTE]
   > El siguiente procedimiento solo funciona en Visual Studio 2015. En versiones anteriores de Visual Studio puede que necesite agregar manualmente los archivos **PDB**.  
   >
   >

### <a name="debug-local-code-in-a-referenced-c-project"></a>Depuración del código local en un proyecto de C# al que se hace referencia

1. Cree un proyecto de ensamblado de C# y compílelo para generar la **DLL** de salida.
2. Registre el archivo **DLL** mediante una instrucción de U-SQL:

   ```sql
   CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
   ```
   
3. Establezca puntos de interrupción en el código de C#.
4. Presione **F5** para depurar el script con referencia a al archivo **DLL** de C# localmente.


## <a name="next-steps"></a>Pasos siguientes

- Para ver un ejemplo de una consulta más compleja, consulte [Tutorial: Análisis de registros de sitios web mediante Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Para ver los detalles del trabajo, consulte [Usar el explorador de trabajos y la vista de trabajos para trabajos de Azure Data Lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md).
- Para usar la vista de ejecución de vértices, vea [Uso de la vista de ejecución de vértices de Data Lake Tools para Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
