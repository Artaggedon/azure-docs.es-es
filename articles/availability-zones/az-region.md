---
title: Servicios de Azure compatibles con Availability Zones
description: Para crear aplicaciones altamente disponibles y resistentes en Azure, las zonas de disponibilidad proporcionan ubicaciones físicamente separadas que puede utilizar para ejecutar sus recursos.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 12/17/2020
ms.author: cynthn
ms.custom: fasttrack-edit, mvc, references_regions
ms.openlocfilehash: 2a2e4ac57eec866d9857f564d6c76ad4a775d223
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98874615"
---
# <a name="azure-services-that-support-availability-zones"></a>Servicios de Azure compatibles con Availability Zones

La infraestructura global de Microsoft Azure está diseñada y construida en cada nivel para ofrecer los niveles más elevados de redundancia y resistencia a sus clientes. La infraestructura de Azure se compone de zonas geográficas, regiones y zonas de disponibilidad, que limitan el radio de impacto de un error y, por tanto, limitan el efecto potencial en los datos y las aplicaciones de los clientes. La construcción de Azure Availability Zones se desarrolló para proporcionar una solución de software y de red para protegerse frente a errores en centros de datos y proporcionar una alta disponibilidad (HA) superior a nuestros clientes.

Las zonas de disponibilidad son ubicaciones físicas exclusivas dentro de una región de Azure. Cada zona de disponibilidad consta de uno o varios centros de datos con alimentación, refrigeración y redes independientes. La separación física de las zonas de disponibilidad dentro de una región limita el impacto en las aplicaciones y los datos de los errores de zona, como desbordamientos a gran escala, grandes tormentas y supertormentas, y otros eventos que pueden interrumpir el acceso al sitio, el paso seguro, el tiempo de actividad de las utilidades extendidas y la disponibilidad de los recursos. Las zonas de disponibilidad y sus centros de datos asociados están diseñados de forma que si una zona se ve comprometida, otras zonas de disponibilidad en la región asumen los servicios, la capacidad y la disponibilidad.

Todos los servicios de administración de Azure están diseñados para ser resistentes ante errores en el nivel de región. Dentro del espectro de errores, uno o varios errores de zona de disponibilidad en una región tienen un radio de error menor en comparación con un error de una región completa. Azure puede recuperarse de un error en un nivel de zona de los servicios de administración dentro de una región. Azure realiza el mantenimiento crítico zona por zona en una región, para evitar que los errores afecten a los recursos del cliente implementados en las zonas de disponibilidad de una región.


![vista conceptual de una región de Azure con 3 zonas](./media/az-region/azure-region-availability-zones.png)


Los servicios de Azure que admiten Availability Zones se dividen en tres categorías: **zonal** y **redundancia de zona** y **no regional**. Las cargas de trabajo de los clientes se pueden categorizar para utilizar cualquiera de estos escenarios de arquitectura con el fin de satisfacer los requisitos de durabilidad y rendimiento de las aplicaciones.

- Con la arquitectura **zonal**, se puede implementar un recurso en una zona de disponibilidad específica y seleccionada automáticamente para lograr unos requisitos de rendimiento o latencia más rigurosos.  La resistencia se ha diseñado automáticamente mediante la replicación de aplicaciones y datos en una o más zonas dentro de la región.  Los recursos se pueden anclar a una zona específica. Puede anclar recursos (por ejemplo, máquinas virtuales, discos administrados o direcciones IP estándar) a una zona específica, lo que permite una mayor resistencia al tener una o más instancias de recursos distribuidas entre zonas.

- Con la arquitectura de **redundancia de zona**, la plataforma Azure replica el recurso y los datos entre zonas.  Microsoft administra la entrega de alta disponibilidad, ya que Azure replica y distribuye automáticamente las instancias dentro de la región.  Por ejemplo, ZRS replica los datos en tres zonas, de modo que un error de zona no afecte a la alta disponibilidad de los datos. 

