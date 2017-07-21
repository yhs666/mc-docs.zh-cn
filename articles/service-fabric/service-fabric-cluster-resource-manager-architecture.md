---
title: "Resource Manager 体系结构 | Azure"
description: "Service Fabric 群集 Resource Manager 的体系结构概述。"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 01/05/2017
ms.date: 02/20/2017
ms.author: v-johch
ms.openlocfilehash: c362e085d3c0336c134e7049e77ff3b45b496465
ms.sourcegitcommit: 86616434c782424b2a592eed97fa89711a2a091c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="cluster-resource-manager-architecture-overview"></a>群集 Resource Manager 体系结构概述
若要管理群集中的资源，Service Fabric 群集 Resource Manager 必须拥有一些相关的信息。 它必须了解目前存在哪些服务，以及这些服务正在使用的资源的当前（默认）数量。 若要跟踪群集中的可用资源，它必须了解群集中的节点容量和每个节点使用的资源量。 给定服务的资源消耗量会随时间变化，服务通常会关注多种类型的资源。 在不同的服务之间，可能会同时测量实际物理资源和物理资源。 服务可能会跟踪内存和磁盘使用情况等物理指标。 更常见的是，服务可能会关注逻辑指标，例如“WorkQueueDepth”或“TotalRequests”。 逻辑和实际指标可能跨许多不同类型的服务使用，或可能仅特定对几项服务使用。

## <a name="other-considerations"></a>其他注意事项
群集的所有者和操作者偶尔可能不是服务作者，或至少是身兼数职的同一人员。 例如，开发服务时，会了解资源需求方面的一些事项，以及应该如何妥善部署不同的组件。 但是，在生产环境中，处理该服务现场情况的人员有其他工作要完成并需要不同的工具。 此外，群集和服务都不是静态配置的：

* 群集中的节点数可能增加也可能减少
* 不同大小和类型的节点可能加入也可能离开
* 服务可以创建、删除和更改其所需的资源分配
* 升级或其他管理操作可在群集中运行，而且操作可能随时失败。

## <a name="cluster-resource-manager-components-and-data-flow"></a>群集 Resource Manager 组件和数据流
群集 Resource Manager 必须跟踪每个服务的要求，以及构成这些服务的每个服务对象的资源使用情况。 为了实现此目的，群集 Resource Manager 提供两个概念组件：在每个节点上运行的代理和一个容错服务。 每个节点上的代理会跟踪服务的负载报告、聚合这些报告，并定期汇报。 群集 Resource Manager 服务会聚合来自本地代理的所有信息，并根据当前配置做出反应。

请查看下图：

<center>
![资源均衡器体系结构][Image1]
</center>

运行时阶段可能会发生很多更改。 例如，假设某些服务使用的资源量更改、某些服务失败以及某些节点加入并离开群集。 节点上的所有更改都要进行汇总，并定期发送到群集 Resource Manager 服务（1、2），并在其中再次聚合、分析和存储。 每隔几秒钟，服务就查看更改，并确定是否需要任何操作 (3)。 例如，它可能注意到某些空节点已添加到群集，并决定要将某些服务移到这些节点。 群集 Resource Manager 可能还注意到特定节点已超载，或者某些服务已失败（或删除），在其他位置释放资源。

请查看下图，了解接下来会发生什么。 假设群集 Resource Manager 确定需要更改。 它与其他系统服务（尤其是故障转移管理器）进行协调，进行必要的更改。 然后将所需命令发送到相应节点 (4)。 在此情况下，我们假设 Resource Manager 注意到节点 5 已重载，因此确定要将服务 B 从 N5 移到 N4。 重新配置 (5) 结束时，群集看起来像这样：

<center>
![资源均衡器体系结构][Image2]
</center>

## <a name="next-steps"></a>后续步骤
- 群集 Resource Manager 提供许多用于描述群集的选项。 若要详细了解这些选项，请查看这篇[描述 Service Fabric 群集](./service-fabric-cluster-resource-manager-cluster-description.md)的文章

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png