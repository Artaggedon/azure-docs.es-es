---
title: 'Autenticación de código de acceso de un solo uso para usuarios invitados de B2B: Azure AD'
description: Cómo usar el código de acceso de un solo uso de correo electrónico para autenticar a los usuarios invitados de B2B sin necesidad de una cuenta de Microsoft.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 02/12/2021
ms.author: mimart
author: msmimart
manager: CelesteDG
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: f37c7e2f21c76fcc902b0922399081b9be949e99
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100365538"
---
# <a name="email-one-time-passcode-authentication"></a>Autenticación con código de acceso de un solo uso por correo electrónico

En este artículo se describe cómo habilitar la autenticación con código de acceso de un solo uso por correo electrónico para usuarios invitados de B2B. La característica de código de acceso de un solo uso por correo electrónico autentica los usuarios invitados de B2B cuando no pueden autenticarse por otros medios, como Azure AD, una cuenta de Microsoft (MSA) o la federación de Google. Con la autenticación por código de acceso de un solo uso, no hay necesidad de crear una cuenta de Microsoft. Cuando el usuario invitado canjea una invitación o accede a un recurso compartido, puede solicitar un código temporal, que se envía a su dirección de correo electrónico. A continuación, escribe este código para continuar con el inicio de sesión.

![Diagrama de información general del código de acceso de un solo uso por correo electrónico](media/one-time-passcode/email-otp.png)

> [!IMPORTANT]
> **A partir de octubre de 2021**, la característica de código de acceso de un solo uso por correo electrónico se activará para todos los inquilinos existentes, y se habilitará de forma predeterminada para los nuevos. Si no desea permitir que esta característica se active automáticamente, puede deshabilitarla. Consulte [Deshabilitación del código de acceso de un solo uso por correo electrónico](#disable-email-one-time-passcode) más adelante.

> [!NOTE]
> Los usuarios con código de acceso de un solo uso deben iniciar sesión con un vínculo que incluya el contexto del inquilino (por ejemplo, `https://myapps.microsoft.com/?tenantid=<tenant id>` o `https://portal.azure.com/<tenant id>`, o en el caso de un dominio verificado, `https://myapps.microsoft.com/<verified domain>.onmicrosoft.com`). Los vínculos directos a aplicaciones y los recursos también funcionan siempre que incluyan el contexto del inquilino. Actualmente, los usuarios invitados no pueden iniciar sesión con puntos de conexión sin contexto de inquilino. Por ejemplo, si se usan `https://myapps.microsoft.com`, `https://portal.azure.com`, se producirá un error.

## <a name="user-experience-for-one-time-passcode-guest-users"></a>Experiencia de usuario para los usuarios invitados de código de acceso de un solo uso

Cuando la característica de código de acceso de un solo uso por correo electrónico está habilitada, los usuarios nuevos invitados [que cumplan ciertas condiciones](#when-does-a-guest-user-get-a-one-time-passcode) usarán la autenticación con código de acceso de un solo uso. Los usuarios invitados que canjearon una invitación antes de la habilitación del código de acceso de un solo uso por correo electrónico seguirán usando su mismo método de autenticación.

Con la autenticación de código de acceso de un solo uso, el usuario invitado puede canjear su invitación haciendo clic en un vínculo directo o mediante el correo electrónico de invitación. En cualquier caso, un mensaje en el explorador indica que se enviará un código a la dirección de correo electrónico del usuario invitado. El usuario invitado selecciona **Enviar código**:

   ![Captura de pantalla que muestra el botón Enviar código.](media/one-time-passcode/otp-send-code.png)

Se envía un código de acceso a la dirección de correo electrónico del usuario. El usuario recupera el código de acceso del correo electrónico y lo escribe en la ventana del explorador:

   ![Captura de pantalla que muestra la página Escribir código.](media/one-time-passcode/otp-enter-code.png)

Ahora el usuario invitado se ha autenticado y puede ver el recurso compartido o continuar con el inicio de sesión.

> [!NOTE]
> Los códigos de acceso de un solo uso son válidos durante 30 minutos. Después de 30 minutos, ese código de acceso de un solo uso específico ya no será válido y el usuario deberá solicitar uno nuevo. Las sesiones del usuario expiran después de 24 horas. Después de ese tiempo, el usuario invitado recibe un nuevo código de acceso cuando accede al recurso. La expiración de la sesión proporciona mayor seguridad, en especial cuando un usuario invitado deja su empresa o ya no necesita tener acceso.

## <a name="when-does-a-guest-user-get-a-one-time-passcode"></a>¿Cuándo un usuario invitado obtiene un código de acceso de un solo uso?

Cuando un usuario invitado canjea una invitación o usa un vínculo a un recurso que se ha compartido con él, recibe un código de acceso de un solo uso si:

- No tiene una cuenta de Azure AD
- No tiene una cuenta de Microsoft
- El inquilino que invita no ha configurado la federación de Google para los usuarios @gmail.com y @googlemail.com

En el momento de la invitación, no hay ninguna indicación de que el usuario al que está invitando usará la autenticación de código de acceso de un solo uso. Pero cuando el usuario invitado inicia sesión, la autenticación de código de acceso de un solo uso será el método de reserva si no se puede usar ningún otro método de autenticación.

Para saber si un usuario invitado se autentica mediante códigos de acceso de un solo uso, compruebe la propiedad **Source** en los detalles del usuario. En el Azure Portal, vaya a **Azure Active Directory** > **Usuarios** y, a continuación, seleccione el usuario para abrir la página de detalles.

![Captura de pantalla que muestra un usuario de código de acceso único con el valor origen de OTP.](media/one-time-passcode/guest-user-properties.png)

> [!NOTE]
> Cuando un usuario canjea un código de acceso de un solo uso y más adelante obtiene una MSA, una cuenta de Azure AD u otra cuenta federada, seguirá autenticándose con un código de acceso de un solo uso. Si quiere actualizar el método de autenticación, puede eliminar la cuenta de usuario invitado y volver a invitarlo.

### <a name="example"></a>Ejemplo

Se invita al usuario invitado teri@gmail.com a Fabrikam, que no tiene configurada la federación de Google. Teri no tiene una cuenta de Microsoft. Recibe un código de acceso de un solo uso para la autenticación.

## <a name="disable-email-one-time-passcode"></a>Deshabilitación del código de acceso de un solo uso por correo electrónico

A partir de octubre de 2021, la característica de código de acceso de un solo uso por correo electrónico se activará para todos los inquilinos existentes, y se habilitará de forma predeterminada para los nuevos. En ese momento, Microsoft dejará de admitir el canje de invitaciones mediante la creación de inquilinos y cuentas de Azure AD no administradas ("virales" o "Just-In-Time") para escenarios de colaboración B2B. Vamos a habilitar la característica de código de acceso de un solo uso por correo electrónico, ya que proporciona un método eficaz de autenticación de reserva para usuarios invitados. No obstante, puede deshabilitar esta característica si prefiere no utilizarla.

> [!NOTE]
>
> Si la característica de código de acceso de un solo uso por correo electrónico se ha habilitado en el inquilino y la desactiva, los usuarios invitados que hayan canjeado un código de acceso de un solo uso no podrán iniciar sesión. Puede eliminar los usuarios invitados y volver a invitarlos para que puedan volver a iniciar sesión con otro método de autenticación.

### <a name="to-disable-the-email-one-time-passcode-feature"></a>Para deshabilitar la característica de código de acceso de un solo uso por correo electrónico

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global de Azure AD.

2. En el panel de navegación, seleccione **Azure Active Directory**.

3. Seleccione **External Identities** > **Configuración de colaboración externa**.

4. En **Email one-time passcode for guests** (Código de acceso de un solo uso por correo electrónico para invitados), seleccione **Disable email one-time passcode for guests** (Deshabilitar el código de acceso de un solo uso por correo electrónico para invitados).

   > [!NOTE]
   > Si ve el siguiente botón en lugar de las opciones de código de acceso de un solo uso por correo electrónico, significa que ya se le ha habilitado, deshabilitado o ha participado en la versión preliminar de la característica. Seleccione **No** para deshabilitar esta característica.
   >
   >![Habilitación de la participación en el código de acceso de un solo uso por correo electrónico](media/delegate-invitations/enable-email-otp-opted-in.png)

5. Seleccione **Guardar**.

## <a name="note-for-public-preview-customers"></a>Nota para los clientes de la versión preliminar pública

Si previamente ha participado en la versión preliminar pública del código de acceso de un solo uso por correo electrónico, la fecha de habilitación automática de la características en octubre de 2021 no es aplicable a su caso, por lo que sus procesos empresariales relacionados no se verán afectados. Además, en Azure Portal, en las propiedades de **Email one-time passcode for guests** (Código de acceso de un solo uso por correo electrónico para invitados), no verá la opción **Automatically enable email one-time passcode for guests in October 2021** (Habilitar automáticamente el código de acceso de un solo uso por correo electrónico para invitados a partir de octubre de 2021). En su lugar, verá el botón **Yes**/**No** (Si/No):

![Habilitación de la participación en el código de acceso de un solo uso por correo electrónico](media/delegate-invitations/enable-email-otp-opted-in.png)

Sin embargo, si prefiere no participar en la característica y permitir que se habilite automáticamente en octubre de 2021, puede revertir a la configuración predeterminada mediante el [tipo de recurso de configuración del método de autenticación por correo electrónico](/graph/api/resources/emailauthenticationmethodconfiguration) de Microsoft Graph API. Después de revertir a la configuración predeterminada, las siguientes opciones estarán disponibles en **Email one-time passcode for guests** (Código de acceso de un solo uso por correo electrónico para invitados):

- **Automatically enable email one-time passcode for guests in October 2021** (Habilitar automáticamente el código de acceso de un solo uso por correo electrónico a partir de octubre de 2021). Valor predeterminado. Si la característica de código de acceso de un solo uso por correo electrónico no está habilitada para el inquilino, se activará automáticamente en octubre de 2021. Si desea que la característica se habilite desde ese momento, no tiene que hacer nada más. Si ya ha habilitado o deshabilitado la característica, esta opción no estará disponible.

- **Enable email one-time passcode for guests effective now** (Habilitar el código de acceso de un solo uso por correo electrónico para invitados desde este momento). Activa la característica de código de acceso de un solo uso por correo electrónico para el inquilino.

- **Disable email one-time passcode for guests** (Deshabilitar el código de acceso de un solo uso por correo electrónico para invitados). Desactiva la característica de código de acceso de un solo uso por correo electrónico para el inquilino y evita que la característica se active en octubre de 2021.