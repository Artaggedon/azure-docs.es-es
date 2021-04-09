---
title: 'Ejemplo de PowerShell: Exportación de secretos y certificados de aplicaciones empresariales del inquilino de Azure Active Directory.'
description: Ejemplo de PowerShell que exporta todos los secretos y certificados de las aplicaciones empresariales especificadas del inquilino de Azure Active Directory.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 03/09/2021
ms.author: kenwith
ms.reviewer: mifarca
ms.openlocfilehash: 20caefe74a7c047fb8690bb1d9e6f4eb9da7e9b7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "102635202"
---
# <a name="export-secrets-and-certificates-for-enterprise-apps"></a>Exportación de secretos y certificados de aplicaciones empresariales
Este ejemplo de script de PowerShell exporta todos los secretos, certificados y propietarios de las aplicaciones empresariales especificadas del directorio a un archivo CSV.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

El ejemplo requiere el [módulo Azure AD V2 de PowerShell para Graph](/powershell/azure/active-directory/install-adv2) (Azure AD) o la [versión preliminar del módulo Azure AD V2 de PowerShell para Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (versión preliminar de Azure AD).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-all-enterprise-apps-secrets-and-certs.ps1 "Exports all secrets and certificates for the specified enterprise apps in your directory.")]

## <a name="script-explanation"></a>Explicación del script

El comando "Add-Member" es responsable de la creación de las columnas en el archivo CSV.
En el caso de que prefiera que la exportación sea no interactiva, puede modificar la variable "$Path" directamente en PowerShell mediante una ruta de acceso del archivo CSV.

| Get-Help | Notas |
|---|---|
| [Get-AzureADServicePrincipal](/powershell/module/azuread/Get-azureADServicePrincipal?view=azureadps-2.0&preserve-view=true) | Recupera una aplicación empresarial del directorio. |
| [Get-AzureADServicePrincipalOwner](/powershell/module/azuread/Get-AzureADServicePrincipalOwner?view=azureadps-2.0&preserve-view=true) | Recupera los propietarios de una aplicación empresarial del directorio. |


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure AD PowerShell, consulte [Información general del módulo de Azure AD PowerShell](/powershell/azure/active-directory/overview).

Para obtener otros ejemplos de PowerShell para la administración de aplicaciones, consulte [Ejemplos de Azure AD PowerShell para la administración de aplicaciones](../app-management-powershell-samples.md).
