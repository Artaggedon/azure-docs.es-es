---
title: 'Azure AD Connect: Solución de problemas del delimitador de origen durante la instalación | Microsoft Docs'
description: En este tema se proporcionan los pasos para solucionar problemas relacionados con el delimitador de origen durante la instalación.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: troubleshooting
ms.date: 04/19/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 4e6460015430850c11fbf70a005d7440ce1b92fe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "91306011"
---
# <a name="troubleshooting-source-anchor-issues-during-installation"></a>Solución de problemas del delimitador de origen durante la instalación
En este artículo se explican los diferentes problemas relacionados con el delimitador de origen que pueden producirse durante la instalación y se ofrecen métodos para resolverlos.

## <a name="invalid-source-anchor-in-azure-active-directory"></a>Delimitador de origen no válido en Azure Active Directory

### <a name="custom-installation"></a>Instalación personalizada

Durante la instalación personalizada, Azure AD Connect lee la directiva del delimitador de origen desde Azure Active Directory. Si la directiva existe en Azure Active Directory, Azure AD Connect aplica la misma directiva, a menos que el cliente la haya invalidado. El asistente indica qué atributo se ha leído. Además, el asistente le advierte si intenta invalidar la directiva del delimitador de origen.

Durante esta operación de lectura, es posible que la directiva del delimitador de origen en Azure Active Directory sea inesperada. En este caso, Azure AD Connect no sabe qué delimitador de origen debe usar y necesita usar la invalidación manual.</br>
![Captura de pantalla que muestra dónde invalidar manualmente el delimitador de origen.](media/tshoot-connect-source-anchor/source1.png)

Para resolver este problema, puede invalidar manualmente el delimitador de origen mediante la selección de un atributo específico. Continúe con esta opción solo si está seguro de qué atributo debe seleccionar. Si no está seguro, póngase en contacto con el [soporte técnico de Microsoft](https://support.microsoft.com/contactus/) para obtener instrucciones. Si cambia la directiva del delimitador de origen, puede interrumpir la asociación entre los usuarios locales y sus recursos de Azure asociados.</br>
![Captura de pantalla que muestra el atributo especificado que invalida el delimitador de origen.](media/tshoot-connect-source-anchor/source2.png)

### <a name="express-installation"></a>Instalación rápida
Durante la instalación rápida, Azure AD Connect lee la directiva del delimitador de origen desde Azure Active Directory. Si la directiva existe en Azure Active Directory, Azure AD Connect aplica la misma directiva. No existe ninguna opción para realizar la invalidación de forma manual.

Durante esta operación de lectura, es posible que la directiva del delimitador de origen en Azure Active Directory sea inesperada. En este caso, Azure AD Connect no sabe qué delimitador de origen debe usar.</br>
![Captura de pantalla que muestra lo que sucede cuando el delimitador de origen en Azure Active Directory es inesperado.](media/tshoot-connect-source-anchor/source3.png)

Para resolver este problema, debe volver a realizar la instalación con el modo personalizado e invalidar manualmente el delimitador de origen mediante la selección de un atributo específico. Continúe con esta opción solo si está seguro de qué atributo debe seleccionar. Si no está seguro, póngase en contacto con el [soporte técnico de Microsoft](https://support.microsoft.com/contactus/) para obtener instrucciones. Si cambia la directiva del delimitador de origen, puede interrumpir la asociación entre los usuarios locales y sus recursos de Azure asociados.

### <a name="invalid-source-anchor-in-sync-engine"></a>Delimitador de origen no válido en el motor de sincronización
Durante la instalación, es posible Azure AD Connect intente configurar el motor de sincronización mediante un delimitador de origen no válido. Esta operación es con toda probabilidad un problema del producto que dará lugar a un error de instalación de Azure AD Connect. Póngase en contacto con el [soporte técnico de Microsoft](https://support.microsoft.com/contactus/) si experimenta este problema.</br>
![Inesperado](media/tshoot-connect-source-anchor/source4.png)


## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
