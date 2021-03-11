---
title: Instalación e implementación del agente basado en C para Linux
description: Aprenda a instalar e implementar el agente de seguridad basado en C de Defender for IoT en Linux.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 6d3f96ed60ca784402b6d24eea7234f37c4fb959
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102449788"
---
# <a name="deploy-defender-for-iot-c-based-security-agent-for-linux"></a>Implementación del agente de seguridad basado en C de Defender for IoT en Linux

En esta guía se explica cómo instalar e implementar el agente de seguridad basado en C de Defender for IoT en Linux.

- Instalar
- Comprobación de la implementación
- Desinstalación del agente
- Solución de problemas

## <a name="prerequisites"></a>Prerrequisitos

Para obtener información sobre otras plataformas y tipos de agente, vea [Elegir el agente de seguridad correcto](how-to-deploy-agent.md).

1. Para implementar el agente de seguridad, se necesitan derechos de administrador local en el equipo de instalación (sudo).

1. [Crear un módulo de seguridad](quickstart-create-security-twin.md) para el dispositivo.

## <a name="installation"></a>Instalación

Para instalar e implementar el agente de seguridad, use el siguiente flujo de trabajo:

1. Descargue en la máquina la última versión desde [GitHub](https://aka.ms/iot-security-github-c).

1. Extraiga el contenido del paquete y vaya a la carpeta _/src/installation_.

1. Agregue permisos de ejecución al **script de InstallSecurityAgent**, para lo que debe ejecutar el siguiente comando:

   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. A continuación, ejecute:

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```

   Para más información acerca de los parámetros de autenticación, consulte [Procedimiento para configurar la autenticación](concept-security-agent-authentication-methods.md).

Este script realiza la siguiente función:

1. Instala los requisitos previos.

1. Agrega un usuario de servicio (con el inicio de sesión interactivo deshabilitado).

1. Instala el agente como **daemon** (se da por hecho que el dispositivo usa **systemd** para la administración del servicio).

1. Configura al agente con los parámetros de autenticación proporcionados.

Para obtener ayuda adicional, ejecute el script con el parámetro – help:

```./InstallSecurityAgent.sh --help```

### <a name="uninstall-the-agent"></a>Desinstalación del agente

Para desinstalar al agente, ejecute el script con el parámetro –-uninstall:

```./InstallSecurityAgent.sh -–uninstall```

## <a name="troubleshooting"></a>Solución de problemas

Compruebe el estado de implementación, para lo que debe ejecutar:

```systemctl status ASCIoTAgent.service```

## <a name="next-steps"></a>Pasos siguientes

- Lea la [información general](overview.md) del servicio Defender for IoT.
- Más información sobre la [arquitectura](architecture.md) de Defender for IoT.
- Habilite el [servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Obtenga información acerca de las [alertas de seguridad](concept-security-alerts.md)
