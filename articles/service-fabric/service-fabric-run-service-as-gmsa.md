---
title: Ejecución de un servicio de Azure Service Fabric en una cuenta gMSA
description: Obtenga información sobre cómo ejecutar un servicio como una cuenta de servicio administrada de grupo (gMSA) en un clúster independiente de Windows de Service Fabric.
ms.topic: how-to
ms.date: 03/29/2018
ms.openlocfilehash: 9750042764306c5df7a391429cc6926704db05ab
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91838915"
---
# <a name="run-a-service-as-a-group-managed-service-account"></a>Ejecución de un servicio como una cuenta de servicio administrada de grupo

En un clúster independiente de Windows Server, puede ejecutar un servicio *como una cuenta de servicio administrada de grupo* (gMSA) mediante una directiva de *RunAs*.  De forma predeterminada, las aplicaciones de Service Fabric se ejecutan en la misma cuenta en que se ejecuta el proceso `Fabric.exe`. La ejecución de aplicaciones en cuentas diferentes, incluso en un entorno hospedado compartido, aumenta la seguridad entre aplicaciones. Con una gMSA, no hay ninguna contraseña ni contraseña cifrada almacenada en el manifiesto de aplicación.  También puede ejecutar un servicio como un [grupo o usuario de Active Directory](service-fabric-run-service-as-ad-user-or-group.md).

En el ejemplo siguiente se muestra cómo crear una cuenta gMSA denominada *svc-Test$* , cómo implementar esa cuenta de servicio administrada en los nodos del clúster y cómo configurar la entidad de seguridad de usuario.

> [!NOTE]
> El uso de una gMSA con un clúster de Service Fabric independiente requiere Active Directory local dentro de su dominio (en lugar de Azure Active Directory [Azure AD]).

Requisitos previos:

- El dominio necesita una clave raíz KDS.
- Debe haber al menos un controlador de dominio de Windows Server 2012 (o R2) en el dominio.

1. Haga que un administrador de dominio de Active Directory cree una cuenta de servicio administrada de grupo mediante el cmdlet `New-ADServiceAccount` y asegúrese de que `PrincipalsAllowedToRetrieveManagedPassword` incluya todos los nodos del clúster de Service Fabric. `AccountName`, `DnsHostName` y `ServicePrincipalName` deben ser únicos.

    ```powershell
    New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
    ```

2. En cada uno de los nodos de clúster de Service Fabric (por ejemplo, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), instale y pruebe la gMSA.
    
    ```powershell
    Add-WindowsFeature RSAT-AD-PowerShell
    Install-AdServiceAccount svc-Test$
    Test-AdServiceAccount svc-Test$
    ```

3. Configure la entidad de seguridad de usuario y `RunAsPolicy` para que hagan referencia al [usuario](./service-fabric-cluster-fabric-settings.md#runas).
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
          <ConfigOverrides />
          <Policies>
              <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
          </Policies>
        </ServiceManifestImport>
      <Principals>
        <Users>
          <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
        </Users>
      </Principals>
    </ApplicationManifest>
    ```

> [!NOTE]
> Si aplica una directiva de RunAs a un servicio y el manifiesto de servicio declara los recursos de punto de conexión con el protocolo HTTP, debe especificar **SecurityAccessPolicy**.  Para obtener más información, consulte [Assign a security access policy for HTTP and HTTPS endpoints](service-fabric-assign-policy-to-endpoint.md) (Asignación de una directiva de acceso de seguridad a los puntos de conexión HTTP y HTTPS).
>

Los artículos siguientes lo guiarán por los próximos pasos:

- [Entender el modelo de aplicación](service-fabric-application-model.md)
- [Especificación de los recursos en un manifiesto de servicio](service-fabric-service-manifest-resources.md)
- [Implementar una aplicación](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
