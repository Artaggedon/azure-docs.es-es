---
title: archivo de inclusión
description: archivo de inclusión
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 10/01/2020
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 30a7f3ae878cebcd1e58287fc59241651dac2bfd
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "94504107"
---
1. En una nueva ventana del explorador, inicie sesión en [Azure Portal](https://portal.azure.com/).

2. En el menú de la izquierda, seleccione **Crear un recurso**.
   
   ![Crear un recurso en Azure Portal](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-0.png)
   
3. En la página **Nuevo**, seleccione **Bases de datos** > **Azure Cosmos DB**.
   
   ![El panel de las bases de datos de Azure Portal](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)
   
3. En la página **Crear una cuenta de Azure Cosmos DB**, especifique la configuración de la nueva cuenta de Azure Cosmos DB. 
 
    Configuración|Value|Descripción
    ---|---|---
    Subscription|Su suscripción|Seleccione la suscripción de Azure que quiere usar para esta cuenta de Azure Cosmos DB. 
    Grupo de recursos|Crear nuevo<br><br>A continuación, escriba el mismo nombre que el nombre de cuenta.|Seleccione **Crear nuevo**. Luego, escriba un nombre nuevo de grupo de recursos para la cuenta. Para simplificar, use el mismo nombre que el de la cuenta de Azure Cosmos. 
    Nombre de cuenta|Escriba un nombre único.|Escriba un nombre único para identificar la cuenta de Azure Cosmos DB. El URI de la cuenta será *cassandra.cosmos.azure.com* y se anexará al nombre único de la cuenta.<br><br>El nombre de la cuenta solo puede utilizar letras minúsculas, números y guiones (-), y debe tener entre 3 y 31 caracteres de longitud.
    API|Cassandra|La API determina el tipo de cuenta que se va a crear. Azure Cosmos DB proporciona cinco API: Core (SQL) para bases de datos de documentos, Gremlin para bases de datos de grafos, MongoDB para bases de datos de documentos, Azure Table y Cassandra. Debe crear una cuenta independiente para cada API. <br><br>Seleccione **Cassandra**, ya que en este inicio rápido va a crear una tabla que funciona con Cassandra API. <br><br>[Más información acerca de Cassandra API](../articles/cosmos-db/cassandra-introduction.md).|
    Location|Seleccionar la región más cercana a los usuarios|Seleccione una ubicación geográfica para hospedar la cuenta de Azure Cosmos DB. Use la ubicación más cercana a los usuarios para proporcionarles el acceso más rápido a los datos.
    Capacity mode (Modo de capacidad)|Rendimiento aprovisionado o sin servidor|Seleccione **Provisioned throughput** (Rendimiento aprovisionado) para crear una cuenta en modo de [rendimiento aprovisionado](../articles/cosmos-db/set-throughput.md). Seleccione **Serverless** (Sin servidor) para crear una cuenta en modo [sin servidor](../articles/cosmos-db/serverless.md).

    Seleccione **Revisar y crear**. Puede omitir las secciones **Redes**, **Copia de seguridad**, **Cifrado** y **Etiquetas**. 

    ![Página de la nueva cuenta de Azure Cosmos DB](./media/cosmos-db-create-dbaccount-cassandra/azure-cosmos-db-create-new-account.png)

4. La cuenta tarda unos minutos en crearse. Espere a que el portal muestre la página que indica **¡Enhorabuena! Se ha creado su cuenta de Azure Cosmos DB**.

