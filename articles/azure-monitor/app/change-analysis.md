---
title: Uso de Application Change Analysis en Azure Monitor para detectar problemas de la aplicación web | Microsoft Docs
description: Use Application Change Analysis en Azure Monitor para solucionar problemas de aplicaciones en sitios activos de Azure App Service.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 05/04/2020
ms.openlocfilehash: 50e199d2d56016086bb409f8690e9828f1d19984
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97881516"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Uso de Application Change Analysis (versión preliminar) en Azure Monitor

Cuando se produce una interrupción o un problema en un sitio activo, determinar rápidamente la causa raíz es fundamental. Las soluciones de supervisión estándares pueden alertarlo sobre un problema. Incluso pueden indicarle qué componente está presentando el error. Sin embargo, esta alerta no siempre explica inmediatamente la causa del error. Sabrá que su sitio funcionaba hace cinco minutos y que ahora no funciona. ¿Qué cambió en los últimos cinco minutos? Esta es la pregunta que Application Change Analysis está diseñado para responder en Azure Monitor.

A partir de la eficacia de [Azure Resource Graph](../../governance/resource-graph/overview.md), Change Analysis proporciona conclusiones sobre los cambios de la aplicación de Azure para aumentar la observación y reducir el tiempo medio para reparación.

> [!IMPORTANT]
> Change Analysis se encuentra actualmente en versión preliminar. Esta versión preliminar se proporciona sin un contrato de nivel de servicio. Esta versión no se recomienda para usarse en cargas de trabajo. Es posible que algunas características no sean compatibles o que sus funcionalidades estén limitadas. Para obtener más información, consulte [Términos de uso complementarios de las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Información general

Change Analysis detecta varios tipos de cambios, desde el nivel de infraestructura hasta la implementación de la aplicación. Es un proveedor de recursos de Azure de nivel de suscripción que comprueba los cambios de recursos en la suscripción. Change Analysis proporciona datos para que varias herramientas de diagnóstico ayuden a los usuarios a comprender qué cambios pueden haber causado los problemas.

El siguiente diagrama muestra la arquitectura de Change Analysis:

![Diagrama de la arquitectura del procedimiento mediante el que Change Analysis obtiene los datos de cambio y los envía a las herramientas de cliente](./media/change-analysis/overview.png)

## <a name="data-sources"></a>Orígenes de datos

Consultas de Application Change Analysis para propiedades controladas de Azure Resource Manager, configuraciones con proxy y cambios en el invitado de la aplicación web. Además, el servicio hace un seguimiento de los cambios en las dependencias de los recursos para diagnosticar y supervisar una aplicación completa.

### <a name="azure-resource-manager-tracked-properties-changes"></a>Cambios en la propiedades controladas de Azure Resource Manager

Mediante [Azure Resource Graph](../../governance/resource-graph/overview.md), Change Analysis proporciona un registro histórico de la forma en que los recursos de Azure que hospedan a la aplicación han cambiado con el tiempo. Es posible detectar la configuración controlada, como las identidades administradas, la actualización del sistema operativo de la plataforma y los nombres de host.

### <a name="azure-resource-manager-proxied-setting-changes"></a>Cambios en la configuración con Proxy de Azure Resource Manager

Las opciones de configuración, como la regla de configuración de IP, configuración de TLS y versiones de extensión, todavía no están disponibles en Azure Resource Graph. Por lo tanto, las consultas de Change Analysis se calculan de forma segura para proporcionar más detalles sobre lo que ha cambiado en la aplicación.

### <a name="changes-in-web-app-deployment-and-configuration-in-guest-changes"></a>Cambios en la implementación y la configuración de aplicaciones web (cambios en invitado)

Change Analysis captura el estado de implementación y configuración de una aplicación cada 4 horas. Puede detectar, por ejemplo, los cambios en las variables de entorno de aplicación. La herramienta calcula las diferencias y presenta qué ha cambiado. A diferencia de los cambios de Resource Manager, es posible que la información sobre cambios en la implementación de código no esté disponible en la herramienta inmediatamente. Para ver los cambios más recientes en Change Analysis, seleccione **Actualizar**.

![Captura de pantalla del botón "Scan changes now"](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Cambios de dependencia

Los cambios realizados en las dependencias de recursos también pueden causar problemas en una aplicación web. Por ejemplo, si llama una aplicación web a una caché de Redis Cache, la SKU la dicha caché podría afectar al rendimiento de la aplicación de web. Para detectar los cambios en las dependencias, Change Analysis comprueba el registro DNS de la aplicación web. De este modo, identifica los cambios en todos los componentes de aplicación que podrían ocasionar problemas.
Actualmente se admiten las siguientes dependencias:
- Web Apps
- Azure Storage
- Azure SQL

## <a name="application-change-analysis-service"></a>Servicio Application Change Analysis

El servicio Application Change Analysis calcula y agrega los datos modificados de los orígenes de datos mencionados anteriormente. Proporciona un conjunto de análisis para que los usuarios naveguen fácilmente por todos los cambios de los recursos y para identificar qué cambio es pertinente en el contexto de solución de problemas o de supervisión.
Es necesario registrar el proveedor de recursos "Microsoft. ChangeAnalysis" con una suscripción para que esté disponible la configuración de propiedades controladas y datos de cambio de configuración con Proxy de Azure Resource Manager. Al entrar en la herramienta Diagnosticar y solucionar problemas de Web Apps o abrir la hoja independiente de Change Analysis, este proveedor de recursos se registra automáticamente. No tiene ningún costo de implementación ni rendimiento para la suscripción. Al habilitar Change Analysis para aplicaciones web (o habilitar la herramienta Diagnosticar y resolver problemas), tendrá un impacto insignificante en el rendimiento de la aplicación web y ningún costo de facturación.
En el caso de cambios en el invitado de la aplicación web, se requiere una habilitación independiente para examinar los archivos de código dentro de una aplicación web. Para más información, consulte la sección [Change Analysis en la herramienta Diagnosticar y solucionar problemas](#application-change-analysis-in-the-diagnose-and-solve-problems-tool) que aparece más adelante en este artículo para obtener más detalles.

## <a name="visualizations-for-application-change-analysis"></a>Visualizaciones para Application Change Analysis

### <a name="standalone-ui"></a>UI independiente

En Azure Monitor, hay un panel independiente para que Change Analysis vea todos los cambios con información sobre los recursos de información y dependencias de aplicaciones.

Busque Change Analysis en la barra de búsqueda de Azure Portal para iniciar la experiencia.

![Captura de pantalla de búsqueda de Change Analysis en Azure Portal](./media/change-analysis/search-change-analysis.png)

Todos los recursos de una suscripción seleccionada se muestran con los cambios de las últimas 24 horas. Para optimizar el rendimiento de carga de páginas, el servicio muestra 10 recursos a la vez. Haga clic en las páginas siguientes para ver más recursos. Estamos trabajando actualmente para poder quitar esta limitación.

![Captura de pantalla de la hoja Change Analysis en Azure Portal](./media/change-analysis/change-analysis-standalone-blade.png)

Haga clic en un recurso para ver todos sus cambios. Si es necesario, explore en profundidad un cambio para ver la información y los detalles sobre el cambio en formato JSON.

![Captura de pantalla de los detalles del cambio](./media/change-analysis/change-details.png)

Si tiene algún comentario, use el botón Enviar comentarios de la hoja o el correo electrónico changeanalysisteam@microsoft.com.

![Captura de pantalla del botón de comentarios de la hoja Change Analysis](./media/change-analysis/change-analysis-feedback.png)

### <a name="web-app-diagnose-and-solve-problems"></a>Diagnóstico y solución de problemas de una aplicación web

En Azure Monitor, Change Analysis también está integrado en la experiencia de autoservicio **Diagnosticar y solucionar problemas**. Obtenga acceso a esta experiencia desde la pagina **Información general** de la aplicación de App Service.

![Captura de pantalla de los botones "Información general" y "Diagnosticar y solucionar problemas"](./media/change-analysis/change-analysis.png)

### <a name="application-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Application Change Analysis en la herramienta Diagnosticar y solucionar problemas

Application Change Analysis es un detector independiente en las herramientas de diagnóstico y solución de problemas de una aplicación web. También se agrega en **Application Crashes** (Bloqueos de la aplicación) y **Web App Down detectors** (Detectores de aplicación web fuera de servicio). Cuando entra en la herramienta Diagnosticar y solucionar problemas, el proveedor de recursos **Microsoft.ChangeAnalysis** se registrará automáticamente. Siga estas instrucciones para habilitar el seguimiento de cambios en el invitado de la aplicación web.

1. Seleccione **Availability and Performance** (Disponibilidad y rendimiento).

    ![Captura de pantalla de opción de solución de problemas "Disponibilidad y rendimiento"](./media/change-analysis/availability-and-performance.png)

2. Seleccione **Application Changes** (Cambios de la aplicación). Esta característica también está disponible en **Application Crashes** (Bloqueos de la aplicación).

   ![Captura de pantalla del botón "Application Crashes"](./media/change-analysis/application-changes.png)

3. El vínculo conduce a la interfaz de usuario de cambio de Application Change Analysis en el ámbito de la aplicación web. Si el seguimiento de cambios en el invitado de la aplicación web no está habilitado, siga el banner para obtener los cambios de configuración de los archivos y aplicaciones.

   ![Captura de las opciones de "Application Crashes"](./media/change-analysis/enable-changeanalysis.png)

4. Active **Change Analysis** y seleccione **Guardar**. La herramienta muestra todas las aplicaciones web en un plan de App Service. Puede usar el modificador nivel de plan para activar Application Change Analysis para todas las aplicaciones web de un plan.

    ![Captura de pantalla de la interfaz de usuario "Habilitar Change Analysis"](./media/change-analysis/change-analysis-on.png)

5. Los datos modificados también están disponibles en ciertos detectores de **Web App Down** (Aplicación web fuera de servicio) y **Application Crashes** (Bloqueo de aplicaciones). Verá un gráfico con un resumen del tipo de cambios a lo largo del tiempo, junto con detalles sobre estos cambios: De forma predeterminada, se muestran los cambios en las últimas 24 horas para ayudar a solucionar problemas inmediatos.

     ![Captura de pantalla de la vista de diferencias de cambios](./media/change-analysis/change-view.png)



### <a name="virtual-machine-diagnose-and-solve-problems"></a>Diagnóstico y solución de problemas de una máquina virtual

Vaya a la herramienta Diagnosticar y solucionar problemas para una máquina virtual.  Vaya a **Herramientas para la solución de problemas**, navegue hacia abajo de la página y seleccione **Analizar cambios recientes** para ver los cambios en la máquina virtual.

![Captura de pantalla de Diagnosticar y solucionar problemas de la máquina virtual](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![Analizador de cambios en herramientas para la solución de problemas](./media/change-analysis/analyze-recent-changes.png)

### <a name="activity-log-change-history"></a>Historial de cambios en el registro de actividad
La característica [View change history](../platform/activity-log.md#view-change-history) (Ver el historial de cambios) en el registro de actividad llama al back-end del servicio Application Change Analysis para obtener los cambios asociados a una operación. **Historial de cambios** antes llamaba directamente a [Azure Resource Graph](../../governance/resource-graph/overview.md), pero se cambió el back-end para llamar a Application Change Analysis, de modo que los cambios devueltos incluirán los cambios de nivel de recursos de [Azure Resource Graph](../../governance/resource-graph/overview.md), las propiedades de los recursos de [Azure Resource Manager](../../azure-resource-manager/management/overview.md) y los cambios en el invitado de los servicios de PaaS, como la aplicación web App Services. Para que el servicio Application Change Analysis pueda examinar en busca de cambios en las suscripciones de los usuarios, es necesario registrar un proveedor de recursos. La primera vez que acceda a la pestaña **Historial de cambios**, la herramienta comenzará automáticamente a registrar al proveedor de recursos **Microsoft.ChangeAnalysis**. Tras su registro, los cambios de **Azure Resource Graph** estarán disponibles inmediatamente y cubrirán los últimos 14 días. Los cambios de otros orígenes estarán disponibles después de aproximadamente 4 horas a partir de la incorporación de la suscripción.

![Integración del historial de cambios en el registro de actividad](./media/change-analysis/activity-log-change-history.png)

### <a name="vm-insights-integration"></a>Integración de Información de máquinas virtuales
Los usuarios que tienen habilitada [Información de máquinas virtuales](../insights/vminsights-overview.md) pueden ver lo que ha cambiado en sus máquinas virtuales que podría haber provocado picos en un gráfico de métricas, como CPU o memoria, y preguntarse qué lo causó. Los datos modificados se integran en la barra de navegación lateral de Información de máquinas virtuales. El usuario puede ver si se han producido cambios en la VM y hacer clic en **Investigate Changes** (Investigar cambios) para ver los detalles de los cambios en la interfaz de usuario independiente de Application Change Analysis.

[![Integración de Información de máquinas virtuales](./media/change-analysis/vm-insights.png)](./media/change-analysis/vm-insights.png#lightbox)



## <a name="enable-change-analysis-at-scale"></a>Habilitación de Change Analysis a gran escala

Si la suscripción incluye varias aplicaciones web, podría resultar ineficaz habilitar el servicio en el nivel de aplicación web. Ejecute el siguiente script para habilitar todas las aplicaciones web de su suscripción.

Requisitos previos:

- Módulo Az de PowerShell. Siga las instrucciones de [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps).

Ejecute el siguiente script:

```PowerShell
# Log in to your Azure subscription
Connect-AzAccount

# Get subscription Id
$SubscriptionId = Read-Host -Prompt 'Input your subscription Id'

# Make Feature Flag visible to the subscription
Set-AzContext -SubscriptionId $SubscriptionId

# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"

# Enable each web app
$webapp_list = Get-AzWebApp | Where-Object {$_.kind -eq 'app'}
foreach ($webapp in $webapp_list)
{
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId $webapp.Id -Tag $tags -Force
}

```

## <a name="troubleshoot"></a>Solución de problemas

### <a name="having-trouble-registering-microsoftchange-analysis-resource-provider-from-change-history-tab"></a>Problemas al registrar el proveedor de recursos de análisis Microsoft.Change Analysis desde la pestaña Historial de cambios
Si es la primera vez que ve el historial de cambios después de su integración en Application Change Analysis, verá que registra automáticamente un proveedor de recursos **Microsoft.ChangeAnalysis**. En contadas ocasiones, podría producirse un error por las razones siguientes:

- **No tiene permisos suficientes para registrar el proveedor de recursos Microsoft.ChangeAnalysis**. Este mensaje de error significa que el rol de la suscripción actual no tiene el ámbito **Microsoft.support/register/action** asociado a ella. Esto puede ocurrir si no es el propietario de una suscripción y obtuvo permisos de acceso compartido a través de un compañero de trabajo, es decir, acceso de visualización en un grupo de recursos. Para corregirlo, puede ponerse en contacto con el propietario de la suscripción para registrar el proveedor de recursos **Microsoft.ChangeAnalysis**. Esto puede hacerse en Azure Portal a través de **Suscripciones | Proveedores de recursos** y buscar ```Microsoft.ChangeAnalysis``` y registrarse en la interfaz de usuario, o a través de Azure PowerShell o la CLI de Azure.

    Registre el proveedor de recursos a través de PowerShell: 
    ```PowerShell
    # Register resource provider
    Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"
    ```

- **No se pudo registrar el proveedor de recursos Microsoft.ChangeAnalysis**. Este mensaje indica que ha habido un error inmediatamente después de que la interfaz de usuario enviara la solicitud para registrar el proveedor de recursos, y que no está relacionada con un problema de permisos. Lo más probable es que se deba a un problema temporal de conectividad a Internet. Intente actualizar la página y comprobar la conexión a Internet. Si el error continúa, póngase en contacto con changeanalysishelp@microsoft.com.

- **Está tardando más de lo esperado**. Este mensaje significa que el registro está tardando más de 2 minutos. Esto es poco habitual, pero no significa necesariamente que haya algo mal. Puede ir a **Suscripciones | Proveedor de recursos** para comprobar el estado de registro del proveedor de recursos **Microsoft.ChangeAnalysis**. Puede intentar usar la interfaz de usuario para anular el registro, volver a registrarse o actualizar para ver si esto ayuda. Si el problema persiste, póngase en contacto con changeanalysishelp@microsoft.com para obtener soporte.
    ![La solución de problemas de registro del proveedor de recursos está tardando demasiado tiempo](./media/change-analysis/troubleshoot-registration-taking-too-long.png)

![Captura de pantalla de la herramienta Diagnosticar y resolver problemas de una máquina virtual con Herramientas para la solución de problemas seleccionado.](./media/change-analysis/vm-dnsp-troubleshootingtools.png)

![Captura de pantalla del mosaico Analizar cambios recientes de la herramienta para la solución de problemas para una máquina virtual.](./media/change-analysis/analyze-recent-changes.png)

### <a name="azure-lighthouse-subscription-is-not-supported"></a>No se admite la suscripción de Azure Lighthouse.

- **Error al consultar al proveedor de recursos Microsoft.ChangeAnalysis** con el mensaje *No se admite la suscripción de Azure Lighthouse; los cambios solo están disponibles en el inquilino principal de la suscripción*. Ahora existe una limitación para que el proveedor de recursos de Change Analysis se registre a través de la suscripción de Azure Lighthouse para los usuarios que no están en el inquilino principal. Esperamos que esta limitación se solucione en un futuro próximo. Si se trata de un problema de bloqueo, hay una solución alternativa que implica la creación de una entidad de servicio y la asignación explícita del rol para permitir el acceso.  Póngase en contacto con changeanalysishelp@microsoft.com para más información al respecto.

## <a name="next-steps"></a>Pasos siguientes

- Habilitar Application Insights para [aplicaciones de Azure App Services](azure-web-apps.md).
- Habilitar Application Insights para [aplicaciones hospedadas en IIS en máquina virtual de Azure y conjunto de escalado de máquinas virtuales de Azure](azure-vm-vmss-apps.md).
- Obtenga más información sobre [Azure Resource Graph](../../governance/resource-graph/overview.md), que ayuda a la eficacia de Change Analysis.