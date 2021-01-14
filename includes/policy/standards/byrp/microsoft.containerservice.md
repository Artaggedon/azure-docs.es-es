---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/08/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 893d1dd500c87b9a889ff6ffb72e4435e1534fab
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98049474"
---
## <a name="azure-security-benchmark"></a>Prueba comparativa de la seguridad de Azure

[Azure Security Benchmark](../../../../articles/security/benchmarks/overview.md) proporciona recomendaciones sobre cómo puede proteger sus soluciones de nube en Azure. Para ver cómo este servicio se asigna por completo a Azure Security Benchmark, consulte los [archivos de asignación de Az Security Benchmark](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Para revisar el modo en que las integraciones de Azure Policy disponibles para todos los servicios de Azure se asignan a este estándar de cumplimiento, consulte [Cumplimiento normativo de Azure Policy: Azure Security Benchmark](../../../../articles/governance/policy/samples/azure-security-benchmark.md).

|Domain |Id. de control |Título de control |Directiva<br /><sub>(Azure Portal)</sub> |Versión de la directiva<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Seguridad de redes |1.1 |Proteja los recursos mediante grupos de seguridad de red o Azure Firewall en su red virtual |[Los intervalos IP autorizados deben definirse en los servicios de Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0e246bcf-5f6f-4f87-bc6f-775d4712c7ea) |[2.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableIpRanges_KubernetesService_Audit.json) |
|Protección de datos |4.6 |Uso de RBAC de Azure para controlar el acceso a los recursos |[Se debe usar el control de acceso basado en rol (RBAC) en los servicios de Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |
|Administración de vulnerabilidades |5.3 |Implemente una solución automatizada de administración de revisiones de software de terceros |[Kubernetes Services se debe actualizar a una versión de Kubernetes no vulnerable](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb893a29-21bb-418c-a157-e99480ec364c) |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UpgradeVersion_KubernetesService_Audit.json) |

## <a name="cis-microsoft-azure-foundations-benchmark"></a>CIS Microsoft Azure Foundations Benchmark

Para revisar el modo en que las integraciones de Azure Policy disponibles para todos los servicios de Azure se corresponden a este estándar de cumplimiento, consulte [Cumplimiento normativo de Azure Policy: CIS Microsoft Azure Foundations Benchmark 1.1.0](../../../../articles/governance/policy/samples/cis-azure-1-1-0.md).
Para más información sobre este estándar de cumplimiento, consulte [CIS Microsoft Azure Foundations Benchmark](https://www.cisecurity.org/benchmark/azure/).

|Domain |Id. de control |Título de control |Directiva<br /><sub>(Azure Portal)</sub> |Versión de la directiva<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Otras consideraciones de seguridad |8.5 |Habilite el control de acceso basado en rol (RBAC) en Azure Kubernetes Services. |[Se debe usar el control de acceso basado en rol (RBAC) en los servicios de Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |

## <a name="hipaa-hitrust-92"></a>HIPAA/HITRUST 9.2

Con el fin de revisar el modo en que las integraciones de Azure Policy disponibles para los servicios de Azure siguen este estándar de cumplimiento, consulte [Cumplimiento normativo de Azure Policy: HIPAA/HITRUST 9.2](../../../../articles/governance/policy/samples/hipaa-hitrust-9-2.md).
Para más información acerca de este estándar de cumplimiento, consulte [HIPAA/HITRUST 9.2](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

|Domain |Id. de control |Título de control |Directiva<br /><sub>(Azure Portal)</sub> |Versión de la directiva<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Administración de privilegios |1149.01c2System.9 - 01.c |Con el fin de facilitar el uso compartido de la información, la organización permite que los usuarios autorizados determinen el acceso de un asociado comercial según unos criterios definidos y el empleo de procesos manuales o mecanismos automatizados para ayudar a los usuarios a tomar decisiones de colaboración y uso compartido de la información. |[Se debe usar el control de acceso basado en rol (RBAC) en los servicios de Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |
|Administración de privilegios |1153.01c3System.35 - 01.c |Todo acceso al sistema de archivos que no sea claramente necesario se deshabilita, y tan solo los usuarios autorizados podrán acceder a los recursos específicamente necesarios para el desempeño de sus obligaciones. |[Se debe usar el control de acceso basado en rol (RBAC) en los servicios de Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |
|Separación de obligaciones |1229.09c1Organizational.1 - 09.c |La separación de obligaciones se utiliza para limitar el riesgo de modificaciones no autorizada o accidentales de la información y los sistemas. |[Se debe usar el control de acceso basado en rol (RBAC) en los servicios de Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |

## <a name="nist-sp-800-171-r2"></a>NIST SP 800-171 R2

Para revisar el modo en que las integraciones de Azure Policy disponibles para todos los servicios de Azure se corresponden a este estándar de cumplimiento, consulte [Cumplimiento normativo de Azure Policy: NIST SP 800-171 R2](../../../../articles/governance/policy/samples/nist-sp-800-171-r2.md).
Para más información acerca de este estándar normativo, consulte [NIST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final).

|Domain |Id. de control |Título de control |Directiva<br /><sub>(Azure Portal)</sub> |Versión de la directiva<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Integridad del sistema y de la información |3.14.1 |Identifica, informa y corrige los errores del sistema de manera puntual. |[Kubernetes Services se debe actualizar a una versión de Kubernetes no vulnerable](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffb893a29-21bb-418c-a157-e99480ec364c) |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UpgradeVersion_KubernetesService_Audit.json) |

