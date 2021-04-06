---
title: 'Agente de aprovisionamiento en la nube de Azure AD Connect: Administrar opciones de registro | Microsoft Docs'
description: En este artículo se describe cómo administrar las opciones de registro del agente de aprovisionamiento en la nube de Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2020
ms.subservice: hybrid
ms.reviewer: chmutali
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f39ef611e2ea15ef3bc3dbfcf09e9624cbcf8b3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98678228"
---
# <a name="manage-agent-registry-options"></a>Administrar las opciones de registro del agente

En esta sección se describen las opciones de registro que se pueden establecer para controlar el comportamiento de procesamiento en tiempo de ejecución del agente de aprovisionamiento de Azure AD Connect. 

## <a name="configure-ldap-connection-timeout"></a>Configuración del tiempo de espera de conexión LDAP
Al realizar operaciones LDAP en los controladores de dominio de Active Directory configurados, de forma predeterminada, el agente de aprovisionamiento usa el valor predeterminado de tiempo de espera de conexión de 30 segundos. Si el controlador de dominio tarda más tiempo en responder, es posible que vea el siguiente mensaje de error en el archivo de registro del agente: 

`
System.DirectoryServices.Protocols.LdapException: The operation was aborted because the client side timeout limit was exceeded.
`

Las operaciones de búsqueda LDAP pueden tardar más tiempo si el atributo de búsqueda no está indizado. Como primer paso, si obtiene el error anterior, compruebe si el atributo search/lookup está [indexado](/windows/win32/ad/indexed-attributes). Si los atributos de búsqueda están indexados y el error persiste, puede aumentar el tiempo de espera de conexión LDAP mediante los pasos siguientes: 

1. Inicie sesión como Administrador en el servidor de Windows que ejecuta el agente de aprovisionamiento de Azure AD Connect.
1. Use el elemento de menú *Ejecutar* para abrir el editor del registro (regedit.exe). 
1. Busque la carpeta de claves **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure AD Connect Agents\Azure AD Connect Provisioning Agent**.
1. Haga clic con el botón secundario y seleccione "Nuevo - > Valor de cadena".
1. Proporcione el nombre: `LdapConnectionTimeoutInMilliseconds`
1. Haga doble clic en **Nombre de valor** y escriba los datos de valor como `60000` milisegundos.
    > [!div class="mx-imgBorder"]
    > ![Tiempo de espera de conexión LDAP](media/how-to-manage-registry-options/ldap-connection-timeout.png)
1. Reinicie el servicio de aprovisionamiento de Azure AD Connect desde la consola *Servicios*.
1. Si han implementado varios agentes de aprovisionamiento, aplique este cambio de registro a todos los agentes para que sea coherentes. 

## <a name="configure-referral-chasing"></a>Configuración del seguimiento de referencia
De forma predeterminada, el agente de aprovisionamiento de Azure AD Connect no realiza un seguimiento de las [referencias](/windows/win32/ad/referrals). Es posible que quiera habilitar el seguimiento de referencias, para admitir ciertos escenarios de aprovisionamiento de entrada de recursos humanos, como: 
* La comprobación de la unicidad de UPN entre varios dominios
* La resolución de referencias del administrador entre dominios

Siga estos pasos para activar el seguimiento de referencias:

1. Inicie sesión como Administrador en el servidor de Windows que ejecuta el agente de aprovisionamiento de Azure AD Connect.
1. Use el elemento de menú *Ejecutar* para abrir el editor del registro (regedit.exe). 
1. Busque la carpeta de claves **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure AD Connect Agents\Azure AD Connect Provisioning Agent**.
1. Haga clic con el botón secundario y seleccione "Nuevo - > Valor de cadena".
1. Proporcione el nombre: `ReferralChasingOptions`
1. Haga doble clic en **Nombre de valor** y escriba los datos de valor como `96`. Este valor corresponde al valor constante de `ReferralChasingOptions.All` y especifica que el agente realizará un seguimiento de las referencias de subárbol y de nivel base. 
    > [!div class="mx-imgBorder"]
    > ![Seguimiento de referencias](media/how-to-manage-registry-options/referral-chasing.png)
1. Reinicie el servicio de aprovisionamiento de Azure AD Connect desde la consola *Servicios*.
1. Si han implementado varios agentes de aprovisionamiento, aplique este cambio de registro a todos los agentes para que sea coherentes.

## <a name="skip-gmsa-configuration"></a>Omisión de la configuración de GMSA
Con la versión del agente 1.1.281.0 +, de forma predeterminada, al ejecutar el Asistente para la configuración de agentes, se le pedirá que configure la [cuenta de servicio administrada de grupo (GMSA)](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). La instalación de GMSA por parte del asistente se usa en el tiempo de ejecución para todas las operaciones de sincronización y aprovisionamiento. 

Si está actualizando desde una versión anterior del agente y ha configurado una cuenta de servicio personalizada con permisos de nivel de unidad organizativa delegados específicos para la topología de Active Directory, puede que desee omitir o posponer la configuración de GMSA y planear este cambio. 

> [!NOTE]
> Esta guía se aplica específicamente a los clientes que han configurado el aprovisionamiento de entrada de recursos humanos (WorkDay/SuccessFactors) con versiones de agente anteriores a 1.1.281.0 y que configuran una cuenta de servicio personalizada para las operaciones del agente. A largo plazo, se recomienda cambiar a GMSA como procedimiento recomendado.  

En este escenario, todavía puede actualizar los archivos binarios del agente y omitir la configuración de GMSA siguiendo estos pasos: 

1. Inicie sesión como Administrador en el servidor de Windows que ejecuta el agente de aprovisionamiento de Azure AD Connect.
1. Ejecute el instalador del agente para instalar los nuevos archivos binarios del agente. Cierre el Asistente para la configuración de agentes que se abre automáticamente después de que la instalación se haya realizado correctamente. 
1. Use el elemento de menú *Ejecutar* para abrir el editor del registro (regedit.exe). 
1. Busque la carpeta de claves **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure AD Connect Agents\Azure AD Connect Provisioning Agent**.
1. Haga clic con el botón derecho y seleccione "Nuevo-> valor DWORD"
1. Proporcione el nombre: `UseCredentials`
1. Haga doble clic en **Nombre de valor** y escriba los datos de valor como `1`.  
    > [!div class="mx-imgBorder"]
    > ![Uso de credenciales](media/how-to-manage-registry-options/use-credentials.png)
1. Reinicie el servicio de aprovisionamiento de Azure AD Connect desde la consola *Servicios*.
1. Si han implementado varios agentes de aprovisionamiento, aplique este cambio de registro a todos los agentes para que sea coherentes.
1. Desde el acceso directo del escritorio, ejecute el Asistente para la configuración del agente. El asistente omitirá la configuración de GMSA. 


> [!NOTE]
> Puede confirmar que las opciones del registro se han establecido habilitando el [registro detallado](how-to-troubleshoot.md#log-files). Los registros emitidos durante el inicio del agente mostrarán los valores de configuración seleccionados del registro. 

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es la sincronización en la nube de Azure AD Connect?](what-is-cloud-sync.md)

