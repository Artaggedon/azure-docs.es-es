---
title: Notas de la versión de Azure Security Center
description: Una descripción de las novedades y los cambios en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/28/2020
ms.author: memildin
ms.openlocfilehash: 488aff74b632cbec97cbe9a07d4add953ba29fea
ms.sourcegitcommit: ab829133ee7f024f9364cd731e9b14edbe96b496
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2020
ms.locfileid: "97796093"
---
# <a name="whats-new-in-azure-security-center"></a>Novedades de Azure Security Center

Defender está en desarrollo activo y recibe mejoras continuas. Para mantenerse al día con los avances más recientes, esta página proporciona información sobre las nuevas características, las correcciones de errores y las funcionalidades en desuso.

Esta página se actualiza con frecuencia, por lo que se recomienda visitarla a menudo. 

Para obtener información sobre los cambios *planeados* que están próximos a materializarse en Security Center, consulte el artículo [Próximos cambios importantes en Azure Security Center](upcoming-changes.md). 

> [!TIP]
> Si busca elementos de más de 6 meses, puede encontrarlos en las [Novedades de Azure Security Center](release-notes-archive.md).


## <a name="december-2020"></a>Diciembre de 2020

Las actualizaciones de diciembre incluyen:

- [Disponibilidad general de Azure Defender para servidores SQL Server en máquinas](#azure-defender-for-sql-servers-on-machines-is-generally-available)
- [Compatibilidad de Azure Defender para SQL para el grupo de SQL dedicado de Azure Synapse Analytics disponible con carácter general](#azure-defender-for-sql-support-for-azure-synapse-analytics-dedicated-sql-pool-is-generally-available)
- [Ahora, los administradores globales se pueden conceder a sí mismos permisos de nivel de inquilino](#global-administrators-can-now-grant-themselves-tenant-level-permissions)
- [Dos nuevos planes de Azure Defender: Azure Defender para DNS y Azure Defender para Resource Manager (en versión preliminar)](#two-new-azure-defender-plans-azure-defender-for-dns-and-azure-defender-for-resource-manager-in-preview)
- [Nueva página de alertas de seguridad en Azure Portal (versión preliminar)](#new-security-alerts-page-in-the-azure-portal-preview)
- [Experiencia de Security Center revitalizada en Azure SQL Database y SQL Managed Instance](#revitalized-security-center-experience-in-azure-sql-database--sql-managed-instance)
- [Se han actualizado las herramientas y filtros para el inventario de recursos](#asset-inventory-tools-and-filters-updated)
- [La recomendación acerca de las aplicaciones web que solicitan certificados SSL ya no forma parte de la puntuación segura](#recommendation-about-web-apps-requesting-ssl-certificates-no-longer-part-of-secure-score)
- [La página Recomendaciones tiene nuevos filtros de entorno, gravedad y respuestas disponibles](#recommendations-page-has-new-filters-for-environment-severity-and-available-responses)
- [La exportación continua obtiene nuevos tipos de datos y directivas deployifnotexist mejoradas](#continuous-export-gets-new-data-types-and-improved-deployifnotexist-policies)


### <a name="azure-defender-for-sql-servers-on-machines-is-generally-available"></a>Disponibilidad general de Azure Defender para servidores SQL Server en máquinas

Azure Security Center ofrece dos planes de Azure Defender para servidores SQL Server:

- **Azure Defender para servidores de bases de datos Azure SQL**: defiende los servidores SQL Server nativos de Azure. 
- **Azure Defender para servidores SQL Server en máquinas**: amplía las mismas protecciones a los servidores SQL Server en entornos híbridos, multinube y locales.

Con este anuncio, **Azure Defender para SQL** ahora protege las bases de datos y sus datos dondequiera que se encuentren.

Azure Defender para SQL incluye funcionalidades de evaluación de vulnerabilidades. La herramienta de evaluación de vulnerabilidades incluye las siguientes características avanzadas:

- La **configuración de línea de base** (nuevo) para limitar de forma inteligente los resultados de los análisis de vulnerabilidades a aquellas que podrían representar problemas de seguridad reales. Una vez establecido el estado de seguridad de la línea de base, la herramienta de evaluación de vulnerabilidades solo informa de las desviaciones con respecto a esa línea de base. Los resultados que coinciden con la línea de base se consideran como correctos en análisis posteriores. Esto le permite a usted y a sus analistas centrar la atención en lo que importa.
- **Información comparativa detallada** para ayudarle a *comprender* los hechos detectados y por qué se relacionan con los recursos.
- **Scripts de corrección** para ayudarle a mitigar los riesgos identificados.

Más información sobre [Azure Defender for SQL](defender-for-sql-introduction.md).


### <a name="azure-defender-for-sql-support-for-azure-synapse-analytics-dedicated-sql-pool-is-generally-available"></a>Compatibilidad de Azure Defender para SQL para Azure Synapse Analytics disponible con carácter general

Azure Synapse Analytics (anteriormente SQL Data Warehouse) es un servicio de análisis que combina almacenamiento de datos empresariales con análisis de macrodatos. Los grupos de SQL dedicados son las características de almacenamiento de datos empresariales de Azure Synapse. Más información en [¿Qué es Azure Synapse Analytics (anteriormente SQL Data Warehouse)?](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Azure Defender para SQL protege los grupos de SQL dedicados con:

- **Protección contra amenazas avanzada** para detectar amenazas y ataques. 
- **Funcionalidades de evaluación de vulnerabilidades** para identificar y corregir configuraciones erróneas de seguridad.

La compatibilidad de Azure Defender para SQL con los grupos de SQL de Azure Synapse Analytics se agrega automáticamente al paquete de bases de datos SQL de Azure en Azure Security Center. Encontrará una nueva pestaña "Azure Defender para SQL" en la página del área de trabajo de Synapse en Azure Portal.

Más información sobre [Azure Defender for SQL](defender-for-sql-introduction.md).


### <a name="global-administrators-can-now-grant-themselves-tenant-level-permissions"></a>Ahora, los administradores globales se pueden conceder a sí mismos permisos de nivel de inquilino.

Un usuario con el rol de **administrador global** de Azure Active Directory puede tener responsabilidades en todo el inquilino, pero carecer de los permisos de Azure para ver la información de toda la organización en Azure Security Center. 

Para asignarse a sí mismo permisos de nivel de inquilino, siga las instrucciones que se indican en [Concederse a uno mismo permisos para todo el inquilino](security-center-management-groups.md#grant-tenant-wide-permissions-to-yourself).


### <a name="two-new-azure-defender-plans-azure-defender-for-dns-and-azure-defender-for-resource-manager-in-preview"></a>Dos nuevos planes de Azure Defender: Azure Defender para DNS y Azure Defender para Resource Manager (en versión preliminar)

Se han agregado dos nuevas funcionalidades de protección contra amenazas con amplitud nativa de nube para el entorno de Azure.

Estas nuevas protecciones mejoran enormemente la resistencia frente a los ataques de actores de amenazas y aumentan considerablemente el número de recursos de Azure protegidos por Azure Defender.

- **Azure Defender para Resource Manager**: supervisa automáticamente todas las operaciones de administración de recursos realizadas en la organización. Para más información, consulte:
    - [Introducción a Azure Defender para Resource Manager](defender-for-resource-manager-introduction.md)
    - [Respuesta a las alertas de Azure Defender para Resource Manager](defender-for-resource-manager-usage.md)
    - [Lista de alertas proporcionadas por Azure Defender para Resource Manager](alerts-reference.md#alerts-resourcemanager)

- **Azure Defender para DNS**: supervisa continuamente todas las consultas de DNS de los recursos de Azure. Para más información, consulte:
    - [Introducción a Azure Defender para DNS](defender-for-dns-introduction.md)
    - [Respuesta a las alertas de Azure Defender para DNS](defender-for-dns-usage.md)
    - [Lista de alertas proporcionadas por Azure Defender para DNS](alerts-reference.md#alerts-dns)


### <a name="new-security-alerts-page-in-the-azure-portal-preview"></a>Nueva página de alertas de seguridad en Azure Portal (versión preliminar)

La página de alertas de seguridad de Azure Security Center se ha rediseñado para proporcionar:

- **Mejora en la evaluación de prioridades en las alertas**: ahora es más fácil ayudar a reducir la fatiga de las alertas y centrarse en las amenazas más importantes, la lista incluye filtros personalizables y opciones de agrupación.
- **Más información en la lista de alertas**: por ejemplo las tácticas ATT y ACK de MITRE
- **Botón para crear alertas de ejemplo**: para evaluar las funcionalidades de Azure Defender y probar la configuración de las alertas (para la integración de SIEM, las notificaciones por correo electrónico y las automatizaciones de los flujos de trabajo), puede crear alertas de ejemplo desde todos los planes de Azure Defender.
- **Alineación con la experiencia de incidentes de Azure Sentinel**: para los clientes que usan ambos productos, cambiar entre ellos ahora es más sencillo y es fácil que uno aprenda del otro.
- **Mejor rendimiento** de listas de alertas grandes.
- **Desplazamiento mediante el teclado** a través de la lista de alertas.
- **Alertas de Azure Resource Graph**: puede consultar las alertas en Azure Resource Graph, la API similar a Kusto para todos los recursos. Esto también resulta útil si va a crear sus propios paneles de alertas. [Más información sobre Azure Resource Graph](../governance/resource-graph/index.yml).

Para acceder a la nueva experiencia, use el vínculo "Intentarlo ahora" del banner de la parte superior de la página de alertas de seguridad.

:::image type="content" source="media/security-center-managing-and-responding-alerts/preview-alerts-experience-banner.png" alt-text="Banner con un vínculo a la versión preliminar de la nueva experiencia de alertas":::

Para crear alertas de ejemplo a partir de la nueva experiencia de alertas, consulte el apartado [Generación de alertas de ejemplo de Azure Defender](security-center-alert-validation.md#generate-sample-azure-defender-alerts).


### <a name="revitalized-security-center-experience-in-azure-sql-database--sql-managed-instance"></a>Experiencia de Security Center revitalizada en Azure SQL Database y SQL Managed Instance 

La experiencia de Security Center en SQL proporciona acceso a las siguientes características de Security Center y Azure Defender para SQL:

- **Recomendaciones de seguridad**: Azure Security Center analiza periódicamente el estado de seguridad de todos los recursos de Azure conectados para identificar posibles configuraciones erróneas de seguridad. A continuación, proporciona recomendaciones sobre cómo corregir esas vulnerabilidades y mejorar la postura de seguridad de las organizaciones.
- **Alertas de seguridad**: un servicio de detección que supervisa continuamente las actividades de Azure SQL para detectar amenazas tales como la inyección de código SQL, ataques por fuerza bruta y abuso de privilegios. Este servicio desencadena alertas de seguridad detalladas y orientadas a acciones en Security Center, y proporciona opciones para continuar las investigaciones con Azure Sentinel, la solución SIEM nativa de Microsoft Azure.
- **Conclusiones**: un servicio de evaluación de vulnerabilidades que supervisa continuamente las configuraciones de Azure SQL y ayuda a corregir las vulnerabilidades. Los análisis de evaluación proporcionan una visión general de los estados de seguridad de Azure SQL junto con conclusiones detalladas de seguridad.     

:::image type="content" source="media/release-notes/azure-security-center-experience-in-sql.png" alt-text="Las características de seguridad de Azure Security Center para SQL están disponibles en Azure SQL":::


### <a name="asset-inventory-tools-and-filters-updated"></a>Se han actualizado las herramientas y filtros para el inventario de recursos

La página Inventario de Azure Security Center se ha actualizado con los cambios siguientes:

- Se han agregado **guías y comentarios** a la barra de herramientas. Esto permite abrir un panel con vínculos a información y herramientas relacionados. 
- Se ha agregado un **filtro de suscripciones** a los filtros predeterminados disponibles para los recursos.
- Se ha agregado el enlace **Abrir consulta** para abrir las opciones de filtro actuales como una consulta de Azure Resource Graph (anteriormente se denominaba "Ver en Resource Graph Explorer").
- **Opciones de operador** para cada filtro. Ahora puede elegir entre otros operadores lógicos adicionales distintos de "=". Por ejemplo, puede que desee buscar todos los recursos con recomendaciones activas cuyos títulos incluyan la cadena "Encrypt". 

    :::image type="content" source="media/release-notes/inventory-filter-operators.png" alt-text="Controles para la opción de operador en los filtros de Asset Inventory":::

Más información acerca del inventario en [Exploración y administración de los recursos con Asset Inventory](asset-inventory.md).


### <a name="recommendation-about-web-apps-requesting-ssl-certificates-no-longer-part-of-secure-score"></a>La recomendación acerca de las aplicaciones web que solicitan certificados SSL ya no forma parte de la puntuación segura

La recomendación "Las aplicaciones web deben solicitar un certificado SSL a todas las solicitudes entrantes" se ha trasladado desde el control de seguridad **Administración de acceso y permisos** (con una puntuación máxima de 4 puntos) a **Implementación de procedimientos recomendados de seguridad** (con ningún punto). 

Asegurarse de que las aplicaciones web solicitan un certificado ciertamente hace que sean más seguras. Sin embargo, para las aplicaciones web de acceso público, esto es irrelevante. Si accede a su sitio a través de HTTP y no de HTTPS, no recibirá ningún certificado de cliente. Por lo tanto, si la aplicación requiere certificados de cliente, no debe permitir solicitudes a la aplicación mediante HTTP. Puede encontrar más información en [Configuración de la autenticación mutua de TLS en Azure App Service](../app-service/app-service-web-configure-tls-mutual-auth.md).

Con este cambio, la recomendación es ahora un procedimiento recomendado que no influye en la puntuación. 

Obtenga información sobre qué recomendaciones se encuentran en cada control de seguridad en [Controles de seguridad y sus recomendaciones](secure-score-security-controls.md#security-controls-and-their-recommendations).


### <a name="recommendations-page-has-new-filters-for-environment-severity-and-available-responses"></a>La página Recomendaciones tiene nuevos filtros de entorno, gravedad y respuestas disponibles

Azure Security Center supervisa todos los recursos conectados y genera recomendaciones de seguridad. Use estas recomendaciones para fortalecer su postura en la nube híbrida y realizar un seguimiento del cumplimiento de las directivas y los estándares importantes para su organización, sector y país.

A medida que Security Center amplía su cobertura y características, la lista de recomendaciones de seguridad crece cada mes. Por ejemplo, consulte [Se han agregado 29 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark](#29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark).

Con una lista en continuo crecimiento, existe la necesidad de filtrar las recomendaciones de mayor interés. En noviembre, hemos agregado filtros a la página Recomendaciones (consulte [La lista de recomendaciones ahora incluye filtros](#recommendations-list-now-includes-filters)).

Los filtros que se han agregado este mes proporcionan opciones para refinar la lista de recomendaciones según:

- El **entorno**: vea las recomendaciones para los recursos de AWS, GCP o Azure (o cualquier combinación).
- La **gravedad**: vea las recomendaciones según la clasificación de gravedad establecida por Security Center.
- Las **acciones de respuesta**: vea las recomendaciones según la disponibilidad de las opciones de respuesta de Security Center: Corrección rápida, denegación y aplicación

    > [!TIP]
    > El filtro de acciones de respuesta reemplaza al filtro **Solución rápida disponible (Sí/No)** . 
    > 
    > Puede aprender más información acerca de cada una de estas opciones de respuesta:
    > - [Solución por corrección rápida](security-center-remediate-recommendations.md#quick-fix-remediation)
    > - [Evitar errores de configuración con las recomendaciones Aplicar o Denegar](prevent-misconfigurations.md).

:::image type="content" source="./media/release-notes/added-recommendations-filters.png" alt-text="Recomendaciones agrupadas por control de seguridad" lightbox="./media/release-notes/added-recommendations-filters.png":::

### <a name="continuous-export-gets-new-data-types-and-improved-deployifnotexist-policies"></a>La exportación continua obtiene nuevos tipos de datos y directivas deployifnotexist mejoradas

Las herramientas de exportación continua de Azure Security Center permiten exportar las recomendaciones y alertas de Security Center para su uso con otras herramientas de supervisión de su entorno.

La exportación continua le permite personalizar completamente qué se exportará y a dónde irá. Para más información, consulte [Exportación continua de datos de Security Center](continuous-export.md).

Estas herramientas se han mejorado y ampliado de las siguientes maneras:

- **Se han mejorado las directivas deployifnotexist de la exportación continua**. Las directivas ahora:

    - **Comprueban si la configuración está habilitada.** Si no es así, la directiva aparecerá como no compatible y creará un recurso compatible. Obtenga más información acerca de las plantillas de Azure Policy proporcionadas en la pestaña "Implementación a gran escala con Azure Policy" en [Configuración de una exportación continua](continuous-export.md#set-up-a-continuous-export).

    - **Compatibilidad con la exportación de conclusiones de seguridad.** Cuando usa las plantillas de Azure Policy, puede configurar la exportación continua para que incluya conclusiones. Esto es importante cuando se exportan recomendaciones que incluyen "subrecomendaciones", como en el caso de los resultados de los escáneres de evaluación de vulnerabilidades o las actualizaciones específicas del sistema para la recomendación principal "Se deben instalar actualizaciones del sistema en las máquinas".
    
    - **Compatibilidad con la exportación de datos de puntuación de seguridad.**

- **Se han agregado datos de evaluación de cumplimiento normativo (en versión preliminar).** Ahora puede exportar continuamente las actualizaciones de las evaluaciones de cumplimiento normativo, incluidas las iniciativas personalizadas, a un área de trabajo Log Analytics o a un centro de eventos. Esta característica no está disponible en las nubes nacionales o soberanas.

    :::image type="content" source="media/release-notes/continuous-export-regulatory-compliance-option.png" alt-text="Opciones para incluir información de evaluación conforme a la normativa con los datos de exportación continua.":::


## <a name="november-2020"></a>Noviembre de 2020

Las actualizaciones de noviembre incluyen:

- [Se han agregado 29 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark](#29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark).
- [Se ha agregado NIST SP 800 171 R2 al panel de cumplimiento normativo de Security Center](#nist-sp-800-171-r2-added-to-security-centers-regulatory-compliance-dashboard).
- [La lista de recomendaciones ahora incluye filtros](#recommendations-list-now-includes-filters)
- [Mejora y ampliación de la experiencia de aprovisionamiento automático](#auto-provisioning-experience-improved-and-expanded)
- [La puntuación de seguridad ahora está disponible en la exportación continua (versión preliminar)](#secure-score-is-now-available-in-continuous-export-preview)
- [La recomendación "Las actualizaciones del sistema deben estar instaladas en las máquinas" ahora incluye recomendaciones secundarias](#system-updates-should-be-installed-on-your-machines-recommendation-now-includes-sub-recommendations)
- [En la página de administración de directivas de Azure Portal ahora se muestra el estado de las asignaciones de directivas predeterminadas](#policy-management-page-in-the-azure-portal-now-shows-status-of-default-policy-assignments)

### <a name="29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark"></a>Se han agregado 29 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark.

Azure Security Benchmark es el conjunto de directrices específico de Azure creado por Microsoft para ofrecer los procedimientos recomendados de seguridad y cumplimiento basados en marcos de cumplimiento comunes. [Mas información sobre Azure Security Benchmark](../security/benchmarks/introduction.md).

Se han agregado a Security Center las siguientes 29 recomendaciones en versión preliminar para aumentar la cobertura de la prueba comparativa.

Las recomendaciones de la versión preliminar no representan un recurso incorrecto y no se incluyen en los cálculos de una puntuación segura. Corríjalas siempre que sea posible, de tal forma que, cuando finalice el período de versión preliminar, contribuyan a la puntuación. Puede encontrar más información sobre cómo responder a estas recomendaciones en [Recomendaciones de corrección en Azure Security Center](security-center-remediate-recommendations.md).

| Control de seguridad                     | Nuevas recomendaciones                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cifrado de los datos en tránsito              | - La aplicación de la conexión SSL debe estar habilitada para los servidores de base de datos PostgreSQL.<br>- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos MySQL.<br>- TLS debe actualizarse a la versión más reciente en la aplicación de API.<br>- TLS debe actualizarse a la versión más reciente en la aplicación de funciones.<br>- TLS debe actualizarse a la versión más reciente en la aplicación web.<br>- Es necesario exigir FTPS en la aplicación de API.<br>- Es necesario exigir FTPS en la aplicación de funciones.<br>- Es necesario exigir FTPS en la aplicación web.                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Administración de acceso y permisos        | - Las aplicaciones web deben solicitar un certificado SSL a todas las solicitudes entrantes.<br>- Se debe usar una identidad administrada en la aplicación de API.<br>- Se debe usar una identidad administrada en la aplicación de funciones.<br>- Se debe usar una identidad administrada en la aplicación web.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Restricción de los accesos de red no autorizados | - El punto de conexión privado debe estar habilitado para servidores PostgreSQL.<br>- El punto de conexión privado debe estar habilitado para servidores MariaDB.<br>- El punto de conexión privado debe estar habilitado para servidores MySQL.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Habilitar auditoría y registro          | - Los registros de diagnóstico de App Services deben estar habilitados                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Implementación de procedimientos recomendados de seguridad    | - Azure Backup debería habilitarse en las máquinas virtuales.<br>- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MariaDB.<br>- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MySQL.<br>- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for PostgreSQL.<br>- PHP debe actualizarse a la versión más reciente en la aplicación de API.<br>- PHP debe actualizarse a la versión más reciente en la aplicación web.<br>- Java debe actualizarse a la versión más reciente en la aplicación de API.<br>- Java debe actualizarse a la versión más reciente en la aplicación de funciones.<br>- Java debe actualizarse a la versión más reciente en la aplicación web.<br>- Python debe actualizarse a la versión más reciente en la aplicación de API.<br>- Python debe actualizarse a la versión más reciente en la aplicación de funciones.<br>- Python debe actualizarse a la versión más reciente en la aplicación web.<br>- La retención de la auditoría en los servidores SQL Server debe establecerse en 90 días, como mínimo. |
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

Vínculos relacionados:

- [Más información sobre Azure Security Benchmark](../security/benchmarks/introduction.md)
- [Más información sobre las aplicaciones de API de Azure](../app-service/app-service-web-tutorial-rest-api.md)
- [Más información sobre las aplicaciones de funciones de Azure](../azure-functions/functions-overview.md)
- [Más información sobre las aplicaciones web de Azure](../app-service/overview.md)
- [Más información sobre Azure Database for MariaDB](../mariadb/overview.md)
- [Más información sobre Azure Database for MySQL](../mysql/overview.md)
- [Más información sobre Azure Database for PostgreSQL](../postgresql/overview.md)


### <a name="nist-sp-800-171-r2-added-to-security-centers-regulatory-compliance-dashboard"></a>Se ha agregado NIST SP 800 171 R2 al panel de cumplimiento normativo de Security Center.

La norma NIST SP 800-171 R2 ahora está disponible como una iniciativa integrada para su uso con el panel de cumplimiento normativo de Azure Security Center. Las asignaciones de los controles se describen en [Detalles de la iniciativa integrada de cumplimiento normativo NIST SP 800-171 R2](../governance/policy/samples/nist-sp-800-171-r2.md). 

Para aplicar la norma a sus suscripciones y supervisar continuamente el estado de cumplimiento, siga las instrucciones que se describen en [Personalización del conjunto de normas del panel de cumplimiento normativo](update-regulatory-compliance-packages.md).

:::image type="content" source="media/release-notes/nist-sp-800-171-r2-standard.png" alt-text="Norma NIST SP 800 171 R2 en el panel de cumplimiento normativo de Security Center":::

Para más información acerca de este estándar normativo, consulte [NIST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final).


### <a name="recommendations-list-now-includes-filters"></a>La lista de recomendaciones ahora incluye filtros

Ahora puede filtrar la lista de recomendaciones de seguridad de acuerdo con una gama de criterios. En el ejemplo siguiente, la lista de recomendaciones se ha filtrado para mostrar recomendaciones que:

- estén **disponibles con carácter general** (es decir, no en versión preliminar).
- sean para **cuentas de almacenamiento**.
- admitan las correcciones de **Corrección rápida**.

:::image type="content" source="media/release-notes/recommendations-filters.png" alt-text="Filtros de la lista de recomendaciones":::


### <a name="auto-provisioning-experience-improved-and-expanded"></a>Mejora y ampliación de la experiencia de aprovisionamiento automático

La característica de aprovisionamiento automático ayuda a reducir la sobrecarga de administración mediante la instalación de las extensiones necesarias en máquinas virtuales de Azure nuevas y existentes para que puedan beneficiarse de la protección de Security Center. 

A medida que crece Azure Security Center, se han desarrollado más extensiones y Security Center puede supervisar una lista más amplia de tipos de recursos. El aprovisionamiento automático se ha ampliado para admitir tipos de recursos y extensiones adicionales que aprovechan las funcionalidades de Azure Policy.

Ahora puede configurar el aprovisionamiento automático de:

- Agente de Log Analytics
- (Nuevo) Complemento de Azure Policy para Kubernetes
- (Nuevo) Microsoft Dependency Agent

Obtenga más información en [Aprovisionamiento automático de agentes y extensiones de Azure Security Center](security-center-enable-data-collection.md).


### <a name="secure-score-is-now-available-in-continuous-export-preview"></a>La puntuación de seguridad ahora está disponible en la exportación continua (versión preliminar)

Con la exportación continua de la puntuación de seguridad, puede transmitir los cambios de la puntuación en tiempo real a Azure Event Hubs o a un área de trabajo de Log Analytics. Use esta funcionalidad para:

- Realizar un seguimiento de la puntuación de seguridad en el tiempo con informes dinámicos
- Exportar datos de puntuación de seguridad a Azure Sentinel (o a cualquier otro SIEM)
- Integrar estos datos con cualquier proceso que ya esté usando para supervisar la puntuación de seguridad en la organización

Obtenga más información sobre la [Exportación continua de datos de Security Center](continuous-export.md).


### <a name="system-updates-should-be-installed-on-your-machines-recommendation-now-includes-sub-recommendations"></a>La recomendación "Las actualizaciones del sistema deben estar instaladas en las máquinas" ahora incluye recomendaciones secundarias

La recomendación **Las actualizaciones del sistema deben estar instaladas en las máquinas** se ha mejorado. La nueva versión incluye recomendaciones secundarias para cada actualización que falta y ofrece las siguientes mejoras:

- Una nueva experiencia en las páginas de Azure Security Center de Azure Portal. La página de detalles de la recomendación **Las actualizaciones del sistema deben estar instaladas en las máquinas** incluye la lista de conclusiones que se muestran a continuación. Al seleccionar una única búsqueda, se abre el panel de detalles con un vínculo a la información de corrección y una lista de los recursos afectados.

    :::image type="content" source="./media/upcoming-changes/system-updates-should-be-installed-subassessment.png" alt-text="Apertura de una de las recomendaciones secundarias en la experiencia del portal para la recomendación actualizada":::

- Datos enriquecidos para la recomendación de Azure Resource Graph (ARG). ARG es un servicio de Azure diseñado para proporcionar una exploración de recursos eficaz. Puede usar ARG para realizar consultas a escala para un conjunto determinado de suscripciones a fin de controlar eficazmente su entorno. 

    Para Azure Security Center, puede usar ARG y el [lenguaje de consulta Kusto (KQL)](/azure/data-explorer/kusto/query/) para consultar una amplia variedad de datos de posición de seguridad.

    Anteriormente, si consultaba esta recomendación en ARG, la única información disponible era que la recomendación debía corregirse en una máquina. La siguiente consulta de la versión mejorada devolverá las actualizaciones del sistema que faltan agrupadas por máquina.

    ```kusto
    securityresources
    | where type =~ "microsoft.security/assessments/subassessments"
    | where extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id) == "4ab6e3c5-74dd-8b35-9ab9-f61b30875b27"
    | where properties.status.code == "Unhealthy"
    ```

### <a name="policy-management-page-in-the-azure-portal-now-shows-status-of-default-policy-assignments"></a>En la página de administración de directivas de Azure Portal ahora se muestra el estado de las asignaciones de directivas predeterminadas

Ahora puede ver si las suscripciones tienen la directiva de Security Center predeterminada asignada, en la página **Directiva de seguridad** de Security Center en Azure Portal.

:::image type="content" source="media/release-notes/policy-assignment-info-per-subscription.png" alt-text="En la página de administración de directivas de Azure Security Center se muestran las asignaciones de directivas predeterminadas":::

## <a name="october-2020"></a>Octubre de 2020

Las actualizaciones de octubre incluyen:
- [Evaluación de vulnerabilidades para máquinas locales y en varias nubes (versión preliminar)](#vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview)
- [Recomendación sobre Azure Firewall agregada (versión preliminar)](#azure-firewall-recommendation-added-preview)
- [La recomendación de que los intervalos IP autorizados deben definirse en los servicios de Kubernetes se ha actualizado con una corrección rápida](#authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix)
- [El panel de cumplimiento normativo ahora incluye la opción para eliminar estándares](#regulatory-compliance-dashboard-now-includes-option-to-remove-standards)
- [Tabla Microsoft.Security/securityStatuses quitada de Azure Resource Graph (ARG)](#microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg)

### <a name="vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview"></a>Evaluación de vulnerabilidades para máquinas locales y en varias nubes (versión preliminar)

El analizador de evaluación de vulnerabilidades integrado de [Azure Defender para servidores](defender-for-servers-introduction.md) (con tecnología de Qualys) ahora analiza los servidores habilitados para Azure Arc.

Al habilitar Azure Arc en máquinas que no son de Azure, Security Center le ofrecerá la opción de implementar en ellas el analizador de vulnerabilidades integrado, manualmente y a escala.

Con esta actualización, puede aprovechar toda la capacidad de **Azure Defender para servidores** para consolidar su programa de administración de vulnerabilidades en todos sus recursos, tanto si son de Azure como si no.

Principales funcionalidades:

- Supervisión del estado de aprovisionamiento del analizador de evaluación de vulnerabilidades en máquinas de Azure Arc.
- Aprovisionamiento del agente de evaluación de vulnerabilidades integrado en máquinas de Azure Arc Windows y Linux (manualmente y a escala).
- Recepción y análisis de vulnerabilidades detectadas por agentes implementados (manualmente y a escala).
- Experiencia unificada para máquinas virtuales de Azure y máquinas de Azure Arc.

[Obtenga más información sobre la implementación del analizador de vulnerabilidades integrado en las máquinas híbridas](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

[Obtenga más información sobre los servidores habilitados para Azure Arc](../azure-arc/servers/index.yml).


### <a name="azure-firewall-recommendation-added-preview"></a>Recomendación sobre Azure Firewall agregada (versión preliminar)

Se ha agregado una nueva recomendación para proteger todas las redes virtuales con Azure Firewall.

La recomendación **Azure Firewall debe proteger las redes virtuales** le aconseja restringir el acceso a las redes virtuales y evitar posibles amenazas mediante el uso de Azure Firewall.

Más información acerca de [Azure Firewall](https://azure.microsoft.com/services/azure-firewall/).


### <a name="authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix"></a>La recomendación de que los intervalos IP autorizados deben definirse en los servicios de Kubernetes se ha actualizado con una corrección rápida

La recomendación **Authorized IP ranges should be defined on Kubernetes Services** (Los intervalos IP autorizados deben definirse en los servicios de Kubernetes) ahora tiene una opción de corrección rápida.

Para más información sobre esta y todas las demás recomendaciones de Security Center, consulte [Guía de referencia sobre las recomendaciones de seguridad](recommendations-reference.md).

:::image type="content" source="./media/release-notes/authorized-ip-ranges-recommendation.png" alt-text="La recomendación Authorized IP ranges should be defined on Kubernetes Services (Los intervalos IP autorizados deben definirse en los servicios de Kubernetes) con la opción de corrección rápida":::


### <a name="regulatory-compliance-dashboard-now-includes-option-to-remove-standards"></a>El panel de cumplimiento normativo ahora incluye la opción para eliminar estándares

En el panel de cumplimiento normativo de Security Center se proporciona información sobre su postura de cumplimiento en función de cómo cumple los requisitos y controles de cumplimiento específicos.

El panel incluye un conjunto predeterminado de estándares normativos. Si alguno de ellos no es pertinente para su organización, quitarlo de la interfaz de usuario ahora es un proceso sencillo en una suscripción. Los estándares se pueden quitar solo en el nivel de *suscripción*, no en el ámbito del grupo de administración.

Encontrará más información en [Eliminación de un estándar del panel](update-regulatory-compliance-packages.md#removing-a-standard-from-your-dashboard).


### <a name="microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg"></a>Tabla Microsoft.Security/securityStatuses quitada de Azure Resource Graph (ARG)

Azure Resource Graph es un servicio de Azure diseñado para proporcionar una exploración de recursos eficaz con la capacidad de consultar a escala a través de un conjunto especificado de suscripciones para que pueda controlar eficazmente el entorno. 

Para Azure Security Center, puede usar ARG y el [lenguaje de consulta Kusto (KQL)](/azure/data-explorer/kusto/query/) para consultar una amplia variedad de datos de posición de seguridad. Por ejemplo:

- El inventario de recursos utiliza ARG.
- Hemos documentado una consulta de ARG de ejemplo sobre la [Identificación de cuentas sin Multi-Factor Authentication (MFA) habilitado](security-center-identity-access.md#identify-accounts-without-multi-factor-authentication-mfa-enabled).

Dentro de ARG hay tablas de datos que se pueden usar en las consultas.

:::image type="content" source="./media/release-notes/azure-resource-graph-tables.png" alt-text="Explorador de Azure Resource Graph y tablas disponibles":::

> [!TIP]
> La documentación de ARG muestra todas las tablas disponibles en la [tabla de Azure Resource Graph y la referencia de tipo de recurso](../governance/resource-graph/reference/supported-tables-resources.md).

A partir de esta actualización, se ha quitado la tabla **Microsoft.Security/securityStatuses**. La API securityStatuses sigue estando disponible.

La tabla Microsoft.Security/Assessments puede usar sustitución de datos.

La diferencia principal entre Microsoft.Security/securityStatuses y Microsoft.Security/Assessments es que, mientras que la primera muestra la agregación de las evaluaciones, la segunda contienen un único registro para cada una.

Por ejemplo, Microsoft.Security/securityStatuses devolvería un resultado con una matriz de dos policyAssessments:

```
{
id: "/subscriptions/449bcidd-3470-4804-ab56-2752595 felab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/securityStatuses/mico-rg-vnet",
name: "mico-rg-vnet",
type: "Microsoft.Security/securityStatuses",
properties:  {
    policyAssessments: [
        {assessmentKey: "e3deicce-f4dd-3b34-e496-8b5381bazd7e", category: "Networking", policyName: "Azure DDOS Protection Standard should be enabled",...},
        {assessmentKey: "sefac66a-1ec5-b063-a824-eb28671dc527", category: "Compute", policyName: "",...}
    ],
    securitystateByCategory: [{category: "Networking", securityState: "None" }, {category: "Compute",...],
    name: "GenericResourceHealthProperties",
    type: "VirtualNetwork",
    securitystate: "High"
}
```
Por su parte, Microsoft.Security/Assessments contendrá un registro para cada una de estas evaluaciones de directivas como se indica a continuación:

```
{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft. Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/e3delcce-f4dd-3b34-e496-8b5381ba2d70",
name: "e3deicce-f4dd-3b34-e496-8b5381ba2d70",
properties:  {
    resourceDetails: {Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet"...},
    displayName: "Azure DDOS Protection Standard should be enabled",
    status: (code: "NotApplicable", cause: "VnetHasNOAppGateways", description: "There are no Application Gateway resources attached to this Virtual Network"...}
}

{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/80fac66a-1ec5-be63-a824-eb28671dc527",
name: "8efac66a-1ec5-be63-a824-eb28671dc527",
properties: {
    resourceDetails: (Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet"...),
    displayName: "Audit diagnostic setting",
    status:  {code: "Unhealthy"}
}
```

**Ejemplo de conversión de una consulta de ARG existente usando securityStatuses para usar ahora la tabla Assessments:**

Consulta que hace referencia a SecurityStatuses:

```kusto
SecurityResources 
| where type == 'microsoft.security/securitystatuses' and properties.type == 'virtualMachine'
| where name in ({vmnames}) 
| project name, resourceGroup, policyAssesments = properties.policyAssessments, resourceRegion = location, id, resourceDetails = properties.resourceDetails
```

Consulta de sustitución para la tabla Assessments:

```kusto
securityresources
| where type == "microsoft.security/assessments" and id contains "virtualMachine"
| extend resourceName = extract(@"(?i)/([^/]*)/providers/Microsoft.Security/assessments", 1, id)
| extend source = tostring(properties.resourceDetails.Source)
| extend resourceId = trim(" ", tolower(tostring(case(source =~ "azure", properties.resourceDetails.Id,
source =~ "aws", properties.additionalData.AzureResourceId,
source =~ "gcp", properties.additionalData.AzureResourceId,
extract("^(.+)/providers/Microsoft.Security/assessments/.+$",1,id)))))
| extend resourceGroup = tolower(tostring(split(resourceId, "/")[4]))
| where resourceName in ({vmnames}) 
| project resourceName, resourceGroup, resourceRegion = location, id, resourceDetails = properties.additionalData
```

Para más información, consulte los siguientes vínculos:
- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)
- [Lenguaje de consulta de Kusto (KQL)](/azure/data-explorer/kusto/query/)


## <a name="september-2020"></a>Septiembre de 2020

Las actualizaciones de septiembre incluyen:
- [Security Center tiene un nuevo aspecto](#security-center-gets-a-new-look)
- [Lanzamiento de Azure Defender](#azure-defender-released)
- [Azure Defender para Key Vault está disponible con carácter general](#azure-defender-for-key-vault-is-generally-available)
- [La protección de Azure Defender para Storage en archivos y ADLS Gen2 está disponible con carácter general](#azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available)
- [Las herramientas para el inventario de recursos ya están disponibles con carácter general](#asset-inventory-tools-are-now-generally-available)
- [Deshabilitación de la detección de una vulnerabilidad específica para análisis de registros de contenedores y máquinas virtuales](#disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines)
- [Exclusión de un recurso de una recomendación](#exempt-a-resource-from-a-recommendation)
- [Los conectores de AWS y GCP de Security Center aportan una experiencia multinube](#aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience)
- [Conjunto de recomendaciones de protección de cargas de trabajo de Kubernetes](#kubernetes-workload-protection-recommendation-bundle)
- [Los resultados de la evaluación de vulnerabilidades ahora están disponibles en la exportación continua](#vulnerability-assessment-findings-are-now-available-in-continuous-export)
- [Imponer recomendaciones al crear recursos para prevenir errores de configuración de seguridad](#prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources)
- [Mejoras de las recomendaciones del grupo de seguridad de red](#network-security-group-recommendations-improved)
- [La recomendación de AKS en versión preliminar "Se deben definir las directivas de seguridad de pod en los servicios de Kubernetes" está en desuso](#deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services)
- [Notificaciones por correo electrónico mejoradas en Azure Security Center](#email-notifications-from-azure-security-center-improved)
- [La puntuación de seguridad no incluye recomendaciones en versión preliminar](#secure-score-doesnt-include-preview-recommendations)
- [Las recomendaciones incluyen ahora un indicador de gravedad y el intervalo de actualización](#recommendations-now-include-a-severity-indicator-and-the-freshness-interval)


### <a name="security-center-gets-a-new-look"></a>Security Center tiene un nuevo aspecto

Hemos lanzado una interfaz de usuario actualizada para las páginas del portal de Security Center. Las nuevas páginas incluyen una nueva página de información general, así como paneles para la puntuación de seguridad, el inventario de recursos y Azure Defender.

La página de información general renovada ahora incorpora un icono para acceder a los paneles de puntuación de seguridad, inventario de recursos y Azure Defender. También tiene un icono vinculado al panel de cumplimiento normativo.

Más información sobre la [página de información general](overview-page.md)


### <a name="azure-defender-released"></a>Lanzamiento de Azure Defender

**Azure Defender** es la plataforma de protección de cargas de trabajo en la nube (CWPP) integrada en Security Center para la protección avanzada e inteligente de las cargas de trabajo híbridas y de Azure. Reemplaza a la opción del plan de tarifa estándar de Security Center. 

Al habilitar Azure Defender en el área **Precios y configuración** de Azure Security Center, se habilitan a la vez los siguientes planes de Defender, que proporcionan defensas completas para las capas de proceso, datos y servicio de su entorno:

- [Azure Defender para servidores](defender-for-servers-introduction.md)
- [Azure Defender para App Service](defender-for-app-service-introduction.md)
- [Azure Defender para Storage](defender-for-storage-introduction.md)
- [Azure Defender para SQL](defender-for-sql-introduction.md)
- [Azure Defender para Key Vault](defender-for-key-vault-introduction.md)
- [Azure Defender para Kubernetes](defender-for-kubernetes-introduction.md)
- [Azure Defender para registros de contenedor](defender-for-container-registries-introduction.md)

Cada uno de estos planes se explica por separado en la documentación de Security Center.

Con su panel dedicado, Azure Defender proporciona alertas de seguridad y protección contra amenazas avanzada para máquinas virtuales, bases de datos SQL, contenedores, aplicaciones web, la red y mucho más.

[Más información sobre Azure Defender](azure-defender.md)

### <a name="azure-defender-for-key-vault-is-generally-available"></a>Azure Defender para Key Vault está disponible con carácter general

Azure Key Vault es un servicio en la nube que protege las claves de cifrado y los secretos, como certificados, cadenas de conexión y contraseñas. 

**Azure Defender para Key Vault** ofrece protección frente a amenazas avanzada, nativa de Azure, para Azure Key Vault, que proporciona una capa adicional de inteligencia de seguridad. Por extensión, Azure Defender para Key Vault protege por consiguiente muchos de los recursos que dependen de las cuentas de Key Vault.

El plan opcional ahora está en disponibilidad general. Esta característica se encontraba en versión preliminar como "protección contra amenazas avanzada para Azure Key Vault".

Además, las páginas de Key Vault de Azure Portal ahora incluyen una página de **Seguridad** dedicada para las recomendaciones y las alertas de **Security Center**.

Más información en [Azure Defender para Key Vault](defender-for-key-vault-introduction.md)


### <a name="azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available"></a>La protección de Azure Defender para Storage en archivos y ADLS Gen2 está disponible con carácter general 

**Azure Defender para Storage** detecta actividades potencialmente dañinas en las cuentas de Azure Storage. Los datos se pueden proteger tanto si se almacenan como contenedores de blobs, recursos compartidos de archivos o lagos de datos.

La compatibilidad con [Azure Files](../storage/files/storage-files-introduction.md) y [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) ahora está disponible con carácter general.

Desde el 1 de octubre de 2020, comenzaremos a cobrar por la protección de los recursos en estos servicios.

Más información en [Azure Defender para Storage](defender-for-storage-introduction.md)


### <a name="asset-inventory-tools-are-now-generally-available"></a>Las herramientas para el inventario de recursos ya están disponibles con carácter general

La página de inventario de recursos de Azure Security Center proporciona una sola página para ver la posición de seguridad de los recursos que se han conectado a Security Center.

Security Center analiza periódicamente el estado de seguridad de los recursos de Azure para identificar posibles puntos vulnerables de la seguridad. A continuación, se proporcionan recomendaciones sobre cómo corregir dichos puntos vulnerables.

Cuando algún recurso tenga recomendaciones pendientes, aparecerán en el inventario.

Más información en [Exploración y administración de los recursos con Asset Inventory](asset-inventory.md).



### <a name="disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines"></a>Deshabilitación de la detección de una vulnerabilidad específica para análisis de registros de contenedores y máquinas virtuales

Azure Defender incluye detectores de vulnerabilidades para examinar imágenes en Azure Container Registry y en las máquinas virtuales.

Si tiene una necesidad organizativa de omitir un resultado, en lugar de corregirlo, tiene la opción de deshabilitarlo. Los resultados deshabilitados no afectan a la puntuación de seguridad ni generan ruido no deseado.

Cuando un resultado coincide con los criterios que ha definido en las reglas de deshabilitación, no aparecerá en la lista de resultados.

Esta opción está disponible en las páginas de detalles de recomendaciones para:

- **Se deben corregir las vulnerabilidades de las imágenes de Azure Container Registry**
- **Es necesario corregir las vulnerabilidades de las máquinas virtuales**

Obtenga más información en [Deshabilitación de resultados específicos para las imágenes de contenedor](defender-for-container-registries-usage.md#disable-specific-findings-preview) y [Deshabilitación de resultados específicos para las máquinas virtuales](remediate-vulnerability-findings-vm.md#disable-specific-findings-preview).


### <a name="exempt-a-resource-from-a-recommendation"></a>Exclusión de un recurso de una recomendación

En ocasiones, un recurso se mostrará como incorrecto en relación con una recomendación específica y, por lo tanto, se reduce la puntuación de seguridad, aunque crea que no debería ser así. Es posible que lo haya corregido un proceso del que Security Center no ha realizado un seguimiento. O, quizás, la organización ha decidido aceptar el riesgo de ese recurso específico. 

En tales casos, puede crear una regla de exención para asegurarse de que el recurso no aparezca entre los recursos incorrectos en el futuro. Estas reglas pueden incluir justificaciones documentadas, tal y como se describe a continuación.

Más información en [Exención de un recurso de las recomendaciones y la puntuación de seguridad](exempt-resource.md)


### <a name="aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience"></a>Los conectores de AWS y GCP de Security Center aportan una experiencia multinube

Las cargas de trabajo de nube abarcan normalmente varias plataformas de nube, por lo que los servicios de seguridad de la nube deben hacer lo mismo.

Azure Security Center ahora protege las cargas de trabajo de Azure, Amazon Web Services (AWS) y Google Cloud Platform (GCP).

Al incorporar las cuentas de AWS y GCP a Security Center, se integran AWS Security Hub, el centro de comandos de seguridad GCP y Azure Security Center. 

Obtenga más información en [Conexión de las cuentas de AWS a Azure Security Center](quickstart-onboard-aws.md) y [Conexión de las cuentas de GCP a Azure Security Center](quickstart-onboard-gcp.md).


### <a name="kubernetes-workload-protection-recommendation-bundle"></a>Conjunto de recomendaciones de protección de cargas de trabajo de Kubernetes

Para asegurarse de que las cargas de trabajo de Kubernetes son seguras de forma predeterminada, Security Center está agregando recomendaciones de protección en el nivel de Kubernetes, incluidas las opciones de cumplimiento con el control de admisión de Kubernetes.

Una vez instalado el complemento de Azure Policy para Kubernetes en el clúster de AKS, todas las solicitudes al servidor de la API de Kubernetes se supervisarán según el conjunto predefinido de procedimientos recomendados antes de que se guarden en el clúster. Después, puede realizar la configurar para aplicar los procedimientos recomendados y exigirlos para futuras cargas de trabajo.

Por ejemplo, puede exigir que no se creen los contenedores con privilegios y que se bloqueen las solicitudes futuras para este fin.

Más información en [Procedimientos recomendados de protección de cargas de trabajo con el control de admisión de Kubernetes](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)


### <a name="vulnerability-assessment-findings-are-now-available-in-continuous-export"></a>Los resultados de la evaluación de vulnerabilidades ahora están disponibles en la exportación continua

Use la exportación continua para transmitir sus alertas y recomendaciones en tiempo real a Azure Event Hubs, áreas de trabajo de Log Analytics o Azure Monitor. A partir de ahí, puede integrar estos datos con SIEM (como Azure Sentinel, Power BI, Azure Data Explorer y mucho más).

Las herramientas de evaluación de vulnerabilidades integradas en Security Center devuelven resultados sobre los recursos, como recomendaciones útiles dentro de una recomendación "principal", tipo "Es necesario corregir las vulnerabilidades de las máquinas virtuales.". 

Los resultados de seguridad ahora están disponibles para la exportación mediante la exportación continua al seleccionar recomendaciones y habilitar la opción **include security findings** (Incluir resultados de seguridad).

:::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="Incluir la alternancia de los resultados de seguridad en la configuración de la exportación continua" :::

Páginas relacionadas:

- [Solución de evaluación de vulnerabilidades integrada en Security Center para Azure Virtual Machines](deploy-vulnerability-assessment-vm.md)
- [Solución de evaluación de vulnerabilidades integrada en Security Center para las imágenes de Azure Container Registry](defender-for-container-registries-usage.md)
- [Exportación continua](continuous-export.md)

### <a name="prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources"></a>Imponer recomendaciones al crear recursos para prevenir errores de configuración de seguridad

Los errores de configuración de seguridad son una causa principal de los incidentes de seguridad. Security Center ofrece ahora la posibilidad de ayudar a *evitar* errores de configuración de los nuevos recursos con respecto a recomendaciones específicas. 

Esta característica puede ayudar a mantener las cargas de trabajo seguras y a estabilizar la puntuación de seguridad.

La aplicación de una configuración segura, basada en una recomendación específica, se ofrece mediante dos opciones:

- Con el efecto **Denegar** de Azure Policy, puede impedir que se creen recursos incorrectos.

- Con la opción **Exigir**, puede sacar partido del efecto de **DeployIfNotExist** de Azure Policy y corregir automáticamente los recursos no compatibles tras la creación.
 
Está disponible para las recomendaciones de seguridad seleccionadas y puede encontrarse en la parte superior de la página de detalles del recurso.

Más información en [Impedir errores de configuración con las recomendaciones Exigir/Denegar](prevent-misconfigurations.md).

###  <a name="network-security-group-recommendations-improved"></a>Mejoras de las recomendaciones del grupo de seguridad de red

Se han mejorado las siguientes recomendaciones de seguridad relacionadas con los grupos de seguridad de red para reducir algunas instancias de falsos positivos.

- Todos los puertos de red deben estar restringidos en el NSG asociado a la máquina virtual
- Se deben cerrar los puertos de administración en las máquinas virtuales
- Las máquinas virtuales accesibles desde Internet deben estar protegidas con grupos de seguridad de red
- Las subredes deben estar asociadas con un grupo de seguridad de red.


### <a name="deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services"></a>La recomendación de AKS en versión preliminar "Se deben definir las directivas de seguridad de pod en los servicios de Kubernetes" está en desuso

La recomendación en versión preliminar "Las directivas de seguridad de pod deben definirse en los servicios de Kubernetes" está en desuso, tal y como se describe en la documentación de [Azure Kubernetes Service](../aks/use-pod-security-policies.md).

La característica de la directiva de seguridad de pod (versión preliminar), está programada para quedar en desuso y dejará de estar disponible a partir del 15 de octubre de 2020 en favor de Azure Policy para AKS.

Una vez que la directiva de seguridad de pod (versión preliminar) haya quedado en desuso, deberá deshabilitar la característica en todos los clústeres existentes que la incluyan para realizar futuras actualizaciones de clústeres y seguir recibiendo el soporte técnico de Azure.


### <a name="email-notifications-from-azure-security-center-improved"></a>Notificaciones por correo electrónico mejoradas en Azure Security Center

Se han mejorado las siguientes áreas de los correos electrónicos con respecto a las alertas de seguridad: 

- Se ha agregado la capacidad de enviar notificaciones por correo electrónico sobre las alertas de todos los niveles de gravedad.
- Se ha agregado la capacidad de notificar a los usuarios con diferentes roles de Azure en la suscripción.
- Se notifica de forma proactiva y predeterminada a los propietarios de la suscripción sobre las alertas de gravedad alta (con una probabilidad elevada de que se produzcan infracciones genuinas).
- Se ha eliminado el campo de número de teléfono de la página de configuración de notificaciones por correo electrónico.

Más información en [Configuración de notificaciones de alertas de seguridad por correo electrónico](security-center-provide-security-contact-details.md)


### <a name="secure-score-doesnt-include-preview-recommendations"></a>La puntuación de seguridad no incluye recomendaciones en versión preliminar 

Security Center evalúa continuamente los recursos, suscripciones y la organización en busca de problemas de seguridad. A continuación, agrega todos los resultados a una sola puntuación para que pueda conocer de un vistazo la situación de la seguridad actual: cuanto mayor sea la puntuación, menor será el nivel de riesgo identificado.

A medida que se detectan nuevas amenazas, se habilitan nuevos consejos de seguridad en Security Center mediante recomendaciones nuevas. Para evitar cambios inesperados en la puntuación de seguridad y ofrecer un período de gracia en el que pueda explorar nuevas recomendaciones antes de que afecten a las puntuaciones, las recomendaciones marcadas como **Versión preliminar** ya no se incluyen en los cálculos de la puntuación de seguridad. Aun así, deben corregirse siempre que sea posible, de tal forma que, cuando finalice el período de versión preliminar, contribuyan a la puntuación.

Además, las recomendaciones en **Versión preliminar** no representan un recurso "Incorrecto".

Un ejemplo de una recomendación en versión preliminar:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Recomendación con la marca de versión preliminar":::

[Más información sobre la puntuación de seguridad](secure-score-security-controls.md)


### <a name="recommendations-now-include-a-severity-indicator-and-the-freshness-interval"></a>Las recomendaciones incluyen ahora un indicador de gravedad y el intervalo de actualización

La página de detalles de las recomendaciones incluye ahora un indicador del intervalo de actualización (siempre que sea pertinente) y una visualización clara de la gravedad de la recomendación.

:::image type="content" source="./media/release-notes/recommendations-severity-freshness-indicators.png" alt-text="Página de recomendaciones que muestra la actualización y la gravedad":::



## <a name="august-2020"></a>Agosto de 2020

Las actualizaciones de agosto incluyen:

- [Inventario de recursos: nueva vista eficaz de la posición de seguridad de sus recursos](#asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets)
- [Compatibilidad agregada con los valores predeterminados de seguridad de Azure Active Directory (para la autenticación multifactor)](#added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication)
- [Recomendación de entidades de servicio agregada](#service-principals-recommendation-added)
- [Evaluación de vulnerabilidades en máquinas virtuales: recomendaciones y directivas consolidadas](#vulnerability-assessment-on-vms---recommendations-and-policies-consolidated)
- [Se han agregado nuevas directivas de seguridad de AKS a la iniciativa de ASC_default para su uso solo por parte de clientes de versión preliminar privada](#new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only)


### <a name="asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets"></a>Inventario de recursos: nueva vista eficaz de la posición de seguridad de sus recursos

El inventario de recursos de Security Center (actualmente en versión preliminar) proporciona una manera de ver la posición de seguridad de los recursos que ha conectado a Security Center.

Security Center analiza periódicamente el estado de seguridad de los recursos de Azure para identificar posibles puntos vulnerables de la seguridad. A continuación, se proporcionan recomendaciones sobre cómo corregir dichos puntos vulnerables. Cuando algún recurso tenga recomendaciones pendientes, aparecerán en el inventario.

Puede usar la vista y sus filtros para explorar los datos de la posición de seguridad y realizar acciones adicionales en función de los resultados.

Obtenga más información sobre el [inventario de recursos](asset-inventory.md).


### <a name="added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication"></a>Compatibilidad agregada con los valores predeterminados de seguridad de Azure Active Directory (para la autenticación multifactor)

Security Center ha agregado compatibilidad total con los [valores predeterminados de seguridad](../active-directory/fundamentals/concept-fundamentals-security-defaults.md), las protecciones de seguridad de la identidad gratuitas de Microsoft.

Los valores predeterminados de seguridad proporcionan una configuración de seguridad de la identidad preconfigurada para proteger su organización frente a ataques habituales relacionados con la identidad. Los valores predeterminados de seguridad ya protegen más de 5 millones de inquilinos en total; 50 000 inquilinos también reciben la protección de Security Center.

Security Center ahora proporciona una recomendación de seguridad cada vez que identifica una suscripción de Azure sin valores predeterminados de seguridad habilitados. Hasta ahora, Security Center recomendaba habilitar la autenticación multifactor mediante el acceso condicional, que forma parte de la licencia Premium de Azure Active Directory (AD). Para los clientes que usen Azure AD de forma gratuita, ahora recomendamos que se habiliten los valores predeterminados de seguridad. 

Nuestro objetivo es animar a más clientes a proteger sus entornos en la nube con MFA y mitigar uno de los mayores riesgos que también es el que tiene más impacto en su [puntuación de seguridad](secure-score-security-controls.md).

Obtenga más información sobre los [valores predeterminados de seguridad](../active-directory/fundamentals/concept-fundamentals-security-defaults.md).


### <a name="service-principals-recommendation-added"></a>Recomendación de entidades de servicio agregada

Se ha agregado una nueva recomendación para recomendar que los clientes de Security Center que usan certificados de administración para administrar sus suscripciones cambien a las entidades de servicio.

La recomendación, **Para proteger las suscripciones,deben usarse entidades de servicio en lugar de certificados de administración**, le aconseja usar entidades de servicio o Azure Resource Manager para administrar de forma más segura sus suscripciones. 

Obtenga más información sobre [Objetos de aplicación y de entidad de servicio de Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object).


### <a name="vulnerability-assessment-on-vms---recommendations-and-policies-consolidated"></a>Evaluación de vulnerabilidades en máquinas virtuales: recomendaciones y directivas consolidadas

Security Center inspecciona las máquinas virtuales para detectar si ejecutan alguna solución de valoración de las vulnerabilidades. Si no encuentra ninguna solución, Security Center recomienda simplificar la implementación.

Si se encuentran vulnerabilidades, Security Center realiza una recomendación en la que se resumen los resultados que se deben investigar y corregir, en caso de que sea necesario.

Para garantizar una experiencia coherente para todos los usuarios, independientemente del tipo de analizador que usen, se han unificado cuatro recomendaciones en las dos siguientes:

|Recomendación unificada|Descripción del cambio|
|----|:----|
|**Debe habilitarse una solución de evaluación de vulnerabilidades en sus máquinas virtuales**|Reemplaza las dos recomendaciones siguientes:<br> **•** Habilitar la solución de evaluación de vulnerabilidades integrada en las máquinas virtuales (con tecnología de Qualys) (ahora en desuso) (se incluye con el nivel Estándar).<br> **•** La solución de evaluación de vulnerabilidades debe instalarse en sus máquinas virtuales (ahora en desuso) (niveles gratuito y Estándar).|
|**Es necesario corregir las vulnerabilidades de las máquinas virtuales**|Reemplaza las dos recomendaciones siguientes:<br>**•** Corregir las vulnerabilidades que se encontraron en las máquinas virtuales (con tecnología de Qualys) (ahora en desuso).<br>**•** Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades (ahora en desuso).|
|||

Ahora usará la misma recomendación para implementar la extensión de evaluación de vulnerabilidades de Security Center o una solución con licencia privada ("BYOL") de un asociado como Qualys o Rapid7.

Además, cuando se detectan vulnerabilidades y se notifican a Security Center, una sola recomendación le avisará de los hallazgos, independientemente de la solución de evaluación de vulnerabilidades que los haya identificado.

#### <a name="updating-dependencies"></a>Actualización de dependencias

Si tiene scripts, consultas o automatizaciones que hacen referencia a las recomendaciones anteriores o a nombres o claves de directivas, use las tablas siguientes para actualizar las referencias:

##### <a name="before-august-2020"></a>Antes de agosto de 2020

|Recomendación|Ámbito|
|----|:----|
|**Habilitar la solución de evaluación de vulnerabilidades integrada en las máquinas virtuales (con tecnología de Qualys)**<br>Clave: 550e890b-e652-4d22-8274-60b3bdb24c63|Integrada|
|**Corregir las vulnerabilidades que se encontraron en las máquinas virtuales (con tecnología de Qualys)**<br>Clave: 1195afff-c881-495e-9bc5-1486211ae03f|Integrada|
|**La solución de evaluación de vulnerabilidades debe instalarse en sus máquinas virtuales**<br>Clave: 01b1ed4c-b733-4fee-b145-f23236e70cf3|BYOL|
|**Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades**<br>Clave: 71992a2a-d168-42e0-b10e-6b45fa2ecddb|BYOL|
||||


|Directiva|Ámbito|
|----|:----|
|**La evaluación de vulnerabilidades debe estar habilitada en las máquinas virtuales**<br>Id. de directiva: 501541f7-f7e7-4cd6-868c-4190fdad3ac9|Integrada|
|**Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades**<br>Id. de directiva: 760a85ff-6162-42b3-8d70-698e268f648c|BYOL|
||||


##### <a name="from-august-2020"></a>Desde agosto de 2020

|Recomendación|Ámbito|
|----|:----|
|**Debe habilitarse una solución de evaluación de vulnerabilidades en sus máquinas virtuales**<br>Clave: ffff0522-1e88-47fc-8382-2a80ba848f5d|Integrada + BYOL|
|**Es necesario corregir las vulnerabilidades de las máquinas virtuales**<br>Clave: 1195afff-c881-495e-9bc5-1486211ae03f|Integrada + BYOL|
||||

|Directiva|Ámbito|
|----|:----|
|[**La evaluación de vulnerabilidades debe estar habilitada en las máquinas virtuales**](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f501541f7-f7e7-4cd6-868c-4190fdad3ac9)<br>Id. de directiva: 501541f7-f7e7-4cd6-868c-4190fdad3ac9 |Integrada + BYOL|
||||


### <a name="new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only"></a>Se han agregado nuevas directivas de seguridad de AKS a la iniciativa de ASC_default para su uso solo por parte de clientes de versión preliminar privada

Para asegurarse de que las cargas de trabajo de Kubernetes son seguras de forma predeterminada, Security Center está agregando directivas en el nivel de Kubernetes y recomendaciones de protección, incluidas las opciones de cumplimiento con el control de admisión de Kubernetes.

La fase temprana de este proyecto incluye una versión preliminar privada y la adición de nuevas directivas (deshabilitadas de forma predeterminada) a la iniciativa ASC_default.

Puede omitir estas directivas con seguridad y no habrá ningún impacto en el entorno. Si desea habilitarlas, regístrese para obtener la versión preliminar en https://aka.ms/SecurityPrP y seleccione una de las opciones siguientes:

1. **Versión preliminar única**: para unirse solo a esta versión preliminar privada. Mencione explícitamente "ASC Continuous SCAN" como la versión preliminar a la que le gustaría unirse.
1. **Programa continuo**: para agregarse a esta y a futuras versiones preliminares privadas. Deberá completar un perfil y un contrato de privacidad.


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
- [Recomendaciones de seguridad para contenedores](recommendations-reference.md#recs-containers)



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
