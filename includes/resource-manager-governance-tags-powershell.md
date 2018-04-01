---
title: include 文件
description: include 文件
services: azure-resource-manager
author: rockboyfor
manager: digimobile
ms.service: azure-resource-manager
ms.topic: include
origin.date: 02/16/2018
ms.date: 03/26/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 1fa035d244add25de0cef3ead2e578507ff5a733
ms.sourcegitcommit: 6d7f98c83372c978ac4030d3935c9829d6415bf4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
若要为资源组添加两个标记，请使用 [Set-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/set-azurermresourcegroup) 命令：

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

让我们假设要添加第三个标记。 每次将标记应用到某个资源或资源组时，都会覆盖该资源或资源组中的现有标记。 若要添加新标记而不会丢失现有标记，必须检索现有标记、添加新标记，并重新应用标记集合：

```azurepowershell-interactive
# Get existing tags and add a new tag
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags += @{Project="Documentation"}

# Reapply the updated set of tags 
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

资源不从资源组继承标记。 目前，资源组有三个标记，但资源没有任何标记。 要将资源组中的所有标记应用于其资源，并且保留资源上不重复的现有标记，请使用以下脚本：

```azurepowershell-interactive
# Get the resource group
$group = Get-AzureRmResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = $group | Find-AzureRmResource

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags

        # Loop through each tag from the resource group
        foreach ($key in $group.Tags.Keys)
        {
            # Check if the resource already has a tag for the key from the resource group. If so, remove it from the resource
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }

        # Add the tags from the resource group to the resource tags
        $resourcetags += $group.Tags

        # Reapply the updated tags to the resource 
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

或者，可以将资源组中的标记应用于资源而不保留现有标记：

```azurepowershell-interactive
# Get the resource group
$g = Get-AzureRmResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

若要将几个值组合到单个标记中，请使用 JSON 字符串。

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

若要删除所有标记，请传递一个空哈希表。

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```
<!--ms.date: 03/26/2018-->