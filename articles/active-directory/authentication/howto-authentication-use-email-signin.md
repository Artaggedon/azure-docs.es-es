---
title: Iniciar sesión con el correo electrónico como id. de inicio de sesión alternativo para Azure Active Directory
description: Aprenda a configurar y permitir que los usuarios inicien sesión en Azure Active Directory mediante su dirección de correo electrónico como id. de inicio de sesión alternativo (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 10/01/2020
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: calui
ms.openlocfilehash: 4e39d7f15e3ca3c6e241c767a5f881d7170c6379
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99255974"
---
# <a name="sign-in-to-azure-active-directory-using-email-as-an-alternate-login-id-preview"></a>Iniciar sesión en Azure mediante el correo electrónico como id. de inicio de sesión alternativo (versión preliminar)

> [!NOTE]
> Iniciar sesión en Azure AD con el correo electrónico como id. de inicio de sesión alternativo es una característica en vista previa (GB) pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Muchas organizaciones desean que los usuarios puedan iniciar sesión en Azure Active Directory (Azure AD) con las mismas credenciales que su entorno de directorio local. Con este enfoque, conocido como autenticación híbrida, los usuarios solo necesitan recordar un conjunto de credenciales.

Algunas organizaciones no han migrado a la autenticación híbrida por las siguientes razones:

* De forma predeterminada, el nombre principal de usuario (UPN) de Azure Active Directory (Azure AD) se establece en el mismo UPN que el directorio local.
* Al cambiar el UPN de Azure AD, se produce una coincidencia incorrecta entre el entorno local y el entorno de Azure que podría dar problemas con ciertas aplicaciones y servicios.
* Debido a motivos empresariales o de cumplimiento, la organización no desea usar el UPN local para iniciar sesión en Azure.

Para facilitar la adopción de la autenticación híbrida, ahora puede configurar Azure AD para que los usuarios puedan iniciar sesión en Azure con un correo electrónico en su dominio verificado como un identificador de inicio de sesión alternativo. Por ejemplo, si *Contoso* cambia de nombre a *Fabrikam*, en lugar de seguir iniciando sesión con el UPN heredado `balas@contoso.com`, ahora se puede usar el correo electrónico como id. de inicio de sesión alternativo. Para acceder a una aplicación o a servicios, los usuarios inician sesión en Azure AD con su correo electrónico asignado, como `balas@fabrikam.com`.

En este artículo se muestra cómo habilitar y usar el correo electrónico como id. de inicio de sesión alternativo. Esta característica está disponible en la edición Azure AD Free y en versiones posteriores.

> [!NOTE]
> Esta característica es solo para usuarios de Azure AD autenticados en la nube.

> [!NOTE]
> Actualmente, esta característica no se admite en dispositivos Windows 10 unidos a Azure AD para inquilinos con la autenticación en la nube. Esta característica no se aplica a los dispositivos unidos a Azure AD híbrido.

## <a name="overview-of-azure-ad-sign-in-approaches"></a>Información general de los métodos de inicio de sesión Azure AD

Para iniciar sesión en Azure AD, los usuarios escriben un nombre que identifica de forma única su cuenta. Antes, solo podía usar el UPN de Azure AD como el nombre de inicio de sesión.

En el caso de las organizaciones en las que el UPN local es el correo electrónico de inicio de sesión elegido por el usuario, este era un enfoque óptimo. Esas organizaciones configuraban el UPN de Azure AD con el mismo valor que el UPN local, y los usuarios tenían una experiencia de inicio de sesión coherente.

Sin embargo, en algunas organizaciones el UPN local no se usaba como nombre de inicio de sesión. En los entornos locales, se configuraba AD DS local para facilitar el inicio de sesión con un identificador de inicio de sesión alternativo. Configurar el UPN de Azure AD con el mismo valor que el UPN local no era una alternativa, ya que Azure AD exigía que los usuarios iniciaran sesión con ese valor.

La solución habitual a este problema era configurar el UPN de Azure AD con la dirección de correo electrónico con la que el usuario espera iniciar sesión. Este enfoque funciona, aunque genera distintos UPN entre la instancia de AD local y Azure AD, y esta configuración no es compatible con todas las cargas de trabajo de Microsoft 365.

Un enfoque diferente consiste en sincronizar los UPN de Azure AD y AD local con el mismo valor y, a continuación, configurar Azure AD para permitir que los usuarios inicien sesión con un correo electrónico verificado. Para ello, defina una o más direcciones de correo electrónico en el atributo *ProxyAddresses* del usuario en el directorio local. El atributo *ProxyAddresses* se sincroniza en Azure AD automáticamente mediante Azure AD Connect.

## <a name="preview-limitations"></a>Limitaciones de vista previa

El inicio de sesión en Azure AD con el correo electrónico como id. de inicio de sesión alternativo está disponible en la edición Azure AD Free y en versiones posteriores.

En el estado de versión preliminar actual, rigen las siguientes limitaciones cuando un usuario inicia sesión con un correo electrónico que no es UPN como identificador de inicio de sesión alternativo:

* Los usuarios pueden ver su UPN, incluso cuando han iniciado sesión con su correo electrónico que no sea UPN. Se puede observar el siguiente comportamiento de ejemplo:
    * Al usuario se le solicita que inicie sesión con su UPN cuando se le dirige al inicio de sesión de Azure AD con `login_hint=<non-UPN email>`.
    * Cuando un usuario inicia sesión con un correo electrónico que no es UPN y escribe una contraseña incorrecta, la página *"Escriba la contraseña"* cambia para mostrar el UPN.
    * En algunos sitios y aplicaciones de Microsoft, como [https://portal.azure.com](https://portal.azure.com) y Microsoft Office, el control **Administrador de cuentas** que se muestra normalmente en la esquina superior derecha puede mostrar el UPN del usuario en lugar del correo electrónico que no sea UPN usado para iniciar sesión.

* Actualmente, algunos flujos no son compatibles con el correo electrónico que no sea UPN, como los siguientes:
    * Identity Protection no coincide actualmente con los identificadores de inicio de sesión alternativos de correo electrónico con la detección de riesgo *Credenciales filtradas*. Esta detección de riesgo usa el UPN para asociar las credenciales que se han filtrado. Para obtener más información, consulte [Detección y corrección de riesgos de Azure AD Identity Protection][identity-protection].
    * Las invitaciones B2B enviadas a un correo electrónico de identificador de inicio de sesión alternativo no se admiten totalmente. Después de aceptar una invitación enviada a un correo electrónico como identificador de inicio de sesión alternativo, es posible que el inicio de sesión con el correo electrónico alternativo no funcione para el usuario en el punto de conexión de inquilino.

## <a name="synchronize-sign-in-email-addresses-to-azure-ad"></a>Sincronizar las direcciones de correo electrónico de inicio de sesión con Azure AD

La autenticación tradicional de Active Directory Domain Services (AD DS) o Servicios de federación de Active Directory (AD FS) se produce directamente en la red y se controla mediante la infraestructura de AD DS. Con la autenticación híbrida, los usuarios pueden iniciar sesión directamente en Azure AD.

Para admitir este enfoque de autenticación híbrida, debe sincronizar el entorno de AD DS local con Azure AD mediante [Azure AD Connect][azure-ad-connect] y configurarlo para que use la sincronización de hash de contraseñas (PHS) o la autenticación de paso a través (PTA).

En ambas opciones de configuración, el usuario envía su nombre de usuario y contraseña a Azure AD, que valida las credenciales y emite un vale. Cuando los usuarios inician sesión en Azure AD, se elimina la necesidad de que su organización albergue y administre una infraestructura de AD FS.

![Diagrama de identidad híbrida de Azure AD con sincronización de hash de contraseñas](media/howto-authentication-use-email-signin/hybrid-password-hash-sync.png)

![Diagrama de identidad híbrida de Azure AD con autenticación de paso a través](media/howto-authentication-use-email-signin/hybrid-pass-through-authentication.png)

Uno de los atributos de usuario que se sincroniza automáticamente mediante Azure AD Connect es *ProxyAddresses*. Si los usuarios tienen una dirección de correo electrónico definida en el entorno de AD DS local como parte del atributo *ProxyAddresses*, se sincroniza automáticamente con Azure AD. Esta dirección de correo electrónico se puede usar directamente en el proceso de inicio de sesión de Azure AD como identificador de inicio de sesión alternativo.

> [!IMPORTANT]
> Solo los mensajes de correo electrónico de los dominios comprobados del inquilino se sincronizan con Azure AD. Cada inquilino de Azure AD tiene uno o varios dominios comprobados, para los que se ha comprobado la propiedad, y se enlazan de forma exclusiva con el inquilino.
>
> Para más información, consulte [Agregar y comprobar un nombre de dominio personalizado en Azure AD][verify-domain].

Para más información, consulte [Selección del método de autenticación adecuado para la solución de identidad híbrida de Azure AD][hybrid-auth-methods].

## <a name="enable-user-sign-in-with-an-email-address"></a>Habilitar el inicio de sesión de usuario con una dirección de correo electrónico

Una vez que los usuarios con el atributo *ProxyAddresses* aplicado se sincronizan con Azure AD mediante Azure AD Connect, necesita habilitar la función para que los usuarios se registren con el correo electrónico como un ID de inicio de sesión alternativo para su inquilino. Esta característica indica a los servidores de inicio de sesión de Azure AD que no solo comprueben el nombre de inicio de sesión con los valores UPN, sino también contra los valores *ProxyAddresses* de la dirección de correo electrónico.

Durante la versión preliminar, actualmente solo puede habilitar el inicio de sesión con correo electrónico como una característica de identificador de inicio de sesión alternativo mediante PowerShell. Necesita permiso de *administrador de inquilinos* para completar los siguientes pasos:

1. Abra una sesión de PowerShell como administrador, luego instale el módulo *AzureADPreview* con el cmdlet [Install-Module][Install-Module]:

    ```powershell
    Install-Module AzureADPreview
    ```

    Si se le solicita, seleccione **Y** para instalar NuGet o para instalar desde un repositorio que no es de confianza.

1. Ingrese a su inquilino Azure AD como *administrador de inquilinos* mediante el cmdlet [Conecta-AzureAD][Connect-AzureAD]:

    ```powershell
    Connect-AzureAD
    ```

    El comando devuelve información acerca de su cuenta, entorno e id. de inquilino.

1. Compruebe si la directiva de *HomeRealmDiscoveryPolicy* ya existe en el inquilino con el cmdlet [Get-AzureADPolicy][Get-AzureADPolicy] como se indica a continuación:

    ```powershell
    Get-AzureADPolicy | Where-Object Type -eq "HomeRealmDiscoveryPolicy" | Format-List *
    ```

1. Si no hay ninguna directiva configurada actualmente, el comando no devuelve nada. Si se devuelve una directiva, omita este paso y vaya al paso siguiente para actualizar una directiva existente.

    Para agregar la directiva de *HomeRealmDiscoveryPolicy* al inquilino, use el cmdlet [New-AzureADPolicy][New-AzureADPolicy] y establezca el atributo *AlternateIdLogin* en *"Enabled": true* como se muestra en el siguiente ejemplo:

    ```powershell
    $AzureADPolicyDefinition = @(
      @{
         "HomeRealmDiscoveryPolicy" = @{
            "AlternateIdLogin" = @{
               "Enabled" = $true
            }
         }
      } | ConvertTo-JSON -Compress
    )
    $AzureADPolicyParameters = @{
      Definition            = $AzureADPolicyDefinition
      DisplayName           = "BasicAutoAccelerationPolicy"
      IsOrganizationDefault = $true
      Type                  = "HomeRealmDiscoveryPolicy"
    }
    New-AzureADPolicy @AzureADPolicyParameters
    ```

    Cuando la política se haya creado con éxito, el comando devuelve el Id. de la política, como se muestra en la siguiente salida de ejemplo:

    ```powershell
    Id                                   DisplayName                 Type                     IsOrganizationDefault
    --                                   -----------                 ----                     ---------------------
    5de3afbe-4b7a-4b33-86b0-7bbe308db7f7 BasicAutoAccelerationPolicy HomeRealmDiscoveryPolicy True
    ```

1. Si ya hay una política configurada, compruebe si el atributo *AlternateIdLogin*  está habilitado, como se muestra en la siguiente salida directiva de ejemplo:

    ```powershell
    Id : 5de3afbe-4b7a-4b33-86b0-7bbe308db7f7
    OdataType :
    AlternativeIdentifier :
    Definition : {{"HomeRealmDiscoveryPolicy" :{"AlternateIdLogin":{"Enabled": true}}}}
    DisplayName : BasicAutoAccelerationPolicy
    IsOrganizationDefault : True
    KeyCredentials : {}
    Type : HomeRealmDiscoveryPolicy
    ```

    Si la directiva existe pero el atributo *AlternateIdLogin* no está presente o habilitado, o si existen otros atributos en la directiva que desea conservar, actualice la directiva existente con el cmdlet [Set-AzureADPolicy][Set-AzureADPolicy].

    > [!IMPORTANT]
    > Al actualizar la directiva, asegúrese de que incluye cualquier configuración anterior y el nuevo atributo *AlternateIdLogin*.

    En el ejemplo siguiente se agrega el atributo *AlternateIdLogin* y se conserva el atributo  *AllowCloudPasswordValidation* que se puede haber establecido:

    ```powershell
    $AzureADPolicyDefinition = @(
      @{
         "HomeRealmDiscoveryPolicy" = @{
            "AllowCloudPasswordValidation" = $true
            "AlternateIdLogin" = @{
               "Enabled" = $true
            }
         }
      } | ConvertTo-JSON -Compress
    )
    $AzureADPolicyParameters = @{
      ID                    = "b581c39c-8fe3-4bb5-b53d-ea3de05abb4b"
      Definition            = $AzureADPolicyDefinition
      DisplayName           = "BasicAutoAccelerationPolicy"
      IsOrganizationDefault = $true
      Type                  = "HomeRealmDiscoveryPolicy"
    }
    
    Set-AzureADPolicy @AzureADPolicyParameters
    ```

    Confirme que la directiva actualizada muestra los cambios y que el atributo *AlternateIdLogin* está habilitado ahora:

    ```powershell
    Get-AzureADPolicy | Where-Object Type -eq "HomeRealmDiscoveryPolicy" | Format-List *
    ```

Una vez que se aplica la directiva, puede tardar hasta una hora en propagarse y para que los usuarios puedan iniciar sesión con su identificador de inicio de sesión alternativo.

## <a name="test-user-sign-in-with-email"></a>Prueba del inicio de sesión de usuario con correo electrónico

Para probar que los usuarios pueden iniciar sesión con el correo electrónico, vaya a [https://myprofile.microsoft.com][my-profile] e inicie sesión con una cuenta de usuario basada en su dirección de correo electrónico, como `balas@fabrikam.com`, no su UPN, como `balas@contoso.com`. La experiencia de inicio de sesión debe verse y sentirse igual que con un evento de inicio de sesión basado en UPN.

## <a name="enable-staged-rollout-to-test-user-sign-in-with-an-email-address"></a>Habilitación del lanzamiento preconfigurado para probar el inicio de sesión de usuario con una dirección de correo electrónico  

El [lanzamiento preconfigurado][staged-rollout] permite a los administradores de inquilinos habilitar características para grupos específicos. Se recomienda que los administradores de inquilinos usen esta característica para probar el inicio de sesión de usuario con una dirección de correo electrónico. Cuando los administradores están preparados para implementar el lanzamiento preconfigurado en todo el inquilino, deberán usar una directiva de detección del dominio de inicio.  


Necesita permiso de *administrador de inquilinos* para completar los siguientes pasos:

1. Abra una sesión de PowerShell como administrador e instale el módulo *AzureADPreview* con el cmdlet [Install-Module][Install-Module]:

    ```powershell
    Install-Module AzureADPreview
    ```

    Si se le solicita, seleccione **Y** para instalar NuGet o para instalar desde un repositorio que no es de confianza.

2. Ingrese a su inquilino Azure AD como *administrador de inquilinos* mediante el cmdlet [Conecta-AzureAD][Connect-AzureAD]:

    ```powershell
    Connect-AzureAD
    ```

    El comando devuelve información acerca de su cuenta, entorno e id. de inquilino.

3. Enumere todas las directivas de lanzamiento preconfigurado existentes con el siguiente cmdlet:
   
   ```powershell
   Get-AzureADMSFeatureRolloutPolicy
   ``` 

4. Si no hay directivas de lanzamiento preconfigurado para esta característica, cree una nueva directiva y tome nota de su identificador:

   ```powershell
   $AzureADMSFeatureRolloutPolicy = @{
      Feature    = "EmailAsAlternateId"
      DisplayName = "EmailAsAlternateId Rollout Policy"
      IsEnabled   = $true
   }
   New-AzureADMSFeatureRolloutPolicy @AzureADMSFeatureRolloutPolicy
   ```

5. Busque el identificador de directoryObject correspondiente al grupo que se va a agregar a la directiva de lanzamiento preconfigurado. Tome nota del valor devuelto para el parámetro *Id*, porque se usará en el paso siguiente.
   
   ```powershell
   Get-AzureADMSGroup -SearchString "Name of group to be added to the staged rollout policy"
   ```

6. Agregue el grupo a la directiva de lanzamiento preconfigurado, tal como se muestra en el ejemplo siguiente. Reemplace el valor del parámetro *-Id* por el valor devuelto de identificador de directiva en el paso 4 y reemplace el valor del parámetro *-RefObjectId* por el valor de *Id* anotado en el paso 5. Puede pasar hasta 1 hora antes de que los usuarios del grupo puedan usar sus direcciones de proxy para iniciar sesión.

   ```powershell
   Add-AzureADMSFeatureRolloutPolicyDirectoryObject -Id "ROLLOUT_POLICY_ID" -RefObjectId "GROUP_OBJECT_ID"
   ```
   
En el caso de los nuevos miembros agregados al grupo, pueden transcurrir hasta 24 horas antes de que puedan usar sus direcciones de proxy para iniciar sesión.

### <a name="removing-groups"></a>Eliminación de grupos

Para quitar un grupo de una directiva de lanzamiento preconfigurado, ejecute el siguiente comando:

```powershell
Remove-AzureADMSFeatureRolloutPolicyDirectoryObject -Id "ROLLOUT_POLICY_ID" -ObjectId "GROUP_OBJECT_ID" 
```

### <a name="removing-policies"></a>Eliminación de directivas

Para quitar una directiva de lanzamiento preconfigurado, deshabilite primero la directiva y después quítela del sistema:

```powershell
Set-AzureADMSFeatureRolloutPolicy -Id "ROLLOUT_POLICY_ID" -IsEnabled $false 
Remove-AzureADMSFeatureRolloutPolicy -Id "ROLLOUT_POLICY_ID"
```

## <a name="troubleshoot"></a>Solución de problemas

Si los usuarios tienen problemas con los eventos de inicio de sesión que usan su dirección de correo electrónico, revise los siguientes pasos de solución de problemas:

1. Asegúrese de que la cuenta de usuario tiene su dirección de correo electrónico establecida para el atributo *ProxyAddresses* en el entorno de AD DS local.
1. Compruebe que Azure AD Connect está configurado y sincroniza correctamente las cuentas de usuario del entorno de AD DS local en Azure AD.
1. Confirme que la directiva Azure AD *HomeRealmDiscoveryPolicy* tiene el atributo *AlternateIdLogin* establecido en *"Enabled": true*:

    ```powershell
    Get-AzureADPolicy | Where-Object Type -eq "HomeRealmDiscoveryPolicy" | Format-List *
    ```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la identidad híbrida, como Proxy de aplicación de Azure AD o Azure AD Domain Services, consulte [Identidad híbrida Azure AD para el acceso y la gestión de las cargas de trabajo locales][hybrid-overview].

Para obtener más información sobre las operaciones de identidad híbrida, consulte cómo funciona [Sincronización hash de contraseña][phs-overview] o [Autenticación de paso a través][pta-overview].

<!-- INTERNAL LINKS -->
[verify-domain]: ../fundamentals/add-custom-domain.md
[hybrid-auth-methods]: ../hybrid/choose-ad-authn.md
[azure-ad-connect]: ../hybrid/whatis-azure-ad-connect.md
[hybrid-overview]: ../hybrid/cloud-governed-management-for-on-premises.md
[phs-overview]: ../hybrid/how-to-connect-password-hash-synchronization.md
[pta-overview]: ../hybrid/how-to-connect-pta-how-it-works.md
[identity-protection]: ../identity-protection/overview-identity-protection.md#risk-detection-and-remediation

<!-- EXTERNAL LINKS -->
[Install-Module]: /powershell/module/powershellget/install-module
[Connect-AzureAD]: /powershell/module/azuread/connect-azuread
[Get-AzureADPolicy]: /powershell/module/azuread/get-azureadpolicy
[New-AzureADPolicy]: /powershell/module/azuread/new-azureadpolicy
[Set-AzureADPolicy]: /powershell/module/azuread/set-azureadpolicy
[staged-rollout]: /powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true#staged-rollout
[my-profile]: https://myprofile.microsoft.com
