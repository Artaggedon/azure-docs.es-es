---
title: Cadenas de conexión
description: Cadenas de conexión para el grupo de SQL de Synapse
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: azure-synapse, seo-lt-2019, devx-track-csharp
ms.openlocfilehash: 8130d6672cf6ef8df5fa2e9854cfeba8a37b8bc7
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98677326"
---
# <a name="connection-strings-for-sql-pools-in-azure-synapse"></a>Cadenas de conexión para grupos de SQL en Azure Synapse

Puede conectarse a un grupo de SQL en Azure Synapse con varios protocolos de aplicación diferentes, como [ADO.NET](/dotnet/framework/data/adonet?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json), [ODBC](/sql/connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true), [PHP](/sql/connect/php/overview-of-the-php-sql-driver?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) y [JDBC](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true). A continuación se muestran algunos ejemplos de cadenas de conexión para cada protocolo.  También puede usar el Portal de Azure para crear la cadena de conexión.  

Para generar la cadena de conexión mediante Azure Portal, vaya a la hoja del grupo de SQL en *Aspectos básicos* y haga clic en *Mostrar cadenas de conexión de base de datos*.

## <a name="sample-adonet-connection-string"></a>Cadena de conexión ADO.NET de ejemplo

```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## <a name="sample-odbc-connection-string"></a>Cadena de conexión ODBC de ejemplo

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## <a name="sample-php-connection-string"></a>Cadena de conexión PHP de ejemplo

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## <a name="sample-jdbc-connection-string"></a>Cadena de conexión JDBC de ejemplo

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [!NOTE]
> Considere la posibilidad de establecer el tiempo de espera de conexión en 300 segundos para permitir que la conexión sobreviva durante breves períodos de falta de disponibilidad.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a realizar consultas en el grupo de SQL con Visual Studio y otras aplicaciones, consulte [Consulta con Visual Studio](sql-data-warehouse-query-visual-studio.md).
