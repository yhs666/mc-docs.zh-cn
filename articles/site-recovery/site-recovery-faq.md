---
title: "Azure Site Recovery：常见问题 | Azure"
description: "本文讨论有关 Azure Site Recovery 的常见问题。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 5cdc4bcd-b4fe-48c7-8be1-1db39bd9c078
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 05/22/2017
ms.date: 07/10/2017
ms.author: v-yeche
ms.openlocfilehash: 508c442852c4fed5e44ffc259d48eaba00b45447
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# Azure Site Recovery：常见问题解答 (FAQ)
<a id="azure-site-recovery-frequently-asked-questions-faq" class="xliff"></a>

本文包含有关 Azure Site Recovery 的常见问题。 如果在阅读本文后有任何问题，请在 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/zh-cn/home?forum=hypervrecovmgr)上发布这些问题。

## 常规
<a id="general" class="xliff"></a>
### Site Recovery 的功能是什么？
<a id="what-does-site-recovery-do" class="xliff"></a>
Site Recovery 可通过协调和自动运行区域之间的 Azure VM 复制、本地虚拟机与物理服务器到 Azure 的复制以及本地计算机到辅助数据中心的复制，来帮助实现业务连续性与灾难恢复 (BCDR) 策略。 [了解详细信息](site-recovery-overview.md)。

### Site Recovery 可以保护哪些计算机？
<a id="what-can-site-recovery-protect" class="xliff"></a>
* **Azure VM**：Site Recovery 可复制任意在支持的 Azure VM 上运行的工作负荷
* **Hyper-V 虚拟机**：Site Recovery 可以保护 Hyper-V VM 上运行的任何工作负荷。
* **物理服务器**：Site Recovery 可以保护运行 Windows 或 Linux 的物理服务器。
* **VMware 虚拟机**：Site Recovery 可以保护 VMware VM 上运行的任何工作负荷。

### Site Recovery 是否支持 Azure Resource Manager 模型？
<a id="does-site-recovery-support-the-azure-resource-manager-model" class="xliff"></a>
Site Recovery 在 Azure 门户中可用，并支持 Resource Manager。 Site Recovery 支持 Azure 经典管理门户中的旧部署。 不能在经典管理门户中创建新保管库，且不支持新功能。

### 我能否复制 Azure VM？
<a id="can-i-replicate-azure-vms" class="xliff"></a>
你能够在 Azure 区域之间复制支持的 Azure VM。
<!-- Not Available [Learn more](site-recovery-azure-to-azure.md) -->

### 在 Hyper-V 中，需要做好哪些准备才能使用 Site Recovery 来协调复制？
<a id="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery" class="xliff"></a>
对于 Hyper-V 主机服务器，用户的所需取决于部署方案。 在以下内容中查看 Hyper-V 先决条件：

