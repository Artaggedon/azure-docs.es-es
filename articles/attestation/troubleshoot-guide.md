---
title: Guía de solución de problemas de Azure Attestation
description: Guía de solución de problemas detectados con frecuencia
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: reference
ms.date: 07/20/2020
ms.author: mbaldwin
ms.openlocfilehash: 3ae3e12c11f194b3efcc149382dc952bd74d38b5
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2020
ms.locfileid: "97704323"
---
# <a name="microsoft-azure-attestation-troubleshooting-guide"></a>Guía de solución de problemas de Microsoft Azure Attestation

El control de errores en Azure Attestation se implementa conforme a las [Directrices para API de REST de Microsoft](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#7102-error-condition-responses). La respuesta de error devuelta por las API de Azure Attestation contiene el código de estado HTTP y los pares de nombre y valor con los nombres "code" y "message". El valor de "code" es legible y es un indicador del tipo de error. El valor de "message" pretende ayudar al usuario y proporciona detalles del error.

Si tiene un problema que no se trata en este artículo, también puede enviar una solicitud de soporte técnico de Azure en la página [Soporte técnico de Azure](https://azure.microsoft.com/support/options/).

A continuación se muestran algunos ejemplos de los errores devueltos por Azure Attestation:

## <a name="1-http401--unauthorized-exception"></a>1. HTTP-401: Excepción no autorizada

### <a name="http-status-code"></a>Código de estado HTTP
401

**Código de error** Unauthorized

**Ejemplos de escenarios**
  - Error de atestación si el usuario no tiene asignado el rol Lector de atestación
  - No se pueden administrar las directivas de atestación porque el usuario no tiene asignados los roles adecuados
  - No se pueden administrar los firmantes de las directivas de atestación porque el usuario no tiene asignados los roles adecuados

Usuario con el rol Lector que intenta editar una directiva de atestación en PowerShell 

  ```powershell
  Set-AzAttestationPolicy : Operation returned HTTP Status Code 401
At line:1 char:1
+ Set-AzAttestationPolicy -Name $attestationProvider -ResourceGroupName ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzAttestationPolicy], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.SetAzureAttestationPolicy
  ```

**Pasos para solucionar problemas**

Para ver las directivas o los firmantes de directivas de atestación, un usuario de Azure AD requiere el permiso para "Actions":
- Microsoft.Attestation/attestationProviders/attestation/read

  Este permiso se puede asignar a un usuario de AD mediante un rol como "Propietario" (permisos comodín), "Lector" (permisos comodín) o "Lector de atestación" (permisos específicos solo para Azure Attestation).

Para agregar o eliminar firmantes de directivas o configurar directivas, un usuario de Azure AD necesita los siguientes permisos para "Actions":
- Microsoft.Attestation/attestationProviders/attestation/write
- Microsoft.Attestation/attestationProviders/attestation/delete

  Estos permisos se pueden asignar a un usuario de AD por medio de un rol como "Propietario" (permisos comodín), "Colaborador" (permisos comodín) o "Colaborador de atestación" (permisos específicos solo para Azure Attestation).

Los clientes pueden elegir entre usar el proveedor predeterminado para la atestación o crear sus propios proveedores con directivas personalizadas. Para enviar solicitudes de atestación a proveedores de atestación personalizados, se requiere el rol "Propietario" (permisos comodín), "Lector" (permisos comodín) o "Lector de atestación" para el usuario. Cualquier usuario de Azure AD puede acceder a los proveedores predeterminados.

Para comprobar los roles en PowerShell, ejecute lo siguiente:

a. Inicie PowerShell e inicie sesión en Azure mediante el cmdlet "Connect-AzAccount"

b. Compruebe la configuración de asignación de roles de Azure


  ```powershell
  $c = Get-AzContext
  Get-AzRoleAssignment -ResourceGroupName $attestationResourceGroup -ResourceName $attestationProvider -ResourceType Microsoft.Attestation/attestationProviders -SignInName $c.Account.Id
  ```

  Deberíamos ver algo parecido a lo siguiente:

  ```
  RoleAssignmentId   :/subscriptions/subscriptionId/providers/Microsoft.Authorization/roleAssignments/roleAssignmentId
  
  Scope              : /subscriptions/subscriptionId
  
  DisplayName        : displayName
  
  SignInName         : signInName
  
  RoleDefinitionName : Reader
  
  RoleDefinitionId   : roleDefinitionId
  
  ObjectId           : objectid
  
  ObjectType         : User
  
  CanDelegate        : False
 
  ```

c. Si no encuentra ninguna asignación de roles adecuada en la lista, siga las instrucciones que se indican [aquí](../role-based-access-control/role-assignments-powershell.md)

## <a name="2-http--400-errors"></a>2. HTTP: errores 400

### <a name="http-status-code"></a>Código de estado HTTP
400

Hay varios motivos por los que una solicitud puede devolver 400. A continuación se muestran algunos ejemplos de errores devueltos por las API de Azure Attestation:

### <a name="21-attestation-failure-due-to-policy-evaluation-errors"></a>2.1. Error de atestación debido a errores de evaluación de directiva

La directiva de atestación incluye reglas de autorización y reglas de emisión. La evidencia de enclave se evalúa en función de las reglas de autorización. Las reglas de emisión definen las notificaciones que se van a incluir en el token de atestación. Si las notificaciones de la evidencia de enclave no cumplen las reglas de autorización, las llamadas de atestación devuelven un error de evaluación de directiva. 

**Código de error** PolicyEvaluationError

**Ejemplos de escenarios** Cuando las notificaciones de la oferta de enclave no coinciden con las reglas de autorización de la directiva de atestación

```
Native operation failed with 65518: G:\Az\security\Attestation\src\AttestationServices\Instance\NativePolicyWrapper\NativePolicyEngine.cpp(168)\(null)!00007FF801762308: (caller: 00007FF80143DCC8) Exception(0) 83FFFFEE Policy Evaluation Error has occurred Msg:[Policy Engine Exception: A Deny claim was issued, authorization failed.]

G:\Az\security\Attestation\src\AttestationServices\Instance\Enclave\api.cpp(840)\(null)!00007FF801739FF3: (caller: 00007FF801232801) LogHr(0) 83FFFFEE Policy Evaluation Error has occurred Msg:[Unhandled Enclave Exception: "Policy Evaluation Error has occurred"]

```

**Pasos para solucionar problemas** Los usuarios pueden evaluar la evidencia de enclave con una directiva de atestación de SGX antes de configurar esta.

Envíe una solicitud a la API de atestación mediante la especificación del texto de directiva del parámetro "draftPolicyForAttestation". La API AttestSgxEnclave usa este documento de directiva durante la llamada de atestación y, además, se puede usar para probar las directivas de atestación antes de que se consuman. El token de atestación generado cuando este campo está presente no está protegido.

Vea [Ejemplos de una directiva de atestación](./policy-examples.md)

### <a name="22-attestation-failure-due-to-invalid-input"></a>2.2. Error de atestación debido a una entrada no válida

**Código de error** InvalidParameter

**Ejemplos de escenarios** Error de atestación de SGX debido a una entrada no válida. A continuación se muestran algunos de los ejemplos de mensajes de error:
- La oferta especificada no era válida debido a un error en el material adjunto de la oferta 
- La oferta especificada no era válida porque el dispositivo en el que se generó la oferta no cumple los requisitos de base de referencia de Azure
- La oferta especificada no era válida porque el elemento TCBInfo o QEID proporcionado por el servicio de caché de PCK no era válido

**Pasos para solucionar problemas**

Microsoft Azure Attestation admite la atestación de ofertas de SGX generadas mediante el SDK de Intel y de Open Enclave.

Vea [ejemplos de código](/samples/browse/?expanded=azure&terms=attestation) para realizar la atestación con el SDK de Open Enclave o Intel

### <a name="23-invalid-certificate-chain-error-while-uploading-policypolicy-signer"></a>2.3. Error de cadena de certificados no válida al cargar directiva o firmante de directivas

**Código de error** InvalidParameter

**Ejemplos de escenarios** Configuración de directiva firmada o incorporación o eliminación de firmante de directivas, que se ha firmado con una cadena de certificados no válida (por ejemplo, cuando la extensión Restricciones básicas del certificado raíz no está establecida en Tipo de asunto = CA)

```
Native operation failed with 65529: C:\source\src\AttestationServices\Instance\SgxPal\sgxcert.cpp(1074)\(null)!00007FFA285CDAED: (caller: 00007FFA285C36E8) Exception(0) 83FFFFF9 The requested item is not found    Msg:[Unable to find issuer certificate CN=attestationsigningcert]
C:\source\src\AttestationServices\Instance\Enclave\api.cpp(618)\(null)!00007FFA286DCBF8: (caller: 00007FFA285860D3) LogHr(0) 83FFFFF9 The requested item is not found    Msg:[Unhandled Enclave Exception: "The requested item is not found"]
At line:1 char:1
+ Set-AzAttestationPolicy -Name "testpolicy1" -ResourceGroupName "BugBa ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzAttestationPolicy], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.SetAzureAttestationPolicy

```

**Pasos para solucionar problemas** El certificado raíz debe marcarse como emitido por una CA (las restricciones básicas X.509), de lo contrario, no se considera un certificado válido. 

Asegúrese de que la extensión Restricciones básicas del certificado raíz esté establecida para indicar que Tipo de asunto = CA

De lo contrario, se considera que la cadena de certificados no es válida.

Vea ejemplos de [firmante de directivas](./policy-signer-examples.md) y [directiva](./policy-examples.md) 

### <a name="24-adddelete-policy-signer-failure"></a>2.4. Error de incorporación o eliminación de firmante de directivas

**Código de error** InvalidOperation

**Ejemplos de escenarios**

Cuando el usuario carga JWS sin la notificación "maa-policyCertificate"

```
Add-AzAttestationPolicySigner : Operation returned HTTP Status Code 400
Code: InvalidOperation
Message: Native operation failed with 74: ..\Enclave\enclave.cpp(2213)\(null)!: (caller: ) Exception(0) 83FF004A Bad
message    Msg:[Could not find "maa-policyCertificate" claim in policy token]
..\Enclave\api.cpp(496)\(null)!: (caller: ) LogHr(0) 83FF004A Bad message    Msg:[Unhandled Enclave Exception: "Bad
message"]
At line:1 char:1
+ Add-AzAttestationPolicySigner -Name $attestationProvider -ResourceGro ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Add-AzAttestationPolicySigner], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.AddAzureAttestationPolicySigner

```

Cuando el usuario no carga un certificado en formato JWS

```
Add-AzAttestationPolicySigner : Operation returned HTTP Status Code 400
Code: InvalidOperation
Message: Native operation failed with 74: ..\JsonWebToken\jsonwebtoken.cpp(375)\(null)!: (caller: ) LogHr(0) 83FF004A
Bad message    Msg:[RETURN_IF_TRUE('(firstPeriod == std::string::npos)') failed with 0x4a: Malformed JWT: Could not
find first period in the token.]
..\Enclave\enclave.cpp(2106)\(null)!: (caller: ) LogHr(0) 83FF004A Bad message
Msg:[THROW_IF_ERROR('DecomposeJsonWebSignature(&policyJws, encodedJoseHeader, encodedJwsBody, jwsSignature)') failed
with 0x4a: 'Bad message']
..\Enclave\enclave.cpp(2106)\(null)!: (caller: ) Exception(0) 83FF004A Bad message
..\Enclave\api.cpp(496)\(null)!: (caller: ) LogHr(0) 83FF004A Bad message    Msg:[Unhandled Enclave Exception: "Bad
message"]
At line:1 char:1
+ Add-AzAttestationPolicySigner -Name $attestationProvider -ResourceGro ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Add-AzAttestationPolicySigner], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.AddAzureAttestationPolicySigner
```

**Pasos para solucionar problemas** Para agregar o eliminar un nuevo certificado de firmante de directivas, use RFC7519 JSON Web Token (JWT) con una notificación denominada "x-ms-policyCertificate". El valor de la notificación es RFC7517 JSON Web Key, que contiene el certificado que se va a agregar. JWT se debe firmar con una clave privada de cualquiera de los certificados de firmante de directivas válidos asociados al proveedor. Vea [Ejemplos de firmante de directivas](./policy-signer-examples.md).

### <a name="25-attestation-policy-configuration-failure"></a>2.5. Error de configuración de directiva de atestación

**Código de error** PolicyParsingError

**Ejemplos de escenarios** Directiva proporcionada con sintaxis incorrecta (por ejemplo, falta punto y coma/directiva JWT válida)

```
Native operation failed with 65526: ..\NativePolicyWrapper\NativePolicyEngine.cpp(31)\(null)!: (caller: ) Exception(0) 83FFFFF6 Invalid policy was specified    Msg:[Policy Parser Exception Thrown: Offending
symbol: '['
Line: '2', Column: '1'
Failure message: 'mismatched input '[' expecting ';''
Failing rule: 'policy > versionInfo']
..\Enclave\api.cpp(618)\(null)!: (caller: ) LogHr(0) 83FFFFF6 Invalid policy was specified    Msg:[Unhandled Enclave Exception: "Invalid policy was specified"]
At line:1 char:1
+ set-AzAttestationPolicy -Name $attestationProvider -ResourceGroupName ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzAttestationPolicy], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.SetAzureAttestationPolicy
```

**Código de error** InvalidOperation

**Ejemplos de escenarios** Contenido no válido proporcionado (por ejemplo, carga de directiva o directiva sin firmar cuando se requiere la firma de la directiva)

```
Native operation failed with 74: ..\Shared\base64url.h(226)\(null)!: (caller: ) Exception(0) 83FF004A Bad message    Msg:[Unknown base64 character: 41 (')')]
..\Enclave\api.cpp(618)\(null)!: (caller: ) LogHr(0) 83FF004A Bad message    Msg:[Unhandled Enclave Exception: "Bad message"]
At line:1 char:1
+ set-AzAttestationPolicy -Name $attestationProvider -ResourceGroupName ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzAttestationPolicy], RestException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Attestation.SetAzureAttestationPolicy
```

**Pasos para solucionar problemas** Asegúrese de que la directiva en formato de texto está codificada con UTF-8.

Si se requiere la firma de la directiva, la directiva de atestación debe configurarse solo en formato RFC7519 JSON Web Token (JWT). Si no se requiere la firma de la directiva, esta se puede configurar en formato de texto o JWT.

Para configurar una directiva en formato JWT, use JWT con una notificación denominada "AttestationPolicy". El valor de la notificación es la versión codificada Base64URL del texto de la directiva. Si el proveedor de atestación se configura con certificados de firmante de directivas, JWT debe estar firmado con clave privada de cualquiera de los certificados de firmante de directivas válidos asociados al proveedor. 

Para configurar una directiva en formato de texto, especifique el texto de la directiva directamente.

En PowerShell, especifique PolicyFormat como JWT para configurar la directiva en formato JWT. El formato de directiva predeterminado es Texto.

Vea [ejemplos de directivas de atestación](./policy-examples.md) y [Procedimiento para crear y firmar una directiva de atestación](./author-sign-policy.md) 

## <a name="3-azattestation-installation-issues-in-powershell"></a>3. Problemas de instalación de Az.Attestation en PowerShell

No se pueden instalar los módulos Az o Az.Attestation en PowerShell

### <a name="error"></a>Error

ADVERTENCIA: No se puede resolver el origen del paquete "https://www.powershellgallery.com/api/v2" PackageManagement\Install-Package: No match was found for the specified search criteria and module name (PackageManagement\Install-Package: No se encontraron coincidencias para los criterios de búsqueda y el nombre de módulo especificados)

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

La Galería de PowerShell ha dejado de usar las versiones 1.0 y 1.1 de Seguridad de la capa de transporte (TLS). 

Se recomienda usar TLS 1.2 o una versión posterior. 

Para seguir interactuando con la Galería de PowerShell, ejecute el siguiente comando antes que los comandos Install-Module:

**[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12**

## <a name="4-policy-accessconfiguration-issues-in-powershell"></a>4. Problemas de acceso y configuración de directivas en PowerShell

Usuario asignado con roles adecuados pero que se enfrenta a problemas de autorización al administrar directivas de atestación mediante PowerShell.

### <a name="error"></a>Error
El cliente con el &lt;identificador de objeto&gt; no tiene autorización para realizar la acción Microsoft.Authorization/roleassignments/write en el ámbito "subcriptions/&lt;subscriptionId&gt;resourcegroups/secure_enclave_poc/providers/Microsoft.Authorization/roleassignments/&lt;role assignmentId&gt;" o el ámbito no es válido. Si el acceso se ha concedido recientemente, actualice las credenciales.

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

Las versiones mínimas de los módulos Az necesarios para admitir las operaciones de atestación son las siguientes: 

 **Az 4.5.0** 
 
 **Az.Accounts 1.9.2**
 
 **Az.Attestation 0.1.8** 

Ejecute el siguiente comando para comprobar la versión instalada de todos los módulos Az: 

```powershell
Get-InstalledModule 
```

Si las versiones no coinciden con el requisito mínimo, ejecute comandos Update-Module,

por ejemplo, - Update-Module -Name Az.Attestation
