---
title: Creación de un clúster de Service Fabric en Azure Portal
description: Obtenga información sobre cómo configurar un clúster de Service Fabric seguro en Azure mediante Azure Portal y Azure Key Vault.
ms.topic: conceptual
ms.date: 09/06/2018
ms.openlocfilehash: c679a804db09b1034f31e9d8da1f7d2ad206f684
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "90563733"
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Creación de un clúster de Service Fabric en Azure mediante el Portal de Azure
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Esta guía paso a paso le ayudará en la configuración de un clúster de Service Fabric (Linux o Windows) seguro en Azure mediante Azure Portal. Estos pasos son los siguientes:

* Cree un clúster en Azure a través de Azure Portal.
* Autenticación de los administradores mediante certificados.

> [!NOTE]
> Para información sobre opciones de seguridad más avanzadas, como la autenticación de usuarios con Azure Active Directory y la configuración de certificados para la seguridad de las aplicaciones, [cree el clúster mediante Azure Resource Manager][create-cluster-arm].
> 
> 

## <a name="cluster-security"></a>Seguridad de clúster 
Los certificados se usan en Service Fabric para proporcionar autenticación y cifrado con el fin de proteger diversos aspectos de un clúster y sus aplicaciones. Para más información sobre el modo en que se usan los certificados en Service Fabric, consulte los [Escenarios de seguridad de los clústeres de Service Fabric][service-fabric-cluster-security].

Si es la primera vez que crea un clúster de Service Fabric o que implementa un clúster para las cargas de trabajo de prueba, puede ir a la siguiente sección (**Crear un clúster en Azure Portal**) y permitir que el sistema genere los certificados necesarios para los clústeres que ejecutan cargas de trabajo de prueba. Si va a configurar un clúster para las cargas de trabajo de producción, siga leyendo.

#### <a name="cluster-and-server-certificate-required"></a>Certificado de clúster y servidor (obligatorio)
Este certificado es necesario para proteger un clúster e impedir el acceso no autorizado. La seguridad adopta dos formas:

* **Autenticación del clúster:** autentica la comunicación de nodo a nodo para la federación del clúster. Solo los nodos que pueden probar su identidad con este certificado pueden unirse al clúster.
* **Autenticación de servidor:** los puntos de conexión de administración del clúster se autentican en un cliente de administración, de forma que este sabe que está hablando con el clúster real. Este certificado proporciona también TLS para la API de administración de HTTPS y para Service Fabric Explorer sobre HTTPS.

Para servir a estos propósitos, el certificado debe cumplir los siguientes requisitos:

* El certificado debe contener una clave privada.
* El certificado debe crearse para el intercambio de claves, que se puedan exportar a un archivo Personal Information Exchange (.pfx).
* El nombre de **firmante del certificado debe coincidir con el nombre del dominio** usado para acceder al clúster de Service Fabric. Este es un requisito para proporcionar TLS a los puntos de conexión de administración HTTPS y de Service Fabric Explorer del clúster. No puede obtener un certificado TLS/SSL de una entidad de certificación (CA) para el dominio `.cloudapp.azure.com`. Adquiera un nombre de dominio personalizado para el clúster. Cuando solicite un certificado de una CA, el nombre de sujeto del certificado debe coincidir con el nombre del dominio personalizado que se usó para el clúster.

#### <a name="client-authentication-certificates"></a>Certificados de autenticación de cliente
Los certificados de cliente adicionales autentican a los administradores en las tareas de administración del clúster. Service Fabric tiene dos niveles de acceso: **administrador** y **usuario de solo lectura**. Se debe usar como mínimo un certificado para el acceso administrativo. Para accesos de nivel de usuario adicionales, se debe proporcionar un certificado diferente. Para obtener más información sobre los roles de acceso, vea [Control de acceso basado en roles para clientes de Service Fabric][service-fabric-cluster-security-roles].

No es necesario cargar los certificados de autenticación de cliente en Key Vault para trabajar con Service Fabric. Estos certificados solo se deben proporcionar a los usuarios que están autorizados para la administración del clúster. 