* [将 Hyper-V VM 复制（不使用 VMM）到 Azure](site-recovery-hyper-v-site-to-azure.md)
* [将 Hyper-V VM 复制（使用 VMM）到 Azure](site-recovery-vmm-to-azure.md)
* [将 Hyper-V VM 复制到辅助数据中心](site-recovery-vmm-to-vmm.md)
* 若要复制到辅助数据中心，请阅读 [Hyper-V 虚拟机的受支持的来宾操作系统](https://technet.microsoft.com/zh-cn/library/mt126277.aspx)。
* 若要复制到 Azure，Site Recovery 支持 [Azure 支持的](https://technet.microsoft.com/zh-cn/library/cc794868%28v=ws.10%29.aspx)所有来宾操作系统。

### 当 Hyper-V 在客户端操作系统上运行时，我可以保护 VM 吗？
<a id="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system" class="xliff"></a>
不可以。VM 必须位于在支持的 Windows 服务器计算机上运行的 Hyper-V 主机服务器上。 如果需要保护客户端计算机，可以将其作为物理计算机复制到 [Azure](./site-recovery-vmware-to-azure.md)。

### 可以使用 Site Recovery 来保护哪些工作负荷？
<a id="what-workloads-can-i-protect-with-site-recovery" class="xliff"></a>
可以使用 Site Recovery 来保护在支持的 VM 或物理服务器上运行的大多数工作负荷。 Site Recovery 为应用程序感知型复制提供支持，因此，应用可以恢复为智能状态。 它除了与 Microsoft 应用程序（例如 SharePoint、Exchange、Dynamics、SQL Server 及 Active Directory）集成之外，还能与行业领先的供应商（包括 Oracle、SAP、IBM 及 Red Hat）紧密配合。 [详细了解](site-recovery-workload.md)工作负荷保护。

### Hyper-V 主机是否需要位于 VMM 云中？
<a id="do-hyper-v-hosts-need-to-be-in-vmm-clouds" class="xliff"></a>
如果要复制到辅助数据中心，那么 Hyper-V VM 就必须位于 VMM 云中的 Hyper-V 主机服务器上。 若要复制到 Azure，可以复制 Hyper-V 主机服务器（无论是否位于 VMM 云中）上的 VM。 [了解详细信息](site-recovery-hyper-v-site-to-azure.md)。

### 如果只有一个 VMM 服务器，可以部署带 VMM 的 Site Recovery 吗？
<a id="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server" class="xliff"></a>

是的。 可以将 VMM 云中 Hyper-V 服务器上的 VM 复制到 Azure，或者在同一台服务器上的 VMM 云之间进行复制。 对于本地到本地复制，建议在主站点与辅助站点中都部署一个 VMM 服务器。  

### 可以保护哪些物理服务器？
<a id="what-physical-servers-can-i-protect" class="xliff"></a>
可以将运行 Windows 或 Linux 的物理服务器复制到 Azure 或辅助站点。 [了解](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)操作系统要求。  无论是将物理服务器复制到 Azure 还是辅助站点，都需要满足相同的要求。

请注意，如果本地服务器关闭，物理服务器将在 Azure 中以 VM 的形式运行。 当前不支持故障回复到本地物理服务器。 对于作为物理机进行保护的计算机，只能故障回复到 VMware 虚拟机。

### 可以保护哪些 VMware VM？
<a id="what-vmware-vms-can-i-protect" class="xliff"></a>

若要保护 VMware VM，则需要 vSphere 虚拟机监控程序和运行 VMware 工具的虚拟机。 我们还建议使用 VMware vCenter 服务器托管虚拟机监控程序。 [了解](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)有关复制 VMware 服务器和 VM 到 Azure 或辅助站点的精确要求。

### 可以使用 Site Recovery 来管理分支机构的灾难恢复吗？
<a id="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery" class="xliff"></a>
是的。 在使用 Site Recovery 来协调分支机构的复制与故障转移时，可在一个中心位置获得所有分支机构工作负荷的统一视图。 不需要前往分支机构，就可以从总部轻松对所有分支机构运行故障转移和管理灾难恢复。

## 定价
<a id="pricing" class="xliff"></a>

### 使用 Azure Site Recovery 会产生哪些费用？
<a id="what-charges-do-i-incur-while-using-azure-site-recovery" class="xliff"></a>
使用 Site Recovery 时，会产生 Site Recovery 许可证、Azure 存储、存储事务和出站数据传输费用。 [了解详细信息](https://www.azure.cn/pricing/details/site-recovery)。

Site Recovery 许可证费用根据受保护的实例收取，实例可以是 VM 或物理服务器。

- 如果将 VM 磁盘复制到标准存储帐户，Azure 存储费用根据存储消耗量收取。 例如，如果源磁盘大小为 1 TB，已使用 400 GB，Site Recovery 将在 Azure 中创建 1 TB 的 VHD，但收费的存储为 400 GB（加上用于复制日志的存储空间量）。
- 如果将 VM 磁盘复制到高级存储帐户，Azure 存储费用将按照预配的存储大小（已根据最接近的高级存储磁盘选项舍入）收取。 例如，如果源磁盘大小为 50 GB，则 Site Recovery 会在 Azure 中创建 50 GB 磁盘，Azure 会将此大小映射到最近的高级存储磁盘 (P10)。  费用将按照 P10 计算，而不是按照 50 GB 磁盘大小计算。  [了解详细信息](../storage/storage-premium-storage.md#pricing-and-billing)。  如果使用高级存储，则还需要为复制日志记录使用一个标准存储帐户，用于这些日志的标准存储空间量也会计费。
- 在执行测试故障转移或故障转移以前，不创建任何磁盘。 在复制状态下，“页 Blob 和磁盘”类别下的存储费用将基于每个[存储定价计算器](https://www.azure.cn/zh-cn/pricing/calculator/)收取。 这些费用的执行基于高级/标准存储类型以及 LRS、GRS、RA-GRS 等数据冗余类型。
<!-- URL matching  https://aka.ms/premium-storage-pricing -- ../storage/storage-premium-storage.md#pricing-and-billing -->
<!-- Not Available Managed disks content block -->
- 将在稳定状态的复制期间以及故障转移/测试故障转移之后针对常规 VM 操作收取存储事务的费用。 但这些费用非常少，几乎可以忽略不计。

测试故障转移期间也会产生 VM、存储、传出流量和存储事务方面的费用。

## “安全”
<a id="security" class="xliff"></a>
### 复制数据是否会发送到 Site Recovery 服务？
<a id="is-replication-data-sent-to-the-site-recovery-service" class="xliff"></a>
不会。站点恢复不拦截复制的数据，也不拥有虚拟机或物理服务器上运行哪些项目的任何相关信息。
复制数据在本地 Hyper-V 主机、VMware 虚拟机监控程序或物理服务器和 Azure 存储或辅助站点之间交换。 站点恢复并不具有拦截该数据的能力。 只有协调复制与故障转移所需的元数据会发送到站点恢复服务。  

Site Recovery 已通过 ISO 27001:2013、27018、HIPAA、DPA 认证，目前正在接受 SOC2 和 FedRAMP JAB 评估。

### 为了遵从法规，即使是本地的元数据，也必须保留在同一个地理区域。 Site Recovery 可以帮助我们吗？
<a id="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us" class="xliff"></a>
是的。 在某个区域中创建 Site Recovery 保管库时，我们确保启用和协调复制与故障转移时所需的一切元数据都保留在该区域的地理边界范围内。

### Site Recovery 是否将复制数据加密？
<a id="does-site-recovery-encrypt-replication" class="xliff"></a>
在本地站点之间复制虚拟机和物理服务器时，支持传输中加密。 将虚拟机和物理服务器复制到 Azure 时，同时支持传输中加密和静态加密（Azure 中）。

## 复制
<a id="replication" class="xliff"></a>

### 能否通过站点到站点 VPN 复制到 Azure？
<a id="can-i-replicate-over-a-site-to-site-vpn-to-azure" class="xliff"></a>
Azure Site Recovery 通过公共终结点将数据复制到 Azure 存储帐户。 复制不是通过站点到站点 VPN 进行。 可以使用 Azure 虚拟网络创建站点到站点 VPN。 这不会干扰 Site Recovery 复制。

### 能否使用 ExpressRoute 将虚拟机复制到 Azure？
<a id="can-i-use-expressroute-to-replicate-virtual-machines-to-azure" class="xliff"></a>
能，可以使用 ExpressRoute 将虚拟机复制到 Azure。 Azure Site Recovery 通过公共终结点将数据复制到 Azure 存储帐户。 需要设置[公共对等互连](../expressroute/expressroute-circuit-peerings.md#public-peering)将 ExpressRoute 用于 Site Recovery 复制。 将虚拟机故障转移到 Azure 虚拟网络以后，即可使用通过 Azure 虚拟网络设置的[专用对等互连](../expressroute/expressroute-circuit-peerings.md#private-peering)对其进行访问。 

### 将虚拟机复制到 Azure 需要满足任何先决条件吗？
<a id="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure" class="xliff"></a>
要复制到 Azure 的虚拟机应符合 [Azure 要求](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements)。

Azure 用户帐户需要具有某些[权限](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)，才能启用新的虚拟机到 Azure 的复制。

### 可以将 Hyper-V 第 2 代虚拟机复制到 Azure 吗？
<a id="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure" class="xliff"></a>
是的。 Site Recovery 在故障转移过程中将从第 2 代转换成第 1 代。 在故障回复时，计算机将转换回到第 2 代。 [了解详细信息](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)。

### 如果复制到 Azure，要支付哪些 Azure VM 费用？
<a id="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms" class="xliff"></a>
在常规复制期间，数据将复制到异地冗余的 Azure 存储，不需要支付任何 Azure IaaS 虚拟机费用（一个明显的优势）。 当故障转移到 Azure 时，Site Recovery 将自动创建 Azure IaaS 虚拟机，此后，需要为在 Azure 中使用的计算资源付费。

### 是否可以使用 SDK 自动执行 Site Recovery 方案？
<a id="can-i-automate-site-recovery-scenarios-with-an-sdk" class="xliff"></a>
是的。 可以使用 Rest API、PowerShell 或 Azure SDK 将 Site Recovery 工作流自动化。 当前支持的使用 PowerShell 部署 Site Recovery 的方案：

* [将 VMM 云中的 Hyper-V VM 复制到 Azure PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
* [将 Hyper-V VM 复制（不使用 VMM）到 Azure PowerShell Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)

### 如果要复制到 Azure，需要哪种存储帐户？
<a id="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need" class="xliff"></a>
* **Azure 经典管理门户**：若要在 Azure 经典管理门户中部署 Site Recovery，需要一个 [标准异地冗余存储帐户](../storage/storage-redundancy.md#geo-redundant-storage)。 当前不支持高级存储。 该帐户必须位于与 Site Recovery 保管库相同的区域中。
* **Azure 经典门户**：若要在 Azure 门户中部署 Site Recovery，需要一个 LRS 或 GRS 存储帐户。 建议使用 GRS，以便在发生区域性故障或无法恢复主要区域时，能够复原数据。 该帐户必须位于与恢复服务保管库相同的区域中。 在 Azure 门户中部署 Site Recovery 时，VMware VM、Hyper-V VM 和物理服务器复制现在支持高级存储。

### 可以多久复制数据一次？
<a id="how-often-can-i-replicate-data" class="xliff"></a>
* **Hyper-V**：可以每隔 30 秒（高级存储除外）、5 分钟或 15 分钟复制一次 Hyper-V VM。 如果已设置 SAN 复制，则复制将是同步的。
* **VMware 和物理服务器：** 复制频率无关紧要。 复制是连续的。

### 可以将复制从现有的恢复站点扩展到其他站点吗？
<a id="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site" class="xliff"></a>
不支持扩展扩展或链式复制。

### 在首次复制到 Azure 时可以进行脱机复制吗？
<a id="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure" class="xliff"></a>
不支持此操作。


### 可以使用动态磁盘来复制虚拟机吗？
<a id="can-i-replicate-virtual-machines-with-dynamic-disks" class="xliff"></a>
复制 Hyper-V 虚拟机时，支持使用动态磁盘。 将 VMware VM 和物理机复制到 Azure 时，支持使用动态磁盘。 操作系统磁盘必须是基本磁盘。

### 能否将新计算机添加到现有的复制组中？
<a id="can-i-add-a-new-machine-to-an-existing-replication-group" class="xliff"></a>
支持将新计算机添加到现有的复制组。 要进行此操作，请从“已复制项目”边栏选项卡中，选择复制组并右键单击/选择复制组中的上下文菜单，然后选择相应的选项。

![添加到复制组](./media/site-recovery-faq/add-server-replication-group.png)

### 可以限制针对 Hyper-V 复制流量分配的带宽吗？
<a id="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic" class="xliff"></a>
是的。 可以从以下部署文章中阅读更多有关限制带宽的信息：

<!-- Not Available for vmware feature -->
- [Capacity planning for replicating Hyper-V VMs in VMM clouds（复制 VMM 云中的 Hyper-V VM 的容量规划）](./site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [复制 Hyper-V VM（不使用 VMM）的容量规划](./site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning) 

## 故障转移
<a id="failover" class="xliff"></a>
### 在故障转移到 Azure 之后，如何访问 Azure 虚拟机？
<a id="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover" class="xliff"></a>
可以通过安全的 Internet 连接或者站点到站点 VPN 或 Azure ExpressRoute 访问 Azure VM。 在连接之前需要做许多准备。 [了解详细信息](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### 如果故障转移到 Azure，Azure 如何确保我的数据可恢复？
<a id="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient" class="xliff"></a>

Azure 具有复原能力。 Site Recovery 已经能够根据需要故障转移到符合 Azure SLA 的辅助 Azure 数据中心。 发生此情况时，我们确保元数据和保管库都保留在为保管库选择的相同地理区域。  

### 如果在两个数据中心之间进行复制，当我的主数据中心发生意外的服务中断时，会出现什么情况？
<a id="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage" class="xliff"></a>

可以从辅助站点触发非计划的故障转移。 Site Recovery 不需要来自主站点的连接即可执行故障转移。

### 故障转移是自动发生的吗？
<a id="is-failover-automatic" class="xliff"></a>

故障转移不是自动的。 可以在门户中单击一下来启动故障转移，或者使用[站点恢复 PowerShell](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.siterecovery) 来触发故障转移。 在 Site Recovery 门户中可以轻松进行故障回复。

若要自动化，可以使用本地 Orchestrator 或 Operations Manager 来检测虚拟机故障，然后使用 SDK 来触发故障转移。

* [详细了解](site-recovery-create-recovery-plans.md)恢复计划。
* [了解详细信息](site-recovery-failover.md) 阅读更多有关故障转移的信息。
<!-- Not Available for vmware feature -->

### 如果我的本地主机未响应或崩溃，我是否可以故障转移回到另一个主机？
<a id="if-my-on-premises-host-is-not-responding-or-crashed-can-i-failover-back-to-a-different-host" class="xliff"></a>
是，可以使用备用位置恢复从 Azure 故障回复到另一个主机。 通过用于 VMware 和 Hyper-v 虚拟机的以下链接详细了解选项。

<!-- Not Available vmware feature -->
* [对于 Hyper-v 虚拟机](site-recovery-failback-from-azure-to-hyper-v.md#failback-to-an-alternate-location)

## 服务提供商
<a id="service-providers" class="xliff"></a>
### 我是服务提供商。 Site Recovery 是否适用于专用和共享的基础结构模型？
<a id="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models" class="xliff"></a>

是的，Site Recovery 同时支持专用与共享的基础结构模型。

### 对于服务提供商而言，我的租户标识是否与 Site Recovery 服务共享？
<a id="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service" class="xliff"></a>

不是。 租户标识是匿名的。 租户不需要访问 Site Recovery 门户。 只有服务提供商管理员才能与门户交互。

### 租户应用程序数据是否会发往 Azure？
<a id="will-tenant-application-data-ever-go-to-azure" class="xliff"></a>

在服务提供商拥有的站点之间进行复制时，永远不会将应用程序数据发送到 Azure。 数据进行传输中加密并直接在服务提供商站点之间复制。

如果是复制到 Azure，应用程序数据将发送到 Azure 存储而不是 Site Recovery 服务。 数据进行传输中加密并在 Azure 中保持加密状态。

### 我的租户会收到来自 Azure 服务的帐单吗？
<a id="will-my-tenants-receive-a-bill-for-any-azure-services" class="xliff"></a>

不是。 Azure 直接与服务提供商保持计费关系。 服务提供商责任为其租户生成特定的帐单。

### 如果我复制到 Azure，需要在 Azure 中随时运行虚拟机吗？
<a id="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times" class="xliff"></a>

不需要。会将数据复制到订阅中的 Azure 存储帐户。 执行测试故障转移（灾难恢复演练）或实际的故障转移时，站点恢复会在订阅中自动创建虚拟机。

### 复制到 Azure 时，是否向我确保提供租户级的隔离？
<a id="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure" class="xliff"></a>

是的。

### 目前支持哪些平台？
<a id="what-platforms-do-you-currently-support" class="xliff"></a>

我们支持 Azure Pack、云平台系统和基于 System Center 的（2012 和更高版本）的部署。 [了解更多](https://technet.microsoft.com/zh-cn/library/dn850370.aspx)有关 Azure Pack 和 Site Recovery 集成的信息。

### 是否支持单一 Azure Pack 和单一 VMM 服务器部署？
<a id="do-you-support-single-azure-pack-and-single-vmm-server-deployments" class="xliff"></a>

是，可将 Hyper-V 虚拟机复制到 Azure，或者在服务提供商站点之间复制。  请注意，如果在服务提供商站点之间复制，将无法使用 Azure Runbook 集成。

## 后续步骤
<a id="next-steps" class="xliff"></a>

* 阅读 [站点恢复概述](site-recovery-overview.md)
* 了解 [Site Recovery 体系结构](site-recovery-components.md)