---
title: Archivo de novedades de Azure Security Center
description: Una descripción de las novedades y los cambios en Azure Security Center desde hace seis meses y versiones anteriores.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2020
ms.author: memildin
ms.openlocfilehash: 835950cc68ca22ea3958d614d669d7a105fefe62
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98132926"
---
# <a name="archive-for-whats-new-in-azure-security-center"></a>Archivo de novedades de Azure Security Center

La página principal de notas de la versión de [novedades de Azure Security Center](release-notes.md) contiene actualizaciones de los últimos seis meses, mientras que esta página contiene elementos más antiguos.

En este página se proporciona información acerca de lo siguiente:

- Nuevas características
- Corrección de errores
- Funciones obsoletas


## <a name="july-2020"></a>Julio de 2020

Las actualizaciones de julio incluyen:
- [Evaluación de vulnerabilidades de las máquinas virtuales disponible ahora para las imágenes que no son de Marketplace](#vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images)
- [Protección contra amenazas para Azure Storage expandido con el fin de incluir Azure Files y Azure Data Lake Storage Gen2 (versión preliminar)](#threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview)
- [Ocho nuevas recomendaciones para habilitar las características de protección contra amenazas](#eight-new-recommendations-to-enable-threat-protection-features)
- [Mejoras en la seguridad de los contenedores: análisis del registro más rápido y documentación actualizada](#container-security-improvements---faster-registry-scanning-and-refreshed-documentation)
- [Controles de aplicaciones adaptables actualizados con una nueva recomendación y compatibilidad para caracteres comodín en reglas de ruta de acceso](#adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules)
- [Novedades de Azure Security Center](#six-policies-for-sql-advanced-data-security-deprecated)




### <a name="vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images"></a>Evaluación de vulnerabilidades de las máquinas virtuales disponible ahora para las imágenes que no son de Marketplace

Anteriormente, al implementar una solución de evaluación de vulnerabilidades, Security Center realizaba una comprobación de validación antes de la implementación. La comprobación se realizaba para confirmar la SKU de Marketplace de la máquina virtual de destino. 

A partir de esta actualización, se ha eliminado esta comprobación y así ahora se pueden implementar herramientas de evaluación de vulnerabilidades en máquinas Windows y Linux personalizadas. Las imágenes personalizadas son aquellas que se han modificado a partir de los valores predeterminados de Marketplace.

Aunque ahora puede implementar la extensión de evaluación de vulnerabilidades integrada (con tecnología de Qualys) en muchas más máquinas, la compatibilidad solo está disponible si usa uno de los sistemas operativos que se indica en [Implementación del detector de vulnerabilidades integrado en VM de nivel estándar](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

Más información sobre el [Detector de vulnerabilidades integrado para máquinas virtuales (requiere Azure Defender)](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner)

Más información sobre el uso de su propia solución de evaluación de vulnerabilidades con licencia privada de Qualys o Rapid7 en [Implementación de una solución de examen de vulnerabilidades de asociados](deploy-vulnerability-assessment-vm.md)


### <a name="threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview"></a>Protección contra amenazas para Azure Storage expandido con el fin de incluir Azure Files y Azure Data Lake Storage Gen2 (versión preliminar)

La protección contra amenazas para Azure Storage detecta actividades potencialmente dañinas en las cuentas de Azure Storage. Security Center muestra alertas cuando detecta intentos de acceder a las cuentas de almacenamiento o de aprovecharlas. 

Los datos se pueden proteger tanto si se almacenan como contenedores de blobs, recursos compartidos de archivos o lagos de datos.




### <a name="eight-new-recommendations-to-enable-threat-protection-features"></a>Ocho nuevas recomendaciones para habilitar las características de protección contra amenazas

Se han agregado ocho nuevas recomendaciones para proporcionar una manera sencilla de habilitar las características de protección contra amenazas de Azure Security Center para los siguientes tipos de recursos: máquinas virtuales, planes de App Service, servidores de Azure SQL Database, servidores SQL Server en máquinas, cuentas de Azure Storage, clústeres de Azure Kubernetes Service, registros de Azure Container Registry y almacenes de Azure Key Vault.

Las nuevas recomendaciones son:

- **Advanced Data Security debe estar habilitado en los servidores de Azure SQL Database**
- **Advanced Data Security debe estar habilitado en los servidores SQL Server en las máquinas**
- **Advanced Threat Protection debe estar habilitado en los planes de Azure App Service**
- **Advanced Threat Protection debe estar habilitado en los registros de Azure Container Registry**
- **Advanced Threat Protection debe estar habilitado en los almacenes de Azure Key Vault**
- **Advanced Threat Protection debe estar habilitado en los clústeres de Azure Kubernetes Service**
- **Advanced Threat Protection debe estar habilitado en las cuentas de almacenamiento de Azure**
- **Advanced Threat Protection debe estar habilitado en Virtual Machines**

Estas nuevas recomendaciones pertenecen al control de seguridad **Habilitar Azure Defender**.

Las recomendaciones también incluyen la funcionalidad de corrección rápida. 

> [!IMPORTANT]
> La corrección de cualquiera de estas recomendaciones dará lugar a cargos por la protección de los recursos pertinentes. Estos cargos se iniciarán de forma inmediata si tiene recursos relacionados en la suscripción actual. O en el futuro, si los agrega en una fecha posterior.
> 
> Por ejemplo, si no tiene ningún clúster de Azure Kubernetes Service en su suscripción y habilita la protección contra amenazas, no se aplicarán cargos. Si, en el futuro, agrega un clúster a la misma suscripción, este se protegerá automáticamente y a partir de ese momento, se iniciarán los cargos.

Más información sobre cada una de estas recomendaciones en la [página de referencia de las recomendaciones de seguridad](recommendations-reference.md).

Más información sobre la [protección contra amenazas en Azure Security Center](azure-defender.md).




### <a name="container-security-improvements---faster-registry-scanning-and-refreshed-documentation"></a>Mejoras en la seguridad de los contenedores: análisis del registro más rápido y documentación actualizada

Como parte de las inversiones continuas en el dominio de la seguridad de los contenedores, nos complace compartir una mejora significativa del rendimiento en los análisis dinámicos de las imágenes de contenedor de Security Center que se almacenan en Azure Container Registry. Los exámenes se completan ahora en aproximadamente dos minutos. En algunos casos, pueden tardar hasta 15 minutos.

Para mejorar la claridad y la orientación con respecto a las funcionalidades de seguridad de los contenedores de Azure Security Center, también hemos actualizado las páginas de documentación sobre la seguridad de los contenedores. 

Más información sobre la seguridad de los contenedores de Security Center en los artículos siguientes:

- [Información general sobre las características de seguridad de los contenedores de Azure Security Center](container-security.md)
- [Detalles de la integración con Azure Container Registry](defender-for-container-registries-introduction.md)
- [Detalles de la integración con Azure Kubernetes Service](defender-for-kubernetes-introduction.md)
- [Procedimientos para el examen de los registros y la protección de los hosts de Docker](container-security.md)
- [Alertas de seguridad de las características de protección contra amenazas para clústeres de Azure Kubernetes Service](alerts-reference.md#alerts-akscluster)
- [Alertas de seguridad de las características de protección contra amenazas para hosts de Azure Kubernetes Service](alerts-reference.md#alerts-containerhost)
- [Recomendaciones de seguridad para contenedores](recommendations-reference.md#recs-compute)



### <a name="adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules"></a>Controles de aplicaciones adaptables actualizados con una nueva recomendación y compatibilidad para caracteres comodín en reglas de ruta de acceso

La característica de controles de aplicaciones adaptables ha recibido dos actualizaciones importantes:

* Una nueva recomendación identifica un comportamiento potencialmente legítimo que no se ha permitido previamente. La nueva recomendación, **Se deben actualizar las reglas de la lista de permitidos de la directiva de controles de aplicaciones adaptables**, le pedirá que agregue nuevas reglas a la directiva existente para reducir el número de falsos positivos en las alertas de infracción de controles de aplicaciones adaptables.

* Las reglas de ruta de acceso ahora admiten caracteres comodín. A partir de esta actualización, puede configurar las reglas de ruta de acceso permitidas mediante caracteres comodín. Se admiten dos escenarios:

    * El uso de un carácter comodín al final de una ruta de acceso para permitir todos los ejecutables dentro de esta carpeta y subcarpetas

    * El uso de un carácter comodín en medio de una ruta de acceso para habilitar un nombre ejecutable conocido con un nombre de carpeta cambiante (por ejemplo, carpetas de usuario personales con un archivo ejecutable conocido, nombres de carpeta generados automáticamente, etc.).


[Más información acerca de los controles de aplicación adaptables](security-center-adaptive-application.md).



### <a name="six-policies-for-sql-advanced-data-security-deprecated"></a>Seis directivas para la seguridad avanzada de datos de SQL en desuso

Seis directivas relacionadas con la seguridad avanzada de datos para máquinas SQL están en desuso:

- Los tipos de Advanced Threat Protection deben definirse como "Todos" en la configuración de Advanced Data Security para SQL Managed Instance.
- Los tipos de Advanced Threat Protection deben definirse como "Todos" en la configuración de Advanced Data Security del servidor SQL Server.
- La configuración de seguridad de datos avanzada para una instancia administrada SQL debe contener una dirección de correo electrónico para recibir alertas de seguridad
- La configuración de seguridad de datos avanzada para SQL Server debe contener una dirección de correo electrónico para recibir alertas de seguridad
- Las notificaciones a los administradores y a los propietarios de la suscripción deben estar habilitadas en la configuración de seguridad avanzada de datos de la instancia administrada de SQL
- Las notificaciones por correo electrónico para administradores y propietarios de suscripciones deben estar habilitadas en la configuración de seguridad avanzada de datos de SQL Server

Obtenga más información sobre las [directivas integradas](./policy-reference.md).



## <a name="june-2020"></a>Junio de 2020

Las actualizaciones de junio incluyen:
- [API de puntuación segura (versión preliminar)](#secure-score-api-preview)
- [Advanced Data Security para máquinas SQL (Azure, otras nubes e instancias locales) (versión preliminar)](#advanced-data-security-for-sql-machines-azure-other-clouds-and-on-premises-preview)
- [Dos nuevas recomendaciones para implementar el agente de Log Analytics en máquinas de Azure Arc (versión preliminar)](#two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview)
- [Nuevas directivas para crear configuraciones de exportación continua y de automatización de flujos de trabajo a escala](#new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale)
- [Nueva recomendación para usar NSG para proteger las máquinas virtuales sin conexión a Internet](#new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines)
- [Nuevas directivas para habilitar la protección contra amenazas y Advanced Data Security](#new-policies-for-enabling-threat-protection-and-advanced-data-security)



### <a name="secure-score-api-preview"></a>API de puntuación segura (versión preliminar)

Ahora puede acceder a la puntuación a través de la [API de puntuación segura](/rest/api/securitycenter/securescores/) (actualmente en versión preliminar). Los métodos de API proporcionan la flexibilidad necesaria para consultar los datos y crear su propio mecanismo de creación de informes de las puntuaciones seguras a lo largo del tiempo. Por ejemplo, puede usar la API **Secure Scores** para obtener la puntuación de una suscripción específica. Además, puede usar la API **Secure Score Controls** para mostrar los controles de seguridad y la puntuación actual de las suscripciones.

Para ver ejemplos de herramientas externas que son posibles gracias a la API de puntuación segura, vea el [área de puntuación segura de la comunidad de GitHub](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score).

Obtenga más información sobre la [puntuación segura y los controles de seguridad en Azure Security Center](secure-score-security-controls.md).



### <a name="advanced-data-security-for-sql-machines-azure-other-clouds-and-on-premises-preview"></a>Advanced Data Security para máquinas SQL (Azure, otras nubes e instancias locales) (versión preliminar)

Ahora, Advanced Data Security de Azure Security Center para máquinas SQL protege los servidores SQL Server hospedados en Azure, en otros entornos en la nube e incluso en máquinas locales. Esto amplía las protecciones de los servidores SQL Server nativos de Azure, de modo que se admitan totalmente los entornos híbridos.

Advanced Data Security proporciona una evaluación de vulnerabilidades y protección contra amenazas avanzada para máquinas SQL dondequiera que se encuentren.

La configuración conlleva dos pasos:

1. Implementación del agente de Log Analytics en la máquina host de SQL Server para proporcionar la conexión a la cuenta de Azure.

1. Habilitación del conjunto de productos opcional en la página de precios y configuración de Security Center.

Obtenga más información sobre [Advanced Data Security para máquinas SQL](defender-for-sql-usage.md).



### <a name="two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview"></a>Dos nuevas recomendaciones para implementar el agente de Log Analytics en máquinas de Azure Arc (versión preliminar)

Se han agregado dos nuevas recomendaciones para ayudar a implementar el [agente de Log Analytics](../azure-monitor/platform/log-analytics-agent.md) en las máquinas de Azure Arc y asegurarse de que están protegidas por Azure Security Center:

- **El agente de Log Analytics debe estar instalado en las máquinas de Azure Arc basadas en Windows (versión preliminar)** .
- **El agente de Log Analytics debe estar instalado en las máquinas de Azure Arc basadas en Linux (versión preliminar)** .

Estas nuevas recomendaciones aparecerán en los mismos cuatro controles de seguridad que la recomendación existente (relacionada) **El agente de supervisión debe estar instalado en las máquinas**: corrija las configuraciones de seguridad, aplique el control de aplicaciones adaptativo, aplique actualizaciones del sistema y habilite Endpoint Protection.

Las recomendaciones también incluyen la función de corrección rápida para ayudar a acelerar el proceso de implementación. 

Obtenga más información sobre estas dos nuevas recomendaciones en la tabla [Recomendaciones de proceso y aplicación](recommendations-reference.md#recs-compute).

Obtenga más información sobre la manera en que Azure Security Center usa el agente en [¿Qué es el agente de Log Analytics?](faq-data-collection-agents.md#what-is-the-log-analytics-agent)

Obtenga más información sobre las [extensiones para máquinas de Azure Arc](../azure-arc/servers/manage-vm-extensions.md).


### <a name="new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale"></a>Nuevas directivas para crear configuraciones de exportación continua y de automatización de flujos de trabajo a escala

La automatización de los procesos de supervisión y respuesta ante incidentes de la organización puede mejorar considerablemente el tiempo necesario para investigar y mitigar los incidentes de seguridad.

Para implementar las configuraciones de automatización en la organización, use estas directivas de Azure "DeployIfdNotExist" integradas para crear y configurar los procedimientos de [exportación continua](continuous-export.md) y [automatización de flujos de trabajo](workflow-automation.md):

Las directivas se pueden encontrar en la directiva de Azure:


|Objetivo  |Directiva  |Id. de directiva  |
|---------|---------|---------|
|Exportación continua al centro de eventos|[Implementar la exportación al centro de eventos para las alertas y recomendaciones de Azure Security Center](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fcdfcce10-4578-4ecd-9703-530938e4abcb)|cdfcce10-4578-4ecd-9703-530938e4abcb|
|Exportación continua a las áreas de trabajo de Log Analytics|[Implementar la exportación al área de trabajo de Log Analytics para las alertas y recomendaciones de Azure Security Center](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fffb6f416-7bd2-4488-8828-56585fef2be9)|ffb6f416-7bd2-4488-8828-56585fef2be9|
|Automatización de flujos de trabajo para alertas de seguridad|[Implementar la automatización del flujo de trabajo para las alertas de Azure Security Center](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2ff1525828-9a90-4fcf-be48-268cdd02361e)|f1525828-9a90-4fcf-be48-268cdd02361e|
|Automatización de flujos de trabajo para recomendaciones de seguridad|[Implementar la automatización del área de trabajo para las recomendaciones de Azure Security Center](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-2475-4850-afd6-43795f3492ef)|73d6ab6c-2475-4850-afd6-43795f3492ef|
||||

Empiece a usar las [plantillas de automatización de flujos de trabajo](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation).

Para más información sobre el uso de las dos directivas de exportación, consulte las secciones sobre la [configuración de la automatización de flujos de trabajo a escala mediante las directivas suministradas](workflow-automation.md#configure-workflow-automation-at-scale-using-the-supplied-policies) y sobre la [configuración de una exportación continua](continuous-export.md#set-up-a-continuous-export).


### <a name="new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines"></a>Nueva recomendación para usar NSG para proteger las máquinas virtuales sin conexión a Internet

El control de seguridad "implementar prácticas recomendadas de seguridad" incluye ahora la siguiente recomendación nueva:

- **Las máquinas virtuales sin conexión a Internet deben protegerse con grupos de seguridad de red**

Una recomendación existente (**Las máquinas virtuales sin conexión a Internet deben protegerse con grupos de seguridad de red**) no distinguía entre las máquinas virtuales con y sin conexión a Internet. En ambos casos, se generaba una recomendación de alta gravedad si una máquina virtual no estaba asignada a un grupo de seguridad de red. Esta nueva recomendación separa las máquinas sin conexión a Internet para reducir los falsos positivos y evitar alertas innecesarias de alta gravedad.

Obtenga más información en la tabla [Recomendaciones de red](recommendations-reference.md#recs-networking).




### <a name="new-policies-for-enabling-threat-protection-and-advanced-data-security"></a>Nuevas directivas para habilitar la protección contra amenazas y Advanced Data Security

Las nuevas directivas que se indican a continuación se agregaron a la iniciativa predeterminada de ASC y están diseñadas para ayudar a habilitar la protección contra amenazas o Advanced Data Security para los tipos de recursos pertinentes.

Las directivas se pueden encontrar en la directiva de Azure:


| Directiva                                                                                                                                                                                                                                                                | Id. de directiva                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| [Advanced Data Security debe estar habilitado en los servidores de Azure SQL Database](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f7fe3b40f-802b-4cdd-8bd4-fd799c948cc2)     | 7fe3b40f-802b-4cdd-8bd4-fd799c948cc2 |
| [Advanced Data Security debe estar habilitado en los servidores SQL Server en las máquinas](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6581d072-105e-4418-827f-bd446d56421b) | 6581d072-105e-4418-827f-bd446d56421b |
| [Advanced Threat Protection debe estar habilitado en las cuentas de almacenamiento de Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f308fbb08-4ab8-4e67-9b29-592e93fb94fa)           | 308fbb08-4ab8-4e67-9b29-592e93fb94fa |
| [Advanced Threat Protection debe estar habilitado en los almacenes de Azure Key Vault](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f0e6763cc-5078-4e64-889d-ff4d9a839047)           | 0e6763cc-5078-4e64-889d-ff4d9a839047 |
| [Advanced Threat Protection debe estar habilitado en los planes de Azure App Service](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f2913021d-f2fd-4f3d-b958-22354e2bdbcb)                | 2913021d-f2fd-4f3d-b958-22354e2bdbcb |
| [Advanced Threat Protection debe estar habilitado en los registros de Azure Container Registry](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fc25d9a16-bc35-4e15-a7e5-9db606bf9ed4)   | c25d9a16-bc35-4e15-a7e5-9db606bf9ed4 |
| [Advanced Threat Protection debe estar habilitado en los clústeres de Azure Kubernetes Service](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f523b5cd1-3e23-492f-a539-13118b6d1e3a)   | 523b5cd1-3e23-492f-a539-13118b6d1e3a |
| [Advanced Threat Protection debe estar habilitado en Virtual Machines](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f4da35fc9-c9e7-4960-aec9-797fe7d9051d)           | 4da35fc9-c9e7-4960-aec9-797fe7d9051d |
|                                                                                                                                                                                                                                                                       |                                      |

Obtenga más información sobre la [protección contra amenazas en Azure Security Center](azure-defender.md).


## <a name="may-2020"></a>Mayo de 2020

Las actualizaciones de mayo incluyen:
- [Reglas de supresión de alertas (versión preliminar)](#alert-suppression-rules-preview)
- [La evaluación de vulnerabilidades de la máquina virtual ya está disponible con carácter general](#virtual-machine-vulnerability-assessment-is-now-generally-available)
- [Cambios en el acceso a la máquina virtual (VM) Just-in-Time (JIT)](#changes-to-just-in-time-jit-virtual-machine-vm-access)
- [Las recomendaciones personalizadas se han migrado a un control de seguridad independiente](#custom-recommendations-have-been-moved-to-a-separate-security-control)
- [Alternancia agregada para ver las recomendaciones en controles o como una lista plana](#toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list)
- [Control de seguridad expandido "implementar prácticas recomendadas de seguridad"](#expanded-security-control-implement-security-best-practices)
- [Las directivas personalizadas con metadatos personalizados ya están disponibles con carácter general](#custom-policies-with-custom-metadata-are-now-generally-available)
- [Migración de funcionalidades de análisis de volcado de memoria a detección de ataques sin archivos](#crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection)


### <a name="alert-suppression-rules-preview"></a>Reglas de supresión de alertas (versión preliminar)

Esta nueva característica (actualmente en versión preliminar) ayuda a reducir la fatiga de la alerta. Use reglas para ocultar automáticamente las alertas que se sabe que son inocuas o relacionadas con las actividades normales de su organización. Esto le permite centrarse en las amenazas más importantes. 

Todavía se generarán alertas que coincidan con las reglas de supresión habilitadas, pero su estado se establecerá en descartado. Puede ver el estado en el Azure Portal o tener acceso a las alertas de seguridad de Security Center.

Las reglas de supresión definen los criterios para los que se deben descartar automáticamente las alertas. Normalmente, se usaría una regla de supresión para:

- Eliminar las alertas identificadas como falsos positivos

- suprimir las alertas que se desencadenan con demasiada frecuencia para ser útiles

Más información sobre la [supresión de alertas de protección contra amenazas de Azure Security Center](alerts-suppression-rules.md).


### <a name="virtual-machine-vulnerability-assessment-is-now-generally-available"></a>La evaluación de vulnerabilidades de la máquina virtual ya está disponible con carácter general

El nivel estándar de Security Center ahora incluye una evaluación de vulnerabilidades integrada para máquinas virtuales sin precio adicional. Esta extensión está basada en Qualys pero notifica sus hallazgos directamente de nuevo a Security Center. No se necesita ninguna licencia ni cuenta de Qualys, ya que todo se administra sin problemas en Security Center.

La nueva solución puede examinar continuamente las máquinas virtuales para encontrar vulnerabilidades y presentar las conclusiones en Security Center. 

Para implementar la solución, use la nueva recomendación de seguridad:

"Habilitar la solución de evaluación de vulnerabilidades integrada en las máquinas virtuales (con tecnología de Qualys)"

Más información sobre la [evaluación de vulnerabilidades integradas de Security Center para máquinas virtuales](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner).



### <a name="changes-to-just-in-time-jit-virtual-machine-vm-access"></a>Cambios en el acceso a la máquina virtual (VM) Just-in-Time (JIT)

Security Center incluye una característica opcional para proteger los puertos de administración de las máquinas virtuales. Esto proporciona una defensa contra la forma más común de ataques por fuerza bruta.

Esta actualización lleva a cabo los siguientes cambios en esta característica:

- Se ha cambiado el nombre de la recomendación que le aconseja habilitar JIT en una máquina virtual. Anteriormente, "el control de acceso a red Just-in-Time se debe aplicar a las máquinas virtuales" ahora es: "Los puertos de administración de las máquinas virtuales deben protegerse con el control de acceso de red Just-In-Time".

- La recomendación se desencadena solo si hay puertos de administración abiertos.

Más información acerca de [la característica de acceso JIT](security-center-just-in-time.md).


### <a name="custom-recommendations-have-been-moved-to-a-separate-security-control"></a>Las recomendaciones personalizadas se han migrado a un control de seguridad independiente

Uno de los controles de seguridad incluidos con la puntuación segura mejorada fue "Implementación de procedimientos recomendados de seguridad". Las recomendaciones personalizadas creadas para las suscripciones se colocan automáticamente en ese control. 

Para que sea más fácil encontrar las recomendaciones personalizadas, las hemos pasado a un control de seguridad dedicado, "recomendaciones personalizadas". Este control no afecta a la puntuación segura.

Más información sobre los controles de seguridad en [puntuación de seguridad mejorada (versión preliminar) de Azure Security Center](secure-score-security-controls.md).


### <a name="toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list"></a>Alternancia agregada para ver las recomendaciones en controles o como una lista plana

Los controles de seguridad son grupos lógicos de recomendaciones de seguridad relacionadas. Reflejan las superficies de ataque vulnerables. Un control es un conjunto de recomendaciones de seguridad con instrucciones que le ayudan a implementar esas recomendaciones.

Para ver de inmediato cómo protege su organización cada superficie de ataque individual, revise las puntuaciones de cada control de seguridad.

De forma predeterminada, las recomendaciones se muestran en los controles de seguridad. A partir de esta actualización también se pueden mostrar como una lista. Para verlos como una lista simple ordenada por el estado de mantenimiento de los recursos afectados, use el nuevo comando de alternancia "agrupar por controles". El comando de alternancia está encima de la lista en el portal.

Los controles de seguridad y esta alternancia forman parte de la nueva experiencia de puntuación segura. No se olvide de enviarnos sus comentarios desde dentro del portal.

Más información sobre los controles de seguridad en [puntuación de seguridad mejorada (versión preliminar) de Azure Security Center](secure-score-security-controls.md).

:::image type="content" source="./media/secure-score-security-controls/recommendations-group-by-toggle.gif" alt-text="Alternancia &quot;agrupar por controles&quot; para recomendaciones":::

### <a name="expanded-security-control-implement-security-best-practices"></a>Control de seguridad expandido "implementar prácticas recomendadas de seguridad" 

Uno de los controles de seguridad que incluye con la puntuación segura mejorada es "Implementación de procedimientos recomendados de seguridad". Cuando una recomendación está en este control, no afecta a la puntuación segura. 

Con esta actualización, se han sacado tres recomendaciones de los controles en los que se colocaron originalmente y en este control de prácticas recomendadas. Hemos realizado este paso porque hemos determinado que el riesgo de estas tres recomendaciones es menor de lo que se pensó inicialmente.

Además, se han incorporado dos nuevas recomendaciones y se han agregado a este control.

Las tres recomendaciones que se movieron son:

- **MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción** (originalmente en el control "habilitar MFA")
- **Las cuentas externas con permisos de lectura deben quitarse de la suscripción** (originalmente en el control "administrar acceso y permisos")
- **Se debe designar un máximo de 3 propietarios para la suscripción** (originalmente en el control "administrar acceso y permisos")

Las dos nuevas recomendaciones que se han agregado al control son:

- **La extensión de configuración de invitado debe instalarse en máquinas virtuales Windows (versión preliminar):** el uso de la [configuración de invitado de Azure Policy](../governance/policy/concepts/guest-configuration.md) proporciona visibilidad dentro de las máquinas virtuales a la configuración de servidor y de aplicación (solo Windows).

- **Protección contra vulnerabilidades de seguridad de Windows Defender debe estar habilitada en las máquinas (versión preliminar)** : Protección contra vulnerabilidades de seguridad de Windows Defender aprovecha el agente de configuración de invitado de Azure Policy. La protección contra vulnerabilidades de seguridad tiene cuatro componentes diseñados para bloquear dispositivos en una amplia variedad de vectores de ataque y comportamientos de bloque utilizados habitualmente en ataques de malware, al tiempo que permiten a las empresas equilibrar los requisitos de productividad y riesgo de seguridad (solo Windows).

Más información sobre la protección contra vulnerabilidades de seguridad de Windows Defender en [Crear e implementar una directiva de protección contra vulnerabilidades](/mem/configmgr/protect/deploy-use/create-deploy-exploit-guard-policy).

Obtenga más información sobre los controles de seguridad en [Puntuación de seguridad mejorada (versión preliminar)](secure-score-security-controls.md).



### <a name="custom-policies-with-custom-metadata-are-now-generally-available"></a>Las directivas personalizadas con metadatos personalizados ya están disponibles con carácter general

Las directivas personalizadas ahora forman parte de la experiencia de recomendaciones de Security Center, la puntuación segura y el panel de normas de cumplimiento normativo. Esta característica ya está disponible con carácter general y permite ampliar la cobertura de evaluación de seguridad de la organización en Security Center. 

Cree una iniciativa personalizada en Azure Policy, agréguele directivas e incorpore a Azure Security Center y visualice como recomendaciones.

Ahora también hemos agregado la opción para editar los metadatos de recomendación personalizados. Las opciones de metadatos incluyen gravedad, pasos de corrección, información de amenazas y mucho más.  

Más información sobre [mejora de las recomendaciones personalizadas con información detallada](custom-security-policies.md#enhance-your-custom-recommendations-with-detailed-information).



### <a name="crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection"></a>Migración de funcionalidades de análisis de volcado de memoria a detección de ataques sin archivos 

Estamos integrando las capacidades de detección del análisis de volcado de memoria de Windows (CDA) en [detección de ataques sin archivos](defender-for-servers-introduction.md#what-are-the-benefits-of-azure-defender-for-servers). El análisis de detección de ataques sin archivos no proporciona versiones mejoradas de las siguientes alertas de seguridad para máquinas Windows: Inyección de código detectada, se detectó un módulo de Windows enmascarado, shellcode detectado y segmento de código sospechoso.

Algunas de las ventajas de esta transición son:

- **Detección de malware proactiva y oportuna**: el enfoque de CDA implicaba esperar a que se produjera un bloqueo y, después, ejecutar el análisis para encontrar artefactos malintencionados. El uso de la detección de ataques sin archivos proporciona una identificación proactiva de las amenazas en memoria mientras se ejecutan. 

- **Alertas enriquecidas**: las alertas de seguridad de la detección de ataques sin archivos no están disponibles en CDA, como la información de las conexiones de red activas. 

- **Agregación de alertas**: cuando CDA detectó varios patrones de ataque en un solo volcado de memoria, desencadenó varias alertas de seguridad. Detección de ataques sin archivos combina todos los patrones de ataque identificados del mismo proceso en una única alerta, lo que elimina la necesidad de poner en correlación varias alertas.

- **Requisitos reducidos en el área de trabajo de Log Analytics**: los volcados que contienen datos potencialmente confidenciales ya no se cargarán en el área de trabajo de Log Analytics.






## <a name="april-2020"></a>Abril de 2020

Las actualizaciones de abril incluyen:
- [Los paquetes de cumplimiento dinámico ya están disponibles con carácter general](#dynamic-compliance-packages-are-now-generally-available)
- [Las recomendaciones de identidad ahora están incluidas en el nivel gratuito de Azure Security Center](#identity-recommendations-now-included-in-azure-security-center-free-tier)


### <a name="dynamic-compliance-packages-are-now-generally-available"></a>Los paquetes de cumplimiento dinámico ya están disponibles con carácter general

El panel de cumplimiento normativo de Azure Security Center ahora incluye **paquetes de cumplimiento dinámico** (ya disponible con carácter general) para realizar un seguimiento de los estándares de la industria y normativas adicionales.

Los paquetes de cumplimiento dinámico se pueden agregar a su suscripción o grupo de administración desde la página de la directiva de seguridad de Security Center. Cuando haya incorporado un estándar o una prueba comparativa, el estándar aparece en el panel de cumplimiento normativo con todos los datos de cumplimiento asociados asignados como evaluaciones. Podrá descargar un informe de resumen para cualquiera de los estándares que se hayan incorporado.

Ahora, puede agregar estándares como:

- **NIST SP 800-53 R4**
- **SWIFT CSP CSCF-v2020**
- **UK Official y UK NHS**
- **Canada Federal PBMM**
- **Azure CIS 1.1.0 (nuevo)** (que es una representación más completa de Azure CIS 1.1.0)

Además, recientemente hemos agregado el **Azure Security Benchmark**, las instrucciones específicas de Azure creadas por Microsoft para las prácticas recomendadas de seguridad y cumplimiento basadas en los marcos de cumplimiento comunes. Se admitirán estándares adicionales en el panel a medida que estén disponibles.  
 
Más información sobre [cómo personalizar el conjunto de estándares en el panel de cumplimiento normativo](update-regulatory-compliance-packages.md).


### <a name="identity-recommendations-now-included-in-azure-security-center-free-tier"></a>Recomendaciones de identidad ahora incluidas en el nivel gratuito de Azure Security Center

Las recomendaciones de seguridad para la identidad y el acceso en el nivel gratuito de Azure Security Center ya están disponibles con carácter general. Esto forma parte del esfuerzo de hacer que las características de la administración de posturas de seguridad en la nube (CSPM) sean gratuitas. Hasta ahora, estas recomendaciones solo estaban disponibles en el plan de tarifa estándar.

Los ejemplos de recomendaciones de identidad y acceso incluyen:

- "La autenticación multifactor debe estar habilitada en las cuentas con permisos de propietario en la suscripción".
- "Debe designar un máximo de tres propietarios para la suscripción".
- "Las cuentas en desuso deben quitarse de la suscripción".

Si tiene suscripciones en el plan de tarifa gratuito, su puntuación de seguridad se verá afectada por este cambio ya que nunca se evaluó su identidad y seguridad de acceso.

Más información sobre [recomendaciones de identidad y acceso](recommendations-reference.md#recs-identityandaccess).

Más información acerca de la [supervisión de la identidad y el acceso](security-center-identity-access.md).



## <a name="march-2020"></a>Marzo de 2020

Las actualizaciones de marzo incluyen:

- [La automatización del flujo de trabajo ya está disponible con carácter general](#workflow-automation-is-now-generally-available)
- [Integración de Azure Security Center con Windows Admin Center](#integration-of-azure-security-center-with-windows-admin-center)
- [Protección para Azure Kubernetes Service](#protection-for-azure-kubernetes-service)
- [Experiencia Just-in-Time mejorada](#improved-just-in-time-experience)
- [Dos recomendaciones de seguridad para las aplicaciones web en desuso](#two-security-recommendations-for-web-applications-deprecated)


### <a name="workflow-automation-is-now-generally-available"></a>La automatización del flujo de trabajo ya está disponible con carácter general

La característica de automatización del flujo de trabajo de Azure Security Center ahora está disponible con carácter general. Úsela para desencadenar automáticamente una instancia de Logic Apps sobre alertas y recomendaciones de seguridad. Además, los desencadenadores manuales están disponibles para las alertas y todas las recomendaciones que tienen la opción corrección rápida disponible.

Cada programa de seguridad incluye varios flujos de trabajo para la respuesta a incidentes. Estos procesos pueden incluir notificaciones para las partes interesadas competentes, iniciar un proceso de administración de cambios y aplicar pasos de corrección específicos. Los expertos en seguridad recomiendan automatizar tantos pasos de esos procedimientos como sea posible. La automatización reduce la sobrecarga y puede mejorar la seguridad asegurándose de que los pasos del proceso se realizan de forma rápida, coherente y según sus requisitos predefinidos.

Para más información acerca de las capacidades de Security Center automática y manual para ejecutar los flujos de trabajo, consulte [ automatización de flujos de trabajo](workflow-automation.md).

Más información acerca de la [creación de Logic Apps](../logic-apps/logic-apps-overview.md).


### <a name="integration-of-azure-security-center-with-windows-admin-center"></a>Integración de Azure Security Center con Windows Admin Center

Ahora es posible migrar los servidores de Windows locales desde Windows Admin Center directamente al Azure Security Center. Security Center, a continuación, se convierte en el único panel para ver la información de seguridad de todos los recursos de Windows Admin Center, incluidos los servidores locales, las máquinas virtuales y las cargas de trabajo PaaS adicionales.

Después de mover un servidor de Windows Admin Center a Azure Security Center, podrá realizar lo siguiente:

- Ver alertas y recomendaciones de seguridad en la extensión de Security Center de Windows Admin Center.
- Ver la postura de seguridad y recuperar información detallada adicional de los servidores administrados de Windows Admin Center en Security Center, mediante Azure Portal (o a través de una API).

Más información sobre [cómo integrar Azure Security Center con Windows Admin Center](windows-admin-center-integration.md).


### <a name="protection-for-azure-kubernetes-service"></a>Protección para Azure Kubernetes Service

Azure Security Center está expandiendo sus características de seguridad de contenedor para proteger Azure Kubernetes Service (AKS).

La popular plataforma de código abierto Kubernetes se ha adoptado tan ampliamente que ahora es un estándar del sector para la orquestación de contenedores. A pesar de esta implementación generalizada, todavía hay una falta de comprensión sobre cómo proteger un entorno de Kubernetes. Defender las superficies de ataque de una aplicación en contenedores requiere conocimientos para asegurarse de que la infraestructura esté configurada de forma segura y se supervise constantemente para detectar posibles amenazas.

La defensa de Security Center incluye:

- **Detección y visibilidad**: detección continua de instancias de AKS administradas dentro de las suscripciones registradas de Security Center.
- **Recomendaciones de seguridad**: recomendaciones útiles para ayudarle a cumplir los procedimientos recomendados de seguridad para AKS. Estas recomendaciones se incluyen en su puntuación segura para asegurarse de que se ven como parte de la postura de seguridad de su organización. Un ejemplo de una recomendación relacionada con AKS que podría ver es que "se debe usar el control de acceso basado en roles para restringir el acceso a un clúster de servicio de Kubernetes".
- **Protección contra amenazas**: gracias al análisis continuo de la implementación de AKS, Security Center le alerta de las amenazas y actividades malintencionadas que se detectan en el host y el nivel de clúster de AKS.

Más información acerca de [integración de Azure Kubernetes Services con Security Center](defender-for-kubernetes-introduction.md).

Más información sobre [las características de seguridad de los contenedores en Security Center](container-security.md).


### <a name="improved-just-in-time-experience"></a>Experiencia Just-in-Time mejorada

Las características, la operación y la interfaz de usuario de las herramientas Just-in-Time de Azure Security Center que protegen los puertos de administración se han mejorado de la manera siguiente: 

- **Campo de justificación**: al solicitar acceso a una máquina virtual (VM) a través de la página Just-in-Time del Azure Portal, hay un nuevo campo opcional disponible para especificar una justificación para la solicitud. Se puede realizar un seguimiento de la información especificada en este campo en el registro de actividad. 
- **Limpieza automática de reglas Just-In-Time (JIT) redundantes**: siempre que se actualiza una directiva JIT, se ejecuta automáticamente una herramienta de limpieza para comprobar la validez de todo el conjunto de reglas. La herramienta busca discrepancias entre las reglas de la directiva y las reglas del grupo de seguridad de red. Si la herramienta de limpieza encuentra una discrepancia, determina la causa y, cuando es seguro hacerlo, quita las reglas integradas que ya no son necesarias. El limpiador no elimina nunca las reglas que ha creado. 

Más información acerca de [la característica de acceso JIT](security-center-just-in-time.md).


### <a name="two-security-recommendations-for-web-applications-deprecated"></a>Dos recomendaciones de seguridad para las aplicaciones web en desuso

Dos recomendaciones de seguridad relacionadas con las aplicaciones web están en desuso: 

- Se deben proteger las reglas de las aplicaciones web en los grupos de seguridad de red de IaaS.
    (Directiva relacionada: Se deben proteger las reglas de NSG para las aplicaciones web en IaaS).

- El acceso a App Services debe estar restringido.
    (Directiva relacionada: El acceso a App Services debe estar restringido [versión preliminar])

Estas recomendaciones ya no aparecerán en la lista de Security Center de recomendaciones. Las directivas relacionadas ya no se incluirán en la iniciativa denominada "valor predeterminado de Security Center".

Más información sobre las [recomendaciones de seguridad](recommendations-reference.md).




## <a name="february-2020"></a>Febrero de 2020

### <a name="fileless-attack-detection-for-linux-preview"></a>Detección de ataques sin archivos para Linux (versión preliminar)

A medida que los atacantes aumentan el empleo de métodos stealthier para evitar la detección, Azure Security Center está ampliando la detección de ataques no archivados para Linux, además de Windows. Los ataques sin archivos no aprovechan las vulnerabilidades del software, insertan cargas malintencionadas en procesos benignos del sistema y se ocultan en la memoria. Estas técnicas:

- minimiza o eliminan seguimientos de malware en disco
- reducen considerablemente las posibilidades de detección mediante soluciones de análisis de malware basadas en disco

Para contrarrestar esta amenaza, Azure Security Center publicó la detección de ataques sin archivos para Windows en octubre de 2018 y ahora ha extendido la detección de ataques sin archivos en Linux. 



## <a name="january-2020"></a>Enero de 2020

### <a name="enhanced-secure-score-preview"></a>Puntuación de seguridad mejorada (versión preliminar)

Ahora hay disponible una versión mejorada de la característica de puntuación segura de Azure Security Center en versión preliminar. En esta versión, se agrupan varias recomendaciones en controles de seguridad que reflejan mejor las superficies de ataque vulnerables (por ejemplo, restringir el acceso a los puertos de administración).

Familiarícese con los cambios de puntuación segura durante la fase de versión preliminar y determine otras correcciones que le ayudarán a proteger más su entorno.

Obtenga más información sobe la [puntuación de seguridad mejorada (versión preliminar)](secure-score-security-controls.md).



## <a name="november-2019"></a>Noviembre de 2019

Las actualizaciones de noviembre incluyen:
 - [Protección contra amenazas para Azure Key Vault en regiones de Norteamérica (versión preliminar)](#threat-protection-for-azure-key-vault-in-north-america-regions-preview)
 - [La protección contra amenazas para Azure Storage incluye el filtrado de reputación de malware](#threat-protection-for-azure-storage-includes-malware-reputation-screening)
 - [Automatización del flujo de trabajo con Logic Apps (versión preliminar)](#workflow-automation-with-logic-apps-preview)
 - [Corrección rápida de los recursos en masa disponibles con carácter general](#quick-fix-for-bulk-resources-generally-available)
 - [Examen de imágenes de contenedor para detectar vulnerabilidades (versión preliminar)](#scan-container-images-for-vulnerabilities-preview)
 - [Estándares de cumplimiento normativo adicionales (versión preliminar)](#additional-regulatory-compliance-standards-preview)
 - [Protección contra amenazas para Azure Kubernetes Service (versión preliminar)](#threat-protection-for-azure-kubernetes-service-preview)
 - [Evaluación de vulnerabilidades de máquinas virtuales (versión preliminar)](#virtual-machine-vulnerability-assessment-preview)
 - [Advanced Data Security para servidores SQL Server en Azure Virtual Machines (versión preliminar)](#advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview)
 - [Compatibilidad con directivas personalizadas (versión preliminar)](#support-for-custom-policies-preview)
 - [Ampliación de la cobertura de Azure Security Center con la plataforma para la comunidad y los asociados](#extending-azure-security-center-coverage-with-platform-for-community-and-partners)
 - [Integraciones avanzadas con la exportación de recomendaciones y alertas (versión preliminar)](#advanced-integrations-with-export-of-recommendations-and-alerts-preview)
 - [Incorporación de servidores locales a Security Center desde Windows Admin Center (versión preliminar)](#onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview)

### <a name="threat-protection-for-azure-key-vault-in-north-america-regions-preview"></a>Protección contra amenazas para Azure Key Vault en regiones de Norteamérica (versión preliminar)

Azure Key Vault es un servicio esencial para proteger los datos y mejorar el rendimiento de las aplicaciones en la nube, ya que ofrece la posibilidad de administrar de forma centralizada claves, secretos, claves criptográficas y directivas en la nube. Como Azure Key Vault almacena datos confidenciales y críticos para la empresa, requiere la máxima seguridad para los almacenes de claves y los datos almacenados en ellos.

El soporte de Azure Security Center para Threat Protection para Azure Key Vault proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a los almacén de claves o vulnerarlos. Esta nueva capa de protección permite a los clientes afrontar las amenazas contra sus almacenes sin necesidad de ser un experto en seguridad ni administrar sistemas de supervisión de la seguridad. La característica está en versión preliminar pública en regiones Norteamérica.


### <a name="threat-protection-for-azure-storage-includes-malware-reputation-screening"></a>La protección contra amenazas para Azure Storage incluye el filtrado de reputación de malware

La protección contra amenazas para Azure Storage ofrece nuevas detecciones basadas en la inteligencia sobre amenazas de Microsoft para detectar cargas de malware en Azure Storage mediante el análisis de reputación de hash y accesos sospechosos desde un nodo de salida de Tor activo (un proxy de anonimización). Ahora puede ver el malware detectado en todas las cuentas de almacenamiento mediante Azure Security Center.


### <a name="workflow-automation-with-logic-apps-preview"></a>Automatización del flujo de trabajo con Logic Apps (versión preliminar)

Organizaciones con seguridad administrada centralmente y de TI/operaciones que implementan procesos de flujo de trabajo internos para impulsar la acción necesaria dentro de la organización cuando se detectan discrepancias en sus entornos. En muchos casos, estos flujos de trabajo son procesos repetibles y la automatización puede optimizar considerablemente los procesos dentro de la organización.

Hoy estamos presentando una nueva funcionalidad en Security Center que permite a los clientes crear configuraciones de automatización que aprovechan Azure Logic Apps y crear directivas que las desencadenarán automáticamente en función de determinados resultados de ASC, como recomendaciones o alertas. La aplicación lógica de Azure se puede configurar para realizar cualquier acción personalizada compatible con la amplia comunidad de conectores de aplicaciones lógicas, o bien usar una de las plantillas proporcionadas por Security Center como enviar un correo electrónico o abrir un vale de ServiceNow™.

Para más información acerca de las capacidades de Security Center automática y manual para ejecutar los flujos de trabajo, consulte [ automatización de flujos de trabajo](workflow-automation.md).

Para información acerca de la creación de Logic Apps, consulte [Azure Logic Apps](../logic-apps/logic-apps-overview.md).


### <a name="quick-fix-for-bulk-resources-generally-available"></a>Corrección rápida de los recursos en masa disponibles con carácter general

Con las muchas tareas que se proporcionan a un usuario como parte de la puntuación segura, la capacidad de corregir los problemas de forma eficaz en una flota de gran tamaño puede resultar complicada.

Para simplificar la corrección de las configuraciones erróneas de seguridad y poder corregir rápidamente las recomendaciones en una gran parte de los recursos y mejorar la puntuación segura, use la corrección rápida.

Esta operación le permitirá seleccionar los recursos a los que desea aplicar la corrección e iniciar una acción correctiva que configurará la configuración en su nombre.

La corrección rápida está disponible con carácter general para los clientes de hoy en día como parte de la página de recomendaciones de Security Center.

Vea qué recomendaciones tiene habilitada la corrección rápida en la [guía de referencia para recomendaciones de seguridad](recommendations-reference.md).


### <a name="scan-container-images-for-vulnerabilities-preview"></a>Examinar imágenes de contenedor para detectar vulnerabilidades (versión preliminar)

Ahora Azure Security Center puede examinar imágenes de contenedor en Azure Container Registry para detectar vulnerabilidades.

El análisis de imágenes funciona mediante el análisis del archivo de imagen del contenedor y la comprobación de la existencia de alguna vulnerabilidad conocida (con la tecnología Qualys).

El propio análisis se desencadena automáticamente al insertar nuevas imágenes de contenedor en Azure Container Registry. Las vulnerabilidades detectadas se verán como recomendaciones de Security Center y se incluirán en la puntuación segura de Azure junto con información sobre cómo aplicarles revisiones para reducir la superficie expuesta a los ataques que permitían.


### <a name="additional-regulatory-compliance-standards-preview"></a>Estándares de cumplimiento normativo adicionales (versión preliminar)

En el panel de cumplimiento normativo se ofrece información sobre la postura de cumplimiento basado en las evaluaciones de Security Center. En el panel se muestra cómo el entorno cumple los controles y requisitos designados por estándares normativos específicos y pruebas comparativas del sector, y proporciona recomendaciones prescriptivas para abordar estos requisitos.

Por lo tanto, el panel de cumplimiento normativo ha admitido cuatro estándares integrados:  Azure CIS 1.1.0, PCI-DSS, ISO 27001, y SOC-TSP. Ahora anunciamos la versión preliminar pública de estándares admitidos adicionales: NIST SP 800-53 R4, SWIFT CSP CSCF v2020, Canada Federal PBMM y UK Official junto con UK NHS. También lanzamos una versión actualizada de Azure CIS 1.1.0, que cubre más controles del estándar y la mejora de la extensibilidad.

[Obtenga más información sobre cómo personalizar el conjunto de estándares en el panel de cumplimiento normativo](update-regulatory-compliance-packages.md).


### <a name="threat-protection-for-azure-kubernetes-service-preview"></a>Protección contra amenazas para Azure Kubernetes Service (versión preliminar)

Kubernetes se convierte rápidamente en el nuevo estándar para implementar y administrar software en la nube. Pocas personas tienen una amplia experiencia con Kubernetes y muchos solo se centran en la ingeniería y administración generales y pasan por alto el aspecto de seguridad. El entorno de Kubernetes debe configurarse cuidadosamente para que sea seguro, asegurándose de que no se deja abierta ninguna puerta expuesta a ataques de contenedor para los atacantes. Security Center está expandiendo su compatibilidad en el espacio del contenedor a uno de los servicios en crecimiento más rápido de Azure: Azure Kubernetes Service (AKS).

Las nuevas capacidades de esta versión preliminar pública incluyen:

- **Detección y visibilidad**: detección continua de instancias de AKS administradas dentro de las suscripciones registradas de Security Center.
- **Recomendaciones de puntuación de seguridad**: Elementos procesables para ayudar a los clientes a cumplir con los procedimientos recomendados de seguridad para AKS y aumentar su puntuación de seguridad. Las recomendaciones incluyen elementos como "El control de acceso basado en rol debe usarse para restringir el acceso a un clúster de servicio de Kubernetes".
- **Detección de amenazas**: análisis basados en host y en clúster, como "Se ha detectado un contenedor con privilegios".


### <a name="virtual-machine-vulnerability-assessment-preview"></a>Evaluación de vulnerabilidades de máquinas virtuales (versión preliminar)

Las aplicaciones que se instalan en máquinas virtuales a menudo pueden tener vulnerabilidades que podrían provocar una infracción de la máquina virtual. Estamos anunciando que el nivel estándar de Security Center incluye una evaluación de vulnerabilidades integrada para máquinas virtuales sin costo adicional. La evaluación de vulnerabilidades, con la tecnología de Qualys en la versión preliminar pública, le permitirá examinar continuamente todas las aplicaciones instaladas en una máquina virtual para encontrar aplicaciones vulnerables y presentar las conclusiones en la experiencia del portal de Security Center. Security Center se encarga de todas las operaciones de implementación para que no sea necesario ningún trabajo adicional por parte del usuario. En adelante, tenemos previsto proporcionar opciones de evaluación de vulnerabilidades para satisfacer las necesidades empresariales únicas de nuestros clientes.

[Más información acerca de evaluaciones de vulnerabilidades para Azure Virtual Machines](deploy-vulnerability-assessment-vm.md).


### <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Advanced Data Security para servidores SQL Server en Azure Virtual Machines (versión preliminar)

La compatibilidad de Azure Security Center para la protección contra amenazas y la evaluación de vulnerabilidades en las bases de SQL que se ejecutan en máquinas virtuales IaaS está ahora en versión preliminar.

[Evaluación de vulnerabilidades](../azure-sql/database/sql-vulnerability-assessment.md) es un servicio fácil de configurar que puede detectar, realizar un seguimiento y ayudarle a corregir posibles puntos vulnerables de una base de datos. Permite ver la postura de seguridad como parte de la puntuación segura de Azure e incluye los pasos para resolver problemas de seguridad y mejorar las defensas de su base de datos.

[Advanced Threat Protection](../azure-sql/database/threat-detection-overview.md) detecta actividades anómalas que indican intentos inusuales y potencialmente perjudiciales de acceder a su servidor SQL Server o de aprovechar sus vulnerabilidades. Supervisa de forma constante la base de datos en busca de actividad sospechosa y proporciona alertas de seguridad sobre patrones de acceso a la base de datos anómalos para que pueda tomar medidas. Las alertas proporcionan detalles de la actividad sospechosa y recomiendan acciones para investigar y mitigar la amenaza.


### <a name="support-for-custom-policies-preview"></a>Compatibilidad con directivas personalizadas (versión preliminar)

Azure Security Center admite ahora directivas personalizadas (en versión preliminar).

Nuestros clientes han estado intentando ampliar su cobertura de evaluaciones de seguridad actual en Security Center con sus propias evaluaciones de seguridad basadas en las directivas que crean en Azure Policy. Gracias a la compatibilidad con las directivas personalizadas, ahora es posible.

Estas nuevas directivas formarán parte de la experiencia de recomendaciones de Security Center, la puntuación segura y el panel de normas de cumplimiento normativo. Con la compatibilidad con las directivas personalizadas, ahora puede crear una iniciativa personalizada en Azure Policy y, a continuación, agregarla como una directiva en Security Center y visualizarla como una recomendación.


### <a name="extending-azure-security-center-coverage-with-platform-for-community-and-partners"></a>Ampliación de la cobertura de Azure Security Center con la plataforma para la comunidad y los asociados

Use Security Center para recibir recomendaciones no solo de Microsoft, sino también de soluciones existentes de asociados como Check Point, Tenable y CyberArk con muchas más integraciones.  El flujo de incorporación simple de Security Center puede conectar las soluciones existentes a Security Center, lo que permite ver las recomendaciones de seguridad en un único lugar, ejecutar informes unificados y aprovechar todas las funcionalidades de Security Center con las recomendaciones integradas y de asociados. También puede exportar las recomendaciones de Security Center a productos de asociados.

[Más información sobre Intelligent Security Association.](https://www.microsoft.com/security/partnerships/intelligent-security-association)



### <a name="advanced-integrations-with-export-of-recommendations-and-alerts-preview"></a>Integraciones avanzadas con la exportación de recomendaciones y alertas (versión preliminar)

Con el fin de habilitar escenarios de nivel empresarial sobre Security Center, ahora es posible consumir alertas y recomendaciones de Security Center en otros lugares, excepto el Azure Portal o la API. Estos se pueden exportar directamente a un centro de eventos y áreas de trabajo de Log Analytics. Estos son algunos de los flujos de trabajo que puede crear en torno a estas nuevas capacidades:

- Con la exportación al área de trabajo de Log Analytics, puede crear paneles personalizados con Power BI.
- Con la exportación al centro de eventos, podrá exportar alertas y recomendaciones de Security Center a los SIEM de terceros, a una solución de terceros en tiempo real o a Azure Data Explorer.


### <a name="onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview"></a>Incorporación de servidores locales a Security Center desde Windows Admin Center (versión preliminar)

Windows Admin Center es un portal de administración para los servidores de Windows que no están implementados en Azure y que ofrecen varias funcionalidades de administración de Azure, como copias de seguridad y actualizaciones del sistema. Recientemente hemos agregado una capacidad para incorporar estos servidores que no son de Azure para que ASC los proteja directamente de la experiencia de Windows Admin Center.

Con esta nueva experiencia, los usuarios van a incorporar un servidor de WAC a Azure Security Center y habilitar la visualización de las alertas y recomendaciones de seguridad directamente en la experiencia de Windows Admin Center.


## <a name="september-2019"></a>Septiembre de 2019

Las actualizaciones de septiembre incluyen:

 - [Administración de reglas con mejoras de controles de aplicación adaptables](#managing-rules-with-adaptive-application-controls-improvements)
 - [Control de la recomendación de seguridad del contenedor mediante Azure Policy](#control-container-security-recommendation-using-azure-policy)

### <a name="managing-rules-with-adaptive-application-controls-improvements"></a>Administrar reglas con mejoras de controles de aplicación adaptables

Se ha mejorado la experiencia de administración de reglas para máquinas virtuales mediante controles de aplicación adaptables. Los controles de aplicación adaptables de Azure Security Center ayudan a controlar qué aplicaciones se pueden ejecutar en las máquinas virtuales. Además de una mejora general en la administración de reglas, una nueva ventaja le permite controlar qué tipos de archivo se protegerán cuando agregue una nueva regla.

[Más información acerca de los controles de aplicación adaptables](security-center-adaptive-application.md).


### <a name="control-container-security-recommendation-using-azure-policy"></a>Controlar la recomendación de seguridad del contenedor mediante Azure Policy

La recomendación de Azure Security Center para corregir vulnerabilidades en la seguridad de los contenedores ahora puede habilitarse o deshabilitarse a través de Azure Policy.

Para ver las directivas de seguridad habilitadas, en Security Center abra la página Directiva de seguridad.


## <a name="august-2019"></a>Agosto de 2019

Las actualizaciones de agosto incluyen:

 - [Acceso a máquina virtual Just-in-Time (JIT) para Azure Firewall](#just-in-time-jit-vm-access-for-azure-firewall)
 - [Corrección de un solo clic para impulsar la postura de seguridad (versión preliminar)](#single-click-remediation-to-boost-your-security-posture-preview)
 - [Administración entre inquilinos](#cross-tenant-management)

### <a name="just-in-time-jit-vm-access-for-azure-firewall"></a>Acceso a máquina virtual Just-in-Time (JIT) para Azure Firewall 

El acceso a máquina virtual Just-in-Time (JIT) para Azure Firewall ya está disponible con carácter general. Úselo para proteger sus entornos protegidos con Azure Firewall además de los entornos protegidos con NSG.

El acceso de máquina virtual JIT reduce la exposición a los ataques volumétricos de red al proporcionar acceso controlado a las máquinas virtuales solo cuando sea necesario, mediante el uso de las reglas de NSG y Azure Firewall.

Al habilitar JIT para las máquinas virtuales, se crea una directiva que determina los puertos que se van a proteger, cuánto tiempo deben permanecer abiertos los puertos y las direcciones IP aprobadas desde donde se puede tener acceso a estos puertos. Esta directiva le ayuda a mantener el control de lo que los usuarios pueden hacer cuando solicitan acceso.

Las solicitudes se registran en el registro de actividad de Azure para que pueda supervisar y auditar fácilmente el acceso. La página Just-in-Time también le ayuda a identificar rápidamente las máquinas virtuales existentes que tienen habilitado JIT y las máquinas virtuales en las que se recomienda JIT.

[Más información acerca de Azure Firewall](../firewall/overview.md).


### <a name="single-click-remediation-to-boost-your-security-posture-preview"></a>Corrección de un solo clic para impulsar la postura de seguridad (versión preliminar)

La puntuación segura es una herramienta que le ayuda a evaluar la postura de seguridad de las cargas de trabajo. Revisa las recomendaciones de seguridad y les asigna una prioridad, para que usted sepa qué recomendaciones realizar primero. De esta manera, puede encontrar las vulnerabilidades de seguridad más graves para dar prioridad a la investigación.

Con el fin de simplificar la corrección de configuraciones incompletas de seguridad y ayudarle a mejorar rápidamente su puntuación segura, hemos agregado una nueva funcionalidad que le permite corregir una recomendación en una gran parte de los recursos con un solo clic.

Esta operación le permitirá seleccionar los recursos a los que desea aplicar la corrección e iniciar una acción correctiva que configurará la configuración en su nombre.

Vea qué recomendaciones tiene habilitada la corrección rápida en la [guía de referencia para recomendaciones de seguridad](recommendations-reference.md).


### <a name="cross-tenant-management"></a>Administración entre inquilinos

Security Center admite ahora escenarios de administración entre inquilinos como parte de Azure Lighthouse. Esto le permite obtener visibilidad y administrar la postura de seguridad de varios inquilinos en Security Center. 

[Más información sobre las experiencias de administración entre inquilinos](security-center-cross-tenant-management.md).


## <a name="july-2019"></a>Julio de 2019

### <a name="updates-to-network-recommendations"></a>Actualizaciones de recomendaciones sobre redes

Azure Security Center (ASC) ha lanzado nuevas recomendaciones de redes y ha mejorado algunas existentes. Ahora, el uso de Security Center garantiza una mayor protección de la red para los recursos. 

[Más información sobre las recomendaciones de redes](recommendations-reference.md#recs-networking).


## <a name="june-2019"></a>Junio de 2019

### <a name="adaptive-network-hardening---generally-available"></a>Protección de redes adaptable: disponible con carácter general

Una de las superficies de ataque más importantes para las cargas de trabajo que se ejecutan en la nube pública son las conexiones hacia y desde la red pública de Internet. Nuestros clientes saben que es difícil saber qué reglas del grupo de seguridad de red (NSG) deben estar en vigor para asegurarse de que las cargas de trabajo de Azure solo están disponibles para los intervalos de origen necesarios. Con esta característica, Security Center aprende los patrones de conectividad y el tráfico de red de las cargas de trabajo de Azure y proporciona recomendaciones de reglas de NSG, para las máquinas virtuales orientadas a Internet. Esto ayuda a nuestro cliente a configurar mejor las directivas de acceso a la red y limitar su exposición a los ataques. 

[Más información sobre la protección de red adaptable](security-center-adaptive-network-hardening.md).