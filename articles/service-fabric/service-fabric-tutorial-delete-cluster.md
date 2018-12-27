---
title: 在 Azure 中删除 Service Fabric 群集 | Azure
description: 在本教程中，你将了解如何删除 Azure 托管的 Service Fabric 群集及其所有资源。 可以删除包含群集的资源组，也可以有选择地删除资源。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 09/26/2018
ms.date: 11/12/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 7fa5d68e2bae8bbe530c7a7e2df31fe94f144a70
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52650818"
---
# <a name="tutorial-remove-a-service-fabric-cluster-running-in-azure"></a>教程：删除在 Azure 中运行的 Service Fabric 群集

本教程是一个系列的第四部分，介绍了如何删除在 Azure 中运行的 Service Fabric 群集。 若要彻底删除 Service Fabric 群集，还需要删除该群集使用的所有资源。 可使用两种方法：删除该群集所在的资源组（此操作将删除群集资源以及该资源组中的所有其他资源），或特定删除群集资源及其关联资源（而不删除资源组中的其他资源）。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 删除资源组及其所有资源
> * 有选择地从资源组中删除资源

在此系列教程中，你将学习如何：
> [!div class="checklist"]
> * 使用模板在 Azure 上创建安全的 [Windows 群集](service-fabric-tutorial-create-vnet-and-windows-cluster.md)或 [Linux 群集](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
> * [缩小或扩大群集](service-fabric-tutorial-scale-cluster.md)
> * [升级群集的运行时](service-fabric-tutorial-upgrade-cluster.md)
> * 删除群集

## <a name="prerequisites"></a>先决条件

在开始学习本教程之前：

* 如果还没有 Azure 订阅，请创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)
* 安装 [Azure Powershell 模块版本 4.1 或更高版本](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)或者 [Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。
* 在 Azure 上创建安全的 [Windows 群集](service-fabric-tutorial-create-vnet-and-windows-cluster.md)或 [Linux 群集](service-fabric-tutorial-create-vnet-and-linux-cluster.md)

## <a name="delete-the-resource-group-containing-the-service-fabric-cluster"></a>删除包含 Service Fabric 群集的资源组
若要删除群集及其占用的所有资源，最简单的方式是删除资源组。

登录到 Azure，选择要删除的群集的订阅 ID。  可通过登录到 [Azure 门户](http://portal.azure.cn)查找订阅 ID。 使用[Remove-AzureRMResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet 或 [az group delete](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-delete) 命令删除资源组和所有群集资源。

```powershell
Connect-AzureRmAccount -Environment AzureChinaCloud
Set-AzureRmContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="selectively-delete-the-cluster-resource-and-the-associated-resources"></a>有选择地删除群集资源和关联的资源
如果资源组仅包含与要删除的 Service Fabric 群集相关的资源，那么删除整个资源组更方便。 如果希望有选择地删除资源组中的资源，并保留未与群集关联的资源，请执行以下步骤。

列出资源组中的资源：

```powershell
Connect-AzureRmAccount -Environment AzureChinaCloud
Set-AzureRmContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Get-AzureRmResource -ResourceGroupName $groupname | ft
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az resource list --resource-group $ResourceGroupName
```

对要删除的每项资源，运行以下脚本：

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "<Resource Type>" --resource-group $ResourceGroupName
```

若要删除群集资源，请运行以下脚本：

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "Microsoft.ServiceFabric/clusters" --resource-group $ResourceGroupName
```

## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 删除资源组及其所有资源
> * 有选择地从资源组中删除资源

现在你已完成了本教程，请尝试执行以下任务：
* 了解如何使用 [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) 检查和管理 Service Fabric 群集。
* 了解如何为群集节点[修补 Windows 操作系统](service-fabric-patch-orchestration-application.md)或[修补 Linux 操作系统](service-fabric-patch-orchestration-application-linux.md)。
* 了解如何为 [Windows 群集](service-fabric-diagnostics-event-aggregation-wad.md)或 [Linux 群集](service-fabric-diagnostics-event-aggregation-lad.md)聚合和收集事件以监视群集事件。

<!-- Not Available on [setup Log Analytics](service-fabric-diagnostics-oms-setup.md)-->

<!-- Update_Description: new articles on service fabric tutorial delete cluster -->
<!--ms.date: 11/12/2018-->