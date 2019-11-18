---
title: 排查 Microsoft Azure SQL 数据库的连接问题 | Microsoft Docs
description: 介绍如何排查 Azure SQL 数据库中的连接问题。
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
author: WenJason
ms.author: v-jay
ms.reviewer: ''
origin.date: 09/27/2019
ms.date: 11/04/2019
ms.openlocfilehash: 1dde938d24e6a91f46b40147f5caadcdf50c01c1
ms.sourcegitcommit: 97fa37512f79417ff8cd86e76fe62bac5d24a1bd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73041377"
---
# <a name="troubleshooting-connectivity-issues-with-azure-sql-database"></a>排查 Azure SQL 数据库的连接问题

与 Azure SQL 数据库连接失败时，会出现错误消息。 连接问题可能是 SQL Azure 数据库重新配置、防火墙设置、连接超时或不正确的登录信息造成的。 此外，如果达到了某些 Azure SQL 数据库资源的最大限制，则无法连接到 Azure SQL 数据库。

## <a name="error-40613-database--x--on-server--y--is-not-currently-available"></a>错误 40613：服务器 < y > 上的数据库 < x > 当前不可用

**详细错误**

``40613: Database <DBname> on server < server name > is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of '< Tracing ID >'.``

若要解决此问题，请执行以下操作：

