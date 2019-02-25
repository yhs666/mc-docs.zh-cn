---
title: 使用 PHP 查询 Azure SQL 数据库 | Microsoft Docs
description: 如何使用 PHP 创建连接到 Azure SQL 数据库的程序并使用 T-SQL 语句对其进行查询。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: php
ms.topic: quickstart
author: WenJason
ms.author: v-jay
ms.reviewer: v-masebo
manager: digimobile
origin.date: 11/28/2018
ms.date: 02/25/2019
ms.openlocfilehash: 150cc435058e0999804fc383dd71a1c03394fb9e
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663643"
---
# <a name="quickstart-use-php-to-query-an-azure-sql-database"></a>快速入门：使用 PHP 查询 Azure SQL 数据库

本文演示了如何使用 [PHP](http://php.net/manual/en/intro-whatis.php) 连接到 Azure SQL 数据库。 然后即可使用 T-SQL 语句来查询数据。

## <a name="prerequisites"></a>先决条件

若要完成此示例，请确保具备以下先决条件：

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- 已为操作系统安装与 PHP 相关的软件：

    - **MacOS**，安装 PHP、ODBC 驱动程序，然后安装 PHP Driver for SQL Server。 请参阅[步骤 1、2 和 3](https://docs.microsoft.com/sql/connect/php/installation-tutorial-linux-mac)。

    - **Linux**，安装 PHP、ODBC 驱动程序，然后安装 PHP Driver for SQL Server。 请参阅[步骤 1、2 和 3](https://docs.microsoft.com/sql/connect/php/installation-tutorial-linux-mac)。

    - **Windows**：安装用于 IIS Express 的 PHP 和 Chocolatey，然后安装 ODBC 驱动程序和 SQLCMD。 请参阅[步骤 1.2 和 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/)。

## <a name="get-database-connection"></a>获取数据库连接

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="add-code-to-query-database"></a>添加用于查询数据库的代码

1. 在喜欢的文本编辑器中，创建新文件 sqltest.php。  

1. 将其内容替换为以下代码。 然后，为服务器、数据库、用户和密码添加相应的值。

   ```PHP
   <?php
       $serverName = "your_server.database.chinacloudapi.cn"; // update me
       $connectionOptions = array(
           "Database" => "your_database", // update me
           "Uid" => "your_username", // update me
           "PWD" => "your_password" // update me
       );
       //Establishes the connection
       $conn = sqlsrv_connect($serverName, $connectionOptions);
       $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
            FROM [SalesLT].[ProductCategory] pc
            JOIN [SalesLT].[Product] p
            ON pc.productcategoryid = p.productcategoryid";
       $getResults= sqlsrv_query($conn, $tsql);
       echo ("Reading data from table" . PHP_EOL);
       if ($getResults == FALSE)
           echo (sqlsrv_errors());
       while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
        echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
       }
       sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>运行代码

1. 在命令提示符下运行此应用。

   ```bash
   php sqltest.php
   ```

1. 验证是否返回了前 20 行，然后关闭应用窗口。

## <a name="next-steps"></a>后续步骤

- [设计第一个 Azure SQL 数据库](sql-database-design-first-database.md)

- [用于 SQL Server 的 Microsoft PHP 驱动程序](https://github.com/Microsoft/msphpsql/)

- [报告问题或提出问题](https://github.com/Microsoft/msphpsql/issues)

- [重试逻辑示例：使用 PHP 弹性连接到 SQL](https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)
