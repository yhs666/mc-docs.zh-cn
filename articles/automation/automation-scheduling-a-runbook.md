---
title: "在 Azure 自动化中计划 Runbook | Azure"
description: "介绍如何在 Azure 自动化中创建一个计划，以便在特定的时间或按重复计划自动启动 Runbook。"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 710979ff-99d8-41e4-ac6d-6bf26b8ea654
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 12/09/2016
ms.date: 01/25/2017
ms.author: v-dazen
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: 6ed598278287c8569b320bf25b307fa0ab67a03a
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>在 Azure 自动化中计划 Runbook
若要将 Azure 自动化中的 Runbook 计划为在指定的时间启动，可以将它链接到一个或多个计划。 可以在 Azure 经典管理门户中将计划配置为运行一次或按每小时或每天计划重复运行，此外，还可以将它们计划为每周、每月、每周或每月的特定几天，或者每月的某一天运行。  可将一个 Runbook 链接到多个计划，一个计划可以链接多个 Runbook。

## <a name="creating-a-schedule"></a>创建计划
可以使用经典管理门户或 Windows PowerShell 为 Runbook 创建新计划。 也可以在使用 Azure 经典管理门户将 Runbook 链接到计划时，选择创建新计划。

> [!NOTE]
> 将计划与 Runbook 相关联时，自动化会将模块的当前版本存储在你的帐户中，并将模块链接到该计划。  这意味着，如果在创建计划时你的帐户中有 1.0 版的模块，然后将模块更新到版本 2.0，该计划将继续使用 1.0 版。  若要使用更新后的模块版本，必须创建新计划。 
> 
> 

### <a name="to-create-a-new-schedule-in-the-azure-classic-management-portal"></a>在 Azure 经典管理门户中创建新计划
1. 在 Azure 经典管理门户中，选择“自动化”，然后选择自动化帐户的名称。
2. 选择“资产”选项卡  。
3. 在窗口底部，单击“添加设置” 。
4. 单击“添加计划”。
5. 为新计划键入“名称”和（可选）“说明”。计划将运行“一次”，或者“每小时”、“每日”、“每周”或“每月”运行。
6. 指定“开始时间”，并根据选择的计划类型指定其他选项  。

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>使用 Windows PowerShell 创建新计划
可以使用 [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet 在 Azure 自动化中为经典 Runbook 创建新计划。 必须指定计划的开始时间以及它应运行的频率。

以下示例命令演示了如何使用 Azure 服务管理 cmdlet 创建一个从 2015 年 1 月 20 日开始在每天下午 3:30 运行的新计划。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule -AutomationAccountName $automationAccountName -Name `
    $scheduleName -StartTime "1/20/2016 15:30:00" -DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>将计划链接到 Runbook
可将一个 Runbook 链接到多个计划，一个计划可以链接多个 Runbook。 如果 Runbook 包含参数，则你可以提供这些参数的值。 必须为所有必需参数提供值，并可以为任何可选参数提供值。  此计划每次启动 Runbook 时，将使用这些值。  你可以将同一个 Runbook 附加到另一个计划，并指定不同的参数值。

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-management-portal"></a>使用 Azure 经典管理门户将计划链接到 Runbook
1. 在 Azure 经典管理门户中，选择“自动化”  ，然后单击自动化帐户的名称。
2. 选择“Runbook”选项卡  。
3. 单击要计划的 Runbook 的名称。
4. 单击“计划”选项卡  。
5. 如果 Runbook 当前未链接到计划，系统会提供“链接到新计划”或“链接到现有计划”选项。  如果 Runbook 当前已链接到计划，请单击窗口底部的“链接”来访问这些选项  。
6. 如果 Runbook 包含参数，系统将提示你输入其值。  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>使用 Windows PowerShell 将计划链接到 Runbook
可以使用 [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) 将计划链接到 Runbook。  可以使用 Parameters 参数为 Runbook 的参数指定值。 有关指定参数值的详细信息，请参阅[在 Azure 自动化中启动 Runbook](automation-starting-a-runbook.md)。

以下示例命令演示了如何使用带参数的 Azure 服务管理 cmdlet 链接计划。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ScheduleName $scheduleName -Parameters $params

## <a name="disabling-a-schedule"></a>禁用计划
禁用某个计划后，链接到该计划的所有 Runbook 将不再按该计划运行。 可以手动禁用计划，也可以在创建带频率的计划时设置其过期时间。 到达过期时间时，将会禁用该计划。

### <a name="to-disable-a-schedule-from-the-azure-classic-management-portal"></a>从 Azure 经典管理门户禁用计划
可以在 Azure 经典管理门户中通过计划的“计划详细信息”页禁用该计划。

1. 在 Azure 经典管理门户中，选择“自动化”，然后单击自动化帐户的名称。
2. 选择“资产”选项卡。
3. 单击某个计划的名称以打开该计划的详细信息页。
4. 将“已启用”更改为“否”。

### <a name="to-disable-a-schedule-with-windows-powershell"></a>使用 Windows PowerShell 禁用计划
可以使用 [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet 更改 Runbook 现有计划的属性。 若要禁用计划，请将“IsEnabled”参数指定为“false”。

以下示例命令演示了如何使用 Azure 服务管理 cmdlet 禁用计划。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule -AutomationAccountName $automationAccountName `
    -Name $scheduleName -IsEnabled $false

## <a name="next-steps"></a>后续步骤
* 若要了解有关使用计划的详细信息，请参阅[在 Azure 自动化中计划资产](/automation/automation-schedules)
* 若要在 Azure 自动化中开始使用 Runbook，请参阅[在 Azure 自动化中启动 Runbook](automation-starting-a-runbook.md)