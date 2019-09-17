---
title: 将 Azure 诊断日志流式传输到 Azure Monitor 中的 Log Analytics 工作区
description: 了解如何将 Azure 诊断日志流式传输到 Azure Monitor 中的 Log Analytics 工作区。
author: lingliw
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
origin.date: 04/18/2019
ms.date: 05/18/2019
ms.author: v-lingwu
ms.subservice: logs
ms.openlocfilehash: 7aded2a9b89f0cc18e097f2f837fec2dff4ea9b1
ms.sourcegitcommit: dd0ff08835dd3f8db3cc55301815ad69ff472b13
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70736881"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics-workspace-in-azure-monitor"></a>将 Azure 诊断日志流式传输到 Azure Monitor 中的 Log Analytics 工作区

可以使用门户、PowerShell cmdlet 或 Azure CLI 将 **[Azure 诊断日志](diagnostic-logs-overview.md)** 近实时地流式传输到 Azure Monitor 中的 Log Analytics 工作区。

## <a name="what-you-can-do-with-diagnostics-logs-in-a-log-analytics-workspace"></a>可对 Log Analytics 工作区中的诊断日志执行的操作

Azure Monitor 提供灵活的日志查询和分析工具让你深入了解从 Azure 资源生成的原始日志数据。 其中一些功能包括：

* **日志查询** - 对日志数据编写高级查询、关联来自各种源的日志，并生成可以固定到 Azure 仪表板的图表。
* **警报** - 当检测到一个或多个事件与特定查询匹配时，使用 Azure Monitor 警报通过电子邮件或 Webhook 调用接收通知。
* **高级分析** - 应用机器学习和模式匹配算法，确定日志揭示的可能问题。

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics-workspace"></a>启用向 Log Analytics 工作区流式传输诊断日志

可以通过门户或使用 [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings) 以编程方式启用诊断日志的流式处理。 无论采用哪种方式，都可以创建一个诊断设置并在其中指定 Log Analytics 工作区，以及要发送到该工作区的日志类别和指标。 诊断日志类别  是一类可由资源提供的日志。

只要配置设置的用户同时拥有两个订阅的相应 RBAC 访问权限，Log Analytics 工作区就不必位于发出日志的资源所在的订阅中。

> [!NOTE]
> 当前不支持通过诊断设置发送多维指标。 多维指标将按平展后的单维指标导出，并跨维值聚合。
>
> 例如：  可以基于每个队列级别浏览和绘制事件中心上的“传入消息”指标。 但是，当通过诊断设置导出时，该指标将表示为事件中心的所有队列中的所有传入消息。
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>使用门户流式传输诊断日志
1. 在门户中，导航到“Azure Monitor”并单击“设置”菜单中的“诊断设置”   。


2. （可选）按资源组或资源类型筛选列表，并单击要为其设置诊断设置的资源。

3. 如果选定的资源上不存在任何设置，系统会提示创建设置。 单击“启用诊断”。

   ![添加诊断设置 - 没有现有的设置](media/diagnostic-logs-stream-log-store/diagnostic-settings-none.png)

   如果资源上有现有的设置，则会看到已在此资源上配置的设置列表。 单击“添加诊断设置”。

   ![添加诊断设置 - 现有的设置](media/diagnostic-logs-stream-log-store/diagnostic-settings-multiple.png)

3. 为设置提供名称，并选中“发送到 Log Analytics”  框，然后选择 Log Analytics 工作区。

   ![添加诊断设置 - 现有的设置](media/diagnostic-logs-stream-log-store/diagnostic-settings-configure.png)

4. 单击“保存”  。

几分钟后，新设置会显示在此资源的设置列表中，只要生成新的事件数据，就会立即将诊断日志流式传输到该工作区。 发出事件后可能需要最多 15 分钟的时间该事件才会出现在 Log Analytics 中。

### <a name="via-powershell-cmdlets"></a>通过 PowerShell Cmdlet

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

若要通过 [Azure PowerShell Cmdlet](../../azure-monitor/platform/powershell-quickstart-samples.md) 启用流式传输，可以使用 `Set-AzDiagnosticSetting` cmdlet 并设置以下参数：

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

请注意，workspaceID 属性采用工作区的完整 Azure 资源 ID，而不是 Log Analytics 门户中显示的工作区 ID/密钥。

