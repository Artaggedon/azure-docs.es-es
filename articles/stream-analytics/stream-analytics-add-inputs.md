---
title: Información sobre las entradas de Azure Stream Analytics
description: En este artículo se describe el concepto de entradas en un trabajo de Azure Stream Analytics y se compara la entrada de streaming con la entrada de datos de referencia.
author: jseb225
ms.author: krishmam
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: 8c62bb2aad266d577a5f4c6f6343d6ed3f4f1979
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98016208"
---
# <a name="understand-inputs-for-azure-stream-analytics"></a>Información sobre las entradas de Azure Stream Analytics

Los trabajos de Azure Stream Analytics se conectan a una o más entradas de datos. Cada entrada define una conexión a un origen de datos existente. Stream Analytics acepta datos procedentes de varios tipos de orígenes de eventos, entre los que se incluyen Event Hubs, IoT Hub y el almacenamiento de blobs. En la consulta SQL de streaming que se escribe para cada trabajo, se hace referencia a las entradas por su nombre. En la consulta, puede combinar varias entradas para fusionar datos o comparar datos de streaming con una búsqueda de datos de referencia y pasar los resultados a las salidas. 

Stream Analytics presenta una integración de primera clase con cuatro tipos de recursos como entradas:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Almacenamiento de blobs de Azure](https://azure.microsoft.com/services/storage/blobs/) 
- [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) 

Estos orígenes de entrada pueden proceder de la misma suscripción de Azure que el trabajo de Stream Analytics o de otra suscripción.

Puede usar [Azure Portal](stream-analytics-quick-create-portal.md#configure-job-input), [Azure PowerShell](/powershell/module/az.streamanalytics/New-azStreamAnalyticsInput), [.NET API](/dotnet/api/microsoft.azure.management.streamanalytics.inputsoperationsextensions), [API REST](/rest/api/streamanalytics/2016-03-01/inputs) y [Visual Studio](stream-analytics-tools-for-visual-studio-install.md) para crear, editar y probar las entradas del trabajo de Stream Analytics.

## <a name="stream-and-reference-inputs"></a>Entradas de flujo y de referencia
A medida que los datos se insertan en un origen de datos, el trabajo de Stream Analytics los consume y los procesa en tiempo real. Las entradas se dividen en dos tipos distintos: entradas de flujo de datos y entradas de datos de referencia.

### <a name="data-stream-input"></a>Entrada de flujo de datos
Un flujo de datos es una secuencia ilimitada de eventos a lo largo del tiempo. Los trabajos de Stream Analytics deben incluir, como mínimo, una entrada de flujo de datos. Event Hubs, IoT Hub, Azure Data Lake Storage Gen2 and Blob Storage se admiten como orígenes de entrada de flujo de datos. Event Hubs sirve para recopilar flujos de eventos desde varios dispositivos y servicios. Es posible que estos flujos incluyan fuentes de actividades de redes sociales, información bursátil o datos de sensores. Los Centros de IoT están optimizados para recopilar datos de dispositivos conectados en escenarios del Internet de las cosas (IoT).  Blob Storage puede usarse como origen de entrada para la ingesta de conjuntos masivos de datos en forma de flujo, como, por ejemplo, archivos de registro.  

Para obtener más información sobre las entradas de streaming, vea [Stream data as input into Stream Analytics](stream-analytics-define-inputs.md) (Datos de flujo como entrada en Stream Analytics).

### <a name="reference-data-input"></a>Entrada de datos de referencia
Stream Analytics también admite la entrada de *datos de referencia*. Los datos de referencia son completamente estáticos o cambian lentamente. Se utilizan normalmente para realizar la correlación y las búsquedas. Por ejemplo, es posible combinar datos de la entrada de flujo de datos con datos de los datos de referencia, de la misma forma que ejecutaría una instrucción SQL JOIN para buscar valores estáticos. Azure Blob Storage, Azure Data Lake Storage Gen2, and Azure SQL Database se admiten actualmente como orígenes de entrada para datos de referencia. Los blobs de origen de datos de referencia tienen un límite de tamaño de 300 MB, según la complejidad de la consulta y las unidades de streaming asignadas (vea la sección [Limitación del tamaño](stream-analytics-use-reference-data.md#size-limitation) de la documentación de datos de referencia para más detalles).

Para obtener más información sobre las entradas de datos de referencia, vea [Uso de datos de referencia para las búsquedas en Stream Analytics](stream-analytics-use-reference-data.md)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Inicio rápido: Creación de un trabajo de Stream Analytics mediante Azure Portal](stream-analytics-quick-create-portal.md)