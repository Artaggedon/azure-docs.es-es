---
title: 'Tutorial: Administración de las marcas de característica con Azure App Configuration'
titleSuffix: Azure App Configuration
description: En este tutorial, aprenderá a administrar las marcas de características por separado desde la aplicación mediante Azure App Configuration.
services: azure-app-configuration
documentationcenter: ''
author: AlexandraKemperMS
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: alkemper
ms.custom: devx-track-csharp, mvc
ms.openlocfilehash: 0410a1cde12b9ef762d348a286d78b35f7b14bfd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96932309"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Tutorial: Administración de las marcas de características en Azure App Configuration

Puede almacenar todas las marcas de características en Azure App Configuration y administrarlas desde un único lugar. App Configuration tiene una interfaz de usuario de portal, denominada **Administrador de características**, que se ha diseñado específicamente para las marcas de características. App Configuration admite también de forma nativa el esquema de datos de las marcas de características de .NET Core.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Definir y administrar las marcas de características en App Configuration.
> * Obtener acceso a las marcas de características desde la aplicación.

## <a name="create-feature-flags"></a>Creación de marcas de características

El Administrador de características de Azure Portal para App Configuration proporciona una interfaz de usuario para crear y administrar las marcas de características que usa en las aplicaciones.

Para agregar una nueva marca de características:

1. Seleccione **Administrador de características** >  **+Agregar** para agregar una marca de características.

    ![Lista de marcas de características](./media/azure-app-configuration-feature-flags.png)

1. Escriba un nombre de clave único para la marca de características. Necesita este nombre para hacer referencia a la marca en el código.

1. Si quiere, escriba una descripción de la marca de características.

1. Establezca el estado inicial de la marca de características. Este estado será normalmente *Desactivado* o *Activado*. El estado *Activado* cambia a *Condicional* si agrega un filtro a la marca de características.

    ![Creación de una marca de características](./media/azure-app-configuration-feature-flag-create.png)

1. Si el estado es *Activado*, seleccione **+Agregar filtro** para especificar cualquier condición adicional que haya que cumplir para tener ese estado. Escriba una clave de filtro integrada o personalizada y, a continuación, seleccione **+Agregar parámetro** para asociar uno o varios parámetros al filtro. Entre los filtros integrados están los siguientes:

    | Clave | Parámetros JSON |
    |---|---|
    | Microsoft.Percentage | {"Value": 0-100 percent} |
    | Microsoft.TimeWindow | {"Start": UTC time, "End": UTC time} |
    | Microsoft.Targeting | { "Audience": blob de JSON que define usuarios, grupos y porcentajes de implementación. Consulte un ejemplo en el elemento `EnabledFor` de [este archivo de configuración](https://github.com/microsoft/FeatureManagement-Dotnet/blob/master/examples/FeatureFlagDemo/appsettings.json) }

    ![Filtro de la marca de características](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Actualización de los estados de las marcas de características

Para cambiar el valor de estado de una marca de características:

1. Seleccione **Administrador de características**.

1. A la derecha de la marca de características que desea modificar, seleccione los puntos suspensivos (**...**) y, a continuación, seleccione **Editar**.

1. Establezca un nuevo estado para la marca de características.

## <a name="access-feature-flags"></a>Acceso a las marcas de características

Las marcas de características creadas por el Administrador de características se almacenan y recuperan como pares clave-valor regulares. Se guardan con un prefijo especial de espacio de nombres `.appconfig.featureflag`. Para ver los pares clave-valor subyacentes, use el Explorador de configuración. La aplicación puede recuperar estos valores mediante los proveedores de configuración de App Configuration, los SDK, las extensiones de la línea de comandos y las API REST.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a administrar las marcas de características y sus estados mediante App Configuration. Para más información sobre la compatibilidad de administración de características en App Configuration y ASP.NET Core, consulte el siguiente artículo:

* [Uso de marcas de características en una aplicación de ASP.NET Core](./use-feature-flags-dotnet-core.md)
