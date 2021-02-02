---
title: Configuración de Identity Protection y el acceso condicional en Azure AD B2C
description: Obtenga información acerca de cómo configurar Identity Protection y el acceso condicional para el inquilino de Azure AD B2C para ver los inicios de sesión de riesgo y otros eventos de riesgo y crear directivas basadas en las detecciones de riesgo.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 09/01/2020
ms.author: mimart
author: msmimart
manager: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 654206bccd25bf09fcdc5c3e7ee72ba97c75af2a
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98785488"
---
# <a name="set-up-identity-protection-and-conditional-access-in-azure-ad-b2c"></a>Configuración de Identity Protection y el acceso condicional en Azure AD B2C

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

Identity Protection proporciona detección continua de riesgos para el inquilino de Azure AD B2C. Si el plan de tarifa del inquilino de Azure AD B2C es Premium P2, puede ver los eventos de riesgo de Identity Protection detallados en Azure Portal. También puede usar directivas de [acceso condicional](../active-directory/conditional-access/overview.md) basadas en estas detecciones de riesgo para determinar las acciones y aplicar las directivas de la organización.

## <a name="prerequisites"></a>Requisitos previos

- El inquilino de Azure AD B2C debe estar [vinculado a una suscripción de Azure AD](billing.md#link-an-azure-ad-b2c-tenant-to-a-subscription).
- Se requiere Azure AD B2C Premium P2 para usar el acceso condicional basado en el riesgo de inicio de sesión y el usuario. Si es necesario, [cambie el plan de tarifa de Azure AD B2C a Premium P2](./billing.md). 
- Para administrar Identity Protection y el acceso condicional en el inquilino de B2C, necesitará una cuenta que tenga asignado el rol Administrador global o el rol Administrador de seguridad.
- Para usar estas características en el inquilino, primero debe cambiar el plan de tarifa de Azure AD B2C a Premium P2.

## <a name="set-up-identity-protection"></a>Configuración de Identity Protection

Identity Protection está activado de forma predeterminada. Para poder ver los eventos de riesgo de Identity Protection en el inquilino de Azure AD B2C, solo tiene que vincular el inquilino de Azure AD B2C a una suscripción de Azure AD y seleccionar el plan de tarifa Premium P2 de Azure AD B2C. Puede ver informes detallados de los eventos de riesgo en Azure Portal.

### <a name="supported-identity-protection-risk-detections"></a>Detecciones de riesgo de Identity Protection admitidas

Actualmente se admiten las siguientes detecciones de riesgo para Azure AD B2C:  

|Tipo de detección de riesgo  |Descripción  |
|---------|---------|
| Viaje atípico     | Iniciar sesión desde una ubicación atípica, en función de los inicios de sesión del usuario recientes.        |
|Dirección IP anónima     | Iniciar sesión desde una dirección IP anónima (por ejemplo: el explorador Tor o redes VPN anonimizadoras).        |
|Dirección IP vinculada al malware     | Se inicia sesión desde una dirección IP vinculada al malware.         |
|Propiedades de inicio de sesión desconocidas     | Iniciar sesión con propiedades que no hemos observado recientemente en el usuario en cuestión.        |
|Vulneración de identidad de usuario confirmada por el administrador    | Un administrador ha indicado que se ha vulnerado la identidad de un usuario.             |
|Difusión de contraseña     | Se inicia sesión mediante un ataque de difusión de contraseña.      |
|Inteligencia de Azure AD sobre amenazas     | Los orígenes de inteligencia sobre amenazas internas y externas de Microsoft han identificado un patrón de ataque conocido.        |

## <a name="view-risk-events-for-your-azure-ad-b2c-tenant"></a>Visualización de los eventos de riesgo del inquilino de Azure AD B2C

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.

1. En Azure Portal, busque y seleccione **Azure AD B2C**.

1. En **Seguridad**, seleccione **Usuarios de riesgo (versión preliminar)** .

   ![Usuarios de riesgo](media/conditional-access-identity-protection-setup/risky-users.png)

1. En **Seguridad**, seleccione **Detecciones de riesgos (versión preliminar)** .

   ![Detecciones de riesgo](media/conditional-access-identity-protection-setup/risk-detections.png)

## <a name="add-a-conditional-access-policy"></a>Adición de una directiva de acceso condicional 

Para agregar una directiva de acceso condicional basada en las detecciones de riesgos de Identity Protection, asegúrese de que los valores predeterminados de seguridad están deshabilitados para el inquilino de Azure AD B2C y, a continuación, cree directivas de acceso condicional.

### <a name="to-disable-security-defaults"></a>Para deshabilitar los valores predeterminados de seguridad

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.

3. En Azure Portal, busque y seleccione **Azure Active Directory**.

4. Seleccione **Propiedades** y, a continuación, seleccione **Administrar valores predeterminados de seguridad**.

   ![Deshabilitar los valores predeterminados de seguridad](media/conditional-access-identity-protection-setup/disable-security-defaults.png)

5. En Activación de los valores predeterminados de seguridad, seleccione No. 

   ![Establecer el conmutador Activación de los valores predeterminados de seguridad en No.](media/conditional-access-identity-protection-setup/enable-security-defaults-toggle.png)

### <a name="to-create-a-conditional-access-policy"></a>Para crear una directiva de acceso condicional

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.

1. En Azure Portal, busque y seleccione **Azure AD B2C**.

1. En **Seguridad**, seleccione **Acceso condicional (versión preliminar)** . Se abre la página **Directivas de acceso condicional**. 

1. Seleccione **Nueva directiva** y siga la documentación del acceso condicional de Azure AD para crear una nueva directiva. En el caso de las directivas basadas en riesgos, tendrá que configurar directivas independientes basadas en el [riesgo de usuario](../active-directory/conditional-access/howto-conditional-access-policy-risk-user.md#enable-with-conditional-access-policy) o [riesgo de inicio de sesión](../active-directory/conditional-access/howto-conditional-access-policy-risk.md#enable-with-conditional-access-policy), en función del tipo de riesgo que desee utilizar como condición. No se recomienda usar ambos tipos de riesgo en una única directiva.

   > [!IMPORTANT]
   > Al seleccionar los usuarios a los que desea aplicar la directiva, no seleccione **Todos los usuarios**, ya que podría bloquear su propio inicio de sesión.

## <a name="test-the-conditional-access-policy"></a>Prueba de la directiva de acceso condicional

1. Cree una directiva de acceso condicional tal y como se indicó anteriormente, con la siguiente configuración:
   
   - En **Usuarios y grupos**, seleccione el usuario de prueba. No seleccione **Todos los usuarios** o bloqueará su propio inicio de sesión.
   - Para **Aplicaciones en la nube o acciones**, elija **Seleccionar aplicaciones** y, después, elija la aplicación de usuario de confianza.
   - En Condiciones, seleccione **Riesgo de inicio de sesión** y los niveles de riesgo **Alto**, **Medio** y **Bajo**.
   - En **Conceder**, elija **Bloquear acceso**.

      ![Elegir Bloquear acceso](media/conditional-access-identity-protection-setup/test-conditional-access-policy.png)

1. Habilite la directiva de acceso condicional de prueba; para ello, seleccione **Crear**.

1. Simule un inicio de sesión de riesgo con el [explorador Tor](https://www.torproject.org/download/). 

1. En el token descodificado de jwt.ms del intento de inicio de sesión, debería ver que el inicio de sesión se ha bloqueado:

   ![Prueba de un inicio de sesión bloqueado](media/conditional-access-identity-protection-setup/test-blocked-sign-in.png)

## <a name="review-conditional-access-outcomes-in-the-audit-report"></a>Revisión de los resultados del acceso condicional en el informe de auditoría

Para revisar el resultado de un evento de acceso condicional:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.

3. En Azure Portal, busque y seleccione **Azure AD B2C**.

4. En **Actividades**, seleccione **Registros de auditoría**.

5. Filtre el registro de auditoría; para ello, establezca **Categoría** en **B2C** y establezca **Tipo de recurso de actividad** en **IdentityProtection**. Luego, seleccione **Aplicar**.

6. Revise la actividad de auditoría de los últimos 7 días. Se incluyen los siguientes tipos de actividad:

   - **Evaluar directivas de acceso condicional**: esta entrada del registro de auditoría indica que se ha realizado una evaluación de acceso condicional durante una autenticación.
   - **Corregir usuario**: esta entrada indica que el usuario final cumplió la concesión o los requisitos de una directiva de acceso condicional y que esta actividad se comunicó al motor de riesgo para reducir el riesgo (mitigar) del usuario.

7. Seleccione una entrada de registro de tipo **Evaluar directivas de acceso condicional** en la lista para abrir la página **Detalles de la actividad: registro de auditoría**, que muestra los identificadores del registro de auditoría, junto con esta información, en la sección **Detalles adicionales**:

   - ConditionalAccessResult: concesión requerida por la evaluación de la directiva condicional.
   - AppliedPolicies: lista de todas las directivas de acceso condicional en las que se cumplieron las condiciones y las directivas están activadas.
   - ReportingPolicies: lista de las directivas de acceso condicional que se establecieron en modo de solo informe y en las que se cumplieron las condiciones.

## <a name="next-steps"></a>Pasos siguientes

[Adición del acceso condicional a un flujo de usuario](conditional-access-user-flow.md).
