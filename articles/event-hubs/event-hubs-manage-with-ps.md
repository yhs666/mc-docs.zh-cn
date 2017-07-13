---
title: "使用 PowerShell 管理 Azure 事件中心资源 | Azure"
description: "使用 PowerShell 模块创建和管理事件中心"
services: event-hubs
documentationcenter: .NET
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date.date: 04/06/2017
ms.date: 07/03/2017
ms.author: v-yeche
ms.openlocfilehash: cb59f58317a3ba3f3ccffd240627bdc070b3f980
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 PowerShell 管理事件中心资源
<a id="use-powershell-to-manage-event-hubs-resources" class="xliff"></a>

Azure PowerShell 是一个脚本编写环境，可用于控制和自动执行 Azure 服务的部署和管理。 本文介绍如何使用本地 Azure PowerShell 控制台或脚本通过[事件中心 Resource Manager PowerShell 模块](https://docs.microsoft.com/powershell/module/azurerm.eventhub)预配和管理事件中心实体（命名空间、事件中心和使用者组）。

还可以使用 Azure Resource Manager 模板管理事件中心资源。 有关详细信息，请参阅文章[使用 Azure Resource Manager 模板创建包含事件中心和使用者组的事件中心命名空间](event-hubs-resource-manager-namespace-event-hub.md)。

## 先决条件
<a id="prerequisites" class="xliff"></a>

在开始之前，你需要具备以下项：

* Azure 订阅。 有关获取订阅的详细信息，请参阅[购买选项][purchase options]、[成员优惠][member offers]或[免费帐户][free account]。
* 配备 Azure PowerShell 的计算机。 有关说明，请参阅 [Azure PowerShell cmdlet 入门](https://docs.microsoft.com/powershell/azure/get-started-azureps)。
* 大致了解 PowerShell 脚本、NuGet 包和 .NET Framework。

## 入门
<a id="get-started" class="xliff"></a>

第一步是使用 PowerShell 登录 Azure 帐户和 Azure 订阅。 按照 [Azure PowerShell cmdlet 入门](https://docs.microsoft.com/powershell/azure/get-started-azureps)中的说明登录 Azure 帐户，检索并访问 Azure 订阅中的资源。

## 预配事件中心命名空间
<a id="provision-an-event-hubs-namespace" class="xliff"></a>

使用事件中心命名空间时，可以使用 [Get-AzureRmEventHubNamespace](https://docs.microsoft.com/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace)、[New-AzureRmEventHubNamespace](https://docs.microsoft.com/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace)、[Remove-AzureRmEventHubNamespace](https://docs.microsoft.com/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace) 和 [Set-AzureRmEventHubNamespace](https://docs.microsoft.com/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlet。

本示例在脚本中创建几个本地变量：`$Namespace` 和 `$Location`。

* `$Namespace` 是我们要使用的事件中心命名空间的名称。
* `$Location` 标识我们要在其中设置命名空间的数据中心。
* `$CurrentNamespace` 将存储我们检索（或创建）的引用命名空间。

在实际脚本中，`$Namespace` 和 `$Location` 可作为参数传递。

此部分脚本执行以下任务：

1. 尝试使用指定的名称检索事件中心命名空间。
2. 如果找到该命名空间，则报告它找到的内容。
3. 如果找不到该命名空间，则会创建该命名空间，然后检索新创建的命名空间。

    ```powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace

    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
    }
    ```

## 创建事件中心
<a id="create-an-event-hub" class="xliff"></a>

若要创建事件中心，请使用上一部分中的脚本执行命名空间检查。 然后，使用 New-[AzureRmEventHub](https://docs.microsoft.com/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet 创建事件中心：

```powershell
# Check if Event Hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "The Event Hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $EventHubName Event Hub does not exist."
    Write-Host "Creating the $EventHubName Event Hub in the $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $EventHubName Event Hub in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### 创建使用者组
<a id="create-a-consumer-group" class="xliff"></a>

若要在事件中心中创建使用者组，请使用上一部分中的脚本执行命名空间和事件中心检查。 然后，使用 [New-AzureRmEventHubConsumerGroup](https://docs.microsoft.com/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet 在事件中心中创建使用者组。 例如：

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "The consumer group $ConsumerGroupName in Event Hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating the $ConsumerGroupName consumer group in the $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $ConsumerGroupName consumer group in Event Hub $EventHubName in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

#### 设置用户元数据
<a id="set-user-metadata" class="xliff"></a>

执行上述部分中的脚本后，可以使用 [Set-AzureRmEventHubConsumerGroup](https://docs.microsoft.com/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet 更新使用者组的属性，如以下示例所示：

```powershell
# Set some user metadata on the CG
Write-Host "Setting the UserMetadata field to 'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## 删除事件中心
<a id="remove-event-hub" class="xliff"></a>

若要删除所创建的事件中心实体，可以使用 `Remove-*` cmdlet，如以下示例所示：

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## 后续步骤
<a id="next-steps" class="xliff"></a>

- 请参阅[此处](https://docs.microsoft.com/powershell/module/azurerm.eventhub)完整的事件中心 Resource Manager PowerShell 模块文档。 此页列出所有可用的 cmdlet。
- 有关使用 Azure Resource Manager 模板的信息，请参阅文章[使用 Azure Resource Manager 模板创建包含事件中心和使用者组的事件中心命名空间](event-hubs-resource-manager-namespace-event-hub.md)。
- 有关[事件中心 .NET 管理库](event-hubs-management-libraries.md)的信息。

[purchase options]: https://www.azure.cn/pricing/overview/
[member offers]: https://www.azure.cn/pricing/member-offers/
[free account]: https://www.azure.cn/pricing/1rmb-trial/