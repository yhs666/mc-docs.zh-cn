---
title: "有关 Azure Service Fabric 的常见问题 | Azure"
description: "有关 Service Fabric 的常见问题及其回答"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 08/18/2017
ms.date: 03/12/2018
ms.author: v-yeche
ms.openlocfilehash: 81a1e68e861052f3a1fab728f0042ad2ccf3e71d
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="commonly-asked-service-fabric-questions"></a>Service Fabric 常见问题

有许多关于 Service Fabric 可以做些什么以及应该如何使用的常见问题。 本文档介绍其中许多常见问题及其答案。

## <a name="cluster-setup-and-management"></a>群集设置和管理

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>是否可以创建跨越多个 Azure 区域或自己的数据中心的群集？

是的。 

核心 Service Fabric 群集技术可用于将世界各地运行的计算机集合到一起，前提是它们相互之间已建立网络连接。 然而，生成并运行这样的群集可能很复杂。

如果你对此方案有兴趣，建议你通过 [Service Fabric Github 问题列表](https://github.com/azure/service-fabric-issues)或通过支持代表联系我们以获取其他指南。 Service Fabric 团队正在努力针对此方案提供其他解释、指南和建议。 

应考虑的一些事项： 

1. 如同构建群集的虚拟机规模集一样，Azure 中的 Service Fabric 群集资源如今也是区域性的。 这意味着如果出现区域性故障，将可能无法通过 Azure 资源管理器或 Azure 门户管理群集。 即使群集仍在运行，且你能够直接与其交互，也可能发生此情况。 此外，Azure 目前不提供创建跨区域可用的单个虚拟网络的功能。 这意味着 Azure 中的多区域群集需要[适用于 VM 规模集中的每个 VM 的公共 IP 地址](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine)或 [Azure VPN 网关](../vpn-gateway/vpn-gateway-about-vpngateways.md)。 这些网络选择对成本、性能以及某种程度上的应用程序设计都有不同的影响，因此需要在选择此类环境前仔细分析和规划。
2. 维护、管理和监视这些计算机可能会变得很复杂，尤其是在跨多种类型环境时，比如不同云提供程序之间或本地资源和 Azure 之间。 必须格外小心，确保在此类环境中运行生产工作负荷前，已了解群集和应用程序的升级、监视、管理和诊断。 如果你已有在 Azure 或自己的数据中心解决这些问题的经验，则很可能这些相同的解决方案在生成或运行 Service Fabric 群集时均适用。 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Service Fabric 节点是否会自动接收操作系统更新？

目前不是，但这也是一个常见的请求，Azure 有意推出此功能。

在此期间，我们已[提供一个应用程序](service-fabric-patch-orchestration-application.md)，Service Fabric 节点下的操作系统不断修补并保持最新状态。

操作系统更新的挑战在于，它们通常需要重启计算机，而这会导致暂时丢失可用性。 就其自身而言，这不是问题，因为 Service Fabric 会自动将这些服务的流量重定向到其他节点。 但是，如果不在群集间协调操作系统更新，可能会有多个节点同时停机。 此类同时重启可能导致某个服务完全失去可用性，或至少让特定分区（对于有状态服务）失去可用性。

将来，我们计划支持在更新域之间完全自动化的、经过协调的 OS 更新策略，确保即使重新启动或发生其他意外故障，也仍能保持可用性。

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>是否可以在我的 SF 群集中使用大型虚拟机规模集？ 

**简短解答** - 否。 

**详细解答** - 尽管通过大型虚拟机规模集可将虚拟机规模集扩展至多达 1000 个 VM 实例，但这是通过使用放置组 (PG) 实现的。 容错域 (FD) 和升级域 (UD) 仅在使用 FD 和 UD 来为服务副本/服务实例做出放置决策的放置组 Service Fabric 中保持一致。 因为 FD 和 UD 仅在放置组中可比较，因此 SF 无法使用它。 例如，如果 PG1 中的 VM1 具有一个 FD=0 的拓扑，并且 PG2 中的 VM9 具有一个 FD=4 的拓扑，这并不意味着 VM1 和 VM2 在两个不同的硬件机架上，因此在这种情况下 SF 无法使用 FD 值做出放置决策。

当前，大型虚拟机规模集还存在其他问题，例如缺少 level-4 负载均衡支持。 请参考[有关大型规模集的详细信息](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)

### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Service Fabric 群集的最小大小如何？ 为什么不能更小？

运行生产工作负荷的 Service Fabric 群集支持的最小大小是五个节点。 对于开发/测试方案，我们支持三节点群集。

存在这些最小值的原因在于，Service Fabric 群集运行一组有状态系统服务，其中包括命名服务和故障转移管理器。 这些服务跟踪哪些服务已部署到群集及其当前的托管位置，取决于非常一致性。 而这种非常一致性又取决于能否获取*仲裁*来更新这些服务的状态，其中，仲裁表示给定服务在严格意义上的大多数副本 (N/2 + 1)。

在了解这种背景的前提下，让我们探讨一些可能的群集配置：

**单节点**：此选项无法提供高可用性，因为出于任何原因丢失单个节点就意味着丢失整个群集。

**双节点**：跨两个节点 (N = 2) 部署的服务的仲裁为 2 (2/2 + 1 = 2)。 丢失单个副本时，无法创建仲裁。 由于执行服务升级要求暂时关闭副本，因此这不是一个有用的配置。

**三节点**：包含三个节点 (N = 3)，创建仲裁仍然要求使用两个节点 (3/2 + 1 = 2)。 这意味着，可以丢失单个节点，在这种情况下，仍可保留仲裁。

开发/测试支持三节点群集配置，因为只要不同时发生，就可以安全地执行升级以及从单独的节点故障中恢复。 对于生产工作负荷，必须具有应对此类同时发生的故障的复原能力，因此需要五个节点。

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>是否可以在夜间/周末关闭群集以节约成本？

一般而言，不可以。 Service Fabric 在本地、临时磁盘上存储状态，这意味着如果虚拟机移到其他主机，数据不会随之移动。 在正常操作中，这是没有问题的，因为其他节点可使新节点保持最新状态。 但是，如果停止所有节点并重启，很可能发生的情况是，大部分节点在新主机上启动，导致系统无法恢复。

如果在部署应用程序之前想要创建群集来测试应用程序，我们建议将这些群集动态创建为[持续集成/持续部署管道](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)的一部分。

### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>如何升级操作系统（例如从 Windows Server 2012 升级到 Windows Server 2016）？

我们致力于改善体验，但现在升级由你负责。 必须升级群集虚拟机上的 OS 映像，一次升级一个 VM。 

### <a name="can-i-encrypt-attached-data-disks-in-a-cluster-node-type-virtual-machine-scale-set"></a>是否可以对群集节点类型（虚拟机规模集）中的附加数据磁盘进行加密？
是的。  有关详细信息，请参阅[创建具有附加数据磁盘的群集](../virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks.md#create-a-service-fabric-cluster-with-attached-data-disks)、[加密磁盘 (PowerShell)](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md) 和[加密磁盘 (CLI)](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-cli.md)。

## <a name="container-support"></a>容器支持

### <a name="why-are-my-containers-that-are-deployed-to-sf-unable-to-resolve-dns-addresses"></a>为什么部署到 SF 的容器无法解析 DNS 地址？

已在 5.6.204.9494 版本的群集上报告了此问题 

**缓解措施**：按[此文档](service-fabric-dnsservice.md)中的说明在群集中启用 DNS Service Fabric 服务。

**修复**：升级到支持的群集版本（版本可用时），该版本需高于 5.6.204.9494。 如果群集设置为自动升级，则群集将自动升级到已解决此问题的版本。

## <a name="application-design"></a>应用程序设计

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>跨可靠集合的分区查询数据的最佳方法是什么？

Reliable Collections 通常已[分区](service-fabric-concepts-partitioning.md)以支持扩展并提高性能和吞吐量。 这意味着，给定服务的状态可以分散在数十甚至数百台计算机上。 若要对这个完整的数据集执行操作，有以下几个选项：

- 创建查询其他服务的所有分区的服务，拉入所需的数据。
- 创建可从其他服务的所有分区接收数据的服务。
- 定期将每个服务中的数据推送到外部存储。 此方法仅适用于要执行的查询不属于核心业务逻辑的情况。

### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>跨执行组件查询数据的最佳方法是什么？

执行组件设计为独立的状态和计算单元，因此，不建议在运行时对执行组件状态执行广泛查询。 如果需要跨执行组件状态的完整集进行查询，应考虑以下方法之一：

- 将执行组件服务替换为有状态 Reliable Services，使得从一定数目的执行组件中收集所有数据的网络请求数等于服务中的分区数。
- 将执行组件设计为定期将其状态推送到外部存储，以方便查询。 如上所述，此方法仅在运行时行为不需要所执行的查询时才可行。

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>可以在可靠集合中存储多少数据？

Reliable Services 通常已分区，因此，存储量仅受限于群集中的计算机数量以及这些计算机的可用内存量。

例如，某个服务中的 Reliable Collection 包含 100 个分区和 3 个副本，存储平均大小为 1 KB 的对象。 现在假设群集中有 10 台计算机，每台计算机的内存为 16 GB。 我们做一个简单且保守的假设：操作系统和系统服务、Service Fabric 运行时以及服务消耗了 6GB 内存，也就是说，每个计算机还剩余 10GB 内存，整个群集还有 100GB 内存。

请记住，每个对象必须存储三次（一个主要副本和两个次要副本），满负荷运行时，集合可提供足够的内存来存储约 3.5 亿个对象。 但是，我们建议能够弹性应对故障域和升级域的同时丢失（它们会占用大约 1/3 的容量），并将对象数目减少到大约 2300 万个。

请注意，此计算还假设：

- 跨分区的数据分布大致是均匀的，或者可向群集资源管理器报告负载指标。 默认情况下，Service Fabric 会根据副本计数执行负载均衡。 在前面的示例中，群集中的每个节点上会放置 10 个主副本和 20 个辅助副本。 这也适用于跨分区均匀分布的负载。 如果负载不均衡，必须报告负载，使 Resource Manager 能够将较小副本打包在一起，增加较大副本在单个节点上占用的内存。

- 讨论的可靠服务是群集中存储状态的唯一服务。 由于可将多个服务部署到群集，因此你需要知道每个服务运行时以及管理其状态时所需的资源。

- 群集本身不会变大或缩小。 如果添加更多计算机，Service Fabric 会重新平衡副本以利用新增容量，直到计算机数量超过服务中的分区数量，因为单独的副本不能跨计算机。 相比之下，如果通过删除计算机来减小群集大小，副本将更加严密地打包，减少总体容量消耗。

### <a name="how-much-data-can-i-store-in-an-actor"></a>可以在执行组件中存储多少数据？

和 Reliable Services 一样，可以在执行组件服务中存储的数据量仅受限于群集中各个节点的总磁盘空间和可用内存。 但是，单独的执行组件在用于封装少量状态和关联的业务逻辑时效率最高。 一般而言，单独的执行组件应具有以千字节为单位的状态。

## <a name="other-questions"></a>其他问题

### <a name="how-does-service-fabric-relate-to-containers"></a>Service Fabric 如何与容器关联？

容器提供打包服务及其依赖项的简单方法，以便它们能够在所有环境中一致地运行并且可在单台计算机上以隔离方式运行。 使用 Service Fabric 可以部署和管理服务，包括[在容器中打包的服务](service-fabric-containers-overview.md)。

### <a name="are-you-planning-to-open-source-service-fabric"></a>你们是否打算开放 Service Fabric 源代码？

我们打算在 GitHub 上开放 Reliable Services 和 Reliable Actors 框架的源代码并接受社区对这些项目的投稿。 有关已发布的更多详细信息，请参阅 [Service Fabric 博客](https://blogs.msdn.microsoft.com/azureservicefabric/)。

当前不打算开放 Service Fabric 运行时的源代码。

## <a name="next-steps"></a>后续步骤

- [了解核心 Service Fabric 概念和最佳做法](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)

<!--Update_Description: update meta properties, wording update, update link -->