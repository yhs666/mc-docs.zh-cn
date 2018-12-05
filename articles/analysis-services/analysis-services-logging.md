---
title: Azure Analysis Services 诊断日志记录 | Azure
description: 了解如何设置 Azure Analysis Services 诊断日志记录。
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 06/15/2018
ms.date: ''
ms.author: v-junlch
ms.reviewer: minewiskan
ms.openlocfilehash: 36d7c50674ba4c46275151ddd989090f74aac027
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52664528"
---
<!--Notice: Verify successfully-->
# <a name="setup-diagnostic-logging"></a>设置诊断日志记录

监视服务器性能对于任何 Analysis Services 解决方案都至关重要。 通过 [Azure 资源诊断日志](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)，可监视日志并将其发送到 [Azure 存储](https://www.azure.cn/home/features/storage/)，将其流式传输到 [Azure 事件中心](https://www.azure.cn/home/features/event-hubs/)。
<!-- Not Available on [Log Analytics](https://www.azure.cn/home/features/log-analytics/)-->
<!-- Not Available on [Azure](https://www.microsoft.com/cloud-platform/operations-management-suite)-->

![存储、事件中心或 Log Analytics 的诊断日志记录](./media/analysis-services-logging/aas-logging-overview.png)

## <a name="whats-logged"></a>会记录哪些内容？

可选择“引擎”“服务”和“指标”类别。

### <a name="engine"></a>引擎

选择“引擎”将记录所有 [xEvents](https://docs.microsoft.com/sql/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events)。 不能选择单个事件。 

|XEvent 类别 |事件名称  |
|---------|---------|
|安全审核    |   审核登录      |
|安全审核    |   审核注销      |
|安全审核    |   审核服务器启动和停止      |
|进度报告     |   进度报告开始      |
|进度报告     |   进度报告结束      |
|进度报告     |   进度报告进行中      |
|查询     |  查询开始       |
|查询     |   查询结束      |
|命令     |  命令开始       |
|命令     |  命令结束       |
|错误和警告     |   错误      |
|发现     |   发现结束      |
|通知     |    通知     |
|会话     |  会话初始化       |
|锁    |  死锁       |
|查询处理     |   VertiPaq SE 查询开始      |
|查询处理     |   VertiPaq SE 查询结束      |
|查询处理     |   VertiPaq SE 查询缓存匹配      |
|查询处理     |   直接查询开始      |
|查询处理     |  直接查询结束       |

### <a name="service"></a>服务

|操作名称  |发生条件  |
|---------|---------|
|ResumeServer     |    恢复服务器     |
|SuspendServer    |   暂停服务器      |
|DeleteServer     |    删除服务器     |
|RestartServer    |     用户通过 SSMS 或 PowerShell 重启服务器    |
|GetServerLogFiles    |    用户通过 PowerShell 导出服务器日志     |
|ExportModel     |   用户通过在 Visual Studio 中使用 Open 在门户中导出模型     |

### <a name="all-metrics"></a>所有指标

“指标”类别会记录“指标”中显示的[服务器指标](analysis-services-monitor.md#server-metrics)。

## <a name="setup-diagnostics-logging"></a>设置诊断日志记录

### <a name="azure-portal"></a>Azure 门户

1. 在 [Azure 门户](https://portal.azure.cn)中找到服务器，单击左侧导航栏中的“诊断日志”，然后单击“启用诊断”。

    ![在 Azure 门户中启用 Azure Cosmos DB 的诊断日志记录](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. 在“诊断设置”中，指定以下选项： 

    * **Name**。 为要创建的日志输入名称。

    * **存档到存储帐户**。 要使用此选项，需要一个可连接到的现有存储帐户。 请参阅[创建存储帐户](../storage/common/storage-create-storage-account.md)。 按照说明创建一个资源管理器常规用途帐户，然后返回到门户中的此页面来选择存储帐户。 新创建的存储帐户可能几分钟后才会显示在下拉菜单中。
    * **流式传输到事件中心**。 要使用此选项，需要一个可连接到的现有事件中心命名空间和事件中心。 若要了解详细信息，请参阅[使用 Azure 门户创建事件中心命名空间和事件中心](../event-hubs/event-hubs-create.md)。 然后在门户中返回到此页，选择事件中心命名空间和策略名称。
    <!--Not Available on * **Send to Log Analytics** -->

    * **引擎** 选择此选项以记录 Xevent。 若要存档到存储帐户，可以选择诊断日志的保留期。 保留期到期后自动删除日期。
    * **服务**。 选择此选项以记录服务级别事件。 若要存档到存储帐户，可以选择诊断日志的保留期。 保留期到期后自动删除日期。
    * **指标** 选择此选项可在[指标](analysis-services-monitor.md#server-metrics)中存储详细数据。 若要存档到存储帐户，可以选择诊断日志的保留期。 保留期到期后自动删除日期。

3. 单击“保存” 。

    <!-- Not Available on [Troubleshoot Azure Diagnostics](/log-analytics/log-analytics-azure-storage)--> 若要在将来的任意时间点更改诊断日志的保存方式，可以返回此页修改设置。

### <a name="powershell"></a>PowerShell

以下基本命令可以帮助你开始使用。 如需有关使用 PowerShell 为存储帐户设置日志记录的逐步指导，请参阅本文稍后部分的教程。

若要使用 PowerShell 启用指标和诊断日志记录，请使用以下命令：

- 若要允许在存储帐户中存储诊断日志，请使用以下命令：

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   存储帐户 ID 是需要向其发送日志的存储帐户的资源 ID。

- 要允许将诊断日志流式传输到事件中心，请使用以下命令：

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure 服务总线规则 ID 是以下格式的字符串：

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 
   
<!-- Not Available on Log Analytics -->

可以结合这些参数启用多个输出选项。

<!-- Not Available on ### REST API-->

<!-- URL is not exist on [change diagnostics settings by using the Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx)-->

### <a name="resource-manager-template"></a>Resource Manager 模板

了解如何[在创建资源时使用资源管理器模板启用诊断设置](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md)。 

## <a name="manage-your-logs"></a>管理日志

通常在设置日志记录数小时后可查看日志。 存储帐户中的日志完全由你管理：

* 请使用标准的 Azure 访问控制方法限制可访问日志的人员，以此保护日志。
* 删除不想继续保留在存储帐户中的日志。
* 请务必设置保留期，以便从存储帐户中删除旧日志。

<!-- Not Available on ## View logs in Log Analytics -->
## <a name="tutorial---turn-on-logging-by-using-powershell"></a>教程 - 使用 PowerShell 启用日志记录
在此快速教程中，你将在 Analysis Services 服务器所在订阅和资源组中创建存储帐户。 然后通过 Set-AzureRmDiagnosticSetting 启用诊断日志记录，将输出发送到新的存储帐户。

### <a name="prerequisites"></a>先决条件
要完成本教程，必须备好以下资源：

* 现有 Azure Analysis Services 服务器。 有关创建服务器资源的说明，请参阅[在 Azure 门户中创建服务器](analysis-services-create-server.md)或[使用 PowerShell 创建 Azure Analysis Services 服务器](analysis-services-create-powershell.md)。

### <a name="aconnect-to-your-subscriptions"></a></a>连接到订阅

启动 Azure PowerShell 会话，并使用以下命令登录用户的 Azure 帐户：  

```powershell
Connect-AzureRmAccount -Environment AzureChinaCloud
```

在弹出的浏览器窗口中，输入 Azure 帐户用户名和密码。 Azure PowerShell 获取与此帐户关联的所有订阅，并按默认使用第一个订阅。

如果有多个订阅，可能需要指定用来创建 Azure 密钥保管库的特定订阅。 键入以下命令查看帐户的订阅：

```powershell
Get-AzureRmSubscription
```

然后，键入以下命令，以指定与要记录的 Azure Analysis Services 帐户关联的订阅：

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> 如果帐户关联了多个订阅，请务必指定该订阅。
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>为日志创建新的存储帐户

如果服务器所属订阅中存在现有存储帐户，可使用该帐户进行日志记录。 对本教程而言，需创建专用于 Analysis Services 日志的新存储帐户。 为方便起见，将存储帐户详细信息将存储到名为 sa 的变量中。

还要使用包含 Analysis Services 服务器的资源组。 将 `awsales_resgroup`、`awsaleslogs` 和 `China North` 的值替换为自己的值：

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'China North'
```

### <a name="identify-the-server-account-for-your-logs"></a>标识用户记录日志的服务器帐户

将帐户名称设置为名为“account”的变量，其中 ResourceName 是帐户的名称。

```powershell
$account = Get-AzureRmResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>启用日志记录

要启用日志记录，请使用 Set-AzureRmDiagnosticSetting cmdlet 并结合新存储帐户、服务器帐户和类别的对应变量。 运行以下命令，将“-Enabled”标志设置为“$true”：

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

输出应类似于：

```powershell
StorageAccountId            : 
/subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourceGroups/awsales_resgroup/providers/Microsoft.Storage/storageAccounts/awsaleslogs
ServiceBusRuleId            :
EventHubAuthorizationRuleId :
Metrics                    
    TimeGrain       : PT1M
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0

Logs                       
    Category        : Engine
    Enabled         : True
    RetentionPolicy
    Enabled : False
    Days    : 0

    Category        : Service
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0

WorkspaceId                 :
Id                          : /subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourcegroups/awsales_resgroup/providers/microsoft.analysisservic
es/servers/awsales/providers/microsoft.insights/diagnosticSettings/service
Name                        : service
Type                        :
Location                    :
Tags                        :
```

此输出确认已为服务器启用日志记录，会将信息保存到存储帐户。

还可为日志设置保留策略，以便自动删除较旧的日志。 例如，使用 **-RetentionEnabled** 标志将保留期策略设置为 **$true**，并将 **-RetentionInDays** 参数设置为 **90**。 将自动删除超过 90 天的日志。

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
 -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>后续步骤

深入了解 [Azure 资源诊断日志记录](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

请参阅 PowerShell 帮助中的 [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/Set-AzureRmDiagnosticSetting)
<!-- Update_Description: new articles on analysis service logging -->
<!--ms.date: 07/16/2018-->