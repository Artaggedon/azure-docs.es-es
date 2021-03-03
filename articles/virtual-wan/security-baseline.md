---
title: Base de referencia de seguridad de Azure para Virtual WAN
description: La base de referencia de seguridad de Virtual WAN proporciona una guía de procedimientos y recursos para implementar las recomendaciones de seguridad especificadas en Azure Security Benchmark.
author: msmbaldwin
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/24/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 6f487467b08332eea4ee19a7fb8836d843bd254f
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100582655"
---
# <a name="azure-security-baseline-for-virtual-wan"></a>Base de referencia de seguridad de Azure para Virtual WAN

Esta base de referencia de seguridad aplica la guía de la [versión 2.0 de Azure Security Benchmark](../security/benchmarks/overview.md) en Microsoft Azure Virtual WAN. Azure Security Benchmark proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. El contenido se agrupa por medio de los **controles de seguridad** definidos por Azure Security Benchmark y la guía relacionada aplicable a Virtual WAN. Se han excluido los **controles** no aplicables a Virtual WAN.

Para ver cómo Virtual WAN se asigna por completo a Azure Security Benchmark, consulte el [archivo de asignación de base de referencia de seguridad de Virtual WAN completo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Seguridad de redes

*Para más información, consulte [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-controls-v2-network-security.md).*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: implementación de la seguridad para el tráfico interno

**Guía**: Microsoft Azure Virtual WAN proporciona funcionalidades de enrutamiento personalizadas y ofrece cifrado del tráfico de ExpressRoute. La administración de todas las rutas la proporciona el enrutador del centro virtual, que también permite la conectividad de tránsito entre las redes virtuales. El cifrado del tráfico de ExpressRoute con Virtual WAN proporciona un tránsito cifrado entre las redes locales y las redes virtuales de Azure a través de ExpressRoute sin pasar por la red pública de Internet ni utilizar direcciones IP públicas. 

- [Cifrado del tráfico de ExpressRoute](virtual-wan-about.md#encryption)

- [Uso de Private Link en Virtual WAN](howto-private-link.md)

- [Escenarios de enrutamiento personalizados](scenario-any-to-any.md)

- [Documentación de la tabla de rutas personalizada](how-to-virtual-hub-routing.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ns-2-connect-private-networks-together"></a>NS-2: Conexión conjunta de redes privadas 

**Guía**: Microsoft Azure ExpressRoute ofrece conectividad privada con Azure Virtual WAN. Como las conexiones ExpressRoute no usan la red pública de Internet, ExpressRoute ofrecen más confiabilidad, velocidad y seguridad, y una menor latencia que las conexiones a Internet habituales. También se puede usar una red privada virtual para conectarse a Azure a través de una VPN de sitio a sitio (S2S) o de una VPN de punto a sitio (P2S).

- [ExpressRoute en Virtual WAN](virtual-wan-expressroute-portal.md)

- [Introducción sobre la VPN de sitio a sitio](virtual-wan-site-to-site-portal.md)

- [Introducción sobre la VPN de punto a sitio](virtual-wan-point-to-site-portal.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: protección de las aplicaciones y servicios de ataques de redes externas

**Guía**: Virtual WAN no expone ningún punto de conexión a redes externas que requieren que se usen las protecciones de red convencionales. Los recursos de las redes virtuales de radios (cualquier red virtual conectada a un centro virtual) se pueden proteger mediante los servicios de protección de redes virtuales. 

Use Azure Firewall para proteger las aplicaciones y los servicios contra el tráfico potencialmente malintencionado de Internet y otras ubicaciones externas. 

Elija DDoS Protection proporcionado por Azure para proteger sus recursos contra ataques en las redes virtuales de Azure. Use Azure Security Center para detectar riesgos de configuraciones incorrectas relacionados con los recursos de red.

- [Documentación sobre Azure Firewall](../firewall/index.yml)

- [Administración de Azure DDoS Protection estándar mediante Azure Portal](../ddos-protection/manage-ddos-protection.md) 

- [Recomendaciones de Azure Security Center](../security-center/recommendations-reference.md#recs-networking)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: implementación de sistemas de prevención de intrusiones y detección de intrusiones (IDS/IPS)

**Guía**: Virtual WAN es un servicio administrado de Microsoft. No ofrece funcionalidades nativas de detección o prevención de intrusiones. Sin embargo, hay funcionalidades de seguridad que se proporcionan a WAN virtual a través de Azure Firewall para habilitar un punto unificado de control de directivas. Puede crear una directiva de Azure Firewall y vincularla a un centro de WAN Virtual para que el centro de WAN Virtual existente funcione como un centro virtual protegido, con los recursos de Azure Firewall necesarios implementados.

- [Configuración de Azure Firewall en un centro de Virtual WAN](howto-firewall.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ns-6-simplify-network-security-rules"></a>NS-6: simplificación de las reglas de seguridad de red

**Guía**: Simplifique las reglas de seguridad de red y aproveche las etiquetas de servicio de Virtual Network para definir controles de acceso a la red en los grupos de seguridad de red o en Azure Firewall. Las etiquetas de servicio se pueden usar en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio en el campo de origen o destino de una regla, puede permitir o denegar el tráfico para el servicio correspondiente. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian.

- [Descripción y uso de las etiquetas de servicio](../virtual-network/service-tags-overview.md)

- [Descripción y uso de los grupos de seguridad de aplicaciones](../virtual-network/network-security-groups-overview.md#application-security-groups)

- [Documentación sobre Azure Firewall](../firewall/index.yml)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ns-7-secure-domain-name-service-dns"></a>NS-7: servicio de nombres de dominio (DNS) seguro

**Guía**: Las funcionalidades de DNS seguras se proporcionan a Virtual WAN con Azure Firewall. Configure Azure Firewall para que actúe como un proxy DNS que se convierta en un intermediario para solicitudes DNS de máquinas virtuales cliente a un servidor DNS. En el caso de las configuraciones de servidores DNS personalizados, habilite un proxy DNS para evitar la falta de coincidencia de la resolución DNS y habilite el filtrado de nombre de dominio completo en las reglas de red. 

- [Documentación sobre Azure Firewall](../firewall/index.yml)

- [Configuración de DNS de Azure Firewall](../firewall/dns-settings.md)

- [Uso de Azure Firewall como reenviador DNS con Private Link](https://github.com/adstuart/azure-privatelink-dns-azurefirewall)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="identity-management"></a>Administración de identidades

*Para más información, consulte [Azure Security Benchmark: Administración de identidades](../security/benchmarks/security-controls-v2-identity-management.md).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: Unificación en Azure Active Directory como sistema central de identidad y autenticación

**Guía**: Azure Active Directory (Azure AD) es el servicio de administración de acceso e identidad para los servicios de Azure, lo que incluye Virtual WAN. Debe unificar Azure AD para controlar la administración de identidades y accesos de la organización en:

- Recursos de Microsoft Cloud, como Azure Portal, Azure Storage, Azure Virtual Machines (Linux y Windows), Azure Key Vault, plataforma como servicio (PaaS) y aplicaciones de software como servicio (SaaS).
- Los recursos de su organización, como aplicaciones en Azure o los recursos de la red corporativa.

Proteja Azure AD como máxima prioridad de la práctica de seguridad en la nube de su organización. Evalúe su postura de identidad y seguridad con la característica de puntuación de seguridad de Security Center para medir el grado de coincidencia de la configuración con los procedimientos recomendados de Microsoft. Si fuera necesario, implemente los procedimientos recomendados de Microsoft para mejorar su postura de seguridad.

Además, Azure AD admite identidades externas, que permiten a los usuarios que no tienen un cuenta de Microsoft iniciar sesión en sus aplicaciones y recursos con su identidad externa. 

Revise la información sobre el uso de Azure AD en escenarios de VPN de punto a sitio en los vínculos a los que se hace referencia.

- [Creación de un inquilino de Azure Active Directory (AD) para conexiones del protocolo P2S OpenVPN](openvpn-azure-ad-tenant-multi-app.md)

- [Configuración de la autenticación de Azure Active Directory para una VPN de usuario](virtual-wan-point-to-site-azure-ad.md)

- [Inquilinos en Azure Active Directory](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Procedimiento para crear y configurar una instancia de Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Uso de controles con autenticación multifactor sólida para todo el acceso basado en Azure Active Directory

**Guía**: Actualmente, la autenticación de Azure Active Directory (Azure AD) se proporciona mediante la integración con una VPN de punto a sitio de Virtual WAN.

Azure Active Directory (Azure AD) es el servicio de administración de acceso e identidad para los servicios de Azure, Azure AD admite controles de autenticación sólida con autenticación multifactor y métodos seguros sin contraseña.

Azure AD recomienda lo siguiente para los controles de autenticación sólida:

- Autenticación multifactor: habilite la autenticación multifactor de Azure AD y siga las recomendaciones de administración de identidades y acceso de Azure Security Center para conocer los procedimientos recomendados de seguridad. Exija la autenticación multifactor a todos los usuarios, a algunos de ellos o en el nivel por usuario en función de los factores de riesgo y las condiciones de inicio de sesión.

- Autenticación sin contraseña: hay disponibles tres opciones de autenticación sin contraseña. Incluyen Windows Hello para empresas, la aplicación Microsoft Authenticator y métodos de autenticación locales, como las tarjetas inteligentes.

Asegúrese de que se usa el nivel más alto del método de autenticación sólida tanto para el administrador como para los usuarios con privilegios, seguido de la implementación de una directiva de autenticación sólida para otros usuarios.

- [Habilitación de MFA en VPN de P2S para Virtual WAN](openvpn-azure-ad-mfa.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: Restricción del acceso a recursos de Azure en función de las condiciones

**Guía**: Habilite la autenticación multifactor de Azure Active Directory (Azure AD) para usuarios de VPN (de punto a sitio) con el uso de la autenticación de Azure AD. Configure la autenticación multifactor usuario a usuario, o bien aproveche la autenticación multifactor con acceso condicional. El acceso condicional permite un control más específico sobre cómo se debe promover un segundo factor. Puede permitir la asignación de autenticación multifactor solo a la red privada virtual y excluir otras aplicaciones vinculadas al inquilino de Azure AD. 

Tenga en cuenta que la autenticación de Azure AD solo está disponible para puertas de enlace que usan el protocolo OpenVPN y clientes que usan Windows.

- [¿Qué es el acceso condicional?](../active-directory/conditional-access/overview.md)

- [Configuración de la autenticación de Azure Active Directory para una VPN de usuario](virtual-wan-point-to-site-azure-ad.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7: Elimine la exposición de credenciales no intencionada

**Guía**: La VPN de sitio a sitio de Virtual WAN usa claves precompartidas (PSK) que el cliente detecta, crea y administra en su instancia de Azure Key Vault. Implemente el escáner de credenciales para identificar las credenciales en el código. El escáner de credenciales también fomenta el traslado de credenciales detectadas a ubicaciones más seguras, como Azure Key Vault.

En GitHub, puede usar la característica de escaneo de secretos nativos para identificar credenciales u otro tipo de secretos en el código.

- [Configuración del escáner de credenciales](https://secdevtools.azurewebsites.net/helpcredscan.html) 

- [Escaneo de secretos de GitHub](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="privileged-access"></a>Acceso con privilegios

*Para más información, consulte [Azure Security Benchmark: Acceso con privilegios](../security/benchmarks/security-controls-v2-privileged-access.md).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: Restricción del acceso administrativo a los sistemas críticos para la empresa

**Guía**: Azure Virtual WAN usa el control de acceso basado en roles de Azure (Azure RBAC) para aislar el acceso a los sistemas críticos para la empresa mediante la restricción de las cuentas a las que se concede acceso con privilegios a las suscripciones y los grupos de administración en los que se encuentran.

Además, restringe el acceso a los sistemas de administración, identidad y seguridad que tienen acceso administrativo a los recursos críticos para su negocio, como controladores de dominio de Active Directory, herramientas de seguridad y herramientas de administración del sistema con agentes instalados en sistemas críticos para la empresa. Los atacantes que ponen en peligro estos sistemas de administración y seguridad pueden convertirlos inmediatamente en un arma para poner en peligro los recursos críticos para la empresa.

Para garantizar un control de acceso coherente, todos los tipos de control de acceso se deben alinear con la estrategia de segmentación de la empresa.

- [Modelo de referencia y componentes de Azure](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 

- [Acceso al grupo de administración](../governance/management-groups/overview.md#management-group-access) 

- [Administradores de la suscripción de Azure](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="data-protection"></a>Protección de datos

*Para más información, consulte [Azure Security Benchmark: protección de datos](../security/benchmarks/security-controls-v2-data-protection.md).*

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Cifrado de la información confidencial en tránsito

**Guía**: Use VPN de punto a sitio, VPN de sis8ito a sitio y ExpressRoute cifrado con Virtual WAN para cubrir sus requisitos de conectividad. El cifrado de VPN protege los datos en tránsito de ataques "fuera de banda" (por ejemplo, la captura de tráfico), con el fin de que los atacantes no puedan leer ni modificar los datos. 

- [VPN de punto a sitio](virtual-wan-point-to-site-portal.md)

- [VPN de sitio a sitio](virtual-wan-site-to-site-portal.md)

- [Cifrado de ExpressRoute](vpn-over-expressroute.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

## <a name="asset-management"></a>Administración de recursos

*Para más información, consulte [Azure Security Benchmark: Administración de recursos](../security/benchmarks/security-controls-v2-asset-management.md).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: Asegurarse de que el equipo de seguridad tiene visibilidad sobre los riesgos para los recursos

**Guía**: Asegúrese de que se conceden a los equipos de seguridad permisos de lector de seguridad en el inquilino y las suscripciones de Azure para que puedan supervisar los riesgos de seguridad mediante Azure Security Center. 

En función de la estructura de las responsabilidades del equipo de seguridad, la supervisión de los riesgos de seguridad puede ser responsabilidad de un equipo de seguridad central o de un equipo local. Con eso, la información y los riesgos de seguridad siempre se deben agregar de forma centralizada dentro de una organización. 

Los permisos de lector de seguridad se pueden aplicar en general a un inquilino completo (grupo de administración raíz) o a grupos de administración o a suscripciones concretas. 

Nota: Es posible que se requieran permisos adicionales para obtener visibilidad de las cargas de trabajo y los servicios. 

- [Introducción al rol de lector de seguridad](../role-based-access-control/built-in-roles.md#security-reader)

- [Información general sobre los grupos de administración de Azure](../governance/management-groups/overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2: Asegurarse de que el equipo de seguridad tiene acceso a los metadatos y al inventario de recursos

**Guía**: Aplique etiquetas a los recursos, grupos de recursos y suscripciones de Azure con el fin de organizarlos de manera lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción. Azure Virtual WAN también admite implementaciones de recursos basadas en Azure Resource Manager, con las que se pueden exportar plantillas de recursos. 

- [Guía de decisiones de nomenclatura y etiquetado de recursos](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=%2fazure%2fazure-resource-manager%2fmanagement%2ftoc.json)

- [Administración del inventario de recursos de Azure Security Center](../security-center/asset-inventory.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="am-3-use-only-approved-azure-services"></a>AM-3: Uso exclusivo de servicios de Azure aprobados

**Guía**: Use Azure Monitor para crear reglas para desencadenar alertas cuando se detecte un servicio no aprobado. Virtual WAN reúne muchas funciones de red, seguridad y enrutamiento para ofrecer una única interfaz operativa. Las puertas de enlace de VPN de Virtual WAN, las puertas de enlace de ExpressRoute y Azure Firewall tienen registros y métricas disponibles a través de Azure Monitor. 
 

- [Registros y métricas de Virtual WAN](logs-metrics.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>AM-5: Limitación de la capacidad de los usuarios para interactuar con Azure Resource Manager

**Guía**: Use el acceso condicional de Azure para limitar la capacidad de los usuarios de interactuar con Azure Resource Manager mediante la configuración de la opción "Bloquear acceso" en la aplicación "Administración de Microsoft Azure".

- [Configuración del acceso condicional para bloquear el acceso a Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="logging-and-threat-detection"></a>registro y detección de amenazas

*Para más información, consulte [Azure Security Benchmark: registro y detección de amenazas](../security/benchmarks/security-controls-v2-logging-threat-detection.md).*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Habilitación de la detección de amenazas para recursos de Azure

**Guía**: VPN de punto a sitio con Virtual WAN se integra en Azure Active Directory (Azure AD). Azure AD proporciona los siguientes registros de usuario que se pueden ver en los informes de Azure AD o que están integrados con Azure Monitor, Azure Sentinel, SIEM u otras herramientas de supervisión para casos de uso de análisis y supervisión de amenazas más sofisticados. Dichos componentes son:

- Inicios de sesión: El informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.
- Los registros de auditoría proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD, como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.
- Inicios de sesión de riesgo: Un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.
- Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Use Azure Security Center para crear alertas sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos, incluidas cuentas en desuso en la suscripción. Además de la supervisión básica de la protección de seguridad, use el módulo de protección contra amenazas de Security Center para recopilar alertas de seguridad más detalladas de recursos individuales de proceso de Azure (máquinas virtuales, contenedores, App Service), recursos de datos (SQL DB y almacenamiento) y niveles de servicio de Azure. Esta funcionalidad le permite ver las anomalías de las cuentas dentro de los recursos individuales.

- [Informes de actividad de auditoría en Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Habilitación de Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración de Azure Firewall en un centro de Virtual WAN](howto-firewall.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Habilitación de la detección de amenazas para la administración de identidades y acceso de Azure

**Guía**: VPN de punto a sitio con Virtual WAN se integra en Azure Active Directory (Azure AD). Azure AD proporciona los siguientes registros de usuario que se pueden ver en los informes de Azure AD o que están integrados con Azure Monitor, Azure Sentinel, SIEM u otras herramientas de supervisión para casos de uso de análisis y supervisión de amenazas más sofisticados. Dichos componentes son:

- Inicios de sesión: El informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.
- Los registros de auditoría proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD, como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.
- Inicios de sesión de riesgo: Un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.
- Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Use Azure Security Center para crear alertas sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos, incluidas cuentas en desuso en la suscripción. Además de la supervisión básica de la protección de seguridad, use el módulo de protección contra amenazas de Security Center para recopilar alertas de seguridad más detalladas de recursos individuales de proceso de Azure (máquinas virtuales, contenedores, App Service), recursos de datos (SQL DB y almacenamiento) y niveles de servicio de Azure. Esta funcionalidad le permite ver las anomalías de las cuentas dentro de los recursos individuales.

- [Informes de actividad de auditoría en Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Habilitación de Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

- [Configuración de Azure Firewall en un centro de Virtual WAN](howto-firewall.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Habilitación del registro para las actividades de red de Azure

**Guía**: Supervise Azure Virtual WAN con Azure Monitor. Virtual WAN reúne muchas funciones de red, seguridad y enrutamiento para ofrecer una única interfaz operativa. Las puertas de enlace de VPN de Virtual WAN, las puertas de enlace de ExpressRoute y Azure Firewall tienen registros y métricas disponibles a través de Azure Monitor. Las entradas del registro de actividad están habilitadas de forma predeterminada y se pueden ver en Azure Portal. Puede usar los registros de actividad de Azure (anteriormente conocidos como registros operativos y registros de auditoría) para ver todas las operaciones enviadas a su suscripción de Azure.

También hay una gran variedad de registros de diagnóstico disponibles para Virtual WAN que se pueden configurar para el recurso de Virtual WAN con Azure Portal.  Puede optar por enviar a Log Analytics, transmitir a un centro de eventos o simplemente archivar en una cuenta de almacenamiento. 
 

- [Registros y métricas de Virtual WAN](logs-metrics.md)

- [Métricas y registros de Azure Firewall](../firewall/logs-and-metrics.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Habilitación del registro para recursos de Azure

**Guía**: Los registros de actividad de Azure, habilitados automáticamente, contienen todas las operaciones de escritura (PUT, POST, DELETE) para los recursos de Azure Virtual WAN, excepto las operaciones de lectura (GET). Los registros de actividad se pueden usar para encontrar errores durante la solución de problemas o para supervisar cómo un usuario de su organización modificó un recurso.

Habilitación de registros de recursos de Azure para Virtual WAN. Puede usar Azure Security Center y Azure Policy para habilitar los registros de recursos y la recopilación de datos de registro. Estos registros pueden ser críticos para la investigación posterior de incidentes de seguridad y la realización de ejercicios forenses.

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../azure-monitor/essentials/diagnostic-settings.md) 

- [Descripción del registro y de los distintos tipos de registro de Azure](../azure-monitor/essentials/platform-logs-overview.md) 

- [Descripción de la recopilación de datos de Azure Security Center](../security-center/security-center-enable-data-collection.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Centralizar la administración y el análisis de los registros de seguridad

**Guía**: Habilite el registro de seguridad para Virtual WAN con Azure Monitor. Virtual WAN reúne muchas funciones de red, seguridad y enrutamiento para ofrecer una única interfaz operativa. Las puertas de enlace de VPN de Virtual WAN, las puertas de enlace de ExpressRoute y Azure Firewall tienen registros y métricas disponibles a través de Azure Monitor. Las entradas del registro de actividad están habilitadas de forma predeterminada y se pueden ver en Azure Portal. Puede usar los registros de actividad de Azure (anteriormente conocidos como registros operativos y registros de auditoría) para ver todas las operaciones enviadas a su suscripción de Azure. 

También hay una gran variedad de registros de diagnóstico disponibles para Virtual WAN que se pueden configurar para el recurso de Virtual WAN con Azure Portal. Envíelos a Log Analytics, transmítalos a un centro de eventos o simplemente archívelos en una cuenta de almacenamiento. Asimismo, habilite e incorpore datos a Azure Sentinel o a una solución de administración de eventos e información de seguridad (SIEM) de terceros. 
 

- [Registros y métricas de Virtual WAN](logs-metrics.md)

- [Métricas y registros de Azure Firewall](../firewall/logs-and-metrics.md)

La seguridad de Azure Virtual WAN se proporciona a través de Azure Firewall. 

- [Documentación sobre Azure Firewall](../firewall/overview.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Compartido

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: Configuración de la retención del almacenamiento de registros

**Guía**: Configure la retención de registros según sus requisitos de cumplimiento, normativos y empresariales. En Azure Monitor, puede establecer el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de área de trabajo de Azure Storage, Data Lake o Log Analytics para el almacenamiento de archivo a largo plazo.

- [Cambio del período de retención de datos en Log Analytics](../azure-monitor/logs/manage-cost-storage.md#change-the-data-retention-period)

- [Configuración de la directiva de retención para los registros de la cuenta de Azure Storage](../storage/common/storage-monitor-storage-account.md#configure-logging)

- [Exportación de alertas y recomendaciones de Azure Security Center](../security-center/continuous-export.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="incident-response"></a>Respuesta a los incidentes

*Para más información, consulte [Azure Security Benchmark: respuesta ante incidentes](../security/benchmarks/security-controls-v2-incident-response.md).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Preparación: actualización del proceso de respuesta a incidentes para Azure

**Guía**: Asegúrese de que la organización tenga procesos para responder a incidentes de seguridad, que haya actualizado estos procesos para Azure y que los ejercite regularmente para garantizar su disponibilidad.

- [Implementación de la seguridad en todo el entorno empresarial](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guía de referencia de respuesta a incidentes](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Preparación: notificación de incidentes de configuración

**Guía**: Configure la información de contacto en caso de incidentes de seguridad en Azure Security Center. Microsoft utilizará esta información para ponerse en contacto con usted si el Centro de respuestas de seguridad de Microsoft (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a sus datos. También hay opciones para personalizar la alerta y notificación de incidentes en diferentes servicios de Azure en función de sus necesidades de respuesta a incidentes. 

- [Establecimiento del contacto de seguridad de Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Customer

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Detección y análisis: creación de incidentes en función de alertas de alta calidad

**Guía**: Asegúrese de que cuenta con un proceso para crear alertas de alta calidad y medir la calidad de las alertas. Esto le permite aprender de incidentes anteriores y clasificar las alertas para los analistas, de modo que no pierdan tiempo con falsos positivos. 

Las alertas de alta calidad se pueden crear en función de la experiencia de pasados incidentes, información validada procedente de la comunidad y herramientas diseñadas para generar y limpiar alertas mediante la fusión y correlación de diversos orígenes de la señal. 

Azure Security Center proporciona alertas de alta calidad en muchos recursos de Azure. Puede usar el conector de datos de ASC para enviar las alertas a Azure Sentinel. Azure Sentinel le permite crear reglas de alerta avanzadas con el fin de generar automáticamente incidentes para investigar. 

Exporte las alertas y recomendaciones de Azure Security Center mediante la característica de exportación para ayudar a identificar riesgos en los recursos de Azure. Esta exportación puede hacerse de forma manual o de modo continuo.

- [Configuración de la exportación](../security-center/continuous-export.md)

- [Transmisión de alertas a Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Detección y análisis: investigación de incidentes

**Guía**: Asegúrese de que los analistas pueden consultar y usar diversos orígenes de datos a medida que investigan posibles incidentes, para conseguir una visión global de lo que ha sucedido. Se deben recopilar diversos registros para realizar un seguimiento de las actividades de un posible atacante en la cadena de eliminación para evitar puntos ciegos.  También debe asegurarse de que se capturan detalles y aprendizajes para otros analistas y para futuras referencias históricas.  

Los orígenes de datos para la investigación incluyen los orígenes de registro centralizados que ya se recopilan de los servicios en el ámbito y los sistemas en ejecución, pero también pueden incluir:

- Datos de red: use registros de flujo de grupos de seguridad de red, Azure Network Watcher y Azure Monitor para capturar registros de flujo de red y otra información de análisis. 

- Instantáneas de sistemas en ejecución: 

    - Use la funcionalidad de instantáneas de la máquina virtual de Azure para crear una instantánea del disco del sistema en ejecución. 

    - Use la funcionalidad de volcado de la memoria nativa del sistema operativo para crear una instantánea de la memoria del sistema en ejecución.

    - Use la característica de instantánea de los servicios de Azure o la propia funcionalidad del software para crear instantáneas de los sistemas en ejecución.

Azure Sentinel proporciona amplios análisis de datos en prácticamente cualquier origen de registro y un portal de administración de casos para administrar todo el ciclo de vida de los incidentes. La información de inteligencia durante una investigación puede asociarse a un incidente con fines de seguimiento e informes. 

- [Instantánea del disco de una máquina Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Instantánea del disco de una máquina Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Recopilación del volcado de memoria e información de diagnóstico del servicio de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Investigación de incidentes con Azure Sentinel](../sentinel/tutorial-investigate-cases.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Detección y análisis: clasificar incidentes

**Guía**: Proporcione contexto a los analistas sobre los incidentes en los que deberán centrarse en primer lugar en función de la gravedad de la alerta y la confidencialidad de los recursos. 

Azure Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en el hallazgo o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Adicionalmente, marque los recursos con etiquetas y cree un sistema de nomenclatura para identificar y clasificar los recursos de Azure, especialmente los que procesan datos confidenciales.  Es su responsabilidad asignar prioridades a la corrección de las alertas en función de la importancia de los recursos y el entorno de Azure donde se produjo el incidente.

- [Alertas de seguridad en el Centro de seguridad de Azure](../security-center/security-center-alerts-overview.md)

- [Uso de etiquetas para organizar los recursos de Azure](../azure-resource-manager/management/tag-resources.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Contención, erradicación y recuperación: automatización del control de incidentes

**Guía**: Automatice las tareas repetitivas manuales para acelerar el tiempo de respuesta y reducir la carga de los analistas. Las tareas manuales tardan más tiempo en ejecutarse, lo que ralentiza cada incidente y reduce el número de incidentes que un analista puede manejar. Las tareas manuales también aumentan la fatiga del analista, lo que aumenta el riesgo del error humano que produce retrasos y reduce la capacidad de los analistas de centrarse de manera efectiva en tareas complejas. Use las características de automatización de flujo de trabajo de Azure Security Center y Azure Sentinel para desencadenar acciones automáticamente o ejecutar un cuaderno de estrategias para responder a las alertas de seguridad entrantes. El cuaderno de estrategias lleva a cabo acciones, como el envío de notificaciones, la deshabilitación de cuentas y el aislamiento de redes problemáticas. 

- [Configuración de la automatización de flujos de trabajo en Security Center](../security-center/workflow-automation.md)

- [Configuración de respuestas automatizadas frente a amenazas en Azure Security Center](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Configuración de respuestas automatizadas frente a amenazas en Azure Sentinel](../sentinel/tutorial-respond-threats-playbook.md)

**Supervisión de Azure Security Center**: no disponible actualmente

**Responsabilidad**: Customer

## <a name="posture-and-vulnerability-management"></a>administración de posturas y vulnerabilidades

*Para más información, consulte [Azure Security Benchmark: administración de posturas y vulnerabilidades](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md).*

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: realización de una simulaciones de ataques periódicas

**Guía**: Según sea necesario, realice pruebas de penetración o actividades de equipo rojo en los recursos de Azure y asegúrese de corregir todos los resultados de seguridad críticos.
siga las reglas de compromiso de la prueba de penetración de Microsoft Cloud para asegurarse de que las pruebas de penetración no infrinjan las directivas de Microsoft. Use la estrategia de Microsoft y la ejecución de las pruebas de penetración del equipo rojo y sitios activos en la infraestructura de nube, los servicios y las aplicaciones administradas por Microsoft.

- [Pruebas de penetración en Azure](../security/fundamentals/pen-testing.md)

- [Reglas de interacción de las pruebas de penetración](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Equipo rojo de Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Compartido

## <a name="endpoint-security"></a>seguridad de los puntos de conexión

*Para más información, consulte [Azure Security Benchmark: seguridad de los puntos de conexión](../security/benchmarks/security-controls-v2-endpoint-security.md).*

### <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: uso de la detección y respuesta de puntos de conexión (EDR)

**Guía**: A los clientes no se les permite explícitamente configurar los valores de detección y respuesta de puntos de conexión. Sin embargo, las máquinas virtuales que se usan en la oferta de Azure Virtual WAN sí usan estas funcionalidades. Obtenga más información sobre estas funcionalidades generales en los vínculos a los que se hace referencia. 

- [Introducción a protección contra amenazas avanzada de Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [Servicio ATP de Microsoft Defender para servidores Windows](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [Servicio ATP de Microsoft Defender para servidores que no son de Windows](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Supervisión de Azure Security Center**: Sí

**Responsabilidad**: Compartido

## <a name="governance-and-strategy"></a>Gobernanza y estrategia

*Para más información, consulte [Azure Security Benchmark: gobernanza y estrategia](../security/benchmarks/security-controls-v2-governance-strategy.md).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definición de la estrategia de protección de datos y administración de recursos 

**Guía**: Asegúrese de documentar y comunicar una estrategia clara para la protección y supervisión continua de sistemas y datos. Dé prioridad a la detección, evaluación, protección y supervisión de los sistemas y datos críticos para la empresa. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Norma de clasificación de datos de acuerdo con los riesgos empresariales

-   Visibilidad en la organización de seguridad de los riesgos y el inventario de recursos 

-   Aprobación de la organización de seguridad de los servicios de Azure para su uso 

-   Seguridad de los recursos durante su ciclo de vida

-   Estrategia de control de acceso necesaria según la clasificación de datos de la organización

-   Uso de las funcionalidades de protección de datos nativa de Azure y de terceros

-   Requisitos de cifrado de datos para casos de uso en tránsito y en reposo

-   Normas criptográficas adecuadas

Para más información, consulte las siguientes referencias:
- [Recomendación para la arquitectura de seguridad de Azure: almacenamiento, datos y cifrado](/azure/architecture/framework/security/storage-data-encryption?bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Aspectos básicos de la seguridad de Azure: seguridad, cifrado y almacenamiento de datos de Azure](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: procedimientos recomendados de cifrado y seguridad de los datos de Azure](../security/fundamentals/data-encryption-best-practices.md?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

- [Azure Security Benchmark: administración de recursos](../security/benchmarks/security-controls-v2-asset-management.md)

- [Azure Security Benchmark: protección de datos](../security/benchmarks/security-controls-v2-data-protection.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definición de una estrategia de segmentación empresarial 

**Guía**: Establezca en toda la empresa una estrategia para segmentar el acceso a los recursos mediante una combinación de identidad, red, aplicación, suscripción, grupo de administración y otros controles.

Equilibre concienzudamente la necesidad de separación de seguridad con la necesidad de habilitar el funcionamiento diario de los sistemas que necesitan comunicarse entre sí y acceder a los datos.

Asegúrese de que la estrategia de segmentación se implementa de forma coherente en todos los tipos de control, como la seguridad de red, los modelos de identidad y acceso, y los modelos de acceso y permisos de las aplicaciones, y los controles de los procesos humanos.

- [Guía de la estrategia de segmentación en Azure (vídeo)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Guía de la estrategia de segmentación en Azure (documento)](/security/compass/governance#enterprise-segmentation-strategy)

- [Alineación de la segmentación de red con la estrategia de segmentación empresarial](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definición de la estrategia de administración de la posición de seguridad

**Guía**: Mida y mitigue continuamente los riesgos de los recursos individuales y el entorno en el que se hospedan. Dé prioridad a los recursos de gran valor y a las superficies de ataque muy expuestas, como las aplicaciones publicadas, los puntos de entrada y salida de red, los puntos de conexión de usuario y administrador, etc.

- [Azure Security Benchmark: administración de posturas y vulnerabilidades](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Alineación de los roles y responsabilidades de la organización

**Guía**: Asegúrese de documentar y comunicar una estrategia clara para los roles y las responsabilidades de la organización de seguridad. Dé prioridad a ofrecer una responsabilidad clara en las decisiones de seguridad, a proporcionar a todos los usuarios formación sobre el modelo de responsabilidad compartida y a los equipos técnicos sobre la tecnología para proteger la nube.

- [Procedimiento recomendado de seguridad de Azure 1. Personas: Formación del equipo sobre el recorrido de seguridad de la nube](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Procedimiento recomendado de seguridad de Azure 2. Personas: Formación del equipo en tecnología de seguridad de la nube](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Procedimiento recomendado de seguridad de Azure 3. Proceso: Asignación de responsabilidades por las decisiones de seguridad en la nube](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Definición de la estrategia de seguridad de red

**Guía**: Establezca un enfoque de seguridad de red de Azure como parte de la estrategia de control de acceso de seguridad general de su organización.  

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Centralización de la responsabilidad de seguridad y la administración de redes

-   Modelo de segmentación de la red virtual alineado con la estrategia de segmentación empresarial

-   Estrategia de corrección en diferentes escenarios de amenazas y ataques

-   Estrategia de entrada y salida y perimetral de Internet

-   Estrategia de interconectividad entre el entorno local y la nube híbrida

-   Artefactos de seguridad de red actualizados (por ejemplo, diagramas de red y arquitectura de red de referencia)

Para más información, consulte las siguientes referencias:
- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Security Benchmark: seguridad de red](../security/benchmarks/security-controls-v2-network-security.md)

- [Azure Network Security Overview (Información general sobre Azure Network Security)](../security/fundamentals/network-overview.md)

- [Estrategia para la arquitectura de red empresarial](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definición de la estrategia de acceso con privilegios e identidades

**Guía**: Establezca una identidad de Azure y enfoques de acceso con privilegios como parte de la estrategia de control de acceso de seguridad general de su organización.  

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Un sistema de identidad y autenticación centralizado y su interconectividad con otros sistemas de identidad internos y externos

-   Métodos de autenticación sólida en diferentes casos de uso y condiciones

-   Protección de usuarios con privilegios elevados

-   Supervisión y control de anomalías en las actividades de los usuarios  

-   Proceso de revisión y conciliación del acceso y la identidad de los usuarios

Para más información, consulte las siguientes referencias:

- [Azure Security Benchmark: administración de identidades](../security/benchmarks/security-controls-v2-identity-management.md)

- [Azure Security Benchmark: acceso con privilegios](../security/benchmarks//security-controls-v2-privileged-access.md)

- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Información general sobre seguridad de administración de identidades de Azure](../security/fundamentals/identity-management-overview.md)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definición de la estrategia de registro y respuesta a amenazas

**Guía**: Establezca una estrategia de registro y respuesta a amenazas para detectar y corregir rápidamente las amenazas mientras cumple los requisitos de cumplimiento. Dé prioridad a ofrecer a los analistas alertas de alta calidad y experiencias fluidas para que puedan centrarse en las amenazas, en lugar de en la integración y los pasos manuales. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

-   Las responsabilidades y el rol de la organización en las operaciones de seguridad (SecOps) 

-   Un proceso de respuesta a incidentes bien definido que esté alineado con NIST u otro marco del sector. 

-   Captura y retención de registros para admitir la detección de amenazas, la respuesta ante incidentes y las necesidades de cumplimiento

-   Visibilidad y correlación centralizada de la información sobre amenazas, mediante SIEM, funcionalidades nativas de Azure y otros orígenes 

-   Plan de comunicación y notificación con sus clientes, proveedores y entidades públicas de interés

-   Uso de plataformas nativas de Azure y de terceros para el tratamiento de incidentes, como el registro y la detección de amenazas, los análisis forenses y la corrección y erradicación de ataques

-   Procesos para controlar incidentes y actividades posteriores a incidentes, como las lecciones aprendidas y la retención de pruebas

Para más información, consulte las siguientes referencias:

- [Azure Security Benchmark: registro y detección de amenazas](../security/benchmarks/security-controls-v2-logging-threat-detection.md)

- [Azure Security Benchmark: respuesta a incidentes](../security/benchmarks/security-controls-v2-incident-response.md)

- [Procedimiento recomendado de seguridad de Azure 4. Proceso: Actualización de los procesos de respuesta a incidentes para la nube](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Guía de decisiones sobre registros e informes en Azure Adoption Framework](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Escala, administración y supervisión empresarial de Azure](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Supervisión de Azure Security Center**: No aplicable

**Responsabilidad**: Customer

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [Información general sobre Azure Security Benchmark V2](../security/benchmarks/overview.md).
- Obtenga más información sobre las [líneas de base de seguridad de Azure](../security/benchmarks/security-baselines-overview.md).
