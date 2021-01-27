---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/06/2019
ms.author: alkohli
ms.openlocfilehash: 10c1680afd6cd6673efe3fe9d2397a0dc6a04859
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98738969"
---
Dependiendo del sistema operativo del cliente, los procedimientos para conectarse de forma remota al dispositivo son diferentes.

### <a name="remotely-connect-from-a-windows-client"></a>Conexión de forma remota desde un cliente de Windows

Antes de comenzar, asegúrese de que el cliente de Windows ejecuta Windows PowerShell 5.0 o posterior.

Siga estos pasos para conectarse de forma remota desde un cliente de Windows.

1. Ejecute una sesión de Windows PowerShell como administrador.
2. Asegúrese de que el servicio Administración remota de Windows se ejecuta en el cliente. En el símbolo del sistema, escriba:

    `winrm quickconfig`

    Para más información, vea [Instalación y configuración de Administración remota de Windows](/windows/win32/winrm/installation-and-configuration-for-windows-remote-management#quick-default-configuration).

3. Asigne una variable a la dirección IP del dispositivo.

    $ip = "<device_ip>"

    Reemplace `<device_ip>` por la dirección IP de su dispositivo.

4. Para agregar la dirección IP del dispositivo a la lista de hosts de confianza del cliente, escriba el siguiente comando:

    `Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force`

5. Inicie una sesión de Windows PowerShell en el dispositivo:

    `Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell`

6. Proporcione la contraseña cuando se le solicite. Use la misma contraseña que se emplea para iniciar sesión en la interfaz de usuario web local. La contraseña de la interfaz de usuario web local predeterminada es *Password1*. Cuando se conecte correctamente al dispositivo mediante PowerShell remoto, verá la siguiente salida de ejemplo:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $ip = "10.100.10.10"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [10.100.10.10]: PS>
    ```

### <a name="remotely-connect-from-a-linux-client"></a>Conexión de forma remota desde un cliente de Linux

En el cliente de Linux que usará para conectarse:

- [Instale la versión más reciente de PowerShell Core para Linux](/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) desde GitHub para obtener la característica de comunicación remota SSH. 
- [Instale solo el paquete `gss-ntlmssp` desde el módulo NTLM](https://github.com/Microsoft/omi/blob/master/Unix/doc/setup-ntlm-omi.md). Para los clientes de Ubuntu, use el siguiente comando:
    - `sudo apt-get install gss-ntlmssp`

Para más información, vaya a [Comunicación remota de PowerShell a través de SSH](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core?view=powershell-6).

Siga estos pasos para conectarse de forma remota desde un cliente NFS.

1. Para abrir la sesión de PowerShell, escriba:

    `sudo pwsh`
 
2. Para conectarse mediante el cliente remoto, escriba:

    `Enter-PSSession -ComputerName $ip -Authentication Negotiate -ConfigurationName Minishell -Credential ~\EdgeUser`

    Cuando se le solicite, proporcione la contraseña usada para iniciar sesión en el dispositivo.
 
> [!NOTE]
> Este procedimiento no funciona en Mac OS.