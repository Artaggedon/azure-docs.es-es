---
title: Parametrización de los archivos de configuración en Azure Service Fabric
description: Aprenda a parametrizar los archivos de configuración en Service Fabric, una técnica útil para administrar varios entornos.
ms.topic: conceptual
ms.date: 10/09/2018
ms.openlocfilehash: ca376230c427c47e839b2dee96e8daa83ccedf15
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96576764"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Cómo parametrizar los archivos de configuración en Service Fabric

En este artículo se muestra cómo parametrizar un archivo de configuración de Service Fabric.  Si aún no conoce los conceptos básicos de administración de aplicaciones para varios entornos, lea [Administración de aplicaciones en varios entornos](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="procedure-for-parameterizing-configuration-files"></a>Procedimiento para parametrizar los archivos de configuración

En este ejemplo, se reemplaza un valor de configuración mediante parámetros en la implementación de la aplicación.

1. Abra el archivo *\<MyService>\PackageRoot\Config\Settings.xml* en el proyecto de servicio.
1. Establezca un nombre del parámetro de configuración y un valor, por ejemplo, el tamaño de caché igual a 25; para ello, agregue el siguiente código XML:

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. Guarde y cierre el archivo.
1. Abra el archivo *\<MyApplication>\ApplicationPackageRoot\ApplicationManifest.xml*.
1. En el archivo ApplicationManifest.xml, declare un parámetro y el valor predeterminado en el elemento `Parameters`.  Se recomienda que el nombre del parámetro contenga el nombre del servicio (por ejemplo, "MiServicio").

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. En la sección `ServiceManifestImport` del archivo ApplicationManifest.xml, agregue un elemento `ConfigOverrides` y `ConfigOverride` que haga referencia al paquete de configuración, la sección y el parámetro.

   ```xml
    <ConfigOverrides>
      <ConfigOverride Name="Config">
          <Settings>
            <Section Name="MyConfigSection">
                <Parameter Name="CacheSize" Value="[MyService_CacheSize]" />
            </Section>
          </Settings>
      </ConfigOverride>
    </ConfigOverrides>
   ```

> [!NOTE]
> Cuando se agrega un elemento ConfigOverride, Service Fabric siempre elige los parámetros de la aplicación o el valor predeterminado especificado en el manifiesto de la aplicación.
>
>

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre otras funcionalidades de administración de aplicaciones disponibles en Visual Studio, vea [Administración de aplicaciones de Service Fabric en Visual Studio](service-fabric-manage-application-in-visual-studio.md).
