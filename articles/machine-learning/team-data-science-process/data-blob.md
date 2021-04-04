---
title: 'Proceso de datos del blob de Azure con análisis avanzado: Proceso de ciencia de datos en equipos'
description: Explore los datos y genere características a partir de los datos almacenados en el Almacenamiento de blobs de Azure con análisis avanzado.
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
ms.openlocfilehash: 5a088d5918a957036b905db9136f9b16e5f0527e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93307169"
---
# <a name="process-azure-blob-data-with-advanced-analytics"></a><a name="heading"></a>Proceso de datos de blob de Azure con análisis avanzado
En este documento se trata la exploración de datos y generación de características a partir de los datos almacenados en Almacenamiento de blobs de Azure. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Carga de los datos en una trama de datos Pandas
Para explorar y manipular un conjunto de datos, se debe descargar desde el origen de blob en un archivo local que se pueda cargar en una trama de datos de Pandas. Estos son los pasos a seguir para realizar este procedimiento:

1. Descargue los datos del blob de Azure con el siguiente código de Python de ejemplo mediante Blob service. Reemplace la variable en el código siguiente por sus valores específicos: 
   
    ```python
    from azure.storage.blob import BlobService
    import tables
   
    STORAGEACCOUNTNAME= <storage_account_name>
    STORAGEACCOUNTKEY= <storage_account_key>
    LOCALFILENAME= <local_file_name>        
    CONTAINERNAME= <container_name>
    BLOBNAME= <blob_name>
   
    #download from blob
    t1=time.time()
    blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
    t2=time.time()
    print(("It takes %s seconds to download "+blobname) % (t2 - t1))
    ```
2. Lea los datos en una trama de datos de Pandas desde el archivo descargado.
   
    ```python
    #LOCALFILE is the file path    
    dataframe_blobdata = pd.read_csv(LOCALFILE)
    ```

Ya puede explorar los datos y generar características en este conjunto de datos.

## <a name="data-exploration"></a><a name="blob-dataexploration"></a>Exploración de datos
A continuación, se muestran algunos ejemplos de formas de explorar datos mediante Pandas:

1. Inspeccionar el número de filas y columnas 
   
    ```python
    print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
    ```
2. Inspeccionar las primeras o las últimas filas del conjunto de datos, como se indica a continuación:
   
    ```python
    dataframe_blobdata.head(10)
   
    dataframe_blobdata.tail(10)
    ```
3. Comprobar el tipo de datos como el que se importó cada columna mediante el siguiente código de ejemplo
   
    ```python
    for col in dataframe_blobdata.columns:
        print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
    ```
4. Comprobar las estadísticas básicas de las columnas del conjunto de datos de la siguiente forma
   
    ```python
    dataframe_blobdata.describe()
    ```
5. Observar el número de entradas de cada valor de columna, como se indica a continuación
   
    ```python
    dataframe_blobdata['<column_name>'].value_counts()
    ```
6. Contar los valores que faltan frente al número real de entradas de cada columna, mediante el siguiente código de ejemplo
   
    ```python
    miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
    print miss_num
    ```
7. Si hay valores que faltan para una columna determinada en los datos, puede quitarlos como se indica:
   
    ```python
    dataframe_blobdata_noNA = dataframe_blobdata.dropna()
    dataframe_blobdata_noNA.shape
    ```
   
   Otra forma de reemplazar los valores que faltan es a través de la función de modo:
   
    ```python
    dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})  
    ```      
8. Crear un gráfico de histograma con un número variable de discretizaciones para trazar la distribución de una variable    
   
    ```python
    dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
    np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    ```
9. Examinar las correlaciones entre las variables mediante un gráfico de dispersión o con la función de correlación integrada
   
    ```python
    #relationship between column_a and column_b using scatter plot
    plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
    #correlation between column_a and column_b
    dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    ```

## <a name="feature-generation"></a><a name="blob-featuregen"></a>Generación de características
Es posible generar características con Python de la siguiente manera:

### <a name="indicator-value-based-feature-generation"></a><a name="blob-countfeature"></a>Generación de características basada en el valor de indicador
Las características de categorías se pueden crear como sigue:

1. Inspeccione la distribución de la columna de categorías:
   
    ```python
    dataframe_blobdata['<categorical_column>'].value_counts()
    ```
2. Genere valores de indicador para cada uno de los valores de columna
   
    ```python
    #generate the indicator column
    dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
    ```
3. Combine la columna de indicador con la trama de datos original 
   
    ```python
    #Join the dummy variables back to the original data frame
    dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
    ```
4. Quite la propia variable original:
   
    ```python
    #Remove the original column rate_code in df1_with_dummy
    dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    ```

### <a name="binning-feature-generation"></a><a name="blob-binningfeature"></a>Generación de características de discretización
Para generar características discretizadas, se procede de la siguiente manera:

1. Agregue una secuencia de columnas para discretizar una columna numérica
   
    ```python
    bins = [0, 1, 2, 4, 10, 40]
    dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
    ```
2. Convierta la discretización en una secuencia de variables booleanas
   
    ```python
    dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    ```
3. Por último, combine las variables de prueba con la trama de datos original
   
    ```python
    dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)  
    ```  

## <a name="writing-data-back-to-azure-blob-and-consuming-in-azure-machine-learning"></a><a name="sql-featuregen"></a>Reescritura de datos en un blob de Azure y consumo en Azure Machine Learning
Cuando haya explorado los datos y creado las características necesarias, puede cargar los datos (muestreados o con características) en un blob de Azure y consumirlos en Azure Machine Learning, mediante los siguientes pasos. También se pueden crear características adicionales en Azure Machine Learning Studio (clásico). 

1. Escriba la trama de datos en el archivo local
   
    ```python
    dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
    ```
2. Cargue los datos en el blob de Azure como se indica a continuación:
   
    ```python
    from azure.storage.blob import BlobService
    import tables
   
    STORAGEACCOUNTNAME= <storage_account_name>
    LOCALFILENAME= <local_file_name>
    STORAGEACCOUNTKEY= <storage_account_key>
    CONTAINERNAME= <container_name>
    BLOBNAME= <blob_name>
   
    output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
    localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
    try:
   
    #perform upload
    output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
    except:            
        print ("Something went wrong with uploading blob:"+BLOBNAME)
    ```
3. Ahora se pueden leer los datos del blob mediante el módulo [Importar datos][import-data] de Azure Machine Learning, como se muestra en la pantalla siguiente:

![lector de blobs][1]

[1]: ./media/data-blob/reader_blob.png


<!-- Module References -->
[import-data]: /azure/machine-learning/studio-module-reference/import-data