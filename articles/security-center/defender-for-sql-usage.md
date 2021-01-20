---
title: Uso de Azure Defender para SQL
description: Aprenda a usar el plan opcional de Azure Defender para SQL de Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/23/2020
ms.author: memildin
ms.openlocfilehash: c9d11f1ddc3e1dd2239bdfce9c41f58fe9e20497
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2021
ms.locfileid: "98539957"
---
# <a name="azure-defender-for-sql-servers-on-machines"></a>Azure Defender para servidores SQL en máquinas 

Este plan de Azure Defender detecta actividades anómalas que indican intentos inusuales y potencialmente peligrosos de obtener acceso a las bases de datos o de vulnerar su seguridad.

Verá alertas cuando haya actividades sospechosas en las bases de datos, posibles vulnerabilidades o ataques por inyección de código SQL, y patrones anómalos de acceso y consulta a las bases de datos.

## <a name="availability"></a>Disponibilidad

|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|Disponible con carácter general|
|Precios:|**Azure Defender para servidores SQL en máquinas** se factura como se indica en la [página de precios](security-center-pricing.md).|
|Versiones de SQL protegidas:|Azure SQL Server (todas las versiones cubiertas con el soporte técnico de Microsoft)|
|Nubes:|![Sí](./media/icons/yes-icon.png) Nubes comerciales<br>![Sí](./media/icons/yes-icon.png) US Gov<br>![No](./media/icons/no-icon.png) China Gov, otros gobiernos|
|||

## <a name="set-up-azure-defender-for-sql-servers-on-machines"></a>Configuración de Azure Defender para servidores SQL en las máquinas

Para habilitar este plan:

* Aprovisione el agente de Log Analytics en el host de SQL Server. Así se proporciona la conexión a Azure.

* Habilite el plan opcional en la página de precios y configuración de Security Center.

Ambos métodos se describen a continuación.

### <a name="step-1-provision-the-log-analytics-agent-on-your-sql-servers-host"></a>Paso 1. Aprovisione el agente de Log Analytics en el host de SQL Server:

