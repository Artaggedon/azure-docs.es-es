---
title: 'Inicio rápido: Creación de un área de trabajo de Synapse'
description: Cree un área de trabajo de Synapse siguiendo los pasos de esta guía.
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: workspace
ms.date: 09/03/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: d2f2ed99af288b72a4c53f07d3cd7ba8b46974c0
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101693635"
---
# <a name="quickstart-create-a-synapse-workspace"></a>Inicio rápido: Creación de un área de trabajo de Synapse
En este inicio rápido se describe cómo crear un área de trabajo de Azure Synapse mediante Azure Portal.

## <a name="create-a-synapse-workspace"></a>Creación de un área de trabajo de Synapse

1. Abra [Azure Portal](https://portal.azure.com) y, en la parte superior, busque **Synapse**.
1. En los resultados de la búsqueda, en **Servicios**, seleccione **Azure Synapse Analytics**.
1. Seleccione **Agregar** para crear un área de trabajo.
1. En la pestaña **Datos básicos**, asigne un nombre único al área de trabajo. Usaremos **mysworkspace** en este documento.
1. Necesita una cuenta de ADLSGEN2 para crear un área de trabajo. La opción más sencilla para crear una nueva. Si desea volver a usar una existente, deberá realizar alguna configuración adicional. 
1. OPCIÓN 1 Creación de una nueva cuenta de ADLSGEN2 
    1. En **Select Data Lake Storage Gen 2** (Seleccionar Data Lake Storage Gen 2), haga clic en **Crear nuevo** y asígnele el nombre **contosolake**.
    1. En **Select Data Lake Storage Gen 2** (Seleccionar Data Lake Storage Gen 2), haga clic en **Sistema de archivos** y asígnele el nombre **contosolake**.
1. OPCIÓN 2 Consulte las instrucciones de **Preparación de una cuenta de almacenamiento** en la parte inferior de este documento.
1. El área de trabajo de Azure Synapse usará esta cuenta de almacenamiento como su cuenta de almacenamiento "principal" y el contenedor para almacenar los datos del área de trabajo. El área de trabajo almacena datos en tablas de Apache Spark. Almacena los registros de aplicaciones de Spark en una carpeta denominada **/synapse/workspacename**.
1. Seleccione **Revisar y crear** > **Crear**. El área de trabajo estará lista en unos minutos.

> [!NOTE]
> Después de crear el área de trabajo de Azure Synapse, no se podrá mover a otro inquilino de Azure Active Directory. Si lo hace mediante la migración de suscripciones u otras acciones, puede perder el acceso a los artefactos del área de trabajo.
> Además, actualmente no se puede crear un área de trabajo de Synapse Analytics en una suscripción del programa [Proveedor de soluciones en la nube (CSP)](https://docs.microsoft.com/partner-center/csp-overview).

## <a name="open-synapse-studio"></a>Abrir Synapse Studio

Una vez creada el área de trabajo de Azure Synapse, hay dos maneras de abrir Synapse Studio:

* Abrir el área de trabajo de Synapse en [Azure Portal](https://portal.azure.com). En la parte superior de la sección **Información general**, seleccione **Iniciar Synapse Studio**.
* Vaya a `https://web.azuresynapse.net` e inicie sesión en el área de trabajo.

## <a name="prepare-an-existing-storage-account-for-use-with-azure-synapse-analytics"></a>Preparación de una cuenta de almacenamiento existente para su uso con Azure Synapse Analytics

1. Abra [Azure Portal](https://portal.azure.com).
1. Navegue a una cuenta de almacenamiento de ADLSGEN2 existente.
1. Seleccione **Access control (IAM)** (Control de acceso [IAM]) en el panel izquierdo. Luego, asigne los siguientes roles o asegúrese de que ya estén asignados:
    * Asígnese el rol **Propietario**.
    * Asígnese el rol **Propietario de datos de Storage Blob**.
1. En el panel izquierdo, seleccione **Contenedores** y cree un contenedor.
1. Puede asignar cualquier nombre al contenedor. En este documento, llamaremos **users** al contenedor.
1. Acepte el valor predeterminado **Nivel de acceso público** y, después, seleccione **Crear**.

### <a name="configure-access-to-the-storage-account-from-your-workspace"></a>Configuración del acceso a la cuenta de almacenamiento desde el área de trabajo

Es posible que las identidades administradas del área de trabajo de Azure Synapse ya tengan acceso a la cuenta de almacenamiento. Siga estos pasos para asegurarse:

1. Abra [Azure Portal](https://portal.azure.com) y la cuenta de almacenamiento principal elegida para el área de trabajo.
1. Seleccione **Control de acceso (IAM)** en el panel izquierdo.
1. Asigne los siguientes roles o asegúrese de que ya estén asignados. Se usa el mismo nombre para la identidad y para el nombre del área de trabajo.
    * Para el rol **Colaborador de datos de Storage Blob** de la cuenta de almacenamiento, asigne **myworkspace** como identidad del área de trabajo.
    * Asigne **myworkspace** como el nombre del área de trabajo.

1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un grupo de SQL dedicado](quickstart-create-sql-pool-studio.md) 
* [Creación de un grupo de Apache Spark sin servidor](quickstart-create-apache-spark-pool-portal.md)
* [Uso de grupos de SQL sin servidor](quickstart-sql-on-demand.md)
