---
title: 'DNS inversos para servicios de Azure: Azure DNS'
description: Con esta ruta de aprendizaje, empiece a configurar búsquedas inversas de DNS para los servicios hospedados en Azure.
services: dns
documentationcenter: na
author: rohinkoul
ms.service: dns
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: rohink
ms.openlocfilehash: c72e17e601ebf87b0f344a4723159ae22abc81d1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98919891"
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Configuración de DNS inversos para servicios hospedados en Azure

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

En este artículo se explica cómo configurar búsquedas inversas de DNS para servicios hospedados en Azure.

Los servicios de Azure utilizan direcciones IP asignadas por Azure y propiedad de Microsoft. Estos registros de DNS inversos (registros PTR) deben crearse en las correspondientes zonas de búsqueda inversa DNS propiedad de Microsoft. En este artículo se explica cómo hacerlo.

Este escenario no debe confundirse con la capacidad de [hospedar las zonas de búsqueda inversa DNS para los intervalos IP asignados en DNS de Azure](dns-reverse-dns-hosting.md). En este caso, los intervalos IP representados por la zona de búsqueda inversa deben asignarse a su organización, normalmente por su ISP.

Antes de leer este artículo, debe estar familiarizado con [Introducción a DNS inverso y compatibilidad en Azure](dns-reverse-dns-overview.md).

En Azure DNS, los recursos de proceso (como las máquinas virtuales, los conjuntos de escalado de máquinas virtuales o los clústeres de Service Fabric) se exponen a través de un recurso de PublicIpAddress. Las búsquedas inversas de DNS se configuran mediante la propiedad 'ReverseFqdn' de PublicIpAddress.


Actualmente no se admite DNS inverso para Azure App Service y Application Gateway.

## <a name="validation-of-reverse-dns-records"></a>Validación de registros de DNS inversos

Otros fabricantes quizá no puedan crear registros de DNS inversos para la asignación del servicio de Azure a los dominios de DNS. Para evitar esto, Azure solo permite la creación de un registro de DNS inverso en el que el nombre de dominio especificado en el registro de DNS inverso sea igual o se resuelva en el nombre DNS o dirección IP de PublicIpAddress o un servicio en la nube en la misma suscripción de Azure.

Solo se realiza esta validación cuando se establece o modifica el registro de DNS inverso. No se llevan a cabo revalidaciones periódicas.

Por ejemplo: supongamos que el recurso PublicIpAddress tiene el nombre DNS contosoapp1.northus.cloudapp.azure.com y la dirección IP 23.96.52.53. ReverseFqdn para PublicIpAddress puede especificarse como:
* El nombre DNS para PublicIpAddress, contosoapp1.northus.cloudapp.azure.com
* El nombre DNS para otra PublicIpAddress de la misma suscripción, como contosoapp2.westus.cloudapp.azure.com
* Un nombre DNS personal, como app1.contoso.com, siempre y cuando este nombre esté configurado *primero* como CNAME en contosoapp1.northus.cloudapp.azure.com o en otra PublicIpAddress de la misma suscripción.
* Un nombre DNS personal, como app1.contoso.com, siempre y cuando este nombre esté configurado *primero* como registro D en la dirección IP 23.96.52.53 o en la dirección IP de otra PublicIpAddress de la misma suscripción.

Se aplican las mismas restricciones a DNS inverso para Cloud Services.


## <a name="reverse-dns-for-publicipaddress-resources"></a>DNS inverso para recursos de PublicIpAddress

Esta sección proporciona instrucciones detalladas acerca de cómo configurar DNS inverso para recursos de PublicIpAddress en el modelo de implementación de Resource Manager, mediante Azure PowerShell, la CLI clásica de Azure o la CLI de Azure. La configuración de DNS inverso para recursos de PublicIpAddress no está actualmente admitida a través de Azure Portal.

Azure admite un DNS inverso en la actualidad solo para los recursos de PublicIpAddress de IPv4. No es compatible para IPv6.

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a>Incorporación de DNS inversos a PublicIpAddresses existentes

#### <a name="powershell"></a>PowerShell

