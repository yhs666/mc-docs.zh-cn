---
title: "实现地理分散的 Azure SQL 数据库解决方案 | Azure"
description: "了解如何配置 Azure SQL 数据库和应用程序以便故障转移到复制的数据库，以及如何测试故障转移。"
services: sql-database
documentationcenter: 
author: Hayley244
manager: digimobile
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
origin.date: 05/26/2017
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 45f1db8a3f83f5426f90e63ad6fa161911079edf
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 实现地理分散的数据库
<a id="implement-a-geo-distributed-database" class="xliff"></a>

在本教程中，将配置 Azure SQL 数据库和应用程序以便故障转移到远程区域中，然后测试故障转移计划。 你将学习如何执行以下操作： 

> [!div class="checklist"]
> * 创建数据库用户并向其授予权限
> * 设置数据库级防火墙规则
> * 创建[异地复制故障转移组](sql-database-geo-replication-overview.md)
> * 创建和编译 Java 应用程序以查询 Azure SQL 数据库
> * 执行灾难恢复演练

## 先决条件
<a id="prerequisites" class="xliff"></a>

若要完成本教程，请确保做好以下准备：

- 最新的 [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs)。 
- Azure SQL 数据库。 本教程使用以下任一快速入门中名为“mySampleDatabase”的 AdventureWorksLT 示例数据库：

   - [创建 DB - 门户](sql-database-get-started-portal.md)
   - [创建 DB - CLI](sql-database-get-started-cli.md)
   - [创建 DB - PowerShell](sql-database-get-started-powershell.md)

