---
title: 通过 .NET 使用 Visual Studio 和 C# 查询 Azure SQL 数据库 | Microsoft Docs
description: 使用 Visual Studio 创建连接到 Azure SQL 数据库的 C# 应用并使用 Transact-SQL 语句对其进行查询。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 12/11/2018
ms.date: 02/25/2019
ms.openlocfilehash: be4a5434ff37e56c0f0026437317149833ed435a
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663726"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-an-azure-sql-database"></a>快速入门：使用 Visual Studio 中的 .NET 和 C# 来连接和查询 Azure SQL 数据库

本快速入门展示了如何使用 Visual Studio 中的 [.NET Framework](https://www.microsoft.com/net/) 和 C# 代码通过 Transact-SQL 语句查询 Azure SQL 数据库。

## <a name="prerequisites"></a>先决条件

若要完成本快速入门，需要以下项：

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) Community、Professional 或 Enterprise 版本。

## <a name="get-sql-server-connection-information"></a>获取 SQL Server 连接信息

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-code-to-query-the-sql-database"></a>创建代码来查询 SQL 数据库

1. 在 Visual Studio 中，选择“文件” > “新建” > “项目”。 
   
1. 在“新建项目”对话框中，选择“Visual C#”，然后选择“控制台应用(.NET Framework)”。
   
1. 输入“sqltest”作为项目名称，然后选择“确定”。 创建新项目。 
   
1. 选择“项目” > “管理 NuGet 包”。 
   
1. 在“NuGet 包管理器”中，选择“浏览”选项卡，然后搜索并选择“System.Data.SqlClient”。
   
1. 在“System.Data.SqlClient”页上选择“安装”。 
   - 如果出现提示，请选择“确定”继续安装。 
   - 如果显示“接受许可证”窗口，则选择“我接受”。
   
1. 安装完成后，可以关闭“NuGet 包管理器”。 
   
1. 在代码编辑器中，将 Program.cs 内容替换为以下代码。 将值替换为 `<server>`、`<username>`、`<password>` 和 `<database>`。
   
   >[!IMPORTANT]
   >本示例中的代码使用示例 AdventureWorksLT 数据，在创建数据库时可以选择该数据作为源。 如果数据库有不同数据，请在 SELECT 查询中使用自己数据库中的表。 
   
   ```csharp
   using System;
   using System.Data.SqlClient;
   using System.Text;
   
   namespace sqltest
   {
       class Program
       {
           static void Main(string[] args)
           {
               try 
               { 
                   SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                   builder.DataSource = "<server>.database.chinacloudapi.cn"; 
                   builder.UserID = "<username>";            
                   builder.Password = "<password>";     
                   builder.InitialCatalog = "<database>";
   
                   using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                   {
                       Console.WriteLine("\nQuery data example:");
                       Console.WriteLine("=========================================\n");
                       
                       connection.Open();       
                       StringBuilder sb = new StringBuilder();
                       sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                       sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                       sb.Append("JOIN [SalesLT].[Product] p ");
                       sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                       String sql = sb.ToString();
   
                       using (SqlCommand command = new SqlCommand(sql, connection))
                       {
                           using (SqlDataReader reader = command.ExecuteReader())
                           {
                               while (reader.Read())
                               {
                                   Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                               }
                           }
                       }                    
                   }
               }
               catch (SqlException e)
               {
                   Console.WriteLine(e.ToString());
               }
               Console.ReadLine();
           }
       }
   }
   ```

## <a name="run-the-code"></a>运行代码

1. 若要运行该应用，请选择“调试” > “开始调试”，或选择工具栏上的“开始”，或按 F5。
1. 验证是否返回了数据库中的前 20 个类别/产品行，然后关闭应用窗口。

## <a name="next-steps"></a>后续步骤

- 了解如何在 Windows/Linux/macOS 中[使用 .NET Core 连接和查询 Azure SQL 数据库](sql-database-connect-query-dotnet-core.md)。  
- 了解[在 Windows/Linux/macOS 中通过命令行使用 .NET Core 入门](https://docs.microsoft.com/dotnet/core/tutorials/using-with-xplat-cli)。
- 了解如何[使用 SSMS 设计第一个 Azure SQL 数据库](sql-database-design-first-database.md)，或者如何[使用 .NET 设计第一个 Azure SQL 数据库](sql-database-design-first-database-csharp.md)。
- 有关 .NET 的详细信息，请参阅 [.NET 文档](https://docs.azure.cn/zh-cn/dotnet/)。
- 重试逻辑示例：[使用 ADO.NET 弹性连接到 SQL][step-4-connect-resiliently-to-sql-with-ado-net-a78n]。


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