- **Servicios no regionales**: los servicios siempre están disponibles en las ubicaciones geográficas de Azure y son resistentes a las interrupciones de toda la zona, así como a las de toda la región. 


Para lograr una continuidad del negocio integral en Azure, cree la arquitectura de aplicación mediante la combinación de zonas de disponibilidad y pares de regiones de Azure. Puede replicar de forma sincrónica las aplicaciones y los datos mediante zonas de disponibilidad dentro de una región de Azure para conseguir alta disponibilidad, y replicar de forma asincrónica entre regiones de Azure para la protección de recuperación ante desastres. Para obtener más información, consulte [Compilación de soluciones para alta disponibilidad mediante Availability Zones](/azure/architecture/high-availability/building-solutions-for-high-availability). 

## <a name="azure-services-supporting-availability-zones"></a>Servicios de Azure compatibles con Availability Zones

 - No se muestran las máquinas virtuales de las generaciones anteriores. Para más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../virtual-machines/sizes-previous-gen.md).
 - Tal como se menciona en [Regiones y zonas de disponibilidad en Azure](az-overview.md), algunos servicios no son regionales. Estos servicios no dependen de una región específica de Azure, por lo que son resistentes a las interrupciones en toda la zona, así como a interrupciones en toda la región.  La lista de servicios no regionales puede consultarse en [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/).


## <a name="azure-regions-with-availability-zones"></a>Regiones de Azure con Availability Zones


| América           | Europa         | Alemania              | África              | Asia Pacífico   |
|--------------------|----------------|----------------------|---------------------|----------------|
|                    |                |                      |                     |                |
| Centro de Canadá     | Centro de Francia | Centro-oeste de Alemania | Norte de Sudáfrica* | Japón Oriental     |
| Centro de EE. UU.         | Norte de Europa   |                      |                     | Sudeste de Asia |
| Este de EE. UU.            | Sur de Reino Unido       |                      |                     | Este de Australia |
| Este de EE. UU. 2          | Oeste de Europa    |                      |                     |                |
| Centro y Sur de EE. UU. |                |                      |                     |                |
| US Gov Virginia* |                |                      |                     |                |
| Oeste de EE. UU. 2        |                |                      |                     |                |


