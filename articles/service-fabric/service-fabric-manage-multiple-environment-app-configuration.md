---
title: Administración de aplicaciones para varios entornos
description: Las aplicaciones de Service Fabric se pueden ejecutar en clústeres cuyo tamaño oscila entre una y miles de máquinas. En algunos casos, deseará configurar su aplicación de forma diferente para cada uno de los entornos. Este artículo explica cómo definir distintos parámetros de aplicación por entorno.
ms.topic: conceptual
ms.date: 02/23/2018
ms.openlocfilehash: c907540c03788ab5f4087a96e301f18ab7ced4ca
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98787984"
---
# <a name="manage-applications-for-multiple-environments"></a>Administración de aplicaciones en varios entornos

Los clústeres de Azure Service Fabric le permiten crear clústeres con cualquier número de máquinas, desde una sola máquina a miles de ellas. En la mayoría de los casos, se verá en la necesidad de implementar la aplicación en varias configuraciones de clúster: el clúster de desarrollo local, un clúster de desarrollo compartido y el clúster de producción. Todos estos clústeres se consideran distintos entornos en los que tiene que ejecutarse el código. Aunque los archivos binarios de la aplicación se pueden ejecutar sin modificaciones en este amplio espectro, con frecuencia querrá configurar la aplicación de manera diferente.

Considere estos dos sencillos ejemplos:
  - El servicio escucha en un puerto definido, pero es necesario que ese puerto sea diferente entre los entornos.
  - Debe proporcionar credenciales de enlace para una base de datos que sea diferente entre los entornos.

## <a name="specifying-configuration"></a>Especificación de la configuración

La configuración que proporcione se puede dividir en dos categorías:

- Configuración que se aplica a cómo se ejecutan los servicios
  - Por ejemplo, el número de puerto de un punto de conexión o el número de instancias de un servicio.
  - Esta configuración se especifica en el archivo de manifiesto de servicio o aplicación.
- Configuración que se aplica al código de aplicación
  - Por ejemplo, la información de enlace para una base de datos.
  - Esta configuración se puede proporcionar a través de archivos de configuración o variables de entorno

> [!NOTE]
> No todos los atributos del archivo de manifiesto de servicio o aplicación admiten parámetros.
> En esos casos, tiene que depender de la sustitución de cadenas como parte del flujo de trabajo de implementación. En Azure DevOps puede usar una extensión como Replace Tokens: https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens, o en Jenkins, podría ejecutar una tarea de script para reemplazar los valores.
>

## <a name="specifying-parameters-during-application-creation"></a>Especificación de parámetros durante la creación de aplicaciones

Al crear instancias de aplicación con nombre en Service Fabric, tiene la opción de pasar parámetros. La manera de hacerlo depende de cómo cree la instancia de la aplicación.

  - En PowerShell, el cmdlet [`New-ServiceFabricApplication`](/powershell/module/servicefabric/new-servicefabricapplication) toma los parámetros de la aplicación como una tabla hash.
  - Mediante sfctl, el comando [`sfctl application create`](./service-fabric-sfctl-application.md#sfctl-application-create) acepta parámetros como una cadena JSON. El script install.sh script usa sfctl.
  - Visual Studio proporciona un conjunto de archivos de parámetros en la carpeta de parámetros del proyecto de aplicación. Estos archivos de parámetros se usan al realizar la publicación desde Visual Studio, mediante Azure DevOps Services o Azure DevOps Server. En Visual Studio, los archivos de parámetros se van a pasar al script Deploy-FabricApplication.ps1.

## <a name="next-steps"></a>Pasos siguientes
En los artículos siguientes se muestra cómo usar algunos de los conceptos aquí descritos:

- [How to specify environment variables for services in Service Fabric](service-fabric-how-to-specify-environment-variables.md) (Especificación de variables de entorno para servicios en Service Fabric)
- [How to specify the port number of a service using parameters in Service Fabric](service-fabric-how-to-specify-port-number-using-parameters.md) (Especificación del número de puerto de un servicio mediante parámetros en Service Fabric)
- [How to parameterize configuration files](service-fabric-how-to-parameterize-configuration-files.md) (Parametrización de los archivos de configuración)

- [Environment variable reference](service-fabric-environment-variables-reference.md) (Referencia de variables de entorno)