> [!NOTE]
> Azure Active Directory es el método recomendado para autenticar a los clientes en las operaciones de administración del clúster. Para usar Azure Active Directory, debe [crear un clúster mediante Azure Resource Manager][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Certificados de aplicación (opcionales)
Se puede instalar un número cualquiera de certificados adicionales en un clúster para proteger la aplicación. Antes de crear el clúster, tenga en cuenta los escenarios de seguridad de las aplicaciones que requieren que se instale un certificado en los nodos, por ejemplo:

* Cifrado y descifrado de los valores de configuración de aplicación
* Cifrado de datos entre nodos durante la replicación 

No se pueden configurar certificados de la aplicación al [crear un clúster mediante Azure Portal](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-creation-via-portal.md). Para configurar certificados de aplicación en el momento de la configuración del clúster, debe [crear un clúster mediante Azure Resource Manager][create-cluster-arm]. También puede agregar certificados de aplicación al clúster después de que se ha creado.

## <a name="create-cluster-in-the-azure-portal"></a>Creación de un clúster en el Portal de Azure

La creación de un clúster de producción para satisfacer las necesidades de la aplicación requiere cierto planeamiento. Por lo tanto, se recomienda que lea y comprenda el documento [Consideraciones de planeación de capacidad del clúster de Service Fabric][service-fabric-cluster-capacity]. 

### <a name="search-for-the-service-fabric-cluster-resource"></a>Búsqueda del recurso de clúster de Service Fabric

Inicie sesión en [Azure Portal][azure-portal].
Haga clic en **Crear un recurso** para agregar una nueva plantilla de recursos. Busque la plantilla Clúster de Service Fabric en **Marketplace** debajo de **Todo**.
Seleccione **Clúster de Service Fabric** en la lista.

![Busque la plantilla del clúster de Service Fabric en el Portal de Azure.][SearchforServiceFabricClusterTemplate]

Vaya a la hoja **Clúster de Service Fabric** y haga clic en **Crear**.

La hoja **Crear clúster de Service Fabric** consta de los cuatro pasos siguientes:

### <a name="1-basics"></a>1. Aspectos básicos
![Captura de pantalla de la creación de un grupo de recursos.][CreateRG]

En la hoja Básico, se deben proporcionar los datos básicos del clúster.

1. Escriba el nombre del clúster.
2. Elija un **nombre de usuario** y una **contraseña** para Escritorio remoto para las máquinas virtuales.
3. No olvide seleccionar la **suscripción** en la que desea que se implemente el clúster, especialmente si tiene varias suscripciones.
4. Cree un **grupo de recursos**. Es mejor asignarle el mismo nombre que el del clúster, pues resulta de utilidad para encontrarlos en el futuro, particularmente cuando está intentando realizar cambios en la implementación o eliminar el clúster.
   
   > [!NOTE]
   > Aunque puede decidir utilizar un grupo de recursos existente, es conveniente crear uno nuevo. Esto facilita la eliminación de clústeres y de todos los recursos que usan.
   > 
   > 
5. Seleccione la **ubicación** en la que quiere crear el clúster. Si planea usar un certificado existente que ya cargó en un almacén de claves, debe usar la misma región en la que se encuentra su almacén de claves. 

### <a name="2-cluster-configuration"></a>2. Configuración del clúster
![Creación de un tipo de nodo][CreateNodeType]

Configure los nodos del clúster. Los tipos de nodos definen los tamaños de máquina virtual, el número de máquinas virtuales y sus propiedades. El clúster puede tener más de un tipo de nodo, pero el tipo de nodo principal (el primero que se define en el portal) debe tener al menos cinco máquinas virtuales. Este es el tipo de nodo donde se encuentran los servicios del sistema de Service Fabric. No configure **Propiedades de ubicación** porque el sistema agrega automáticamente una propiedad de ubicación predeterminada de "NodeTypeName".

> [!NOTE]
> Un escenario común para varios tipos de nodos es una aplicación que contiene un servicio front-end y un servicio back-end. Quiere colocar el servicio front-end en VM más pequeñas (tamaños de VM como D2_V2) con puertos abiertos a Internet y quiere que el servicio back-end esté en VM más grandes (con tamaños de VM como D3_V2, D6_V2, D15_V2, etc.) sin puertos abiertos accesibles desde Internet.
> 

1. Elija un nombre para el tipo de nodo (con una longitud de 1 a 12 caracteres y que contenga solamente letras y números).
2. El **tamaño** mínimo de máquinas virtuales para el tipo de nodo principal depende del nivel de **durabilidad** que elija para el clúster. El valor predeterminado del nivel de durabilidad es Bronze. Para obtener más información sobre la durabilidad, vea [Elección de los niveles de durabilidad del clúster de Service Fabric][service-fabric-cluster-durability].
3. Seleccione el **tamaño de la máquina virtual**. Las máquinas virtuales de la serie D tienen unidades SSD y son muy recomendables para aplicaciones con estado. No use cualquier SKU de VM que tenga núcleos parciales o menos de 10 GB de capacidad de disco disponible. Consulte el documento [Consideraciones de planeación de capacidad del clúster de Service Fabric][service-fabric-cluster-capacity] para obtener ayuda con la selección del tamaño de la máquina virtual.
4.  Los **clúster de un solo nodo y clústeres de tres nodos** están diseñados únicamente con fines de prueba. Estos clústeres no se admiten para la ejecución de cargas de trabajo de producción.
5. Elija la **capacidad inicial del conjunto de escalado de máquina virtual** para el tipo de nodo. Puede escalar o reducir horizontalmente el número de VM en un tipo de nodo más adelante, pero en el tipo de nodo principal, el número mínimo necesario para cargas de trabajo de producción es de cinco máquinas. Otros tipos de nodo pueden tener una máquina virtual como mínimo. El **número** mínimo de máquinas virtuales para el tipo de nodo principal afecta al nivel de **confiabilidad** que elija.  
6. Configure los **puntos de conexión personalizados**. Este campo le permite especificar una lista de puertos separados por coma que quiere exponer mediante Azure Load Balancer a la Internet pública para sus aplicaciones. Por ejemplo, si planea implementar una aplicación web en el clúster, escriba aquí "80" para permitir el paso del tráfico del puerto 80 al clúster. Para más información sobre los puntos de conexión, vea la [comunicación con las aplicaciones][service-fabric-connect-and-communicate-with-services].
7. **Habilitación del proxy inverso**.  El [proxy inverso de Service Fabric](service-fabric-reverseproxy.md) ayuda a que los microservicios que se ejecutan en un clúster de Service Fabric detecten otros servicios que tienen puntos de conexión HTTP y se comuniquen con dichos servicios.
8. En la hoja **Configuración del clúster**, en **+Mostrar configuración opcional**, configure el **diagnóstico** del clúster. De forma predeterminada, los diagnósticos se habilitan en el clúster para ayudar a solucionar los problemas. Si quiere deshabilitar los diagnósticos, cambie el botón de alternancia **Estado** a **Desactivado**. **No** se recomienda desactivar los diagnósticos. Si ya creó el proyecto de Application Insights, proporcione su clave para que los rastros de la aplicación se enruten hacia ella.
9. **Inclusión del servicio DNS**.  El [servicio DNS](service-fabric-dnsservice.md) es un servicio opcional que le permite buscar otros servicios mediante el protocolo DNS.
10. Seleccione el **modo de actualización de Fabric** que desea establecer en el clúster. Seleccione **Automático** si desea que el sistema coja la última versión disponible automáticamente e intente actualizar el clúster con ella. Establezca el modo en **Manual** si desea elegir una versión compatible. Para más información sobre el modo de actualización de Service Fabric, vea el [documento de actualización de un clúster de Service Fabric][service-fabric-cluster-upgrade].

> [!NOTE]
> Se admiten solo los clústeres que ejecutan versiones compatibles de Service Fabric. Si selecciona el modo **Manual** , está aceptando la responsabilidad de actualizar el clúster a una versión compatible.
> 

### <a name="3-security"></a>3. Seguridad
![Captura de pantalla de las configuraciones de seguridad de Azure Portal.][BasicSecurityConfigs]

Para facilitar la configuración de un clúster de prueba seguro, hemos proporcionado la opción **Básico**. Si ya dispone de un certificado y lo ha cargado en su [almacén de claves](../key-vault/index.yml) (y habilitó el almacén de claves para la implementación), use la opción **Personalizado**

#### <a name="basic-option"></a>Opción Básico
Siga las instrucciones que se muestran en la pantalla para agregar o volver a usar un almacén de claves existente y agregar un certificado. La adición del certificado es un proceso sincrónico, por lo que tendrá que esperar a que se cree el certificado.

Resista la tentación de salir de la pantalla hasta que se complete el proceso anterior.

![Captura de pantalla que muestra la página Seguridad con la opción Básico seleccionada con el panel Almacén de claves y el panel Crear almacén de claves.][CreateKeyVault]

Una vez creado el almacén de claves, edite las directivas de acceso para el almacén de claves. 

![Captura de pantalla que muestra el panel Crear clúster de Service Fabric con la opción 3. Seguridad seleccionada y una explicación de que el almacén de claves no está habilitado.][CreateKeyVault2]

Haga clic en **Editar directivas de acceso** y, a continuación, en **Mostrar directivas de acceso avanzadas** para habilitar el acceso a las máquinas virtuales para la implementación. Se recomienda habilitar también la implementación de plantilla. Cuando haya realizado las selecciones, no olvide hacer clic en el botón **Guardar** y salir del panel **Directivas de acceso**.

![Captura de pantalla que muestra el panel Crear clúster de Service Fabric con el panel Seguridad abierto y el panel Directivas de acceso abierto.][CreateKeyVault3]

Escriba correctamente el nombre del certificado y haga clic en **Aceptar**.

![Captura de pantalla que muestra el panel Crear clúster de Service Fabric con Seguridad seleccionado como antes pero sin la explicación de que el almacén de claves no está habilitado.][CreateKeyVault4]

#### <a name="custom-option"></a>Opción Personalizado
Omita esta sección si ya realizó los pasos descritos en la opción **Básico**.

![Captura de pantalla que muestra el cuadro de diálogo de configuración de los parámetros de seguridad del clúster.][SecurityCustomOption]

Necesita la información sobre el almacén de claves de origen, la dirección URL del certificado y la huella digital del certificado para completar la página de seguridad. Si no dispone de estos datos, abra otra ventana del explorador y, en Azure Portal, haga lo siguiente

1. Vaya a su servicio de almacén de claves.
2. Seleccione la pestaña "Propiedades" y copia el valor de "RESOURCE ID" a"Almacén de claves de origen" en la otra ventana del explorador. 

    ![Captura de pantalla que muestra la ventana Propiedades del almacén de claves.][CertInfo0]

3. A continuación, seleccione la pestaña "Certificados".
4. Haga clic en la huella digital del certificado, lo que le llevará a la página de versiones.
5. Haga clic en los GUID que se muestran en la versión actual.

    ![Captura de pantalla que muestra la ventana Certificado para el almacén de claves][CertInfo1]

6. Ahora debería estar en una pantalla como la que se muestra a continuación. Copie la huella digital SHA-1 hexadecimal en "Huella digital del certificado" en la otra ventana del explorador.
7. Copie la información de "Identificador secreto" en "Dirección URL del certificado" en otra ventana del explorador.

    ![Captura de pantalla que muestra el cuadro de diálogo Versión de certificado con una opción para copiar el Identificador del certificado.][CertInfo2]

Marque el cuadro **Configurar opciones avanzadas** para insertar certificados de cliente para el **cliente de administración** y el **cliente de solo lectura**. En estos campos, inserte la huella digital del certificado de cliente de administración y la huella digital del certificado de cliente de solo lectura, si procede. Cuando los administradores intenten conectarse al clúster, se les concederá acceso únicamente si tienen un certificado con una huella digital que coincida con los valores de huella digital especificados aquí.  

### <a name="4-summary"></a>4. Resumen

Ahora está listo para implementar el clúster. Antes de hacerlo, descargue el certificado y busque el vínculo en el cuadro informativo azul de gran tamaño. Asegúrese de guardar el certificado en un lugar seguro. Lo necesitará para conectarse a su clúster. Puesto que el certificado que descargó no tiene contraseña, se recomienda agregar una.

Para completar la creación del clúster, haga clic en **Crear**. De manera opcional, también puede descargar la plantilla.

![Captura de pantalla que muestra la página Resumen de Crear clúster de Service Fabric con un vínculo para ver y descargar un certificado.][Summary]

Puede ver el progreso de creación en las notificaciones. (Haga clic en el icono de la "campana" cerca de la barra de estado en la parte superior derecha de la pantalla). Si hizo clic en **Anclar a Panel de inicio** al crear el clúster, verá **Deploying Service Fabric Cluster** (Implementando clúster de Service Fabric) anclado al panel de **Inicio**. Este proceso puede tardar un tiempo. 

Para realizar operaciones de administración en el clúster con Powershell o la CLI, deberá conectarse a su clúster. Para más información al respecto en [Conexión a un clúster seguro](service-fabric-connect-to-secure-cluster.md).

## <a name="view-your-cluster-status"></a>Visualización del estado del clúster
![Captura de pantalla de los detalles del clúster en el panel.][ClusterDashboard]

Una vez finalizado el clúster, puede inspeccionarlo en el portal:

1. Vaya a **Examinar** y haga clic en **Clústeres de Service Fabric**.
2. Busque su clúster y haga clic en él.
3. Ahora puede ver los detalles del clúster en el panel, incluido el punto de conexión público del clúster y un vínculo a Service Fabric Explorer.

En la sección **Node Monitor** (Monitor de nodo) de la hoja del panel del clúster se indica el número de máquinas virtuales correctas e incorrectas. Puede encontrar más detalles sobre el estado del clúster en [Introducción a la supervisión del mantenimiento de Service Fabric][service-fabric-health-introduction].

> [!NOTE]
> Los clústeres de Service Fabric requieren que un cierto número de nodos estén activos en todo momento con el fin de mantener la disponibilidad y conservar el estado (esto se conoce como "mantenimiento del cuórum"). Por lo tanto, normalmente no es seguro apagar todas las máquinas del clúster a menos que antes haya realizado una [copia de seguridad completa del estado][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Conexión remota a una instancia de conjunto de escalado de máquinas virtuales (VMSS) o a un nodo del clúster
Cada uno de los tipos de nodos que especifica en el clúster da lugar a una configuración del conjunto de escalado de máquinas virtuales. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Pasos siguientes
En este punto, tiene un clúster seguro mediante certificados para la autenticación de administración. Después, [conéctese al clúster](service-fabric-connect-to-secure-cluster.md) y aprenda a [administrar secretos de aplicación](service-fabric-application-secret-management.md).  Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)

<!-- Links -->
[azure-powershell]: /powershell/azure/
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/general/overview.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-cluster-durability]: service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[BasicSecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/BasicSecurityConfigs.PNG
[CreateKeyVault]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault.PNG
[CreateKeyVault2]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault2.PNG
[CreateKeyVault3]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault3.PNG
[CreateKeyVault4]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault4.PNG
[CertInfo0]: ./media/service-fabric-cluster-creation-via-portal/CertInfo0.PNG
[CertInfo1]: ./media/service-fabric-cluster-creation-via-portal/CertInfo1.PNG
[CertInfo2]: ./media/service-fabric-cluster-creation-via-portal/CertInfo2.PNG
[SecurityCustomOption]: ./media/service-fabric-cluster-creation-via-portal/SecurityCustomOption.PNG
[DownloadCert]: ./media/service-fabric-cluster-creation-via-portal/DownloadCert.PNG
[Summary]: ./media/service-fabric-cluster-creation-via-portal/Summary.PNG
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