1. 检查 Azure [服务仪表板](https://www.azure.cn/zh-cn/support/service-dashboard/)以查看是否存在任何已知的服务中断。 
2. 如果未出现已知的服务中断，请转到 [Azure 支持网站](https://support.azure.cn/zh-cn/support/contact/)提出支持案例。

有关详细信息，请参阅[排查“服务器上的数据库当前不可用”错误](/sql-database/sql-database-troubleshoot-common-connection-issues#troubleshoot-transient-errors)。

## <a name="a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server"></a>与 SQL Server 建立连接时，出现网络相关或特定于实例的错误

如果应用程序无法连接到服务器，则会出现此问题。

若要解决此问题，请尝试执行[解决常见连接问题的步骤](#steps-to-fix-common-connection-issues)部分所述的步骤（按所述顺序执行）。

## <a name="the-server-was-not-found-or-was-not-accessible-errors-26-40-10053"></a>找不到或无法访问服务器（错误 26、40、10053）

### <a name="error-26-error-locating-serverinstance-specified"></a>错误 26：查找指定的服务器/实例时出错

**详细错误**

``System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.(provider: SQL Network Interfaces, error: 26 – Error Locating Server/Instance Specified)``

### <a name="error-40-could-not-open-a-connection-to-sql-server"></a>错误 40：无法与 SQL Server 建立连接

**详细错误**

``A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)``

### <a name="error-10053-a-transport-level-error-has-occurred-when-receiving-results-from-the-server"></a>错误 10053：在接收来自服务器的结果时发生传输级错误

**详细错误**

``10053: A transport-level error has occurred when receiving results from the server. (Provider: TCP Provider, error: 0 - An established connection was aborted by the software in your host machine)``

发生这些错误的原因是应用程序无法连接到服务器。

若要解决此问题，请尝试执行[解决常见连接问题的步骤](#steps-to-fix-common-connection-issues)部分所述的步骤（按所述顺序执行）。

## <a name="cannot-connect-to-servername-due-to-firewall-issues"></a>由于防火墙问题而无法连接到 <servername>

### <a name="error-40615-cannot-connect-to--servername-"></a>错误 40615：无法连接到 <服务器名称>

若要解决此问题，请[通过 Azure 门户在 SQL 数据库中配置防火墙设置](/sql-database/sql-database-firewall-configure)。

### <a name="error-5-cannot-connect-to--servername-"></a>错误 5：无法连接到 <服务器名称>

若要解决此问题，请在客户端与 Internet 之间的所有防火墙上，确保为出站连接打开端口 1433。

有关详细信息，请参阅[配置 Windows 防火墙以便能够访问 SQL Server](/sql-database/sql-database-firewall-configure)。

## <a name="unable-to-log-in-to-the-server-errors-18456-40531"></a>无法登录到服务器（错误 18456、40531）

### <a name="login-failed-for-user--user-name-"></a>用户 '< 用户名>' 登录失败

**详细错误**

``Login failed for user '<User name>'.This session has been assigned a tracing ID of '<Tracing ID>'. Provide this tracing ID to customer support when you need assistance. (Microsoft SQL Server, Error: 18456)``

若要解决此问题，请让服务管理员为你提供有效的 SQL Server 用户名和密码。

服务管理员通常可使用以下步骤添加登录凭据：

1. 使用 SQL Server Management Studio (SSMS) 登录到服务器。
2. 运行以下 SQL 查询，以检查是否已禁用登录名：

   ```
   SELECT name, is_disabled FROM sys.sql_logins
   ```

3. 如果禁用了相应的名称，请使用以下语句来启用它： 

   ```
   Alter login <User name> enable
   ```

4. 如果该 SQL 登录用户名不存在，请执行以下步骤创建该用户名：

   1. 在 SSMS 中，双击“安全性”将其展开。  
   2. 右键单击“登录”  ，然后选择“新建登录名”  。 
   3. 在生成的带占位符的脚本中，编辑并运行以下 SQL 查询：
 
   ```
   CREATE LOGIN <SQL_login_name, sysname, login_name>
   WITH PASSWORD = ‘<password, sysname, Change_Password>’
   GO
   ```       
5. 双击“数据库”  。 
6. 选择要授予用户权限的数据库。
7. 双击“安全性”。  
8. 右键单击“用户”  ，然后选择“新建用户”  。 
9. 在生成的带占位符的脚本中，编辑并运行以下 SQL 查询： 

   ```
   CREATE USER <user_name, sysname, user_name>          
   FOR LOGIN <login_name, sysname, login_name>
   WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
   GO
   
   -- Add user to the database owner role

   EXEC sp_addrolemember N’db_owner’, N’<user_name, sysname, user_name>’
   GO
   ```
   
   > [!NOTE]
   > 也可使用 `sp_addrolemember` 将特定的用户映射到特定的数据库角色。

有关详细信息，请参阅[在 Azure SQL 数据库中管理数据库和登录名](/sql-database/sql-database-manage-logins)。

## <a name="connection-timeout-expired-errors"></a>连接超时过期错误

### <a name="systemdatasqlclientsqlexception-0x80131904-connection-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904)：连接超时过期

**详细错误**

``System.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed while attempting to consume the pre-login handshake acknowledgement. This could be because the pre-login handshake failed or the server was unable to respond back in time. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=3; handshake=29995;``

### <a name="systemdatasqlclientsqlexception-0x80131904-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904)：超时过期

**详细错误**

``System.Data.SqlClient.SqlException (0x80131904): Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding.``

### <a name="systemdataentitycoreentityexception-the-underlying-provider-failed-on-open"></a>System.Data.Entity.Core.EntityException：基础提供程序在打开时失败

**详细错误**

``System.Data.Entity.Core.EntityException: The underlying provider failed on Open. -> System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. -> System.ComponentModel.Win32Exception: The wait operation timed out``

### <a name="cannot-connect-to--server-name-"></a>无法连接到 <服务器名称>

**详细错误**

``Cannot connect to <server name>.ADDITIONAL INFORMATION:Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=231; handshake=983; [Login] initialization=0; authentication=0; [Post-Login] complete=13000; (Microsoft SQL Server, Error: -2) For help, click: https://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=-2&LinkId=20476 The wait operation timed out``

这些异常可能是连接或查询问题造成的。 若要确认此错误是否由连接问题造成，请参阅[确认错误是否由连接问题造成](#confirm-whether-an-error-is-caused-by-a-connectivity-issue)。

发生连接超时的原因是应用程序无法连接到服务器。 若要解决此问题，请尝试执行[解决常见连接问题的步骤](#steps-to-fix-common-connection-issues)部分所述的步骤（按所述顺序执行）。

## <a name="transient-errors-errors-40197-40545"></a>暂时性错误（错误 40197、40545）

### <a name="error-40197-the-service-has-encountered-an-error-processing-your-request-please-try-again-error-code--code-"></a>错误 40197：该服务在处理你的请求时遇到错误。 请重试。 错误代码 <代码>

出现此问题的原因是在后端重新配置或故障转移期间遇到了暂时性的错误。

若要解决此问题，请等待片刻时间，然后重试。 除非此问题持续出现，否则不需要提出支持案例。

最佳做法是确保实施重试逻辑。 有关重试逻辑的详细信息，请参阅[排查 SQL 数据库的暂时性故障和连接错误](/sql-database/sql-database-connectivity-issues)。

## <a name="connection-terminated-because-of-a-system-defined-limit"></a>由于达到系统定义的限制而终止了连接

### <a name="error-10928-resource-id-d"></a>错误 10928：资源 ID: %d

**详细错误**

``10928: Resource ID: %d. The %s limit for the database is %d and has been reached. See https://docs.azure.cn/zh-cn/sql-database/sql-database-resource-limits-database-server for assistance. The Resource ID value in error message indicates the resource for which limit has been reached. For sessions, Resource ID = 2.``

若要解决此问题，请尝试以下方法之一：

* 检查是否存在长时间运行的查询。

  > [!NOTE]
  > 这是最简单的方法，但不一定能够解决问题。

  1. 运行以下 SQL 查询来检查 [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) 视图，以确定是否存在任何阻塞的请求：

             ```
             SELECT * FROM dm_exec_requests
             ```

  2. 确定最前面的阻塞查询的**输入缓冲区**。
  3. 优化最前面的阻塞查询。

    有关详细的故障排除过程，请参阅[我的查询是否在云中正常运行？](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)。

* 如果即使解决了长时间运行的阻塞查询，数据库也仍不断地达到其限制，请考虑升级到新的预览版（例如[标准版或高级版](https://azure.cn/pricing/details/sql-database/)）。

有关 SQL 数据库定价选项的详细信息，请参阅 [Azure SQL 数据库定价](https://azure.cn/pricing/details/sql-database/)。

有关动态管理视图的详细信息，请参阅[系统动态管理视图](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)。

有关此错误消息的详细信息，请参阅 [Azure SQL 数据库服务器的 SQL 数据库资源限制](/sql-database/sql-database-resource-limits-database-server)。

### <a name="error-10929-resource-id-1"></a>错误 10929：资源 ID：1

**详细错误**

``10929: Resource ID: 1. The %s minimum guarantee is %d, maximum limit is %d and the current usage for the database is %d. However, the server is currently too busy to support requests greater than %d for this database. See https://docs.azure.cn/zh-cn/sql-database/sql-database-resource-limits-database-server for assistance. Otherwise, please try again later.``

有关此错误的详细信息，请参阅 [SQL 数据库客户端程序的错误消息](/sql-database/sql-database-develop-error-messages)。

### <a name="error-40501-the-service-is-currently-busy"></a>错误 40501：服务当前繁忙

**详细错误**

``40501: The service is currently busy. Retry the request after 10 seconds. Incident ID: %ls. Code: %d.``

这是一个引擎限制错误，表示超过了资源限制。

有关资源限制的详细信息，请参阅[数据库服务器资源限制](/sql-database/sql-database-resource-limits-database-server)。

### <a name="error-40544-the-database-has-reached-its-size-quota"></a>错误 40544：数据库已达到其大小配额

**详细错误**

``40544: The database has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions. Incident ID: <ID>. Code: <code>.``

当数据库达到其大小配额时会发生此错误。

以下步骤可帮助你解决此问题，或提供更多的选项：

1. 使用 Azure 门户中的仪表板检查数据库的当前大小。

   > [!NOTE]
   > 若要识别哪些表消耗了最多的空间，因此确定可能需要清理哪些表，请运行以下 SQL 查询：

   ```
   SELECT o.name,
    a.SUM(p.row_count) AS 'Row Count',
    b.SUM(p.reserved_page_count) * 8.0 / 1024 AS 'Table Size (MB)'
   FROM sys.objects o
   JOIN sys.dm_db_partition_stats p on p.object_id = o.object_id
   GROUP BY o.name
   ORDER BY [Table Size (MB)] DESC
   ```

2. 如果当前大小未超过所用版本支持的最大大小，你可以使用 ALTER DATABASE 来增大 MAXSIZE 设置。 
3. 如果数据库已超过所用版本支持的最大大小，请尝试以下一个或多个步骤：
   - 执行一般的数据库清理活动。 例如，使用截断/删除操作清理不需要的数据，或者使用 SQL Server Integration Services (SSIS) 或批量复制程序 (bcp) 实用工具移出数据。
   - 请将数据分区或删除、删除索引或查阅文档以找到可能的解决方案。

   - 有关数据库缩放的信息，请参阅[缩放单一数据库资源](/sql-database/sql-database-single-database-scale)和[缩放弹性池资源](/sql-database/sql-database-elastic-pool-scale)。

### <a name="error-40549-session-is-terminated-because-you-have-a-long-running-transaction"></a>错误 40549：存在长时间运行的事务，因此已终止会话

**详细错误**

``40549: Session is terminated because you have a long-running transaction. Try shortening your transaction.``

如果反复出现此错误，请尝试执行以下步骤来解决问题： 

1. 检查 sys.dm_exec_requests 视图，以确定是否有任何打开的会话对 total_elapsed_time 列使用了较大的值。 运行以下 SQL 脚本来执行此项检查：

   ```
   SELECT * FROM dm_exec_requests
   ```
2. 确定长时间运行的查询的输入缓冲区。 
3. 优化该查询。

还应考虑批处理查询。 有关批处理的信息，请参阅[如何使用批处理来改善 SQL 数据库应用程序的性能](/sql-database/sql-database-use-batching-to-improve-performance)。

有关详细的故障排除过程，请参阅[我的查询是否在云中正常运行？](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)。

### <a name="error-40551-the-session-has-been-terminated-because-of-excessive-tempdb-usage"></a>错误 40551：由于过度使用 TEMPDB，已终止会话

**详细错误**

``40551: The session has been terminated because of excessive TEMPDB usage. Try modifying your query to reduce the temporary table space usage.``

若要解决此问题，请执行以下步骤：

1. 更改查询以减少临时表空间的用量。 
2. 删除不再需要的临时对象。 
3. 截断表，或删除未使用的表。

### <a name="error-40552-the-session-has-been-terminated-because-of-excessive-transaction-log-space-usage"></a>错误 40552：由于过度使用事务日志空间，已终止会话

**详细错误**

``40552: The session has been terminated because of excessive transaction log space usage. Try modifying fewer rows in a single transaction.``

若要解决此问题，请尝试执行以下方法：

* 此问题可能是插入、更新或删除操作造成的。 通过立即实施批处理或拆分为多个较小的事务，来尝试减少操作的行数。
* 此问题可能是索引重建操作造成的。 若要解决此问题，请确保表中受影响的行数 * (所更新字段的平均字节大小 + 80) < 2 GB。

  > [!NOTE]
  > 对于索引重建，所更新字段的平均大小应替换为平均索引大小。

### <a name="error-40553-the-session-has-been-terminated-because-of-excessive-memory-usage"></a>错误 40553：由于过度使用内存，已终止会话

**详细错误**

``40553 : The session has been terminated because of excessive memory usage. Try modifying your query to process fewer rows.``

若要解决此问题，请尝试优化查询。

有关详细的故障排除过程，请参阅[我的查询是否在云中正常运行？](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)。


### <a name="cannot-open-database-master-requested-by-the-login-the-login-failed"></a>无法打开登录名请求的数据库 "master"。 登录失败。

之所以出现此问题，是因为该帐户无权访问 master 数据库。 但默认情况下，SQL Server Management Studio (SSMS) 会尝试连接到 master 数据库。

若要解决此问题，请执行以下步骤：

1. 在 SSMS 的登录屏幕上选择“选项”，然后选择“连接属性”。   
2. 在“连接到数据库”字段中，输入用户的默认数据库名称作为默认登录数据库，然后选择“连接”。  

   ![连接属性](media/troubleshoot-connectivity-issues-microsoft-azure-sql-database/cannot-open-database-master.png)

## <a name="confirm-whether-an-error-is-caused-by-a-connectivity-issue"></a>确认错误是否由连接问题造成

若要确认某个错误是否由连接问题造成，请查看帧的堆栈跟踪，其中会显示打开连接的调用，如下所示（请注意对 **SqlConnection** 类的引用）：

```
System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
 at System.Data.SqlClient.SqlConnection.Open()
 at AzureConnectionTest.Program.Main(String[] args)
ClientConnectionId:<Client connection ID>
```

如果异常是由查询问题触发的，则你会看到如下所示的调用堆栈（请注意对 **SqlCommand** 类的引用）。 对于这种情况，请[优化查询](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)。

```
  at System.Data.SqlClient.SqlCommand.ExecuteReader()
  at AzureConnectionTest.Program.Main(String[] args)
  ClientConnectionId:<Client ID>
```
有关优化性能的更多指导，请参阅以下资源：

* [如何维护 Azure SQL 索引和统计信息](https://techcommunity.microsoft.com/t5/Azure-Database-Support-Blog/How-to-maintain-Azure-SQL-Indexes-and-Statistics/ba-p/368787)
* [手动优化 Azure SQL 数据库中的查询性能](/sql-database/sql-database-performance-guidance)
* [使用动态管理视图监视 Azure SQL 数据库的性能](/sql-database/sql-database-monitoring-with-dmvs)
* [在 Azure SQL 数据库中操作 Query Store](/sql-database/sql-database-operate-query-store)


## <a name="steps-to-fix-common-connection-issues"></a>解决常见连接问题的步骤

1. 确保已在应用程序服务器上启用 TCP/IP 作为客户端协议。 有关详细信息，请参阅[配置客户端协议](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-client-protocols)。 在未安装 SQL Server 工具的应用程序服务器上，运行 **cliconfg.exe**（SQL Server 客户端网络实用工具）来验证是否已启用 TCP/IP。 
2. 检查应用程序的连接字符串，确保配置正确。 例如，确保连接字符串指定了正确的端口 (1433) 和完全限定的服务器名称。
参阅[获取 SQL Server 连接信息](/sql-database/sql-database-connect-query-ssms#get-sql-server-connection-information)。
3. 尝试增大连接超时值。 我们建议至少使用 30 秒连接超时。 
4. 使用 [SQL Server Management Studio (SSMS)](/sql-database/sql-database-connect-query-ssms)、UDL 文件、ping 或 telnet 来测试应用程序服务器与 Azure SQL 数据库之间的连接。 有关详细信息，请参阅[连接问题诊断](/sql-database/sql-database-connectivity-issues#diagnostics)。

   > [!NOTE]
   > 作为故障排除步骤，还可以在不同的客户端计算机上测试连接。

5. 最佳做法是确保实施重试逻辑。 有关重试逻辑的详细信息，请参阅[排查 SQL 数据库的暂时性故障和连接错误](/sql-database/sql-database-connectivity-issues)。

如果这些步骤无法解决问题，请尝试收集更多的数据，然后联系支持人员。 如果应用程序是云服务，请启用日志记录。 此步骤会返回发生失败时的 UTC 时间戳。 此外，SQL Azure 会返回跟踪 ID。 [Azure 客户支持服务](https://support.azure.cn/zh-cn/support/contact/)可使用此信息。 

有关如何启用日志记录的详细信息，请参阅[为 Azure 应用服务中的应用启用诊断日志记录](/app-service/troubleshoot-diagnostic-logs)。

**相关文档**

* [Azure SQL 连接体系结构](/sql-database/sql-database-connectivity-architecture)<br>
* [Azure SQL 数据库和数据仓库网络访问控制](/sql-database/sql-database-networkaccess-overview)
