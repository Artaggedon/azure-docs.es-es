---
title: archivo de inclusión
description: archivo de inclusión
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/12/2018
ms.author: raynew
ms.openlocfilehash: f4cd9cad3813378fcdc3f06e8ab1c28eced4f93c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "95999199"
---
Nombre | Dirección URL comercial | Dirección URL gubernamental | Descripción
--- | --- | --- | ---
Azure Active Directory | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Se usa para el control de acceso y la administración de identidades mediante Azure Active Directory. 
Copia de seguridad | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Se usa para la transferencia de datos de replicación y coordinación.
Replicación | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Se usa para las operaciones de administración de replicación y coordinación.
Storage | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Se usa para tener acceso a la cuenta de almacenamiento que almacena los datos replicados.
Datos de telemetría (opcional) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Se usa para la telemetría.
Sincronización de hora | ``time.windows.com`` | ``time.nist.gov`` | Se usa para comprobar la sincronización de la hora entre el sistema y la hora global en todas las implementaciones.


