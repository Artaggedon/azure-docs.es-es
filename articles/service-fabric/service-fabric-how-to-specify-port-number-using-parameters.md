---
title: Especificación de número de puerto de un servicio mediante parámetros
description: Se muestra cómo usar los parámetros para especificar el puerto de una aplicación en Service Fabric
ms.topic: conceptual
ms.date: 12/06/2017
ms.openlocfilehash: ba2fb459dc9c981ad168aca4d0edf969650ccf48
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "96576713"
---
# <a name="how-to-specify-the-port-number-of-a-service-using-parameters-in-service-fabric"></a>Cómo especificar el número de puerto de un servicio mediante parámetros en Service Fabric

Este artículo muestra cómo especificar el número de puerto de un servicio mediante parámetros en Service Fabric con Visual Studio.

## <a name="procedure-for-specifying-the-port-number-of-a-service-using-parameters"></a>Procedimiento para especificar el número de puerto de un servicio mediante parámetros

En este ejemplo se establece el número de puerto de la API web de ASP.NET Core usando un parámetro.

1. Abra Visual Studio y cree una nueva aplicación de Service Fabric.
1. Elija la plantilla de ASP.NET Core sin estado.
1. Elija Web API.
1. Abra el archivo ServiceManifest.xml.
1. Tome nota del nombre del punto de conexión especificado para el servicio. El valor predeterminado es `ServiceEndpoint`.
1. Abra el archivo ApplicationManifest.xml
1. En el elemento `ServiceManifestImport`, agregue un nuevo elemento `RessourceOverrides` con una referencia al punto de conexión en el archivo ServiceManifest.xml.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. En el elemento `Endpoint`, ahora puede invalidar cualquier atributo mediante un parámetro. En este ejemplo, especifique `Port` y establézcalo en un nombre de parámetro utilizando corchetes: por ejemplo, `[MyWebAPI_PortNumber]`

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint" Port="[MyWebAPI_PortNumber]"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. Todavía en el archivo ApplicationManifest.xml, a continuación, especifique el parámetro en el elemento `Parameters`

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" />
      </Parameters>
    ```

1. Y defina un `DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="8080" />
      </Parameters>
    ```

1. Abra la carpeta ApplicationParameters y el archivo `Cloud.xml`
1. Para especificar un puerto diferente que se usará al publicar en un clúster remoto, agregue el parámetro con el número de puerto en este archivo.

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" Value="80" />
      </Parameters>
    ```

Al publicar la aplicación desde Visual Studio con el perfil de publicación Cloud.xml, el servicio está configurado para utilizar el puerto 80. Si implementa la aplicación sin especificar el parámetro MyWebAPI_PortNumber, el servicio utiliza el puerto 8080.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre algunos de los conceptos principales que se describen en este artículo, consulte los artículos de [Administración de aplicaciones para varios entornos](service-fabric-manage-multiple-environment-app-configuration.md).

Para obtener más información sobre otras funcionalidades de administración de aplicaciones disponibles en Visual Studio, vea [Administración de aplicaciones de Service Fabric en Visual Studio](service-fabric-manage-application-in-visual-studio.md).
