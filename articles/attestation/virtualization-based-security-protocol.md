---
title: Protocolo de seguridad basada en virtualización (VBS) para Azure Attestation
description: Protocolo de atestación de VBS
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: reference
ms.date: 07/20/2020
ms.author: mbaldwin
ms.openlocfilehash: 38012c5b4bb9338c1200d9583256193ee8402c98
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99507920"
---
# <a name="trusted-platform-module-tpm-and-virtualization-based-securityvbs-enclave-attestation-protocol"></a>Módulo de plataforma segura (TPM) y protocolo de atestación de enclave de seguridad basada en virtualización (VBS) 

Para proporcionar una sólida garantía de seguridad, Microsoft Azure Attestation se basa en la comprobación de una cadena de confianza que se mantiene desde una raíz de confianza (TPM) hasta el inicio del hipervisor y el kernel seguro. Para lograrlo, Azure Attestation debe atestiguar en el estado de arranque de la máquina antes de poder establecer la confianza en el enclave seguro. El sistema operativo, el hipervisor y los binarios de kernel seguros deben estar firmados por las autoridades oficiales de Microsoft adecuadas y estar configurados de forma segura. Una vez que se ha enlazado la confianza entre el Módulo de plataforma segura (TPM) y el estado del hipervisor, podemos confiar en los IDK del encave de seguridad basada en virtualización (VBS) que se proporcionan en el registro Arranque medido; con esto se puede validar que el enclave ha generado un par de claves y en el que se muestra un informe de atestación que enlaza la confianza en esa clave y contiene otras notificaciones, como el nivel de seguridad y las propiedades de atestación de arranque. 

Los enclaves de VBS requieren un TPM para proporcionar la medida para validar la base de seguridad. Los enclaves de VBS se certifican mediante el punto de conexión de TPM con una adición al objeto de solicitud en el protocolo. 

## <a name="protocol-messages"></a>Mensajes de protocolo

### <a name="init-message"></a>Mensaje init

#### <a name="direction"></a>Dirección

Cliente -> Azure Attestation

#### <a name="payload"></a>Carga

```
{ 
  "type": "aikcert" 
} 
```

"type" (cadena ASCII): representa el tipo de atestación solicitado. Actualmente, solo se admite"aikcert".

### <a name="challenge-message"></a>Mensaje de desafío

#### <a name="direction"></a>Dirección

Azure Attestation -> Cliente

#### <a name="payload"></a>Carga

```
{ 
  "challenge": "<BASE64URL(CHALLENGE)>", 
  "service_context": "<BASE64URL(SERVICECONTEXT)>" 
} 
```

**challenge** (BASE64URL(OCTETS)): el valor aleatorio emitido por el servicio.

**service_context** (BASE64URL(OCTETS)): Contexto opaco creado por el servicio.


### <a name="request-message"></a>Mensaje de solicitud

#### <a name="direction"></a>Dirección

Cliente -> Azure Attestation 

#### <a name="payload"></a>Carga

```
{
  "request": "<JWS>"
}
```

**request** (JWS): La solicitud consta de una estructura de firma web JSON (JWS). A continuación se muestran el encabezado protegido JWS y la carga JWS. Como en cualquier estructura JWS, el valor final consta de:

BASE64URL(UTF8(JWS Protected Header)) || '.' ||

BASE64URL(JWS Payload) || '.' ||

BASE64URL(JWS Signature)

##### <a name="jws-protected-header"></a>Encabezado protegido JWS

```
{
  "alg": "PS256",
  "typ": "attReq"
  // no "kid" parameter as the key specified by attest_key MUST sign this JWS to prove possession.
}
```

##### <a name="jws-payload"></a>Carga JWS

La carga JWS puede ser de tipo básico o VBS. Se utiliza el tipo básico cuando la evidencia de la atestación no incluye datos VBS. 

Ejemplo solo de TPM: 

``` 
{ 
  "att_type": "basic", 
  "att_data": { 
    "rp_id": "<URL>", 
    "rp_data": "<BASE64URL(RPCUSTOMDATA)>", 
    "challenge": "<BASE64URL(CHALLENGE)>", 

    "tpm_att_data": { 
      "srtm_boot_log": "<BASE64URL(SRTMBOOTLOG)>", 
      "srtm_resume_log": "<BASE64URL(SRTMRESUMELOG)>", 
      "drtm_boot_log": "<BASE64URL(DRTMBOOTLOG)>", 
      "drtm_resume_log": "<BASE64URL(DRTMRESUMELOG)>", 
      "aik_cert": "<BASE64URL(AIKCERTIFICATE)>", 

      // aik_pub is represented as a JSON Web Key (JWK) object (RFC 7517). 

      "aik_pub": { 
        "kty": "RSA", 
        "n": "<Base64urlUInt(MODULUS)>", 
        "e": "<Base64urlUInt(EXPONENT)>" 
      }, 
      "current_claim": "<BASE64URL(CURRENTCLAIM)>", 
      "boot_claim": "<BASE64URL(BOOTCLAIM)>" 
    }, 

    // attest_key is represented as a JSON Web Key (JWK) object (RFC 7517). 

    "attest_key": { 
      "kty": "RSA", 
      "n": "<Base64urlUInt(MODULUS)>", 
      "e": "<Base64urlUInt(EXPONENT)>" 
    }, 
    "custom_claims": [ 
      { 
        "name": "<name>", 
        "value": "<value>", 
        "value_type": "<value_type>" 
      }, 
      { 
        "name": "<name>", 
        "value": "<value>", 
        "value_type": "<value_type>" 
      } 
    ], 
    "service_context": "<BASE64URL(SERVICECONTEXT)>" 
  } 
} 
```

Ejemplo de enclave de TPM + VBS: 

``` 
{ 
  "att_type": "vbs", 
  "att_data": { 
    "report_signed": { 
      "rp_id": "<URL>", 
      "rp_data": "<BASE64URL(RPCUSTOMDATA)>", 
      "challenge": "<BASE64URL(CHALLENGE)>", 
      "tpm_att_data": { 
        "srtm_boot_log": "<BASE64URL(SRTMBOOTLOG)>", 
        "srtm_resume_log": "<BASE64URL(SRTMRESUMELOG)>", 
        "drtm_boot_log": "<BASE64URL(DRTMBOOTLOG)>", 
        "drtm_resume_log": "<BASE64URL(DRTMRESUMELOG)>", 
        "aik_cert": "<BASE64URL(AIKCERTIFICATE)>", 

        // aik_pub is represented as a JSON Web Key (JWK) object (RFC 7517). 

        "aik_pub": { 
          "kty": "RSA", 
          "n": "<Base64urlUInt(MODULUS)>", 
          "e": "<Base64urlUInt(EXPONENT)>" 
        }, 
        "current_claim": "<BASE64URL(CURRENTCLAIM)>", 
        "boot_claim": "<BASE64URL(BOOTCLAIM)>" 
      }, 

      // attest_key is represented as a JSON Web Key (JWK) object (RFC 7517). 

      "attest_key": { 
        "kty": "RSA", 
        "n": "<Base64urlUInt(MODULUS)>", 
        "e": "<Base64urlUInt(EXPONENT)>" 
      }, 
      "custom_claims": [ 
        { 
          "name": "<name>", 
          "value": "<value>", 
          "value_type": "<value_type>" 
        }, 
        { 
          "name": "<name>", 
          "value": "<value>", 
          "value_type": "<value_type>" 
        } 
      ], 
      "service_context": "<BASE64URL(SERVICECONTEXT)>" 
    }, 
    "vsm_report": "<BASE64URL(REPORT)>" 
  } 
} 
``` 

**rp_id** (StringOrURI): identificador para usuario de confianza. Utilizado por el servicio en el cálculo de la notificación del identificador de máquina.

**rp_data** (BASE64URL(OCTETS)): datos opacos pasados por el usuario de confianza. Normalmente, lo utiliza el usuario de confianza como un nonce para garantizar la actualización del informe.