### <a name="via-azure-cli"></a>通过 Azure CLI

若要通过 [Azure CLI](../../azure-monitor/platform/cli-samples.md) 启用流式传输，可以使用 [az monitor diagnostic-settings create](https://docs.azure.cn/zh-cn/cli/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) 命令。

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

可以通过将字典添加到以 `--logs` 参数传递的 JSON 数组来将其他类别添加到诊断日志。

仅当 `--workspace` 不是对象 ID 时，才需要 `--resource-group` 参数。

## <a name="how-do-i-query-the-data-from-a-log-analytics-workspace"></a>如何查询 Log Analytics 工作区中的数据？

在 Azure Monitor 门户上的“日志”边栏选项卡中，可以在“AzureDiagnostics”表下面查询作为日志管理解决方案的一部分提供的诊断日志。 此外，还可以安装[适用于 Azure 资源的多个监视解决方案](../../azure-monitor/insights/solutions.md)，以立即深入了解发送到 Azure Monitor 的日志数据。

### <a name="examples"></a>示例

```Kusto
// Resources that collect diagnostic logs into this Log Analytics workspace, using Diagnostic Settings
AzureDiagnostics
| distinct _ResourceId
```
```Kusto
// Resource providers collecting diagnostic logs into this Log Analytics worksapce, with log volume per category
AzureDiagnostics
| summarize count() by ResourceProvider, Category
```
```Kusto
// Resource types collecting diagnostic logs into this Log Analytics workspace, with number of resources onboarded
AzureDiagnostics
| summarize ResourcesOnboarded=dcount(_ResourceId) by ResourceType
```
```Kusto
// Operations logged by specific resource provider, in this example - KeyVault
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.KEYVAULT"
| distinct OperationName
```

## <a name="azure-diagnostics-vs-resource-specific"></a>“Azure 诊断”与“特定于资源”  
在 Azure 诊断配置中启用 Log Analytics 目标后，将以两种不同的方法在工作区中显示数据：  
- **Azure 诊断** - 这是目前大多数 Azure 服务使用的传统方法。 在此模式下，来自任何诊断设置的、指向给定工作区的数据最终会出现在 _AzureDiagnostics_ 表中。 
<br><br>由于许多资源将数据发送到同一个表 (_AzureDiagnostics_)，因此，此表的架构是所要收集的所有不同数据类型的架构的超集。 例如，如果你创建了以下诊断设置用于收集以下数据类型，而所有这些数据类型都会发送到同一个工作区：
    - 资源 1 的审核日志（架构由列 A、B 和 C 组成）  
    - 资源 2 的错误日志（架构由列 D、E 和 F 组成）  
    - 资源 3 的数据流日志（架构由列 G、H 和 I 组成）  

    则包含示例数据的 AzureDiagnostics 表的外观如下所示：  

    | ResourceProvider | Category | A | B | C | D | E | F | G | H | I |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
    | Microsoft.Resource2 | ErrorLogs | | | | q1 | w1 | e1 |
    | Microsoft.Resource3 | DataFlowLogs | | | | | | | j1 | k1 | l1|
    | Microsoft.Resource2 | ErrorLogs | | | | q2 | w2 | e2 |
    | Microsoft.Resource3 | DataFlowLogs | | | | | | | j3 | k3 | l3|
    | Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
    | ... |

- **特定于资源** - 在此模式下，将会根据在“诊断设置”配置中选择的每个类别，在所选工作区中创建各个表。 使用这种新方法可以通过显式关注点分离更轻松地找到确切所需的内容：每个类别的表。 此外，它对动态类型的支持也能带来便利。 某些 Azure 资源类型已经采用了这种模式，例如 [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) 或 [Intune](https://docs.microsoft.com/intune/review-logs-using-azure-monitor) 日志。 我们预计，每种数据类型最终都会迁移到“特定于资源”模式。 

    以上示例会创建三个表： 
    - 如下所示的表 _AuditLogs_：

        | ResourceProvider | Category | A | B | C |
        | -- | -- | -- | -- | -- |
        | Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
        | Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
        | ... |

    - 如下所示的表 _ErrorLogs_：  

        | ResourceProvider | Category | D | E | F |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource2 | ErrorLogs | q1 | w1 | e1 |
        | Microsoft.Resource2 | ErrorLogs | q2 | w2 | e2 |
        | ... |

    - 如下所示的表 _DataFlowLogs_：  

        | ResourceProvider | Category | G | H | I |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource3 | DataFlowLogs | j1 | k1 | l1|
        | Microsoft.Resource3 | DataFlowLogs | j3 | k3 | l3|
        | ... |

    使用“特定于资源”模式的其他好处包括，改善引入延迟和查询时间方面的性能、更好地发现架构及其结构、可以授予针对特定表的 RBAC 权限，等等。

### <a name="selecting-azure-diagnostic-vs-resource-specific-mode"></a>选择“Azure 诊断”模式还是“特定于资源”模式
对于大多数 Azure 资源，你无法选择是要使用“Azure 诊断”模式还是“特定于资源”模式；数据会自动通过资源选择使用的方法传送。 有关当前正在使用的模式的详细信息，请参阅用于将数据发送到 Log Analytics 的资源所提供的文档。 

如前一部分中所述，Azure monitor 的最终目标是让 Azure 中的所有服务都使用“特定于资源”模式。 为了简化这种迁移并确保在迁移过程中数据不会丢失，某些 Azure 服务在加入 Log Analytics 时会让你选择模式：  
   ![诊断设置模式选择器](media/diagnostic-logs-stream-log-store/diagnostic-settings-mode-selector.png)

我们**强烈**建议对所有新建的诊断设置使用“以资源为中心”模式，以免迁移过程变得复杂。  

特定的 Azure 资源启用现有诊断设置后，你可以回溯方式从“Azure 诊断”模式切换到“特定于资源”模式。 以前引入的数据将继续在 _AzureDiagnostics_ 表中提供，直到它们按照工作区中配置的保留期设置过期，但所有新数据将发送到专用表。 这意味着，对于必须涵盖旧数据（尚未过期）和新数据的任何查询，需要在查询中使用一个 [union](https://docs.microsoft.com/azure/kusto/query/unionoperator) 运算符来组合两个数据集。

请在 [Azure 更新](https://www.azure.cn/zh-cn/what-is-new/)博客中，查看有关支持“特定于资源”模式日志的 Azure 服务的最新信息！

### <a name="known-limitation-column-limit-in-azurediagnostics"></a>已知的限制：AzureDiagnostics 中的列限制
包含的列数不超过 500 个的任意给定 Azure 日志表具有明确的限制。 一旦达到该限制，在引入时，包含不属于前 500 个列的数据的行将被删除。 AzureDiagnostics 表特别容易受到此限制的影响。 原因往往是需要将多种不同的数据源发送到同一个工作区，或者要将多个详细数据源发送到同一个工作区。 

#### <a name="azure-data-factory"></a>Azure 数据工厂  
众所周知，Azure 数据工厂就是一个特别容易受此限制影响的资源，因为它是一个非常详细的日志集。 具体而言，对于在启用“特定于资源”模式之前配置的诊断设置，或者出于反向兼容的原因显式选择使用“特定于资源”模式的诊断设置：  
- *针对管道中的任一活动定义的用户参数*：将会针对任一活动，为每个唯一命名的用户参数创建一个新列。 
- *活动输入和输出*：这些活动各不相同且非常详细，因此会生成大量的列。 
 
除了下面提出的各种解决方法以外，我们还建议迁移日志，以尽快使用“特定于资源”模式。 如果无法立即做到这一点，一种临时的替代方案是将 ADF 日志隔离到其自身的工作区，以尽量减少这些日志影响工作区中收集的其他日志类型的可能性。 
 
#### <a name="workarounds"></a>解决方法
简短而言，在所有 Azure 服务支持“特定于资源”模式之前，对于尚不支持“特定于资源”模式的任何服务，我们建议将这些服务发布的详细数据类型隔离到独立的工作区，以减少达到该限制的可能性。  
 
从长远来看，Azure 诊断会逐步让所有 Azure 服务支持“特定于资源”模式。 我们建议尽快迁移到此模式，以减少 500 列限制造成影响的可能性。  


## <a name="next-steps"></a>后续步骤

* [详细了解 Azure 诊断日志](diagnostic-logs-overview.md)





