---
title: 在 Azure 自动化中执行 Runbook
description: 详细介绍如何处理 Azure 自动化中的 Runbook。
services: automation
ms.service: automation
ms.component: process-automation
author: WenJason
ms.author: v-jay
origin.date: 10/08/2018
ms.date: 11/05/2018
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: 88b20f6a66a80b50675786c807c38b4f760f91fd
ms.sourcegitcommit: d26e5d0d625a61d6b130800d10c81f47c83fb1e0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "50745511"
---
# <a name="runbook-execution-in-azure-automation"></a>在 Azure 自动化中执行 Runbook

在 Azure 自动化中启动 Runbook 时，会创建一个作业。 作业是 Runbook 的单一执行实例。 分配一个 Azure 自动化工作线程来运行每个作业。 尽管辅助角色由多个 Azure 帐户共享，但不同自动化帐户中的作业是相互独立的。 无法控制要由哪个辅助角色为作业请求提供服务。 一个 runbook 可以同时运行多个作业。 可以重用同一自动化帐户中的作业的执行环境。 在 Azure 门户中查看 Runbook 列表时，列表中会列出为每个 Runbook 启动的所有作业的状态。 可以查看每个 runbook 的作业列表以跟踪每个作业的状态。 有关不同作业状态的说明，请参阅[作业状态](#job-statuses)。

下图显示[图形 Runbook](automation-runbook-types.md#graphical-runbooks) 和 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks) 的 Runbook 作业生命周期。

![作业状态 - PowerShell 工作流](./media/automation-runbook-execution/job-statuses.png)

下图显示 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) 的 Runbook 作业生命周期。

![作业状态 - PowerShell 脚本](./media/automation-runbook-execution/job-statuses-script.png)

作业可以通过与 Azure 订阅建立连接来访问 Azure 资源。 仅当数据中心内的资源可从公有云访问时，作业才能访问这些资源。

## <a name="job-statuses"></a>作业状态

下表描述了作业的各种可能状态。 PowerShell 具有两种类型的错误，终止性和非终止性错误。 终止性错误在发生时将 runbook 状态设置为“失败”。 非终止性错误允许在发生错误时继续运行脚本。 非终止错误的示例为：对不存在的路径使用 `Get-ChildItem` cmdlet。 PowerShell 发现路径不存在，然后引发错误，并继续转到下一文件夹。 此错误不会将 runbook 状态设置为“失败”，并且可能标记为“完成”。 若要强制 runbook 在发生非终止性错误时停止，可以使用 `-ErrorAction Stop` cmdlet。

| 状态 | 说明 |
|:--- |:--- |
| 已完成 |作业已成功完成。 |
| 已失败 |对于[图形 Runbook 和 PowerShell 工作流 Runbook](automation-runbook-types.md)，Runbook 未能编译。 对于 [PowerShell 脚本 runbook](automation-runbook-types.md)，runbook 未能启动或作业遇到异常。 |
| 失败，正在等待资源 |作业失败，因为它已达到 [公平份额](#fair-share) 限制三次，并且每次都已从同一个检查点或 Runbook 开始处启动。 |
| 已排队 |作业正在等待提供自动化工作线程的资源，以便能够启动。 |
| 正在启动 |作业已分配给辅助角色，并且系统正在将它启动。 |
| 正在恢复 |系统正在恢复已暂停的作业。 |
| 正在运行 |作业正在运行。 |
| 正在运行，正在等待资源 |作业已卸载，因为它已达到 [公平份额](#fair-share) 限制。 片刻之后，它会从其上一个检查点恢复。 |
| 已停止 |作业在完成之前已被用户停止。 |
| 正在停止 |系统正在停止作业。 |
| 已挂起 |作业已被用户、系统或 Runbook 中的命令暂停。 如果 runbook 没有检查点，它将从 runbook 的开端启动。 如果它有检查点，它将重新启动并从其上一个检查点继续。 只有发生异常时，系统才会将 Runbook 挂起。 默认情况下，ErrorActionPreference 设置为 **Continue**，表示出错时作业会保持运行。 如果此首选项变量设置为 **Stop**，则出错时作业会挂起。 仅适用于[图形 Runbook 和 PowerShell 工作流 Runbook](automation-runbook-types.md)。 |
| 正在暂停 |系统正在尝试按用户请求暂停作业。 Runbook 只有在达到其下一个检查点后才能挂起。 如果 Runbook 越过了最后一个检查点，则只有在完成后才能挂起。 仅适用于[图形 Runbook 和 PowerShell 工作流 Runbook](automation-runbook-types.md)。 |

## <a name="viewing-job-status-from-the-azure-portal"></a>从 Azure 门户查看作业状态

可以查看所有 runbook 作业的摘要状态，或深入到 Azure 门户中特定 runbook 作业的详细信息。

### <a name="automation-runbook-jobs-summary"></a>自动化 Runbook 作业摘要

在所选的“自动化帐户”右侧，可在“作业统计信息”磁贴下看到所有 Runbook 作业的摘要。

![作业统计信息磁贴](./media/automation-runbook-execution/automation-account-job-status-summary.png)

此磁贴显示执行的所有作业的作业状态计数和图形表示形式。

单击可显示“作业”页的磁贴，此页包括所有已执行作业的摘要列表。 此页显示状态、开始时间和完成时间。

![自动化帐户作业页](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

可以通过选择“筛选作业”来筛选作业列表并筛选特定的 runbook 和作业状态，或从下拉列表中的时间范围中进行搜索。

![筛选作业状态](./media/automation-runbook-execution/automation-account-jobs-filter.png)

或者，可通过从自动化帐户中的“runbook”页上选择特定的 runbook，并选择“作业”磁贴，来查看该 runbook 的作业摘要详情。 此操作将显示“作业”页，在此可以单击作业记录，查看作业详细信息和输出。

![自动化帐户作业页](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>作业摘要

可以查看为特定 Runbook 创建的所有作业的列表及其最新状态。 可以按作业状态以及上次对作业进行更改的日期范围筛选此列表。 若要查看作业的详细信息和输出，请单击其名称。 作业的详细视图包括提供给该作业的 Runbook 参数值。

可以使用以下步骤查看 Runbook 的作业。

1. 在 Azure 门户中，选择“自动化”，然后选择自动化帐户的名称。
2. 从中心选择“Runbook”，然后在“Runbook”页的列表中选择一个 Runbook。
3. 在所选 runbook 的页上，单击“作业”磁贴。
4. 单击列表中的一个作业，然后可在 runbook 作业详细信息页上查看其详细信息和输出。

## <a name="retrieving-job-status-using-windows-powershell"></a>使用 Windows PowerShell 检索作业状态

可以使用 [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) 检索为 Runbook 创建的作业和特定作业的详细信息。 如果在 Windows PowerShell 中使用 [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) 启动 Runbook，则会返回生成的作业。 使用 [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) 可以获取作业的输出。

以下示例命令检索示例 Runbook 的最后一个作业，并显示其状态、为 Runbook 参数提供的值以及作业的输出。

```azurepowershell
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

以下示例检索特定作业的输出并返回每个记录。 如果某个记录存在异常，则会写出该异常而不是记录的值。 此行为非常有用，因为异常可以提供在输出过程中无法正常记录的其他信息。

```azurepowershell
$output = Get-AzureRmAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzureRmAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="get-details-from-activity-log"></a>从活动日志中获取详细信息

可以从自动化帐户的活动日志中检索其他详细信息，例如启动了 Runbook 的人员或帐户。 以下 PowerShell 示例提供运行相关 Runbook 的最后一个用户：

```powershell
$SubID = "00000000-0000-0000-0000-000000000000"
$rg = "ResourceGroup01"
$AutomationAccount = "MyAutomationAccount"
$RunbookName = "Test-Runbook"
$JobResourceID = "/subscriptions/$subid/resourcegroups/$rg/providers/Microsoft.Automation/automationAccounts/$AutomationAccount/jobs"

Get-AzureRmLog -ResourceId $JobResourceID -MaxRecord 1 | Select Caller
```

## <a name="fair-share"></a>公平共享

为了在云中的所有 Runbook 之间共享资源，Azure 自动化在任何作业运行三小时后都会将其暂时卸载。 在此期间，[基于 PowerShell 的 Runbook](automation-runbook-types.md#powershell-runbooks) 的作业都将停止且不会重新启动。 作业状态显示“已停止”。 此类型的 Runbook 始终从头开始重新启动，因为它们不支持检查点。

[基于 PowerShell 工作流的 Runbook](automation-runbook-types.md#powershell-workflow-runbooks) 会从最后一个[检查点](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints)进行恢复。 运行三小时后，Runbook 作业会由服务挂起，且其状态显示为“正在运行等待资源”。 沙盒变得可用时，Runbook 将通过自动化服务自动重新启动，并从最后一个检查点进行恢复。 此行为是实现挂起/重启的正常 PowerShell 工作流行为。 如果 Runbook 再次超过三小时的运行时，将重复该过程，最多三次。 在第三次重新启动后，如果 Runbook 仍未在三小时内完成，则 Runbook 作业将失败，且作业状态显示为“失败，正在等待资源”。 在此情况下，会收到以下异常和失败。

*该作业无法继续运行，因为它已反复被系统从同一个检查点逐出。请确保 Runbook 在未保持其状态的情况下没有执行冗长的操作。*

此行为是为了防止服务出现 Runbook 无限期运行而无法完成的情况，因为在不重新卸载的情况下，Runbook 无法到达下一个检查点。

如果该 Runbook 没有检查点或者作业在卸载之前尚未达到第一个检查点，则会从开始处重启。

如果在 Azure 上使用 PowerShell 工作流 Runbook，则在创建 Runbook 时，应确保在两个检查点之间运行任何活动的时间不超过三小时。 可能需要向 Runbook 添加检查点以确保它不会达到此三小时限制，或者需要将长时间运行的操作分解。 例如，Runbook 可能会对大型 SQL 数据库运行重新编制索引操作。 如果这一项操作未在公平份额限制内完成，则作业会卸载并从开始处重启。 在此情况下，应将重新编制索引操作拆分成多个步骤（例如，一次重新编制一个表的索引），然后在每项操作的后面插入一个检查点，使作业能够在上次操作后恢复并得以完成。

## <a name="next-steps"></a>后续步骤

* 若要详细了解可用于在 Azure 自动化中启动 Runbook 的不同方法，请参阅[在 Azure 自动化中启动 Runbook](automation-starting-a-runbook.md)

<!--Update_Description: wording update-->