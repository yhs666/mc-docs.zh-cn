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
origin.date: 02/12/2019
ms.date: 03/11/2019
ms.openlocfilehash: 69ca29c23c9bddde248e3fff36c26a84c21a210c
ms.sourcegitcommit: 0ccbf718e90bc4e374df83b1460585d3b17239ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2019
ms.locfileid: "57347207"
---
# <a name="quickstart-use-php-to-query-an-azure-sql-database"></a>快速入门：使用 PHP 查询 Azure SQL 数据库

本文演示了如何使用 [PHP](http://php.net/manual/en/intro-whatis.php) 连接到 Azure SQL 数据库。 然后即可使用 T-SQL 语句来查询数据。

## <a name="prerequisites"></a>先决条件

若要完成此示例，请确保具备以下先决条件：

- Azure SQL 数据库。 可以根据下述快速入门中的一个的说明在 Azure SQL 数据库中创建数据库，然后对其进行配置：

  || 单一数据库 |
  |:--- |:--- |
  | 创建| [Portal](sql-database-single-database-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) |
  | 配置 | [服务器级别 IP 防火墙规则](sql-database-server-level-firewall-rule.md)|
  |加载数据|根据快速入门加载的 Adventure Works|
  |||

- 已为操作系统安装与 PHP 相关的软件：

    - **MacOS**，安装 PHP、ODBC 驱动程序，然后安装 PHP Driver for SQL Server。 请参阅[步骤 1、2 和 3](https://docs.microsoft.com/sql/connect/php/installation-tutorial-linux-mac)。

    - **Linux**，安装 PHP、ODBC 驱动程序，然后安装 PHP Driver for SQL Server。 请参阅[步骤 1、2 和 3](https://docs.microsoft.com/sql/connect/php/installation-tutorial-linux-mac)。

  - **Windows**：安装用于 IIS Express 的 PHP 和 Chocolatey，然后安装 ODBC 驱动程序和 SQLCMD。 请参阅[步骤 1.2 和 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/)。

## <a name="get-sql-server-connection-information"></a>获取 SQL Server 连接信息

获取连接到 Azure SQL 数据库所需的连接信息。 在后续过程中，将需要完全限定的服务器名称或主机名称、数据库名称和登录信息。

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

2. 导航到“SQL 数据库”页面。

3. 在“概述”页中，查看单一数据库的“服务器名称”旁边的完全限定的服务器名称。 若要复制服务器名称或主机名称，请将鼠标悬停在其上方，然后选择“复制”图标。

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