Para actualizar un DNS inverso a una PublicIpAddress existente:

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzPublicIpAddress -PublicIpAddress $pip
```

Para agregar un DNS inverso a una PublicIpAddress existente que no tenga aún un nombre DNS, debe especificar también un nombre DNS:

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-classic-cli"></a>CLI de Azure clásica

Para agregar un DNS inverso a una PublicIpAddress existente:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

Para agregar un DNS inverso a una PublicIpAddress existente que no tenga aún un nombre DNS, debe especificar también un nombre DNS:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli"></a>Azure CLI

Para agregar un DNS inverso a una PublicIpAddress existente:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

Para agregar un DNS inverso a una PublicIpAddress existente que no tenga aún un nombre DNS, debe especificar también un nombre DNS:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Creación de una dirección IP pública con un DNS inverso

Para crear una nueva PublicIpAddress con la propiedad de DNS inverso ya especificada:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-classic-cli"></a>CLI de Azure clásica

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Visualización de DNS inverso para una PublicIpAddress existente

Para ver el valor configurado para una PublicIpAddress existente:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-classic-cli"></a>CLI de Azure clásica

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Eliminación de DNS inversos de direcciones IP públicas existentes

Para quitar una propiedad de DNS inverso de una PublicIpAddress existente:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-classic-cli"></a>CLI de Azure clásica

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Configuración de DNS inverso para Cloud Services

Esta sección proporciona instrucciones detalladas sobre cómo configurar DNS inverso para Cloud Services en el modelo de implementación clásico, mediante Azure PowerShell. No se admite la configuración de DNS inverso para Cloud Services mediante Azure Portal, la CLI clásica de Azure o la CLI de Azure.

### <a name="add-reverse-dns-to-existing-cloud-services"></a>Adición de DNS inverso a Cloud Services existentes

Para agregar un registro DNS inverso a un servicio en la nube existente:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Creación de un servicio en la nube con un DNS inverso

Para crear un nuevo servicio en la nube con la propiedad de DNS inverso ya especificada:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Visualización de DNS inverso para las instancias de Cloud Services existentes

Para ver la propiedad de DNS inverso de un servicio en la nube existente:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Eliminación de DNS inverso de instancias de Cloud Services existentes

Para quitar un propiedad de DNS inverso de un servicio en la nube existente:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>Preguntas más frecuentes

### <a name="how-much-do-reverse-dns-records-cost"></a>¿Cuánto cuestan los registros de DNS inversos?

Son gratuitos.  No existe ningún coste adicional para las consultas o los registros de DNS inversos.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>¿Se resolverán mis registros de DNS inversos desde Internet?

Sí. Cuando haya configurado la propiedad de DNS inverso para el servicio de Azure, Azure administrará todas las delegaciones y zonas DNS necesarias para garantizar que el registro de DNS inverso se resuelve para todos los usuarios de Internet.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>¿Se crean registros de DNS inverso predeterminados para mis servicios de Azure?

No. El DNS inverso es una característica opcional. Si decide no configurarla, no se crea ningún registro de DNS inverso predeterminado.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>¿Cuál es el formato del nombre de dominio completo (FQDN)?

Los FQDN se especifican en orden progresivo y deben terminar con un punto (por ejemplo, "app1.contoso.com.").

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a>¿Qué ocurre si no se supera la comprobación de validación para el DNS inverso que he especificado?

Cuando se produce un error en la comprobación de validación de DNS inverso, se produce un error en la operación para configurar el registro de DNS inverso. Corrija el valor de DNS inverso según sea necesario y vuelva a intentarlo.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>¿Puedo configurar DNS inverso para Azure App Service?

No. No se admite DNS inverso para Azure App Service.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>¿Puedo configurar varios registros de DNS inversos para mi servicio de Azure?

No. Azure admite un único registro de DNS inverso por cada servicio en la nube de Azure o PublicIpAddress.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>¿Puedo configurar DNS inverso para los recursos de PublicIpAddress de IPv6?

No. Azure admite un DNS inverso en la actualidad solo para los recursos de PublicIpAddress de IPv4 y Cloud Services.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>¿Puedo enviar correos electrónicos a dominios externos desde mis instancias de Azure Compute Services?

La capacidad técnica para enviar correos electrónicos directamente desde una implementación de Azure depende del tipo de suscripción. Independientemente del tipo de suscripción, Microsoft recomienda el uso de servicios de retransmisión de correo electrónico de confianza para enviar el correo saliente. Para obtener más información, consulte [Enhanced Azure Security for sending Emails](../virtual-network/troubleshoot-outbound-smtp-connectivity.md) (Seguridad de Azure mejorada para el envío de correos electrónicos): actualización de noviembre de 2017.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los registros de DNS inverso, consulte información sobre la [búsqueda de DNS inverso en Wikipedia](https://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Aprenda cómo [hospedar la zona de búsqueda inversa para el intervalo IP asignado por el ISP en DNS de Azure](dns-reverse-dns-for-azure-services.md).
