---
title: Preguntas frecuentes de la sincronización en la nube de Azure AD Connect
description: En este documento se describen las preguntas frecuentes sobre la sincronización en la nube.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/25/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39d1554fd1b6cac1a90a794cfd93def97e494bfe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98612720"
---
# <a name="azure-active-directory-connect-cloud-sync-faq"></a>Preguntas frecuentes de sincronización en la nube de Azure Active Directory Connect

Lea las preguntas más frecuentes sobre la sincronización en la nube de Azure Active Directory (Azure AD) Connect.

## <a name="general-installation"></a>Instalación general

**P: ¿Con qué frecuencia se ejecuta la sincronización en la nube?**

El aprovisionamiento en la nube está programado para ejecutarse cada 2 minutos. Cada 2 minutos, los cambios de hash de cualquier usuario, grupo y contraseña se aprovisionarán en Azure AD.

**P: Aparecen errores de sincronización del hash de contraseñas en la primera ejecución. ¿Por qué?**

Se espera que esto sea así. Los errores se deben a que el objeto de usuario no está presente en Azure AD. Una vez que el usuario se aprovisiona en Azure AD, los hashes de contraseña deben aprovisionarse en la ejecución posterior. Espere un par de ejecuciones y confirme que la sincronización de los hashes de contraseña no genera los errores.

**P: ¿Qué ocurre si la instancia de Active Directory tiene atributos que no son compatibles con la sincronización en la nube (por ejemplo, las extensiones de directorio)?**

El aprovisionamiento en la nube se ejecutará y aprovisionará los atributos admitidos. Los atributos no admitidos no se aprovisionarán para Azure AD. Revise las extensiones de directorio en Active Directory y asegúrese de que no necesita que esos atributos fluyan a Azure AD. Si se requieren uno o varios atributos, considere la posibilidad de usar la sincronización de Azure AD Connect o mover la información necesaria a uno de los atributos admitidos (por ejemplo, los atributos de extensión 1-15).

**P: ¿En qué se diferencian la sincronización de Azure AD Connect y la sincronización en la nube?**

Con la sincronización de Azure AD Connect, el aprovisionamiento se ejecuta en el servidor de sincronización local. La configuración se almacena en el servidor de sincronización local. Con la sincronización en la nube de Azure AD Connect, la configuración de aprovisionamiento se almacena en la nube y se ejecuta en esta como parte del servicio de aprovisionamiento de Azure AD. 

**P: ¿Puedo usar la sincronización en la nube para sincronizar desde varios bosques de Active Directory?**

Sí. El aprovisionamiento en la nube se puede usar para sincronizar desde varios bosques de Active Directory. En un entorno de varios bosques, todas las referencias (por ejemplo, administrador) deben estar dentro del dominio.  

**P: ¿Cómo se actualiza el agente?**

Microsoft actualiza automáticamente los agentes. Para el equipo de TI, esto reduce la carga de tener que probar y validar las nuevas versiones del agente. 

**P: ¿Puedo deshabilitar la actualización automática?**

No se admite ninguna manera de deshabilitar la actualización automática.

**P: ¿Puedo cambiar el delimitador de origen para la sincronización en la nube?**

De manera predeterminada, la sincronización en la nube usa ms-ds-consistency-GUID con una reserva para ObjectGUID como delimitador de origen. No se admite ninguna manera de cambiar el delimitador de origen.

**P: Veo nuevas entidades de servicio con los nombres de dominio de AD al usar la sincronización en la nube. ¿Es normal?**

Sí, la sincronización en la nube crea una entidad de servicio para la configuración de aprovisionamiento con el nombre de dominio como el nombre de la entidad de servicio. No realice ningún cambio en la configuración de la entidad de servicio.

**P: ¿Qué ocurre cuando se solicita a un usuario sincronizado cambiar la contraseña en el siguiente inicio de sesión?**

Si la sincronización de hash de la contraseña está habilitada en la sincronización en la nube y se requiere que el usuario sincronizado cambie la contraseña en el siguiente inicio de sesión en AD local, la sincronización en la nube no aprovisiona el hash de contraseña que se debe cambiar en Azure AD. Una vez que el usuario cambia la contraseña, el hash de la contraseña del usuario se aprovisiona desde AD a Azure AD.

**P: ¿Admite la sincronización en la nube la escritura diferida de ms-ds-consistencyGUID en cualquier objeto?**

No, la sincronización en la nube no admite la escritura diferida de ms-ds-consistencyGUID en ningún objeto (incluidos los objetos de usuario). 

**P: Estoy aprovisionando usuarios que utilizan la sincronización en la nube. He eliminado la configuración. ¿Por qué sigo viendo los objetos sincronizados antiguos en Azure AD?** 

Al eliminar la configuración, la sincronización en la nube no elimina automáticamente los objetos sincronizados en Azure AD. Para asegurarse de que no tiene los objetos antiguos, cambie el ámbito de la configuración a un grupo o a unidades organizativas vacíos. Una vez que el aprovisionamiento se ejecuta y limpia los objetos, deshabilite la configuración y elimínela. 

**P:  ¿Qué significa que no se admite Exchange híbrido?**

La característica de implementación híbrida de Exchange permite la coexistencia de buzones de Exchange en un entorno local y en Microsoft 365. Azure AD Connect sincroniza un conjunto específico de atributos de Azure AD en su directorio local.  Actualmente, el agente de aprovisionamiento en la nube no vuelve a sincronizar estos atributos en el directorio local y, por tanto, no se admite como reemplazo de Azure AD Connect.

**P:  ¿Puedo instalar el agente de aprovisionamiento en la nube en Windows Server Core?**

No, no se admite la instalación del agente en Server Core.

**P:  ¿Puedo usar un servidor provisional con el agente de aprovisionamiento en la nube?**

No, no se admiten servidores provisionales.

**P:  ¿Puedo sincronizar cuentas de usuarios invitados?**

No, no se admite la sincronización de cuentas de usuarios invitados.

**P:  Si muevo un usuario de una unidad organizativa, cuyo ámbito es la sincronización en la nube, a una unidad organizativa cuyo ámbito es Azure AD Connect, ¿qué ocurre?**

Se eliminará el usuario y se volverá a crear.  Mover un usuario de una unidad organizativa cuyo ámbito sea la sincronización en la nube se verá como una operación de eliminación.  Si el usuario se mueve a una unidad organizativa administrada por Azure AD Connect, se volverá a aprovisionar a Azure AD y se creará un usuario.

**P:  Si cambio el nombre de la unidad organizativa que se encuentra en el ámbito del filtro de sincronización en la nube o la muevo, ¿qué ocurre con el usuario que se creó en Azure AD?**

Nada.  Los usuarios no se eliminarán si se cambia el nombre de la unidad organizativa o se mueve.

**P:  ¿Admite la sincronización en la nube de Azure AD Connect grupos grandes?**

Sí. En la actualidad, se admiten hasta 50 000 miembros del grupo sincronizados con el filtrado del ámbito de unidad organizativa. Al mismo tiempo, cuando se usa el filtrado de ámbito de grupo, se recomienda mantener el tamaño de grupo en menos de 1500 miembros. El motivo es que, aunque puede sincronizar un grupo grande como parte del filtro de ámbito de grupo, al agregar miembros a ese grupo por lotes de más de 1500, se producirá un error en la sincronización diferencial. 

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es la sincronización en la nube de Azure AD Connect?](what-is-cloud-sync.md)
