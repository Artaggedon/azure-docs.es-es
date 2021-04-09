---
title: 'Generación de una cadena de conexión con PowerShell: Azure Database for MySQL'
description: Este artículo proporciona un ejemplo de Azure PowerShell en el que se genera una cadena de conexión para conectarse a Azure Database for MySQL.
author: savjani
ms.author: pariks
ms.service: mysql
ms.custom: mvc, devx-track-azurepowershell
ms.topic: how-to
ms.date: 8/5/2020
ms.openlocfilehash: 9f960d32dcf1f359327dccc01eeb06825cc3a062
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "94541477"
---
# <a name="how-to-generate-an-azure-database-for-mysql-connection-string-with-powershell"></a>Procedimientos para generar una cadena de conexión de Azure Database for MySQL con PowerShell

En este artículo se muestra cómo generar una cadena de conexión para un servidor de Azure Database for MySQL. Puede usar una cadena de conexión para conectarse a Azure Database for MySQL desde muchas aplicaciones diferentes.

## <a name="requirements"></a>Requisitos

En este artículo se usan los recursos creados en la siguiente guía como punto de partida:

* [Inicio rápido: Creación de un servidor de Azure Database for MySQL mediante PowerShell](quickstart-create-mysql-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Obtención de la cadena de conexión

El cmdlet `Get-AzMySqlConnectionString` se usa para generar una cadena de conexión para conectar aplicaciones a Azure Database for MySQL. En el ejemplo siguiente se devuelve la cadena de conexión de un cliente PHP desde **mydemoserver**.

```azurepowershell-interactive
Get-AzMySqlConnectionString -Client PHP -Name mydemoserver -ResourceGroupName myresourcegroup
```

```Output
$con=mysqli_init();mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "mydemoserver.mysql.database.azure.com", "myadmin@mydemoserver", {your_password}, {your_database}, 3306);
```

Los valores válidos del parámetro `Client` incluyen:

* ADO&#46;NET
* JDBC
* Node.js
* PHP
* Python
* Ruby
* WebApp

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Personalización de parámetros de servidor de Azure Database for MySQL mediante PowerShell](howto-configure-server-parameters-using-powershell.md)
