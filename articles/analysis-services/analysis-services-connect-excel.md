---
title: Conexión a Azure Analysis Services con Excel | Microsoft Docs
description: Aprenda a conectarse a un servidor de Azure Analysis Services mediante Excel. Una vez conectados, los usuarios pueden crear tablas dinámicas para explorar los datos.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/01/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: fccb919a7eac79080fefeac70326b8aa7a967622
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96499477"
---
# <a name="connect-with-excel"></a>Conexión con Excel

Una vez que se ha creado un servidor y se ha implementado en él un modelo tabular, los clientes pueden conectarse y comenzar a explorar los datos. 

## <a name="before-you-begin"></a>Antes de empezar

La cuenta con la que inicie sesión debe pertenecer a un rol de base de datos modelo con, por lo menos, permisos de lectura. Para más información, consulte [Permisos de usuario y autenticación](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Conexión en Excel

La conexión a un servidor en Excel se admite mediante Obtener datos en Excel 2016 y versiones posteriores. No se admite la conexión mediante el Asistente para importar tablas en Power Pivot. 

1. En la cinta **Datos** de Excel, haga clic en **Obtener datos** > **Desde base de datos** > **Desde Analysis Services**.

2. En el Asistente para conexión de datos, en **Nombre del servidor**, escriba el nombre del servidor incluidos el protocolo y el URI. Por ejemplo, asazure://westcentralus.asazure.windows.net/advworks. Luego, en **Credenciales de inicio de sesión**, seleccione **Utilizar el nombre de usuario y la contraseña siguientes** y escriba el nombre de usuario de la organización (por ejemplo, nancy@adventureworks.com) y la contraseña.

    > [!IMPORTANT]
    > Si inicia sesión mediante una cuenta Microsoft, Windows Live ID, Yahoo, Gmail, etc. o le piden que utilice la autenticación multifactor para iniciar sesión, deje el campo de la contraseña en blanco. Le pedirán la contraseña cuando haga clic en Siguiente. 

    ![Conexión desde el inicio de sesión de Excel](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. En **Seleccionar base de datos y tabla**, seleccione la base de datos y el modelo o la perspectiva y haga clic en **Finalizar**.
   
    ![Conexión desde el modelo de selección de Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Consulte también

[Bibliotecas de cliente](/analysis-services/client-libraries?view=azure-analysis-services-current&preserve-view=true)   
[Administración del servidor](analysis-services-manage.md)