Para obtener más información sobre la compatibilidad de Availability Zones y los servicios disponibles en estas regiones, póngase en contacto con el representante de ventas o de clientes de Microsoft. Para las próximas regiones que van a admitir Availability Zones, consulte [Zonas geográficas de Azure](https://azure.microsoft.com/en-us/global-infrastructure/geographies/).


## <a name="azure-services-supporting-availability-zones"></a>Servicios de Azure compatibles con Availability Zones

- No se muestran las máquinas virtuales de las generaciones mostradas a continuación. Para obtener más información, consulte [Generaciones anteriores de tamaños de máquina virtual](../virtual-machines/sizes-previous-gen.md).

- Algunos servicios no son regionales; para obtener más información, consulte [Regiones y zonas de disponibilidad en Azure](az-overview.md). Estos servicios no dependen de una región específica de Azure, lo que los hace más resistentes a las interrupciones en toda la zona, así como a interrupciones en toda la región.  La lista de servicios no regionales puede consultarse en [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/).


### <a name="zone-resilient-services"></a>Servicios resistentes de zona 

:globe_with_meridians: Servicios no regionales: los servicios siempre están disponibles en las ubicaciones geográficas de Azure y son resistentes a las interrupciones de toda la zona, así como a las de toda la región.

:large_blue_diamond:   Resistente a las interrupciones en toda la zona 

**Servicios fundamentales**

|     Productos                                                    | Resistencia             |
|-----------------------------------------------------------------|:----------------------------:|
|     Cuenta de almacenamiento                                           | :large_blue_diamond:  |
|     Application Gateway (V2)                                  | :large_blue_diamond:  |
|     Azure Backup                                                | :large_blue_diamond:  |
|     Azure Cosmos DB                                           | :large_blue_diamond:  |
|     Azure Data Lake Storage Gen 2                             | :large_blue_diamond:  |
|     Azure Express Route                                       | :large_blue_diamond:  |
|     IP pública de Azure                                           | :large_blue_diamond:  |
|     Azure SQL Database (nivel de uso general)                 | :large_blue_diamond:  |
|     Azure SQL Database (niveles Prémium y Crítico para la empresa)     | :large_blue_diamond:  |
|     Disk Storage                                                | :large_blue_diamond:  |
|     Event Hubs                                                  | :large_blue_diamond:  |
|     Key Vault                                                   | :large_blue_diamond:  |
|     Load Balancer                                               | :large_blue_diamond:  |
|     Azure Service Bus                                                 | :large_blue_diamond:  |
|     Service Fabric                                            | :large_blue_diamond:  |
|     Almacenamiento:   Niveles de Blob Storage de acceso frecuente y de acceso esporádico                      | :large_blue_diamond:  |
|     Almacenamiento:   Managed Disks                                    | :large_blue_diamond:  |
|     Conjuntos de escalado de máquinas virtuales                               | :large_blue_diamond:  |
|     Virtual Machines                                          | :large_blue_diamond:  |
|     Virtual Machines: Serie Av2                              | :large_blue_diamond:  |
|     Virtual Machines: Serie Bs                               | :large_blue_diamond:  |
|     Virtual Machines: Serie DSv2                             | :large_blue_diamond:  |
|     Virtual Machines: Serie DSv3                             | :large_blue_diamond:  |
|     Virtual Machines: Serie Dv2                              | :large_blue_diamond:  |
|     Virtual Machines: Serie Dv3                              | :large_blue_diamond:  |
|     Virtual Machines: Serie ESv3                             | :large_blue_diamond:  |
|     Virtual Machines: Serie Ev3                              | :large_blue_diamond:  |
|     Virtual Network                                           | :large_blue_diamond:  |
|     VPN Gateway                                                 | :large_blue_diamond:  |


**Servicio estándar**

| Productos                                        | Resistencia |
|-------------------------------------------------|:------------:|
| Entornos de App Service                        |      :large_blue_diamond:  |
| Azure Active Directory Domain Services          |      :large_blue_diamond:  |
| Azure Bastion                                   |      :large_blue_diamond:  |
| Azure Cache for Redis                           |      :large_blue_diamond:  |
| Azure Cognitive Services: Text Analytics        |      :large_blue_diamond:  |
| Explorador de datos de Azure                             |      :large_blue_diamond:  |
| Azure Database for MySQL: servidor flexible      |      :large_blue_diamond:  |
| Azure Database for PostgreSQL: servidor flexible |      :large_blue_diamond:  |
| Azure DDoS Protection                           |      :large_blue_diamond:  |
| Azure Firewall                                  |      :large_blue_diamond:  |
| Azure Firewall Manager                          |      :large_blue_diamond:  |
| Azure Kubernetes Service (AKS)                  |      :large_blue_diamond:  |
| Azure Private Link                              |      :large_blue_diamond:  |
| Red Hat OpenShift en Azure                         |      :large_blue_diamond:  |
| Azure Site Recovery                             |      :large_blue_diamond:  |
| Container Registry                              |      :large_blue_diamond:  |
| Event Grid                                      |      :large_blue_diamond:  |
| Network Watcher                                 |      :large_blue_diamond:  |
| Power BI Embedded                               |      :large_blue_diamond:  |
| Premium Blob Storage                            |      :large_blue_diamond:  |
| Virtual Machines: Serie Ddsv4                  |      :large_blue_diamond:  |
| Virtual Machines: Serie Ddv4                   |      :large_blue_diamond:  |
| Virtual Machines: Serie Dsv4                   |      :large_blue_diamond:  |
| Virtual Machines: Serie Dv4                    |      :large_blue_diamond:  |
| Virtual Machines: Serie Edsv4                  |      :large_blue_diamond:  |
| Virtual Machines: Serie Edv4                   |      :large_blue_diamond:  |
| Virtual Machines: Serie Esv4                   |      :large_blue_diamond:  |
| Virtual Machines: Serie Ev4                    |      :large_blue_diamond:  |
| Virtual Machines: Serie Fsv2                   |      :large_blue_diamond:  |
| Virtual Machines: Serie M                      |      :large_blue_diamond:  |
| Virtual WAN                                     |      :large_blue_diamond:  |


**No regional**

|     Productos                                  |     Resistencia    |
|-----------------------------------------------|:-------------------:|
|     Azure DNS                                 |     :globe_with_meridians:             |
|     Azure Active Directory                  |     :globe_with_meridians:             |
|     Azure Advisor                             |     :globe_with_meridians:             |
|     Azure Bot Services                        |     :globe_with_meridians:             |
|     Azure Defender para IoT                  |     :globe_with_meridians:             |
|     Azure Information Protection            |     :globe_with_meridians:             |
|     Azure Lighthouse                        |     :globe_with_meridians:             |
|     Azure Managed Applications              |     :globe_with_meridians:             |
|     Azure Maps                                |     :globe_with_meridians:             |
|     Azure Policy                              |     :globe_with_meridians:             |
|     Azure Resource Graph                    |     :globe_with_meridians:             |
|     Azure Stack                               |     :globe_with_meridians:             |
|     Azure Stack Edge                        |     :globe_with_meridians:             |
|     Cloud Shell                               |     :globe_with_meridians:             |
|     Caja de seguridad del cliente de Microsoft Azure    |     :globe_with_meridians:             |
|     Microsoft Azure Peering Service         |     :globe_with_meridians:             |
|     Microsoft Azure Portal                  |     :globe_with_meridians:             |
|     Security Center                         |     :globe_with_meridians:             |
|     Traffic Manager                         |     :globe_with_meridians:             |


## <a name="pricing-for-vms-in-availability-zones"></a>Precios de máquinas virtuales en Availability Zones

No hay ningún costo adicional asociado a las máquinas virtuales implementadas en una zona de disponibilidad. Para obtener más información, revise la página [Precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/).


## <a name="get-started-with-availability-zones"></a>Introducción a las zonas de disponibilidad

- [Creación de una máquina virtual](../virtual-machines/windows/create-portal-availability-zone.md)
- [Agregación de un disco administrado mediante PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Creación de un conjunto de escalado de máquinas virtuales con redundancia de zona](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Equilibrio de carga de máquinas virtuales en distintas zonas con un equilibrador de carga estándar con un front-end con redundancia de zona](../load-balancer/quickstart-load-balancer-standard-public-cli.md?tabs=option-1-create-load-balancer-standard)
- [Equilibrio de carga de máquinas virtuales dentro de una zona con un equilibrador de carga estándar con un front-end de zona](../load-balancer/quickstart-load-balancer-standard-public-cli.md?tabs=option-1-create-load-balancer-standard)
- [Almacenamiento con redundancia de zona](../storage/common/storage-redundancy.md)
- [Nivel de uso general de SQL Database](../azure-sql/database/high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)
- [Recuperación ante desastres geográfica de Event Hubs](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Recuperación ante desastres geográfica de Service Bus](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Crear una puerta de enlace de red virtual con redundancia de zona](../vpn-gateway/create-zone-redundant-vnet-gateway.md)
- [Adición de una región con redundancia de zona para Azure Cosmos DB](../cosmos-db/high-availability.md#availability-zone-support)
- [Introducción a las zonas de disponibilidad de Azure Cache for Redis](https://gist.github.com/JonCole/92c669ea482bbb7996f6428fb6c3eb97#file-redisazgettingstarted-md)
- [Creación de una instancia de Azure Active Directory Domain Services](../active-directory-domain-services/tutorial-create-instance.md)
- [Creación de un clúster de Azure Kubernetes Service (AKS) que use zonas de disponibilidad](../aks/availability-zones.md)


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Regiones y Availability Zones en Azure](az-overview.md)