此外，若要针对自己的数据库执行 SQL 脚本，可使用以下查询工具之一：
   - [Azure 门户](https://portal.azure.cn)中的查询编辑器。 有关使用 Azure 门户中的查询编辑器的详细信息，请参阅[使用查询编辑器进行连接和查询](sql-database-get-started-portal.md#query-the-sql-database)。
   - 最新版本的 [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)，它是用于管理任何 SQL 基础结构（从适用于 Microsoft Windows 的 SQL Server 到 SQL 数据库，不一而足）的集成环境。
   - 最新版本的 [Visual Studio Code](https://code.visualstudio.com/docs)，它是一种图形代码编辑器，适用于 Linux、macOS 和 Windows，并且支持各种扩展，其中包括 [mssql 扩展](https://aka.ms/mssql-marketplace)（用于查询 Microsoft SQL Server、Azure SQL 数据库和 SQL 数据仓库）。 有关对 Azure SQL 数据库使用此工具的详细信息，请参阅[使用 VS Code 进行连接和查询](sql-database-connect-query-vscode.md)。 

## 创建数据库用户并授予权限
<a id="create-database-users-and-grant-permissions" class="xliff"></a>

连接到数据库并使用以下查询工具之一创建用户帐户：

- Azure 门户中的查询编辑器
- SQL Server Management Studio
- Visual Studio Code

这些用户帐户将自动复制到辅助服务器（并保持同步）。 若要使用 SQL Server Management Studio 或 Visual Studio Code，如果进行连接的客户端所在的 IP 地址尚未配置防火墙，则需要配置防火墙规则。 有关详细步骤，请参阅[创建服务器级防火墙规则](sql-database-get-started-portal.md#create-a-server-level-firewall-rule)。

- 在查询窗口中执行以下查询，在数据库中创建两个用户帐户。 此脚本授予“app_admin”帐户“db_owner”权限，并授予“app_user”帐户“SELECT”（选择）和“UPDATE”（更新）权限。 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## 创建数据库级防火墙
<a id="create-database-level-firewall" class="xliff"></a>

为 SQL 数据库创建[数据库级防火墙规则](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)。 此数据库级防火墙规则将自动复制到在本教程中创建的辅助服务器。 为简单起见（在本教程中），使用执行本教程中步骤的计算机的公共 IP 地址。 若要确定用于服务器级防火墙规则（针对当前计算机）的 IP 地址，请参阅[创建服务器级防火墙](sql-database-get-started-portal.md#create-a-server-level-firewall-rule)。  

- 在打开的查询窗口中，将之前的查询替换为以下查询，将 IP 地址替换为环境中相应的 IP 地址。  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## 创建活动异地复制自动故障转移组
<a id="create-an-active-geo-replication-auto-failover-group" class="xliff"></a> 

使用 Azure PowerShell 在 Azure 区域中的现有 Azure SQL 服务器和新的空 Azure SQL 服务器之间创建[活动异地复制自动故障转移组](sql-database-geo-replication-overview.md)，然后将示例数据库添加到故障转移组。

> [!IMPORTANT]
> 这些 cmdlet 需要 Azure PowerShell 4.0。 [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. 使用现有服务器和示例数据库的值为 PowerShell 脚本填充变量，并为故障转移组名称提供全局唯一值。

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. 在故障转移区域中创建空的备份服务器。

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. 在这两个服务器之间创建故障转移组。

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      -FailoverGroupName $myfailovergroupname `
      -FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. 将数据库添加到故障转移组。

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## 安装 Java 软件
<a id="install-java-software" class="xliff"></a>

本部分中的步骤假定你熟悉使用 Java 开发，但不熟悉如何使用 Azure SQL 数据库。 

### **Mac OS**
<a id="mac-os" class="xliff"></a>
打开终端并导航到要在其中创建 Java 项目的目录。 输入以下命令安装 **brew** 和 **Maven**： 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

有关安装和配置 Java 和 Maven 环境的详细指导，请转到[使用 SQL Server 生成应用](https://www.microsoft.com/sql-server/developer-get-started/)，依次选择“Java”和“MacOS”，然后按照步骤 1.2 和 1.3 中有关配置 Java 和 Maven 的详细说明进行操作。

### **Linux (Ubuntu)**
<a id="linux-ubuntu" class="xliff"></a>
打开终端并导航到要在其中创建 Java 项目的目录。 输入以下命令安装 **Maven**：

```bash
sudo apt-get install maven
```

有关安装和配置 Java 和 Maven 环境的详细指导，请转到[使用 SQL Server 生成应用](https://www.microsoft.com/sql-server/developer-get-started/)，依次选择“Java”和“Ubuntu”，然后遵循步骤 1.2、1.3 和 1.4 中配置 Java 和 Maven 的详细说明。

### **Windows**
<a id="windows" class="xliff"></a>
使用官方安装程序安装 [Maven](https://maven.apache.org/download.cgi)。 使用 Maven 帮助管理依赖项、内部版本、测试和运行 Java 项目。 有关安装和配置 Java 和 Maven 环境的详细指导，请转到[使用 SQL Server 生成应用](https://www.microsoft.com/sql-server/developer-get-started/)，依次选择“Java”和“Windows”，然后遵循步骤 1.2 和 1.3 中配置 Java 和 Maven 的详细说明。

## 创建 SqlDbSample 项目
<a id="create-sqldbsample-project" class="xliff"></a>

1. 在命令控制台（例如 Bash）中，创建一个 Maven 项目。 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. 键入“Y”并单击“Enter”。
3. 将目录更改到新创建的项目。

   ```bash
   cd SqlDbSamples
   ```

4. 使用最常用的编辑器，在打开项目文件夹中的 pom.xml 文件。 

5. 通过打开最常用的文本编辑器并将以下行复制和粘贴到 pom.xml 文件中，将 Microsoft JDBC Driver for SQL Server 依赖项添加到 Maven 项目。 不会覆盖预先填充在文件中的现有值。 JDBC 依赖项必须粘贴在更大的“依赖项”部分 ( ) 内。   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. 通过将以下“属性”部分添加到 pom.xml 文件中的“依赖项”部分之后，指定编译项目时面向的 Java 版本。 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. 将以下“生成”部分添加到 pom.xml 文件的“属性”部分之后，以支持 jar 中的清单文件。       

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```
8. 保存并关闭 pom.xml 文件。
9. 打开 App.java 文件 (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) 并将内容替换为以下内容。 将故障转移组名称替换为自己的故障转移组的名称。 如果已更改数据库名、用户或密码的值，那么也要更改这些值。

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";

      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.chinacloudapi.cn:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.chinacloudapi.cn;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.chinacloudapi.cn:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.chinacloudapi.cn;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name that we can use to find the product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data that was previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```
6. 保存并关闭 App.java 文件。

## 编译并运行 SqlDbSample 项目
<a id="compile-and-run-the-sqldbsample-project" class="xliff"></a>

1. 在命令控制台中，执行以下命令。

   ```bash
   mvn package
   ```
2. 完成后，执行以下命令运行该应用程序（除非手动停止，否则该应用程序将运行大约 1 小时）：

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"

   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## 执行灾难恢复演练
<a id="perform-disaster-recovery-drill" class="xliff"></a>

1. 调用故障转移组的手动故障转移。 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. 在故障转移期间观察应用程序结果。 DNS 缓存刷新时，某些插入会失败。     

3. 了解灾难恢复服务器正在执行的角色。

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. 故障回复。

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. 在故障回复期间观察应用程序结果。 DNS 缓存刷新时，某些插入会失败。     

6. 了解灾难恢复服务器正在执行的角色。

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## 后续步骤
<a id="next-steps" class="xliff"></a> 

- 有关详细信息，请参阅[活动异地复制和故障转移组](sql-database-geo-replication-overview.md)。