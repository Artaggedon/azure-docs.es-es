---
title: 'Exploración de datos en una máquina virtual de SQL Server: proceso de ciencia de datos en equipos'
description: Explore y procese datos, además de generar características mediante Python o SQL en una máquina virtual de SQL Server de Azure.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8be878cf40967356d68e9be0765e898c81b5ba0a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93314624"
---
# <a name="process-data-in-sql-server-virtual-machine-on-azure"></a><a name="heading"></a>Proceso de datos en una máquina virtual de SQL Server en Azure
En este documento se aborda cómo explorar datos y generar características para los datos almacenados en una VM de SQL Server en Azure. Este objetivo puede alcanzarse mediante la administración de datos usando SQL o mediante un lenguaje de programación, como Python.

> [!NOTE]
> En las instrucciones SQL de ejemplo de este documento se supone que los datos están en SQL Server. Si no lo están, consulte el mapa de proceso de ciencia de datos en la nube para obtener información sobre cómo mover los datos a SQL Server.
> 
> 

## <a name="using-sql"></a><a name="SQL"></a>Uso de SQL
En esta sección, se describen las siguientes tareas de tratamiento de datos mediante SQL:

1. [Exploración de datos](#sql-dataexploration)
2. [Generación de características](#sql-featuregen)

### <a name="data-exploration"></a><a name="sql-dataexploration"></a>Exploración de datos
A continuación se muestran algunos scripts de SQL de ejemplo que se pueden usar para explorar los almacenes de datos en SQL Server.

> [!NOTE]
> Para obtener un ejemplo práctico, puede usar el conjunto de datos [NYC Taxi dataset](https://www.andresmh.com/nyctaxitrips/) y consultar el IPNB denominado [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) (Tratamiento de datos de la ciudad de Nueva York mediante IPython Notebook y SQL Server), que ofrece un tutorial completo.
> 
> 

1. Obtener el número de observaciones por día
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Obtención de los niveles de una columna de categorías 
   
    `select  distinct <column_name> from <databasename>`
3. Obtener el número de niveles de combinación de dos columnas de categorías 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Obtener la distribución para columnas numéricas
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="feature-generation"></a><a name="sql-featuregen"></a>Generación de características
En esta sección, se describen formas de generar características mediante SQL:  

1. [Generación de características basadas en recuentos](#sql-countfeature)
2. [Generación de características de discretización](#sql-binningfeature)
3. [Implementación de las características de una sola columna](#sql-featurerollout)

> [!NOTE]
> Cuando genere características adicionales, puede agregarlas como columnas a la tabla existente o crear una nueva tabla con las características adicionales y la clave principal, que se pueden combinar con la tabla original. 
> 
> 

### <a name="count-based-feature-generation"></a><a name="sql-countfeature"></a>Generación de características basadas en recuentos
Los ejemplos siguientes muestran dos formas de generar características de recuento. El primer método usa la suma condicional y el segundo utiliza la cláusula 'where'. Estos resultados pueden combinarse luego con la tabla original (con columnas de clave principal) para disponer de características de recuento junto con los datos originales.

```sql
select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 
```

### <a name="binning-feature-generation"></a><a name="sql-binningfeature"></a>Generación de características de discretización
En el ejemplo siguiente se muestra cómo generar características discretizadas mediante la discretización (con cinco discretizaciones) de una columna numérica que puede usarse en su lugar como una característica:

```sql
SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>
```

### <a name="rolling-out-the-features-from-a-single-column"></a><a name="sql-featurerollout"></a>Implementación de las características de una sola columna
En esta sección, se muestra cómo se implementa una sola columna de una tabla para generar características adicionales. En el ejemplo se supone que hay una columna de latitud o longitud en la tabla a partir de la cual está intentando generar características.

Aquí se incluye un breve manual sobre los datos de ubicación de latitud y longitud (extraído de stackoverflow [How to measure the accuracy of latitude and longitude?](https://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)(¿Cómo medir la precisión de la latitud y la longitud?)). Es útil comprender esta guía antes de incluir la ubicación como una o más características:

* La señal indica si estamos en el norte o sur, y este u oeste del mundo.
* Un dígito de las centenas distinto de cero indica que se usa la longitud y no la latitud.
* El dígito de las decenas ofrece una posición a aproximadamente 1.000 kilómetros. Nos brinda información útil sobre el continente u océano en el que nos encontramos.
* El dígito de las unidades (un grado decimal) indica una posición de hasta 111 kilómetros (60 millas náuticas, aproximadamente 69 millas). Puede indicarle aproximadamente el estado, el país o la región en que se encuentra.
* La primera posición decimal tiene un valor de hasta 11,1 km: puede distinguir la posición de una ciudad grande de otra ciudad grande vecina.
* La segundo posición decimal tiene un valor de hasta 1,1 km: puede separar un pueblo del siguiente.
* La tercera posición decimal tiene un valor de hasta 110 m: puede identificar un campo agrícola extenso o campus universitario.
* La cuarta posición decimal tiene un valor de hasta 11 m: puede identificar una parcela de tierra. Es comparable a la precisión típica de una unidad GPS sin corregir y sin interferencias.
* La quinta posición decimal tiene un valor de hasta 1,1 m: puede distinguir entre distintos árboles. Solo es posible conseguir una precisión de este nivel con unidades GPS comerciales con corrección diferencial.
* La sexta posición decimal tiene un valor de hasta 0,11 m: puede usarse para diseñar estructuras en detalle, para el diseño de paisajes o la construcción de carreteras. Debería ser más que suficiente para realizar el seguimiento de los movimientos de glaciares y ríos. Esto se consigue al tomar medidas meticulosas con GPS, como GPS corregido de forma diferencial.

La información de ubicación se puede caracterizar como sigue, con diferencias entre la información de región, ubicación y ciudad. También es posible llamar a un punto de conexión de REST, como la API de mapas de Bing disponible en [Encontrar una ubicación por punto](/bingmaps/rest-services/locations/find-a-location-by-point) para obtener la información de la región o el distrito.

```sql
select 
    <location_columnname>
    ,round(<location_columnname>,0) as l1        
    ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
    ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
    ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
    ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
    ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
    ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
from <tablename>
```

Estas características basadas en ubicación se pueden usar aún más para generar características de recuento adicionales, tal y como se describió anteriormente. 

> [!TIP]
> Puede insertar mediante programación los registros con el lenguaje que prefiera. Es posible que deba insertar los datos en fragmentos para mejorar el rendimiento de escritura (para obtener un ejemplo de cómo obtener esto mediante pyodbc, consulte [A HelloWorld sample to access SQLServer with python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)(Un ejemplo Hola a todos para acceder a SQLServer con python)). Otra alternativa consiste en insertar datos en la base de datos mediante la [utilidad BCP](/sql/tools/bcp-utility).
> 
> 

### <a name="connecting-to-azure-machine-learning"></a><a name="sql-aml"></a>Conexión con Azure Machine Learning
La característica recién generada se puede agregar como una columna a una tabla existente o se puede almacenar en una tabla nueva y combinar con la tabla original para el aprendizaje automático. Es posible generar o tener acceso a las características si ya se han creado, mediante el módulo [Importar datos][import-data] en Azure Machine Learning, como se muestra a continuación:

![Lectores de azureml][1] 

## <a name="using-a-programming-language-like-python"></a><a name="python"></a>Uso de un lenguaje de programación como Python
Usar Python para generar explorar datos y generar características cuando los datos están en SQL Server es parecido a procesar los datos en blobs de Azure mediante Python, como se documenta en [Proceso de datos de blobs de Azure en su entorno de ciencia de datos](data-blob.md). Cargue los datos de la base de datos en una trama de datos Pandas para un mayor procesamiento. Se documenta el proceso de conexión a la base de datos y carga de los datos en la trama de datos de esta sección.

El formato de cadena de conexión siguiente puede usarse para conectarse a una base de datos de SQL Server desde Python mediante pyodbc (reemplace servername, dbname, username y password con sus valores específicos):

```python
#Set up the SQL Azure connection
import pyodbc    
conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')
```

La [biblioteca Pandas](https://pandas.pydata.org/) en Python ofrece un amplio conjunto de herramientas de análisis de datos y estructuras de datos para la manipulación de datos para la programación en Python. El código siguiente lee los resultados que se devuelven desde una base de datos de SQL Server en una trama de datos de Pandas:

```python
# Query database and load the returned results in pandas data frame
data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)
```

Ya puede trabajar con la trama de datos de Pandas como se explica en el artículo [Proceso de datos de blobs de Azure en su entorno de ciencia de datos](data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Ejemplo de ciencia de datos de Azure en acción
Para obtener un ejemplo de tutorial completo del Proceso de ciencia de datos de Azure mediante un conjunto de datos público, consulte [Proceso de ciencia de datos de Azure en acción](sql-walkthrough.md).

[1]: ./media/sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: /azure/machine-learning/studio-module-reference/import-data