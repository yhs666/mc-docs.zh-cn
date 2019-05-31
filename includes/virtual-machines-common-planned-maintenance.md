---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 04/30/2019
ms.date: 05/20/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 09e24c8dc18b10e23d365847317b7023c62f8b3b
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004263"
---
Azure 定期更新平台，以提高虚拟机的主机基础结构的可靠性、性能及安全性。 此类更新包括修补宿主环境中的软件组件、升级网络组件以及硬件解除授权等多项内容。 大多数此类更新不影响托管的虚拟机。 但是，有时候更新会产生影响，这种情况下 Azure 会选择影响最小的方法进行更新：

- 如果可进行非重启性更新，则会在更新主机时暂停 VM，或者会将 VM 实时迁移到已更新的主机。

- 如果维护需重新启动，系统会告知计划维护的时间。 Azure 还会提供一个时间范围，方便你在合适的时间自行启动维护。 除非紧急执行维护，否则自我维护时限通常为四周。 Azure 也在投资相关技术，以减少进行计划内平台维护时必须重启 VM 的情况。 

本页介绍 Azure 如何执行上述两种类型的维护。 有关非计划事件（故障）的详细信息，请参阅管理适用于 [Windows](../articles/virtual-machines/windows/manage-availability.md) 或 [Linux](../articles/virtual-machines/linux/manage-availability.md) 的虚拟机的可用性。

使用适用于 [Windows](../articles/virtual-machines/windows/scheduled-events.md) 或 [Linux](../articles/virtual-machines/linux/scheduled-events.md) 的计划事件即可获得有关即将进行的维护的 VM 内通知。

有关管理计划维护的“操作说明”信息，请参阅 [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) 或 [Windows](../articles/virtual-machines/windows/maintenance-notifications.md) 的“处理计划维护通知”。

## <a name="maintenance-not-requiring-a-reboot"></a>不需要重新启动的维护

大多数不需要重新启动的、完全无影响的维护的目标是确保 VM 的暂停时间不到 10 秒。 Azure 会选择对客户 VM 影响最小的更新机制。 某些情况下会使用内存保留维护机制，VM 暂停时间会长达 30 秒，并且会在 RAM 中保留内存。 VM 随后会恢复，其时钟会自动同步。 Azure 逐渐趋向于使用实时迁移技术并改进内存保留维护机制，目的是缩短暂停时间。  

这些非重启型维护操作会一个容错域接着一个容错域地应用。如果收到任何警告性运行状况信号，则进度会停止。 

这些类型的更新可能会影响某些应用程序。 将 VM 实时迁移到另一主机时，某些敏感的工作负荷可能会注意到，在导致 VM 暂停的几分钟内性能会略微下降。 使用适用于 [Windows](../articles/virtual-machines/windows/scheduled-events.md) 或 [Linux](../articles/virtual-machines/linux/scheduled-events.md) 的计划事件进行 VM 维护准备对此类应用程序很有用，在 Azure 维护期间不会造成任何影响。 Azure 还会为此类超敏感的应用程序推出维护控制功能。 

## <a name="live-migration"></a>实时迁移

实时迁移是一个无需重新启动的操作，它可为 VM 预留内存，并限制限暂停或冻结时间（通常持续不超过 5 秒）。 目前，所有基础结构即服务 (IaaS) 虚拟机以及 G、M、N 和 H 系列都可以实时迁移。 也就是说，可部署到 Azure 机群的 90% 以上的 IaaS VM 都可以实时迁移。 

在以下情况下，Azure Fabric 会启动实时迁移：
- 计划内维护
- 硬件故障
- 分配优化

某些计划内维护方案利用实时迁移，可以使用计划事件来提前了解实时迁移操作何时开始。

还可以使用实时迁移从即将发生机器学习算法所检测到的预测故障的硬件中移出虚拟机，以及优化虚拟机分配。 若要详细了解可以检测降级硬件实例的预测性建模，请参阅标题为 [Improving Azure Virtual Machine resiliency with predictive ML and live migration](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure)（使用预测性机器学习和实时迁移改善 Azure 虚拟机的复原能力）的博客文章。 客户始终会在 Azure 门户的“监视”>“服务运行状况日志”中收到实时迁移通知，并可以通过计划事件（如果正在使用）接收通知。

## <a name="maintenance-requiring-a-reboot"></a>需要重新启动的维护

在极少数情况下，VM 需重启以进行计划内维护，这种情况下系统会提前通知。 计划内维护有两个阶段：自助式时段和计划维护时段。

可以使用**自助时段**在 VM 上启动维护。 在此时段（通常为四周）内，可以通过查询每个 VM 来了解其状态，并查看上次维护请求的结果。

启动自助维护时，VM 会重新部署到已更新的某个节点。 由于 VM 重新启动，临时磁盘会丢失，而与虚拟网络接口关联的动态 IP 地址会更新。

如果在启动自助式维护的过程中出错，系统会停止操作，不更新 VM，并让你选择是否重试自助维护。 

自助式维护时段过后，就会开始**计划维护时段**。 在这段时间内，仍可以查询维护时段，但不能自行启动维护。

有关管理需要重启的维护的信息，请参阅 [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) 或 [Windows](../articles/virtual-machines/windows/maintenance-notifications.md)的“处理计划维护通知”。 

### <a name="availability-considerations-during-scheduled-maintenance"></a>计划内维护期间的可用性注意事项 

如果决定一直等到计划性维护时段，则为了保持 VM 的最高可用性，需注意一些事项。 

#### <a name="paired-regions"></a>配对区域

每个 Azure 区域与同一地理位置中另一个区域配对，共同组成一个区域对。 在计划性维护阶段，Azure 只会更新一个区域对中单个区域的 VM。 例如，更新中国北部的 VM 时，Azure 不会同时更新中国东部的任何 VM。 

<!-- Not Available  Understanding how region pairs work can help you better distribute your VMs across regions. -->
<!-- Not Available on  For more information, see [Azure region pairs](/best-practices-availability-paired-regions) -->
#### <a name="availability-sets-and-scale-sets"></a>可用性集和规模集

在 Azure VM 上部署工作负荷时，可以在可用性集中创建 VM，向应用程序提供高可用性。 这样可确保在发生故障或重启性维护事件期间，至少有一个 VM 可用。

在可用性集中，各个 VM 可分布在最多 20 个更新域 (UD) 中。 在计划性维护期间，任意给定时间都只有一个更新域进行更新。 不一定按顺序来更新更新域。 

虚拟机规模集是一种 Azure 计算资源，支持将一组相同的 VM 作为单个资源进行部署和管理。 规模集自动跨更新域进行部署，此类更新域就像可用性集中的 VM 一样。 在计划性维护期间使用规模集时，就像使用可用性集一样，在任意给定的时间都只会更新单个更新域。

有关配置 VM 以实现高可用性的详细信息，请参阅“管理 [Windows](../articles/virtual-machines/windows/manage-availability.md) 或 [Linux](../articles/virtual-machines/linux/manage-availability.md) 虚拟机的可用性”。

<!--Update_Description: update meta properties, wording update, update link -->