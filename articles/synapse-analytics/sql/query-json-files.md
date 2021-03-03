---
title: Consulta de archivos JSON mediante un grupo de SQL sin servidor
description: En esta sección se explica cómo leer archivos de JSON mediante un grupo de SQL sin servidor en Azure Synapse Analytics.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 56d9c621579e19cf2c32562560e40fe42ff3989b
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101677514"
---
# <a name="query-json-files-using-serverless-sql-pool-in-azure-synapse-analytics"></a>Consulta de archivos JSON mediante un grupo de SQL sin servidor en Azure Synapse Analytics

En este artículo, aprenderá a escribir una consulta mediante un grupo de SQL sin servidor en Azure Synapse Analytics. El objetivo de la consulta es leer archivos JSON con [OPENROWSET](develop-openrowset.md). 
- Archivos JSON estándar en los que se almacenan varios documentos JSON como una matriz JSON.
- Archivos JSON delimitados por líneas, donde los documentos JSON se separan con un carácter de nueva línea. Las extensiones comunes para estos tipos de archivos son `jsonl`, `ldjson` y `ndjson`.

## <a name="read-json-documents"></a>Lectura de documentos JSON

La forma más fácil de ver el contenido del archivo JSON es proporcionar la dirección URL del archivo a la función `OPENROWSET`, especificar el elemento `FORMAT` de CSV y establecer los valores `0x0b` para `fieldterminator` y `fieldquote`. Si necesita leer archivos JSON delimitados por líneas, esto es suficiente. Si tiene un archivo JSON clásico, debería establecer valores `0x0b` para `rowterminator`. La función `OPENROWSET` analizarán JSON y devolverá cada documento en el formato siguiente:

| doc |
| --- |
|{"date_rep":"2020-07-24","day":24,"month":7,"year":2020,"cases":3,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-25","day":25,"month":7,"year":2020,"cases":7,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-26","day":26,"month":7,"year":2020,"cases":4,"deaths":0,"geo_id":"AF"}|
|{"date_rep":"2020-07-27","day":27,"month":7,"year":2020,"cases":8,"deaths":0,"geo_id":"AF"}|

Si el archivo está disponible públicamente o si la identidad de Azure AD puede acceder a este archivo, debería poder ver el contenido del archivo mediante una consulta como la que se muestra en los ejemplos siguientes.

### <a name="read-json-files"></a>Lectura de archivos JSON

En la consulta de ejemplo siguiente se leen archivos JSON y archivos JSON delimitados por líneas y se devuelven todos los documentos como una fila independiente.

```sql
select top 10 *
from openrowset(
        bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.jsonl',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
go
select top 10 *
from openrowset(
        bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b',
        rowterminator = '0x0b' --> You need to override rowterminator to read classic JSON
    ) with (doc nvarchar(max)) as rows
```

El documento JSON de la consulta de ejemplo anterior incluye una matriz de objetos. La consulta devuelve cada objeto como fila independiente en el conjunto de resultados. Asegúrese de que puede tener acceso a este archivo. Si el archivo está protegido con una clave SAS o una identidad personalizada, deberá configurar una [credencial de nivel de servidor para el inicio de sesión de SQL](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#server-scoped-credential). 

### <a name="data-source-usage"></a>Uso del origen de datos

En el ejemplo anterior se usa la ruta de acceso completa al archivo. Como alternativa, puede crear un origen de datos externo con la ubicación que apunta a la carpeta raíz del almacenamiento y usar ese origen de datos y la ruta de acceso relativa al archivo en la función `OPENROWSET`:

```sql
create external data source covid
with ( location = 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases' );
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
go
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.json',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b',
        rowterminator = '0x0b' --> You need to override rowterminator to read classic JSON
    ) with (doc nvarchar(max)) as rows
```

Si un origen de datos está protegido con una clave SAS o una identidad personalizada, puede configurar el [origen de datos con una credencial de ámbito de base de datos](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#database-scoped-credential).

En las secciones siguientes, puede ver cómo consultar varios tipos de archivos JSON.

## <a name="parse-json-documents"></a>Análisis de documentos JSON

En las consultas de los ejemplos anteriores se devuelve cada documento JSON como una sola cadena en una fila independiente del conjunto de resultados. Puede usar las funciones `JSON_VALUE` y `OPENJSON` para analizar los valores de documentos JSON y devolverlos como valores relacionales, tal como se muestra en el ejemplo siguiente:

| date\_rep | cases | geo\_id |
| --- | --- | --- |
| 2020-07-24 | 3 | AF |
| 2020-07-25 | 7 | AF |
| 2020-07-26 | 4 | AF |
| 2020-07-27 | 8| AF |

### <a name="sample-json-document"></a>Documento JSON de ejemplo

Los ejemplos de consulta leen archivos *json* que contienen documentos con la infraestructura siguiente:

```json
{
    "date_rep":"2020-07-24",
    "day":24,"month":7,"year":2020,
    "cases":13,"deaths":0,
    "countries_and_territories":"Afghanistan",
    "geo_id":"AF",
    "country_territory_code":"AFG",
    "continent_exp":"Asia",
    "load_date":"2020-07-25 00:05:14",
    "iso_country":"AF"
}
```

> [!NOTE]
> Si estos documentos se almacenan como JSON delimitado por líneas, debe establecer `FIELDTERMINATOR` y `FIELDQUOTE` en 0x0b. Si tiene el formato JSON estándar, debe establecer `ROWTERMINATOR` en 0x0b.

### <a name="query-json-files-using-json_value"></a>Consulta de archivos JSON mediante JSON_VALUE

En la consulta siguiente se muestra cómo usar [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?view=azure-sqldw-latest&preserve-view=true) para recuperar valores escalares (título, publicador) de un documento JSON:

```sql
select
    JSON_VALUE(doc, '$.date_rep') AS date_reported,
    JSON_VALUE(doc, '$.countries_and_territories') AS country,
    JSON_VALUE(doc, '$.cases') as cases,
    doc
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
order by JSON_VALUE(doc, '$.geo_id') desc
```

### <a name="query-json-files-using-openjson"></a>Consulta de archivos JSON mediante OPENJSON

En la siguiente consulta se usa [OPENJSON](/sql/t-sql/functions/openjson-transact-sql?view=azure-sqldw-latest&preserve-view=true). Recuperará las estadísticas de COVID informadas en Serbia:

```sql
select
    *
from openrowset(
        bulk 'latest/ecdc_cases.jsonl',
        data_source = 'covid',
        format = 'csv',
        fieldterminator ='0x0b',
        fieldquote = '0x0b'
    ) with (doc nvarchar(max)) as rows
    cross apply openjson (doc)
        with (  date_rep datetime2,
                cases int,
                fatal int '$.deaths',
                country varchar(100) '$.countries_and_territories')
where country = 'Serbia'
order by country, date_rep desc;
```

## <a name="next-steps"></a>Pasos siguientes

En los siguientes artículos de esta serie se muestra:

- [Consulta de carpetas y varios archivos](query-folders-multiple-csv-files.md)
- [Creación y uso de vistas](create-use-views.md)
