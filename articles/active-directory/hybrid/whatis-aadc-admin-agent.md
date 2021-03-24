---
title: 'Qué es el agente de administración de Azure AD Connect: Azure AD Connect | Microsoft Docs'
description: Describe las herramientas utilizadas para sincronizar y supervisar el entorno local con Azure AD.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 09/04/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: da4e1cfc68dff4ad2bc0552c6d35fe1230779306
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "91312965"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>¿Qué es el agente administrador de Azure AD Connect? 
El agente de administración de Azure AD Connect es un nuevo componente de Azure Active Directory Connect que se puede instalar en un servidor de Azure Active Directory Connect. Se utiliza para recopilar datos específicos de su entorno de Active Directory que ayudan al ingeniero de soporte técnico de Microsoft a solucionar problemas cuando el usuario abre una incidencia de soporte técnico. 

>[!NOTE]
>El agente de administración no está instalado y habilitado de forma predeterminada.  Debe instalarlo para poder recopilar datos y ayudar en casos de soporte técnico.

Una vez instalado, el agente de administración de Azure AD Connect espera solicitudes de datos específicas de Azure Active Directory, obtiene los datos solicitados del entorno de sincronización y los envía a Azure Active Directory, donde se presentarán al ingeniero de soporte técnico de Microsoft. 

La información que recupera el agente de administración Azure AD Connect del entorno no se almacena de ninguna manera; solo se muestra al ingeniero de soporte técnico de Microsoft para ayudarle a investigar y solucionar problemas del caso de soporte técnico relacionado con Azure Active Directory Connect que ha abierto. El agente de administración de Azure AD Connect no está instalado en el servidor de Azure AD Connect de forma predeterminada. 

## <a name="install-the-azure-ad-connect-administration-agent-on-the-azure-ad-connect-server"></a>Instale al agente de administración de Azure AD Connect en el servidor de Azure AD Connect 

Requisitos previos:
1.    Azure AD Connect está instalado en el servidor
2.    Azure AD Connect Health está instalado en el servidor

![agente de administración](media/whatis-aadc-admin-agent/adminagent0.png)

Los archivos binarios del agente de administración de Azure AD Connect se colocan en el servidor de AAD Connect. Para instalar al agente, realice las siguientes operaciones:

1.    Abra PowerShell en modo de administrador.
2.    Vaya al directorio donde está ubicada la aplicación, con "C:\Program Files\Microsoft Azure Active Directory Connect\Tools".
3.    Ejecute ConfigureAdminAgent.ps1.

Cuando se le solicite, escriba las credenciales de administrador global de Azure AD. Deben ser las mismas credenciales que se especificaron durante la instalación de Azure AD Connect.

Después de instalar el agente, verá los siguientes dos programas nuevos en la lista "Agregar o quitar programas" en el Panel de control del servidor: 

![Captura de pantalla que muestra la lista Agregar o quitar programas que incluye los nuevos programas que ha agregado.](media/whatis-aadc-admin-agent/adminagent1.png)

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>¿Qué datos de mi servicio de sincronización se muestran al ingeniero de servicios de Microsoft? 
Al abrir un caso de soporte técnico, el ingeniero de soporte técnico de Microsoft puede ver, de un usuario determinado, los datos pertinentes de Active Directory, el espacio del conector de Active Directory en el servidor de Azure Active Directory Connect, el espacio del conector de Azure Active Directory en el servidor de Azure Active Directory Connect y el metaverso en el servidor de Azure Active Directory Connect. 

El ingeniero de soporte técnico de Microsoft no puede cambiar ningún dato del sistema ni ver las contraseñas. 

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>¿Qué debo hacer si no quiero que el ingeniero de soporte técnico de Microsoft acceda a mis datos? 
Una vez que el agente está instalado, si no quiere que el ingeniero de servicios de Microsoft acceda a sus datos en una llamada de soporte técnico, puede modificar el archivo de configuración del servicio para deshabilitar la funcionalidad, como se describe a continuación: 

1. Abra **C:\Program Files\Microsoft Azure AD Connect Administration Agent\AzureADConnectAdministrationAgentService.exe.config** en el Bloc de notas.
2. Deshabilite la opción **UserDataEnabled** como se muestra a continuación. Si la opción **UserDataEnabled** ya existe y está establecida en true, establézcala en false. Si la opción no existe, agréguela como se muestra a continuación.    

    ```xml
    <appSettings>
      <add key="TraceFilename" value="ADAdministrationAgent.log" />
      <add key="UserDataEnabled" value="false" />
    </appSettings>
    ```

3. Guarde el archivo de configuración.
4. Reinicie el servicio del agente de administración de Azure AD Connect como se muestra a continuación.

![Captura de pantalla que muestra dónde reiniciar el servicio del agente administrador de Azure AD.](media/whatis-aadc-admin-agent/adminagent2.png)

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
