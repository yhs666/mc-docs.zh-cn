---
title: 使用 Node.js 查询 Azure SQL 数据库 | Microsoft Docs
description: 如何使用 Node.js 创建连接到 Azure SQL 数据库的程序并使用 T-SQL 语句对其进行查询。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: WenJason
ms.author: v-jay
ms.reviewer: v-masebo
manager: digimobile
origin.date: 11/26/2018
ms.date: 02/25/2019
ms.openlocfilehash: be00b0d7aa331546e84706b8b9efdd5d7fe920c9
ms.sourcegitcommit: 0ccbf718e90bc4e374df83b1460585d3b17239ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2019
ms.locfileid: "57347062"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>快速入门：使用 Node.js 查询 Azure SQL 数据库

本文演示了如何使用 [Node.js](https://nodejs.org) 连接到 Azure SQL 数据库。 然后即可使用 T-SQL 语句来查询数据。

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

- 适用于操作系统的 Node.js 相关软件：

  - **MacOS**：安装 Homebrew 和 Node.js，然后安装 ODBC 驱动程序和 SQLCMD。 请参阅[步骤 1.2 和 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/)。
  
  - **Ubuntu**：安装 Node.js，然后安装 ODBC 驱动程序和 SQLCMD。 请参阅[步骤 1.2 和 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/)。
  
  - **Windows**：安装 Chocolatey 和 Node.js，然后安装 ODBC 驱动程序和 SQLCMD。 请参阅[步骤 1.2 和 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/)。

## <a name="get-sql-server-connection-information"></a>获取 SQL Server 连接信息

获取连接到 Azure SQL 数据库所需的连接信息。 在后续过程中，将需要完全限定的服务器名称或主机名称、数据库名称和登录信息。

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

2. 导航到“SQL 数据库”页面。

3. 在“概述”页中，查看单一数据库的“服务器名称”旁边的完全限定的服务器名称。 若要复制服务器名称或主机名称，请将鼠标悬停在其上方，然后选择“复制”图标。 

## <a name="create-the-project"></a>创建项目

打开命令提示符，并创建一个名为 *sqltest* 的文件夹。 导航到已创建的文件夹，并运行以下命令：

  ```bash
  npm init -y
  npm install tedious
  npm install async
  ```

## <a name="add-code-to-query-database"></a>添加用于查询数据库的代码

1. 在喜欢的文本编辑器中，创建新文件 *sqltest.js*。

1. 将其内容替换为以下代码。 然后，为服务器、数据库、用户和密码添加相应的值。

    ```js
    var Connection = require('tedious').Connection;
    var Request = require('tedious').Request;

    // Create connection to database
    var config =
    {
        userName: 'your_username', // update me
        password: 'your_password', // update me
        server: 'your_server.database.chinacloudapi.cn', // update me
        options:
        {
            database: 'your_database', //update me
            encrypt: true
        }
    }
    var connection = new Connection(config);

    // Attempt to connect and execute queries if connection goes through
    connection.on('connect', function(err)
        {
            if (err)
            {
                console.log(err)
            }
            else
            {
                queryDatabase()
            }
        }
    );

    function queryDatabase()
    {
        console.log('Reading rows from the Table...');

        // Read all rows from table
        var request = new Request(
            "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc "
                + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
            function(err, rowCount, rows)
            {
                console.log(rowCount + ' row(s) returned');
                process.exit();
            }
        );

        request.on('row', function(columns) {
            columns.forEach(function(column) {
                console.log("%s\t%s", column.metadata.colName, column.value);
            });
        });
        connection.execSql(request);
    }
    ```

> [!NOTE]
> 代码示例使用适用于 Azure SQL 的 **AdventureWorksLT** 示例数据库。

## <a name="run-the-code"></a>运行代码

1. 在命令提示符下运行此程序。

    ```bash
    node sqltest.js
    ```

1. 验证是否已返回前 20 行，然后关闭应用程序窗口。

## <a name="next-steps"></a>后续步骤

- [用于 SQL Server 的 Microsoft Node.js 驱动程序](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)

- 通过 [.NET core](sql-database-connect-query-dotnet-core.md)、[Visual Studio Code](sql-database-connect-query-vscode.md) 或 [SSMS](sql-database-connect-query-ssms.md)（仅限 Windows）在 Windows/Linux/macOS 上进行连接和查询

- [在 Windows/Linux/macOS 中通过命令行使用 .NET Core 入门](https://docs.microsoft.com/dotnet/core/tutorials/using-with-xplat-cli)

- 使用 [.NET](sql-database-design-first-database-csharp.md) 或 [SSMS](sql-database-design-first-database.md) 设计第一个 Azure SQL 数据库
