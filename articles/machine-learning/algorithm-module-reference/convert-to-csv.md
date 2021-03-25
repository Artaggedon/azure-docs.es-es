---
title: 'Convertir a CSV: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo de conversión a CSV en el diseñador de Azure Machine Learning para convertir un conjunto de archivos en un archivo CSV que se pueda usar más adelante.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: cc58689e30e9b03e490c0871f3decd50372371fa
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93421930"
---
# <a name="convert-to-csv-module"></a>Módulo Convertir a CSV

En este artículo se describe un módulo del diseñador de Azure Machine Learning.

Use este módulo para convertir un conjunto de datos a un formato CSV que se pueda descargar, exportar o compartir con módulos de script de R o Python.

### <a name="more-about-the-csv-format"></a>Más información sobre el formato CSV 

El formato CSV, que es el acrónimo de "valores separados por comas", es un formato de archivo utilizado por muchas herramientas externas de Machine Learning. CSV es un formato de intercambio común cuando se trabaja con lenguajes de código abierto como R o Python.

Incluso si realiza la mayoría del trabajo en Azure Machine Learning, hay veces en que le resultará útil convertir el conjunto de datos a CSV para usarlo en herramientas externas. Por ejemplo:

+ Descargue el archivo CSV para abrirlo con Excel o importarlo a una base de datos relacional.  
+ Guarde el archivo CSV en el almacenamiento en la nube y conéctese desde Power BI para crear visualizaciones.  
+ Use el formato CSV para preparar los datos para su uso en R y Python. 

Al convertir un conjunto de datos a formato CSV, el archivo se guarda en el área de trabajo de Azure Machine Learning. Puede usar una utilidad de almacenamiento de Azure para abrir y usar el archivo directamente. También puede acceder al CSV en el diseñador. Para ello, debe seleccionar el módulo **Convert to CSV** (Convertir a CSV) y, después, seleccionar el icono del histograma en la pestaña **Resultados** del panel derecho para ver el resultado. Puede descargar el CSV de la carpeta Results a un directorio local.  

## <a name="how-to-configure-convert-to-csv"></a>Procedimiento para configurar Convertir a CSV


1.  Agregue el módulo Convert to CSV (Convertir a CSV) a una canalización. Este módulo se puede encontrar en la categoría **Transformación de datos** del diseñador. 

2. Conéctelo a cualquier módulo que genere un conjunto de datos.   
  
3.  Envíe la canalización.

### <a name="results"></a>Results
  

Seleccione la pestaña **Salidas** en el panel derecho de **Convert to CSV** (Convertir a CSV) y seleccione uno de estos iconos en **Port outputs** (Salidas de puerto).  

+ **Registrar conjunto de datos**: seleccione el icono y vuelva a guardar el archivo CSV en el área de trabajo de Azure ML como un conjunto de datos independiente. EL conjunto de datos lo encontrará en forma de módulo en el árbol de módulos de la sección **My Datasets** (Mis conjuntos de datos).

 + **Ver salida**: Seleccione el icono del ojo y siga las instrucciones para explorar la carpeta **Results_dataset** y descargar el archivo data.csv.

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 