**challenge** (BASE64URL(OCTETS)): el valor aleatorio emitido por el servicio.

**tpm_att_data**: datos de atestación relacionados con el módulo de plataforma segura.

- **srtm_boot_log (BASE64URL(OCTETS))** : Registros de arranque SRTM tal como los ha recuperado la función Tbsi_Get_TCG_Log_Ex con el tipo de registro = TBS_TCGLOG_SRTM_BOOT.

- **srtm_resume_log (BASE64URL(OCTETS))** : Registros de reanudación SRTM tal como los ha recuperado la función Tbsi_Get_TCG_Log_Ex con el tipo de registro = TBS_TCGLOG_SRTM_RESUME.

- **drtm_boot_log (BASE64URL(OCTETS))** : Registros de arranque DRTM tal como los ha recuperado la función Tbsi_Get_TCG_Log_Ex con el tipo de registro = TBS_TCGLOG_DRTM_BOOT.

- **drtm_resume_log (BASE64URL(OCTETS))** : Registros de reanudación DRTM tal como los ha recuperado la función Tbsi_Get_TCG_Log_Ex con el tipo de registro = TBS_TCGLOG_DRTM_RESUME.

- **aik_cert (BASE64URL(OCTETS))** : certificado X. 509 para AIK tal como lo ha devuelto la función NCryptGetProperty con el tipo de registro = NCRYPT_CERTIFICATE_PROPERTY.

- **aik_pub**: la parte pública de AIK representada como objeto de clave web JSON (JWK) (RFC 7517).

- **current_claim (BASE64URL(OCTETS))** : la notificación de atestación para el estado del registro de configuración de la plataforma actual tal como la devuelve la función NCryptCreateClaim con dwClaimType = NCRYPT_CLAIM_PLATFORM y el parámetro NCRYPTBUFFER_TPM_PLATFORM_CLAIM_PCR_MASK establecido para incluir todos los registros de configuración de la plataforma. El desafío que envía el servicio también debe utilizarse en el cálculo de esta notificación.

- **boot_claim (BASE64URL(OCTETS))** : la notificación de atestación para el estado del registro de configuración de la plataforma en el arranque tal como la devuelve la función NCryptCreateClaim con dwClaimType = NCRYPT_CLAIM_PLATFORM y el parámetro NCRYPTBUFFER_TPM_PLATFORM_CLAIM_PCR_MASK establecido para incluir todos los registros de configuración de la plataforma.

**vsm_report** (BASE64URL(OCTETS)): el informe de atestación del enclave VBS tal como lo ha devuelto la función EnclaveGetAttestationReport. El parámetro EnclaveData debe ser el hash SHA-512 del valor de report_signed (incluidas las llaves de apertura y cierre). La entrada de la función hash es UTF8(report_signed).

**attest_key**: la parte pública de la clave del enclave representada como un objeto de clave web JSON (JWK) (RFC 7517).

**custom_claims**: la matriz de notificaciones de enclave personalizadas enviadas al servicio que la directiva puede evaluar. La notificación

- **name (String)** : el nombre de la notificación. Este nombre lo anexará a una dirección URL determinada el servicio de atestación (para evitar conflictos), y la cadena concatenada se convertirá en el tipo de notificación que se puede usar en la directiva.

- **value (String)** : el valor de la notificación.

- **value_type (String)** : el tipo de datos del valor de la notificación.

**service_context** (BASE64URL(OCTETS)): Contexto opaco creado por el servicio.

### <a name="report-message"></a>Mensaje de informe

#### <a name="direction"></a>Dirección

Azure Attestation -> Cliente

#### <a name="payload"></a>Carga

```
{
  "report": "<JWT>"
}
```

**report** (JWT): el informe de atestación en formato de token web JSON (JWT) (RFC 7519).

## <a name="next-steps"></a>Pasos siguientes

- [Flujo de trabajo de Azure Attestation](workflow.md)
