---
title: API REST de Azure Enterprise
description: En este artículo se describen las API REST que se usan para la inscripción de Azure Enterprise.
author: bandersmsft
ms.author: banders
ms.date: 01/21/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: enterprise
ms.reviewer: boalcsva
ms.openlocfilehash: 1fdf64053a55eb33d80ed461c231e8c6dd84d63b
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98677738"
---
# <a name="azure-enterprise-rest-apis"></a>API REST de Azure Enterprise

En este artículo se describen las API REST que se usan para la inscripción de Azure Enterprise. En él se explica también cómo resolver problemas comunes con las API REST.

## <a name="consumption-and-usage-apis"></a>API de consumo y de uso

Los clientes de Microsoft Enterprise Azure pueden obtener información acerca del uso y la facturación mediante las API REST. El propietario del rol (administrador de organización, administrador de departamento o propietario de la cuenta) debe permitir el acceso a la API mediante la generación de una clave desde el portal del Contrato Enterprise de Azure. A partir de ese momento, cualquiera que tenga el número y la clave de inscripción puede acceder a los datos mediante la API.

### <a name="available-apis"></a>API disponibles

**Balance and Summary**: [Balance and Summary API](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) proporciona un resumen mensual de información acerca de los saldos las nuevas compras, los cargos de los servicios de Azure Marketplace, los ajustes y los cargos por el uso por encima del límite. Para más información, consulte [API de informes para clientes de Enterprise: Balance and Summary](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary).

**Usage Detail**: [Usage Detail API](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) ofrece un desglose diario de las cantidades consumidas y los cargos estimados por inscripción. El resultado también incluye información acerca de las instancias, los medidores y los departamentos. La API se puede consultar por período de facturación o por una fecha de inicio y finalización específicas. Para más información, consulte [API de informes para clientes de Enterprise: detalles de uso](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail).

**Marketplace Store Charge**: [Marketplace Store Charge API](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) devuelve el desglose de los gastos de Marketplace basado en el uso por día para el período de facturación o las fechas de inicio y finalización especificadas. Para más información, consulte [API de informes para clientes de Enterprise: Marketplace Store Charge](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge).

**Price Sheet**: [Price Sheet API](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) proporciona el tipo aplicable a cada medidor en los periodos de inscripción y facturación. Para más información, consulte [API de informes para clientes de Enterprise: Price Sheet](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet).

**Billing Periods**: [Billing Periods API](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) devuelve una lista de períodos de facturación que tienen datos de consumo para una inscripción en orden cronológico inverso. Cada período contiene una propiedad que apunta a la ruta de la API de los cuatro conjuntos de datos: BalanceSummary, UsageDetails, Marketplace Charges y PriceSheet. Para más información, consulte [API de informes para clientes de Enterprise: Billing Periods](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods).

### <a name="enable-api-data-access"></a>Habilitación del acceso a datos de API

Los propietarios de los roles pueden seguir estos pasos en el portal del Contrato Enterprise de Azure. Vaya a **Reports** (Informes) > **Download Usage** (Descargar uso) > **API Access Key** (Clave de acceso a API). Luego, pueden:

- Generar las claves de acceso principal y secundaria.
- Deshabilitar las claves de acceso.
- Ver las fechas de inicio y de finalización de las claves de acceso.

### <a name="generate-or-retrieve-the-api-key"></a>Generación o recuperación de la clave de API

1. Inicie sesión como administrador de empresa.
2. Haga clic en **Reports** (Informes) en la ventana de navegación izquierda y, después, haga clic en la pestaña  **Download Usage** (Descargar uso).
3. Haga clic en **API Access Key** (Clave de acceso de API).
4. En **Enrollment Access Keys** (Claves de acceso de inscripción), seleccione el símbolo de generar clave para generar una clave principal o secundaria.
5. Seleccione **Expand Key** (Expandir clave) para ver la clave de acceso de API generada completa.
6. Seleccione **Copy** (Copiar) para obtener la clave de acceso de la API para su uso inmediato.

![Ejemplo de la página API Access Key (Clave de acceso a API)](./media/ea-portal-rest-apis/ea-create-generate-or-retrieve-the-api-key.png)

Si desea dar las claves de acceso a la API a personas que no sean administradores de empresa en su inscripción, siga estos pasos:

1. En la ventana de navegación de la izquierda, haga clic en **Manage** (Administrar).
2. Haga clic en el símbolo del lápiz que hay junto a **DA view charges** (cargos de la vista del administrador de departamento).
3. Seleccione **Enable** (Habilitar) y después haga clic en **Save** (Guardar).
4. Haga clic en el símbolo del lápiz que hay junto a **AO view charges** (cargos de la vista del propietario de la cuenta).
5. Seleccione **Enable** (Habilitar) y después haga clic en **Save** (Guardar).

![Ejemplo que muestra los cargos de las vistas del administrador de departamento y del propietario de la cuenta habilitados](./media/ea-portal-rest-apis/create-ea-generate-or-retrieve-api-key-enable-ao-do-view.png). Los pasos anteriores proporcionan a los titulares de las claves de acceso de API acceso a la información de costos y precios en los informes de uso.

