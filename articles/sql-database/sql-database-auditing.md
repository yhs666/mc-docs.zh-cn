---
title: "Azure SQL 数据库审核入门 | Azure"
description: "Azure SQL 数据库审核入门"
services: sql-database
documentationcenter: 
author: Hayley244
manager: digimobile
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/07/2017
ms.date: 07/10/2017
ms.author: v-johch
ms.openlocfilehash: e603c2e96073b71ba12518166b83510c3acab215
ms.sourcegitcommit: f2f4389152bed7e17371546ddbe1e52c21c0686a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="get-started-with-sql-database-auditing"></a>SQL 数据库审核入门
Azure SQL 数据库审核跟踪数据库事件，并将事件写入 Azure 存储帐户中的审核日志。

审核可帮助你一直保持遵从法规、了解数据库活动，以及深入了解可以指明业务考量因素或疑似安全违规的偏差和异常。

审核有助于遵从法规标准，但不能保证遵从法规。 有关支持标准法规的 Azure 计划的详细信息，请参阅 [Azure 信任中心](https://www.trustcenter.cn/zh-cn/compliance/default.html)。

## <a id="subheading-1"></a>Azure SQL 数据库审核概述
SQL 数据库审核可让你：

* **保留** 选定事件的审核痕迹。 可以定义要审核的数据库操作的类别。
* **报告** 数据库活动。 可以使用预配置的报告和仪表板快速开始使用活动和事件报告。
* **分析** 报告。 可以查找可疑事件、异常活动和趋势。

可按[为数据库设置审核](#subheading-2)部分中所述，为不同类型的事件类别配置审核。

审核日志会写入 Azure 订阅中的 Azure Blob 存储。

## <a id="subheading-8"></a>服务器级与数据库级审核策略

可以为特定数据库定义审核策略，也可以将审核策略定义为默认服务器策略。

1. 服务器策略将应用到服务器上的所有现有数据库和新建数据库。

2. 如果**启用了服务器 blob 审核**，则会**始终应用于数据库**（即对数据库进行审核），而不考虑数据库审核设置。

3. 除了在服务器上启用 Blob 审核外，还对数据库启用 Blob 审核**不**会覆盖或更改服务器 Blob 审核的任何设置 - 这两种审核将并存。 换而言之，将并行对数据库审核两次（一次按服务器策略审核，一次按数据库策略审核）。

    > [!NOTE]
    > 应避免同时启用服务器 Blob 审核和数据库 Blob 审核，除非：
    > * 需要对特定数据库使用不同的存储帐户或保留期。
    > * 你希望为特定数据库审核事件类型或类别，而这些事件类型或类别不同于要为此服务器上其余数据库审核的事件类型或类别（例如，如果只有特定数据库需要审核表插入）。
    > <br><br>
    > 否则，**建议仅启用服务器级 Blob 审核**，并将所有数据库的数据库级审核保留为禁用。

## <a id="subheading-2"></a>为数据库设置审核
以下部分介绍如何使用 Azure 门户配置审核。

1. 启动 [Azure 门户](https://portal.azure.cn) (https://portal.azure.cn)。
2. 导航到你要审核的 SQL 数据库/SQL Server 的设置边栏选项卡。 在“设置”边栏选项卡中，选择“审核和威胁检测”。

    <a id="auditing-screenshot"></a> ![导航窗格][1]
3. 如果想要设置服务器审核策略（将应用于此服务器上的所有现有数据库和新建数据库），可单击数据库审核边栏选项卡中的“查看服务器设置”链接，然后，可从此上下文查看或修改服务器审核设置。

    ![导航窗格][2]
4. 如果想要在数据库级别启用 Blob 审核（在服务器级别以外或取代服务器级别），请将“审核”设置为“打开”，并为“审核类型”选择“Blob”。

   > 如果启用了服务器 Blob 审核，数据库配置的审核将与服务器 Blob 审核并存。  

    ![导航窗格][3]
5. 选择“存储详细信息”打开“审核日志存储”边栏选项卡。 选择日志要保存到的 Azure 存储帐户以及保留期（超过此期限的旧日志将被删除），然后单击底部的“确定”。 **提示：**为所有审核的数据库使用相同的存储帐户，以便充分利用审核报告模板。

    <a id="storage-screenshot"></a> ![导航窗格][4]
6. 若要自定义审核的事件，可以使用 PowerShell 或 REST API - 有关更多详细信息，请参阅[自动化 (PowerShell/REST API)](#subheading-7) 部分。
7. 配置审核设置后，可以打开新的**威胁检测**功能，并配置电子邮件用于接收安全警报。 使用威胁检测可以接收针对异常数据库活动（可能表示潜在的安全威胁）发出的前瞻性警报。 有关更多详细信息，请参阅[威胁检测入门](sql-database-threat-detection-get-started.md)。
8. 单击“保存” 。

## <a id="subheading-3"></a>分析审核日志和报告
审核日志将在安装期间选择的 Azure 存储帐户中进行聚合。

可以使用 [Azure 存储资源管理器](http://storageexplorer.com/)等工具浏览审核日志。

Blob 审核日志以 Blob 文件集合的形式保存在名为“**sqldbauditlogs**”的容器中。

有关 Blob 审核日志存储文件夹层次结构、blob 命名约定和日志格式的详细信息，请参阅 [Blob 审核日志格式参考（doc 文件下载）](https://go.microsoft.com/fwlink/?linkid=829599)。

可通过多种方法查看 Blob 审核日志：

1. 通过 [Azure 门户](https://portal.azure.cn)打开相关数据库。 在数据库的“审核和威胁检测”边栏选项卡的顶部，单击“查看审核日志”。

    ![导航窗格][7]

    此时将打开“审核记录”边栏选项卡，可在其中查看日志。

    - 可以通过单击“审核记录”边栏选项卡顶部区域中的“筛选”，选择查看特定的日期
    - 可以在服务器策略审核或数据库策略审核创建的审核记录之间切换

    ![导航窗格][8]

2. 使用系统函数 sys.fn_get_audit_file (T-SQL) 可以表格格式返回审核日志数据。 有关使用此函数的详细信息，请参阅 [sys.fn_get_audit_file 文档](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql)。

3. 使用 SSMS 中的“合并审核文件”功能（从 SSMS 17 开始）：  
    - 在 SSMS 顶部菜单中，单击“文件” --> “打开” --> “合并审核文件...”

        ![导航窗格][9]
    - 此时将打开一个对话框窗口，请单击“添加...”
    - 在后续页面中，选择是合并来自本地磁盘的审核文件还是从 Azure 存储导入（需要提供 Azure 存储详细信息和帐户密钥）。

        ![导航窗格][10]
    - 添加要合并的所有文件后，单击“确定”完成合并操作。
    - 合并的文件将在 SSMS 中打开，可在其中进行查看和分析，以及将其导出为 XEL/CSV 文件或表格。

4. 我们已创建一个**同步应用程序**，它在 Azure 中运行，并利用 OMS Log Analytics 公共 API 将 SQL 审核日志推送到 OMS Log Analytics，以便于通过 OMS Log Analytics 仪表板使用（[此处提供了详细信息](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration)）。

5. Power BI - 可在 Power BI 中查看和分析审核日志数据（[此处提供了更多信息和可下载的模板](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/)）

6. 通过门户或使用 [Azure 存储资源管理器](http://storageexplorer.com/)等工具从 Azure 存储 Blob 容器下载日志文件。

    在本地下载日志文件后，可以双击该文件将它打开，然后在 SSMS 中查看和分析日志。

    还可以通过 Azure 存储资源管理器**同时下载多个文件** - 右键单击特定的子文件夹（例如包含特定日期生成的所有日志文件的子文件夹），然后选择“另存为”，将文件保存到本地文件夹中。

    其他方法：

   * 下载多个文件后（或者一整天的日志文件，如上所述），可按前文的 SSMS“合并审核文件”说明中所述，在本地将它们合并。

   * 以编程方式：

     * 扩展事件读取器 **C# 库**（[详细信息](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/)）
     * 使用 **PowerShell** 查询扩展事件文件（[详细信息](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/)）

## <a id="subheading-5"></a>生产环境中的用法实践
<!--The description in this section refers to screen captures above.-->

### <a id="subheading-6">审核异地复制的数据库</a>
使用异地复制的数据库时，可以根据审核类型，针对主数据库和/或辅助数据库设置审核。

按照以下说明操作：

1. **主数据库** - 根据[为数据库设置审核](#subheading-2-1)部分中所述，在服务器或数据库本身上打开“Blob 审核”。
2. **辅助数据库** - 仅可以通过主数据库审核设置打开/关闭 Blob 审核。

   * 根据[为数据库设置审核](#subheading-2-1)部分中所述，对“主数据库”打开“Blob 审核”。 必须在*主数据库本身*上启用 Blob 审核，而不要在服务器上启用。
   * 在主数据库上启用 Blob 审核后，也会在辅助数据库上启用 Blob 审核。

    > [!IMPORTANT]
    > 默认情况下，辅助数据库的**存储设置**与主数据库相同，因而会导致生成**跨区域流量**。 在**辅助服务器**上启用 Blob 审核并在辅助服务器存储设置中配置**本地存储**可以避免此问题（这会覆盖辅助数据库的存储位置，导致每个数据库将审核日志保存到本地存储中）。  

<br>

### <a id="subheading-6">重新生成存储密钥</a>
在生产环境中，可能会定期刷新存储密钥。 刷新密钥时，需要重新保存审核策略。 过程如下：

1. 在存储详细信息边栏选项卡中，将“存储访问密钥”从“主要”切换为“辅助”，然后单击底部的“确定”。 然后，单击审核配置边栏选项卡顶部的“保存”。

    ![导航窗格][5]
2. 转到存储配置边栏选项卡，并 **重新生成***主访问密钥*。

    ![导航窗格][6]
3. 返回到审核配置边栏选项卡，将“存储访问密钥”从“辅助”切换为“主要”，然后单击底部的“确定”。 然后，单击审核配置边栏选项卡顶部的“保存”。
4. 返回到存储配置边栏选项卡并**重新生成***辅助访问密钥*（为下一个密钥刷新周期做好准备）。

## <a id="subheading-7"></a>自动化 (PowerShell/REST API)
也可以使用以下自动化工具在 Azure SQL 数据库中配置审核：

1. **PowerShell cmdlet**

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Remove-AzureRMSqlDatabaseAuditing][103]
   * [Remove-AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Use-AzureRMSqlServerAuditingPolicy][107]

   有关脚本示例，请参阅[使用 PowerShell 配置审核和威胁检测](scripts/sql-database-auditing-and-threat-detection-powershell.md)。

2. **REST API - Blob 审核**

   * [创建或更新数据库 Blob 审核策略](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [创建或更新服务器 Blob 审核策略](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [获取数据库 Blob 审核策略](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [获取服务器 Blob 审核策略](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [获取服务器 Blob 审核操作结果](https://msdn.microsoft.com/library/azure/mt771862.aspx)

<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)  

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: https://docs.microsoft.com/powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: https://docs.microsoft.com/powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy
