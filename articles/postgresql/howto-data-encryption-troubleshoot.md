---
title: 'Solución de problemas del cifrado de datos: Azure Database for PostgreSQL: servidor único'
description: Aprenda a solucionar problemas del cifrado de datos en el servidor único de Azure Database for PostgreSQL.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 02/13/2020
ms.openlocfilehash: c315e1df473f3d23bab7e2a78ce166f22272ee70
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "93242252"
---
# <a name="troubleshoot-data-encryption-in-azure-database-for-postgresql---single-server"></a>Solución de problemas del cifrado de datos en un servidor único de Azure Database for PostgreSQL

Este artículo le ayuda a identificar y resolver problemas habituales que se pueden producir en la implementación de un servidor único de Azure Database for PostgreSQL cuando se configura con cifrado de datos mediante una clave administrada por el cliente.

## <a name="introduction"></a>Introducción

Cuando configura el cifrado de datos para que use una clave administrada por el cliente en Azure Key Vault, el servidor requiere un acceso continuo a la clave. Si el servidor pierde el acceso a la clave administrada por el cliente en Azure Key Vault, denegará todas las conexiones con su correspondiente mensaje de error y cambiará su estado a ***Inaccesible*** en Azure Portal.

Si ya no se necesita un servidor de Azure Database for PostgreSQL inaccesible, puede eliminarlo para dejar de incurrir en gastos. No se permite ninguna otra acción en el servidor hasta que se haya restaurado el acceso al almacén de claves y el servidor vuelva a estar disponible. Tampoco es posible cambiar la opción de cifrado de datos de `Yes`(administrada por el cliente) a `No` (administrada por el servicio) en un servidor inaccesible mientras este está cifrado con una clave administrada por el cliente. Tendrá que volver a validar la clave manualmente antes de que se pueda acceder al servidor de nuevo. Esta acción es necesaria para proteger los datos contra el acceso no autorizado mientras se revocan los permisos para la clave administrada por el cliente.

## <a name="common-errors-causing-server-to-become-inaccessible"></a>Errores comunes que hacen que el servidor sea inaccesible

Las siguientes configuraciones incorrectas producen la mayoría de los problemas del cifrado de datos que usa claves de Azure Key Vault:

- El almacén de claves no está disponible o no existe:
  - El almacén se claves se eliminó por error.
  - Un error de red intermitente hace que el almacén de claves no esté disponible.

- No tiene permisos para acceder al almacén de claves o la clave no existe:
  - La clave se ha eliminado accidentalmente, se ha deshabilitado o ha expirado.
- La identidad administrada de la instancia de Azure Database for PostgreSQL se eliminó accidentalmente.
  - La identidad administrada de la instancia de Azure Database for PostgreSQL tiene permisos de claves insuficientes. Por ejemplo, los permisos no incluyen las operaciones Get, Wrap y Unwrap.
  - Los permisos de la identidad administrada en la instancia de Azure Database for PostgreSQL se han revocado o eliminado.

## <a name="identify-and-resolve-common-errors"></a>Identificación y resolución de los errores comunes

### <a name="errors-on-the-key-vault"></a>Errores en el almacén de claves

#### <a name="disabled-key-vault"></a>Almacén de claves deshabilitado

- `AzureKeyVaultKeyDisabledMessage`
- **Explicación**: La operación no pudo completarse en el servidor porque la clave de Azure Key Vault está deshabilitada.

#### <a name="missing-key-vault-permissions"></a>Permisos de Key Vault que faltan

- `AzureKeyVaultMissingPermissionsMessage`
- **Explicación**: El servidor no tiene los permisos Get, Wrap y Unwrap necesarios para Azure Key Vault. Conceda los permisos que faltan a la entidad de servicio con identificador.

### <a name="mitigation"></a>Mitigación

- Confirme que la clave administrada por el cliente está presente en el almacén de claves.
- Identifique el almacén de claves y vaya a él en Azure Portal.
- Asegúrese de que el URI de clave identifica una clave que está presente.

## <a name="next-steps"></a>Pasos siguientes

[Uso de Azure Portal para configurar el cifrado de datos con una clave administrada por el cliente en Azure Database for PostgreSQL](howto-data-encryption-portal.md)