### <a name="pass-keys-in-the-api"></a>Uso de claves en la API

Use la clave de API en todas las llamadas para su autenticación y autorización. Use la siguiente propiedad en los encabezados HTTP:

| Clave de encabezado de solicitud | Value |
| --- | --- |
| Authorization | Especifique el valor con este formato: **bearer {API\_KEY}**
Ejemplo: bearer \&lt;APIKey\&gt; |

### <a name="swagger"></a>Swagger

Un punto de conexión de Swagger está disponible en las [API de Enterprise Reporting v3](https://consumption.azure.com/swagger/ui/index)para las siguientes API. Swagger ayuda a inspeccionar la API. Use Swagger para generar SDK de cliente mediante [AutoRest](https://github.com/Azure/AutoRest) o [Swagger CodeGen](https://swagger.io/swagger-codegen/). Los datos disponibles después del 1 de mayo de 2014 están disponibles a través de la API.

### <a name="api-response-codes"></a>Códigos de respuesta de la API

Cuando se usa una API, se muestran códigos de estado de respuesta, que se describen en la siguiente tabla.

| Código de estado de respuesta | Message | Descripción |
| --- | --- | --- |
| 200 | Aceptar | Sin errores |
| 401 | No autorizado | Clave de API no encontrada, no válida, expirada, etc. |
| 404 | No disponible | Punto de conexión de informe no encontrado |
| 400 | Bad Request | Parámetros no válidos: intervalos de fechas, números de EA, etc. |
| 500 | Error de servidor | Error inesperado al procesar la solicitud |

### <a name="usage-and-billing-data-update-frequency"></a>Frecuencia de actualización de datos de uso y facturación

Los archivos de datos de uso y facturación se actualizan cada 24 horas en lo que se refiere al mes de facturación actual. Sin embargo, los datos pueden tener una latencia máxima de tres días. Por ejemplo, si el uso se realiza un lunes, es posible que los datos no aparezcan en el archivo de datos hasta el jueves.

### <a name="azure-service-catalog"></a>Catálogo de servicios de Azure

Todos los servicios de Azure se publican en un catálogo en formato CSV que se encuentra en un blog de Azure Storage. El catálogo es útil si se necesita crear un catálogo con mantenimiento de todos los servicios de Azure para el sistema. El catálogo actual está en [https://azurecatalog.blob.core.windows.net/catalog/AzureCatalog.csv](https://azurecatalog.blob.core.windows.net/catalog/AzureCatalog.csv).

### <a name="csv-data-file-details"></a>Detalles del archivo de datos CSV

La siguiente información describe las propiedades de los informes de la API.

#### <a name="usage-summary"></a>Resumen de uso

El formato JSON se genera a partir del informe CSV. En consecuencia, el formato es el mismo que el formato CSV de resumen. El nombre de la columna se puede controlar, por lo que debe deserializarlo en una tabla de datos cuando consuma los datos del resumen de JSON.

| Nombre de columna CSV | Nombre de columna JSON | Nueva columna JSON | Comentario |
| --- | --- | --- | --- |
| AccountOwnerId | AccountOwnerLiveId | AccountOwnerLiveId |   |
| Nombre de cuenta | AccountName | AccountName |   |
| ServiceAdministratorId | ServiceAdministratorLiveId | ServiceAdministratorLiveId |   |
| SubscriptionId | SubscriptionId | SubscriptionId |   |
| SubscriptionGuid | MOCPSubscriptionGuid | SubscriptionGuid |   |
| Nombre de suscripción | SubscriptionName | SubscriptionName |   |
| Date | Date | Date | Muestra la fecha en que se ejecutó el informe del catálogo de servicios. El formato es una cadena de fecha sin marca de tiempo. |
| Mes | Mes | Month |   |
| Día | Día | Día |   |
| Año | Año | Año |   |
| Product | BillableItemName | Product |   |
| Meter ID | ResourceGUID | MeterId |   |
| Categoría de medidor | Servicio | MeterCategory | Resulta útil para ayudar a encontrar servicios. Relevante para los servicios que tienen varios ServiceType. Por ejemplo, Virtual Machines. |
| Meter Sub-Category | ServiceType | MeterSubCategory | Proporciona un segundo nivel de detalles para un servicio. Por ejemplo, A1 VM (no Windows).  |
| Meter Region | ServiceRegion | MeterRegion | El tercer nivel de detalle necesario para un servicio. Útil para buscar el contexto regional de ResourceGUID. |
| Medidor Nombre | ServiceResource | MeterName | El nombre del servicio. |
| Consumed Quantity | ResourceQtyConsumed | ConsumedQuantity |   |
| ResourceRate | ResourceRate | ResourceRate |   |
| ExtendedCost | ExtendedCost | ExtendedCost |   |
| Resource Location | ServiceSubRegion | ResourceLocation |   |
| Consumed Service | ServiceInfo | ConsumedService |   |
| Instance ID | Componente | InstanceId |   |
| ServiceInfo1 | ServiceInfo1 | ServiceInfo1 |   |
| ServiceInfo2 | ServiceInfo2 | ServiceInfo2 |   |
| AdditionalInfo | AdditionalInfo | AdditionalInfo |   |
| Etiquetas | Etiquetas | Etiquetas |   |
| Store Service Identifier   | OrderNumber | StoreServiceIdentifier   |   |
| Nombre de departamento | DepartmentName | DepartmentName |   |
| Cost Center | CostCenter | CostCenter |   |
| Unidad de medida | UnitOfMeasure | UnitOfMeasure | Valores de ejemplo: horas, GB, eventos, inserciones, unidad, horas por unidad, MB, unidades diarias |
| ResourceGroup | ResourceGroup | ResourceGroup |   |

#### <a name="azure-marketplace-report"></a>Informe de Azure Marketplace

| Nombre de columna CSV | Nombre de columna JSON | Nueva columna JSON |
| --- | --- | --- |
| AccountOwnerId | AccountOwnerId | AccountOwnerId |
| Nombre de cuenta | AccountName | AccountName |
| SubscriptionId | SubscriptionId | SubscriptionId |
| SubscriptionGuid | SubscriptionGuid | SubscriptionGuid |
| Nombre de suscripción | SubscriptionName |  SubscriptionName |
| Date | BillingCycle |  Date (solo cadena de fecha. No marca de tiempo)
| Mes | Mes |  Month |
| Día | Día |  Día |
| Año | Año |  Año |
| Meter ID | MeterResourceId |  MeterId |
| Nombre de publicador | PublisherFriendlyName |  PublisherName |
| Nombre de la oferta | OfferFriendlyName |  OfferName |
| Plan Name | PlanFriendlyName |  PlanName |
| Consumed Quantity | BilledQty |  ConsumedQuantity |
| ResourceRate | ResourceRate | ResourceRate |
| ExtendedCost | ExtendedCost | ExtendedCost |
| Unidad de medida | UnitOfMeasure | UnitOfMeasure |
| Instance ID | InstanceId | InstanceId |
| Información adicional | AdditionalInfo | AdditionalInfo |
| Etiquetas | Etiquetas | Etiquetas |
| Order Number | OrderNumber | OrderNumber |
| Nombre de departamento | DepartmentNames | DepartmentName |
| Cost Center | CostCenters |  CostCenter |
| Grupo de recursos | ResourceGroup |  ResourceGroup |

#### <a name="price-sheet"></a>Hoja de precios

| Nombre de columna CSV | Nombre de columna JSON | Comentario |
| --- | --- | --- |
| Servicio | Servicio |  Sin cambio de precio |
| Unidad de medida | UnitOfMeasure |   |
| Número de referencia de uso por encima del límite | ConsumptionPartNumber |   |
| Precio unitario de uso por encima del límite | ConsumptionPrice |   |
| Código de divisa | CurrencyCode |     |

### <a name="common-api-issues"></a>Problemas de API comunes

Al usar las API REST de Azure Enterprise puede aparecer cualquiera de los siguientes problemas habituales.

Puede intentar usar una clave API que no tenga el tipo de autorización correcto. Las claves de API las genera el:

- Administrador de empresa
- Administrador de departamento (DA)
- Propietario de la cuenta (AO)

Una clave generada por el administrador de EA da acceso a toda la información de la inscripción. Un administrador de EA de solo lectura no puede generar claves de API.

Una clave generada por un DA o un AO no proporciona acceso a la información del saldo, del cargo y de la hoja de precios.

Las claves de API expiran cada seis meses. Si expiran, es preciso volver a generarlas.

Si recibe un error de tiempo de expiración, puede resolverlo aumentando el límite del umbral de tiempo de expiración.

Es posible que reciba un error 401 (no autorizado), que es un error de expiración. Normalmente, el error lo provoca que alguna de las claves haya expirado. Si la clave ha expirado, puede volver a generarla.

Es posible que reciba los errores 400 y 404 (no disponible), que devuelve una llamada API cuando no hay datos actuales disponibles para el intervalo de fechas seleccionado. Por ejemplo, este error se puede producirse porque se inició recientemente la transferencia de una inscripción. Los datos de una fecha específica, y posteriores, ahora se encuentran en una inscripción nueva. De lo contrario, el error puede producirse si usa un nuevo número de inscripción para recuperar la información que reside en una inscripción antigua.

## <a name="next-steps"></a>Pasos siguientes

- Los administradores del portal del Contrato Enterprise de Azure deben leer el artículo acerca de la [administración del portal del Contrato Enterprise de Azure](ea-portal-administration.md) para obtener información sobre las tareas administrativas comunes.
- Si necesita ayuda para solucionar problemas con el portal del Contrato Enterprise de Azure, consulte [Solución de problemas de acceso al portal del Contrato Enterprise de Azure](ea-portal-troubleshoot.md).