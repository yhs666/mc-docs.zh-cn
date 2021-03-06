---
title: 扩展或缩减 Service Fabric 群集 | Azure
description: 通过为每个节点类型/虚拟机规模集设置自动缩放规则，根据需要扩展或缩减 Service Fabric 群集。 在 Service Fabric 群集中添加或删除节点
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/22/2017
ms.date: 08/07/2017
ms.author: v-yeche
ms.openlocfilehash: 3497219e6fb789d8c80246c41c4f48a2eeac0fec
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52657165"
---
# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>使用自动缩放规则扩展可缩减 Service Fabric 群集
虚拟机规模集是一种 Azure 计算资源，可用于将一组虚拟机作为一个集进行部署和管理。 在 Service Fabric 群集中定义的每个节点类型将设置为不同的虚拟机规模集。 然后，每个节点类型可以独立缩减或扩展、打开不同的端口集，并可以有不同的容量指标。 可在 [Service Fabric nodetypes](service-fabric-cluster-nodetypes.md) 文档中了解有关详细信息。 由于群集中的 Service Fabric 节点类型由后端的虚拟机规模集构成，因此需要为每个节点类型/虚拟机规模集设置自动缩放规则。

> [!NOTE]
> 订阅必须有足够的核心用于添加构成此群集的新 VM。 当前没有模型验证，因此如果达到任何配额限制，会遇到部署时故障。
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>选择要缩放的节点类型/虚拟机规模集
当前无法使用门户为虚拟机规模集指定自动缩放规则，因此我们来使用 Azure PowerShell (1.0+) 列出节点类型，并向它们添加自动缩放规则。

若要获取构成群集的虚拟机规模集的列表，请运行以下 cmdlet：

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

<!-- Not Available ## Set auto-scale rules for the node type/Virtual Machine scale set -->

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>手动将 VM 添加到节点类型/虚拟机规模集
根据 [快速入门模板库](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) 中的示例/说明更改每个 Nodetype 的 VM 数目。 

> [!NOTE]
> 添加 VM 需要时间，请不要期待马上就有结果。 因此请妥善规划增加容量的时间，在副本/服务实例可以使用 VM 容量之前有超过 10 分钟的时间让一切就绪。
> 
> 

## <a name="manually-remove-vms-from-the-primary-node-typevirtual-machine-scale-set"></a>手动从主节点类型/虚拟机规模集中删除 VM
> [!NOTE]
> Service Fabric 系统服务在群集中以主节点类型运行。 因此请不要关闭该节点类型的实例，或者将该节点类型的实例数目缩减到少于可靠性层所需的数目。 在 [此处](service-fabric-cluster-capacity.md)了解有关可靠性层的详细信息。 
> 
> 

需以一次一个 VM 实例的形式执行以下步骤。 这可让系统服务（以及有状态服务）在要删除的 VM 实例上正常关闭，并在其他节点上创建新副本。

1. 运行 [Disable-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) 加上“RemoveNode”可禁用要删除的节点（该节点类型的最高实例）。
2. 运行 [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) 可确保节点确实转换为禁用。 如果没有，请等到节点已禁用。 此步骤不能一蹴而就。
3. 根据 [快速入门模板库](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) 中的示例/说明逐个更改 Nodetype 的 VM 数目。 删除的实例是最高 VM 实例。 
4. 根据需要重复步骤 1 到 3，但切勿将主节点类型的实例数目缩减到少于可靠性层所需的数目。 在[此处](service-fabric-cluster-capacity.md)了解有关可靠性层的详细信息。 

## <a name="manually-remove-vms-from-the-non-primary-node-typevirtual-machine-scale-set"></a>手动从非主节点类型/虚拟机规模集中删除 VM
> [!NOTE]
> 对于有状态服务，需要一些始终启动的节点来保持可用性，以及保持服务的状态。 至少需要与分区/服务的目标副本集计数相等的节点数目。 
> 
> 

必须以一次一个 VM 实例的形式执行以下步骤。 这可让系统服务（以及有状态服务）在要删除的 VM 实例上正常关闭，并在其他位置创建新副本。

1. 运行 [Disable-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) 加上“RemoveNode”可禁用要删除的节点（该节点类型的最高实例）。
2. 运行 [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) 可确保节点确实转换为禁用。 如果没有，请等到节点禁用。 此步骤不能一蹴而就。
3. 根据 [快速入门模板库](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) 中的示例/说明逐个更改 Nodetype 的 VM 数目。 现在，删除最高 VM 实例。 
4. 根据需要重复步骤 1 到 3，但切勿将主节点类型的实例数目缩减到少于可靠性层所需的数目。 在[此处](service-fabric-cluster-capacity.md)了解有关可靠性层的详细信息。

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>可能会在 Service Fabric Explorer 中观察到的行为
扩展群集时，Service Fabric Explorer 会反映属于群集一部分的节点（虚拟机规模集实例）数。  但是在缩减群集时，会看到已删除的节点/VM 实例显示为不正常状态，除非使用相应节点名称来调用 [Remove-ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx)。   

下面是此行为的说明。

Service Fabric Explorer 中列出的节点是 Service Fabric 系统服务（特别是 FM）在群集具有的节点数方面所了解的信息的反映。 缩减虚拟机规模集时，VM 已删除，但是 FM 系统服务仍然认为节点（映射到已删除的 VM）会恢复。 因此 Service Fabric Explorer 会继续显示该节点（尽管运行状况状态可能是错误或未知）。

若要确保在删除 VM 时删除节点，有两个选项：

1) 为群集中的节点类型选择金级或银级（即将推出）持续性级别，这会提供基础结构集成。 这随后会在进行减少时自动从我们的系统服务 (FM) 状态中删除节点。
在[此处](service-fabric-cluster-capacity.md)了解有关持续性级别的详细信息

2) 减少 VM 实例之后，需要调用 [Remove-ServiceFabricNodeState cmdlet](https://msdn.microsoft.com/library/mt125993.aspx)。

> [!NOTE]
> Service Fabric 群集需要有一定数量的节点可随时启动，以保持可用性和状态 - 称为“维持仲裁”。 因此，除非已事先执行[状态的完整备份](service-fabric-reliable-services-backup-restore.md)，否则关闭群集中的所有计算机通常是不安全的做法。
> 
> 

## <a name="next-steps"></a>后续步骤
参阅以下文章以另外了解如何规划群集容量、升级群集以及对服务进行分区：

* [规划群集容量](service-fabric-cluster-capacity.md)
* [群集升级](service-fabric-cluster-upgrade.md)
* [为有状态服务分区以最大程度地实现缩放](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
<!-- Not Exist File [ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png -->

<!--Update_Description: new articles on service fabric cluster manually scale -->