- **SQL Server en Azure VM**: si la máquina SQL se hospeda en una máquina virtual de Azure, puede [habilitar el aprovisionamiento automático del agente de Log Analytics <a name="auto-provision-mma"></a>](security-center-enable-data-collection.md#auto-provision-mma). Como alternativa, puede seguir el procedimiento manual para [incorporar máquinas virtuales de Azure Stack](quickstart-onboard-machines.md#onboard-your-azure-stack-vms).
- **SQL Server en Azure Arc**: si el servidor SQL Server está administrado por servidores habilitados para [Azure Arc](../azure-arc/index.yml), puede implementar el agente de Log Analytics con la recomendación "El agente de Log Analytics debe estar instalado en las máquinas basadas en Windows de Azure Arc (versión preliminar)" de Security Center. Como alternativa, puede seguir los métodos de instalación que se describen en la [documentación de Azure Arc](../azure-arc/servers/manage-vm-extensions.md).

- **SQL Server local**: si el servidor SQL Server se hospeda en una máquina local de Windows sin Azure Arc, tiene dos opciones para conectarlo a Azure:
    
    - **Implementación de Azure Arc**: puede conectar cualquier máquina de Windows a Security Center. Sin embargo, Azure Arc proporciona una integración más profunda entre *todas* las de su entorno de Azure. Si configura Azure Arc, verá la página **SQL Server: Azure Arc** en el portal y las alertas de seguridad aparecerán en una pestaña **Seguridad** dedicada en esa página. Por lo tanto, la primera opción (y la recomendada) es [configurar Azure Arc en el host](../azure-arc/servers/onboard-portal.md#install-and-validate-the-agent-on-windows) y seguir las instrucciones anteriores para **SQL Server en Azure Arc**.
        
    - **Conexión de la máquina Windows sin Azure Arc**: si opta por conectar un servidor SQL Server que se ejecute en una máquina Windows sin usar Azure Arc, siga las instrucciones del artículo sobre cómo [conectar máquinas Windows a Azure Monitor](../azure-monitor/platform/agent-windows.md).


### <a name="step-2-enable-the-optional-plan-in-security-centers-pricing-and-settings-page"></a>Paso 2. Habilite el plan opcional en la página de precios y configuración de Security Center:

1. En el menú de Security Center, abra la página **Precios y configuración**.

    - Si usa el **área de trabajo predeterminada de Azure Security Center** (denominada "defaultworkspace-[su identificador de suscripción]-[región]"), seleccione la **suscripción** correspondiente.

    - Si utiliza **un área de trabajo no predeterminada**, seleccione el **área de trabajo** correspondiente (escriba el nombre del área de trabajo en el filtro si es necesario):

        ![Búsqueda por título en el área de trabajo no predeterminada](./media/security-center-advanced-iaas-data/pricing-and-settings-workspaces.png)

1. Establezca la opción del plan **Azure Defender para servidores SQL en máquinas**  en **Activada**. 

    ![Página de precios de Security Center con planes opcionales](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)

    El plan se habilitará en todos los servidores SQL conectados al área de trabajo seleccionada. La protección de datos estará totalmente activa después del primer reinicio de la instancia de SQL Server.

    >[!TIP] 
    > Para crear una nueva área de trabajo, siga las instrucciones de [Crear un área de trabajo de Log Analytics](../azure-monitor/learn/quick-create-workspace.md).


1. Opcionalmente, configure notificaciones de alertas de seguridad por correo electrónico. 
    Puede establecer una lista de destinatarios que reciban una notificación por correo electrónico cuando se generen alertas de Security Center. El correo electrónico contiene un vínculo directo a la alerta de Azure Security Center con todos los detalles pertinentes. Para más información, consulte el artículo sobre cómo [configurar las notificaciones por correo electrónico para recibir alertas](security-center-provide-security-contact-details.md).



## <a name="explore-vulnerability-assessment-reports"></a>Exploración de los informes de evaluación de vulnerabilidades

El servicio de evaluación de vulnerabilidades examina las bases de datos una vez a la semana. Los exámenes se ejecutan el mismo día de la semana en que se habilitó el servicio.

El panel de evaluación de vulnerabilidades proporciona una visión general de los resultados de la evaluación en todas las bases de datos, junto con un resumen de las bases de datos correctas y en mal estado, además de un resumen general de las comprobaciones con errores en función de la distribución del riesgo.

Puede ver los resultados de la evaluación de vulnerabilidades directamente desde Security Center.

1. En la barra lateral de Security Center, abra la página **Recomendaciones** y seleccione la recomendación **Es necesario corregir las vulnerabilidades de los servidores SQL Server en las máquinas (versión preliminar)** . Para más información, consulte [Recomendaciones de Security Center](security-center-recommendations.md). 

    :::image type="content" source="./media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png" alt-text="Es necesario corregir los resultados de la evaluación de vulnerabilidades de los servidores SQL Server en las máquinas (versión preliminar)":::

    Aparece la vista detallada de esta recomendación.

    :::image type="content" source="./media/security-center-advanced-iaas-data/all-servers-view.png" alt-text="Vista detallada de la recomendación":::

1. Para más detalles, explore en profundidad lo siguiente:

    * Para obtener información general de los recursos examinados (bases de datos) y la lista de comprobaciones de seguridad que se probaron, seleccione el servidor que le interesa.

    * Para obtener información general sobre las vulnerabilidades agrupadas por una base de datos SQL específica, seleccione la base de datos que le interesa.

    En cada vista, las comprobaciones de seguridad se ordenan por **gravedad**. Haga clic en una comprobación de seguridad específica para ver un panel de detalles con una **descripción**, cómo **corregir** el problema y otra información relacionada como, por ejemplo, el **impacto** o el **banco de pruebas**.

## <a name="azure-defender-for-sql-alerts"></a>Alertas de Azure Defender para SQL
Las alertas se generan a partir de intentos inusuales y potencialmente dañinos para acceder a las máquinas SQL o aprovechar sus vulnerabilidades. Estos eventos pueden desencadenar alertas que se muestran en la [página de referencia de alertas](alerts-reference.md#alerts-sql-db-and-warehouse).

## <a name="explore-and-investigate-security-alerts"></a>Exploración e investigación de las alertas de seguridad

Las alertas de SQL de Azure Defender están disponibles en la página de alertas de Security Center, en la pestaña seguridad del recurso, el [panel de Azure Defender](azure-defender-dashboard.md) o a través del vínculo directo de los correos electrónicos de alerta.

1. Para verlas, seleccione **Alertas de seguridad** en el menú de Security Center y seleccione una alerta.

1. Las alertas están diseñadas para ser independientes, con pasos de corrección detallados e información de investigación en cada una. Puede investigar más con otras funcionalidades de Azure Security Center y Azure Sentinel para obtener una visión más general:

    * Habilite la característica de auditoría de SQL Server para realizar más investigaciones. Si es usuario de Azure Sentinel, puede cargar los registros de auditoría de SQL desde los eventos del registro de seguridad de Windows a Sentinel y disfrutar de una experiencia de investigación enriquecida. [Obtenga más información sobre cómo realizar auditorías de SQL Server](/sql/relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification?preserve-view=true&view=sql-server-ver15).
    * Para mejorar la posición de seguridad, siga las recomendaciones de Security Center para la máquina host indicadas en cada alerta. Esto reducirá los riesgos de futuros ataques. 

    [Obtenga más información sobre cómo administrar y responder a alertas](security-center-managing-and-responding-alerts.md).


## <a name="next-steps"></a>Pasos siguientes

Para obtener material relacionado, vea el siguiente artículo:

- [Alertas de seguridad para SQL Database y Azure Synapse Analytics](alerts-reference.md#alerts-sql-db-and-warehouse)
- [Configuración de notificaciones de alertas de seguridad por correo electrónico](security-center-provide-security-contact-details.md)
- [Más información sobre Azure Sentinel](../sentinel/index.yml)