---
title: 将网络安全组与 Azure Site Recovery 配合使用 | Azure
description: 介绍如何将网络安全组与 Azure Site Recovery 配合使用来实现灾难恢复和迁移
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: conceptual
origin.date: 11/27/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: 4ac83f6a1545fa0486a7f21203d8b08b40c93857
ms.sourcegitcommit: 26957f1f0cd708f4c9e6f18890861c44eb3f8adf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "54363479"
---
# <a name="network-security-groups-with-azure-site-recovery"></a>将网络安全组与 Azure Site Recovery 配合使用

可以使用网络安全组限制发往虚拟网络中的资源的网络流量。 [网络安全组 (NSG)](../virtual-network/security-overview.md#network-security-groups) 包含一个安全规则列表，这些规则可根据源或目标 IP 地址、端口和协议允许或拒绝入站或出站网络流量。

可以在资源管理器部署模型下将 NSG 关联到子网或单个网络接口。 将 NSG 关联到子网时，规则适用于连接到该子网的所有资源。 也可将 NSG 关联到已经有关联 NSG 的子网中的单个网络接口，以便进一步限制流量。

本文介绍如何才能将网络安全组与 Azure Site Recovery 配合使用。

## <a name="using-network-security-groups"></a>使用网络安全组

单个子网可以有零个或一个关联的 NSG。 单个网络接口也可以有零个或一个关联的 NSG。 因此，可以先将一个 NSG 关联到子网，再将另一个 NSG 关联到 VM 的网络接口，对虚拟机进行有效的双重流量限制。 在这种情况下，NSG 规则的应用取决于流量的方向以及所应用安全规则的优先级。

考虑一个包含一个虚拟机的简单示例，如下所示：
-   虚拟机置于 **Contoso 子网**中。
-   **Contoso 子网**与**子网 NSG** 相关联。
-   另外，VM 网络接口又与 **VM NSG** 相关联。

![将 NSG 与 Site Recovery 配合使用](./media/concepts-network-security-group-with-site-recovery/site-recovery-with-network-security-group.png)

在此示例中，对于入站流量，会先评估子网 NSG。 然后由 VM NSG 评估允许通过子网 NSG 的任何流量。 对于出站流量，则可应用相反的顺序，先评估 VM NSG。 然后由子网 NSG 评估允许通过 VM NSG 的任何流量。

这样就可以细致地应用安全规则。 例如，可能需要允许以入站 Internet 访问的方式访问某个子网中的一些应用程序 VM（例如前端 VM），但需限制以入站 Internet 访问的方式访问其他 VM（例如数据库和其他后端 VM）。 在这种情况下，可以在子网 NSG 上设置较宽松的规则，允许 Internet 流量，但拒绝在 VM NSG 上进行访问，以限制对特定 VM 的访问。 这同样适用于出站流量。

设置此类 NSG 配置时，请确保对[安全规则](../virtual-network/security-overview.md#security-rules)应用正确的优先级。 规则按优先顺序进行处理。先处理编号较小的规则，因为编号越小，优先级越高。 一旦流量与某个规则匹配，处理即会停止。 因此，不会处理优先级较低（编号较大）的、其属性与高优先级规则相同的所有规则。

将网络安全组同时应用到网络接口和子网时，你不一定总能察觉得到。 可以通过查看网络接口的[有效安全规则](../virtual-network/virtual-network-network-interface.md#view-effective-security-rules)，验证已应用到网络接口的聚合规则。 还可以使用 [Azure 网络观察程序](../network-watcher/network-watcher-monitoring-overview.md)中的 [IP 流验证](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md)功能来确定是否允许发往或发自网络接口的通信。 该工具会告知通信是否受允许，以及哪个网络安全规则允许或拒绝流量。

## <a name="on-premises-to-azure-replication-with-nsg"></a>使用 NSG 进行的本地到 Azure 的复制

Azure Site Recovery 支持从本地 [Hyper-V 虚拟机](hyper-v-azure-architecture.md)、[VMware 虚拟机](vmware-azure-architecture.md)和[物理服务器](physical-azure-architecture.md)向 Azure 进行灾难恢复和迁移。 对于所有本地到 Azure 的方案，复制数据都发送到 Azure 存储帐户并存储在其中。 在复制期间，无需支付任何虚拟机费用。 故障转移到 Azure 时，Site Recovery 会自动创建 Azure IaaS 虚拟机。

一旦在故障转移到 Azure 后创建 VM，即可使用 NSG 限制发往虚拟网络和 VM 的网络流量。 Site Recovery 不在故障转移操作过程中创建 NSG。 建议在启动故障转移之前创建所需的 Azure NSG。 然后即可将自动化脚本与 Site Recovery 的强大[恢复计划](site-recovery-create-recovery-plans.md)配合使用，通过关联 NSG 在故障转移期间自动进行 VM 故障转移。

例如，如果故障转移后 VM 配置类似于上面详述的[示例方案](concepts-network-security-group-with-site-recovery.md#using-network-security-groups)，则可执行以下操作：
-   按照 DR 规划在目标 Azure 区域创建 **Contoso VNet** 和 **Contoso 子网**。
-   也可通过同一 DR 规划来创建和配置**子网 NSG** 和 **VM NSG**。
-   然后可以立即将**子网 NSG** 与 **Contoso 子网**相关联，因为 NSG 和子网均已可用。
-   可以在故障转移期间使用恢复计划将 **VM NSG** 与 VM 相关联。

创建并配置 NSG 后，建议运行[测试故障转移](site-recovery-test-failover-to-azure.md)来验证脚本化的 NSG 关联和故障转移后的 VM 连接。

## <a name="azure-to-azure-replication-with-nsg"></a>使用 NSG 进行从 Azure 到 Azure 的复制

Azure Site Recovery 支持对 [Azure 虚拟机](azure-to-azure-architecture.md)进行灾难恢复。 为 Azure VM 启用复制以后，Site Recovery 可以在目标区域中创建副本虚拟网络（包括子网和网关子网），并在源与目标虚拟网络之间创建所需的映射。 还可以预先创建目标端网络和子网，并在启用复制时使用相同的网络和子网。 Site Recovery 不会在[故障转移](azure-to-azure-tutorial-failover-failback.md)之前在目标 Azure 区域创建任何 VM。

对于 Azure VM 复制，请确保源 Azure 区域的 NSG 规则允许复制流量的[出站连接](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges)。 也可通过此[示例 NSG 配置](azure-to-azure-about-networking.md)测试并验证这些必需的规则。

<!-- Archor wait for PM reply on #example-nsg-configuration-->

Site Recovery 不在故障转移操作过程中创建或复制 NSG。 建议在启动故障转移之前在目标 Azure 区域创建所需的 NSG。 然后即可将自动化脚本与 Site Recovery 的强大[恢复计划](site-recovery-create-recovery-plans.md)配合使用，通过关联 NSG 在故障转移期间自动进行 VM 故障转移。

考虑一下此前介绍的[示例方案](concepts-network-security-group-with-site-recovery.md#using-network-security-groups)：
-   为 VM 启用复制后，Site Recovery 就可以在目标 Azure 区域创建 **Contoso VNet** 和 **Contoso 子网**的副本。
-   可以在目标 Azure 区域创建**子网 NSG** 和 **VM NSG** 的所需副本（例如**目标子网 NSG** 和**目标 VM NSG**），以便在目标区域设置任何其他的必需规则。
-   然后可以立即将**目标子网 NSG** 与目标区域子网相关联，因为 NSG 和子网均已可用。
-   可以在故障转移期间使用恢复计划将**目标 VM NSG** 与 VM 相关联。

创建并配置 NSG 以后，建议运行[测试故障转移](azure-to-azure-tutorial-dr-drill.md)来验证脚本化的 NSG 关联和故障转移后的 VM 连接性。

## <a name="next-steps"></a>后续步骤
-   详细了解[网络安全组](../virtual-network/security-overview.md#network-security-groups)。
-   详细了解 NSG [安全规则](../virtual-network/security-overview.md#security-rules)。
-   详细了解对 NSG [有效的安全规则](../virtual-network/diagnose-network-traffic-filter-problem.md)。
-   详细了解如何使用[恢复计划](site-recovery-create-recovery-plans.md)自动执行应用程序故障转移。

<!-- Update_Description: update meta properties -->