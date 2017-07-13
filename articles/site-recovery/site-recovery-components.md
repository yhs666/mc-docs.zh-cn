---
title: "Site Recovery 的工作原理是什么？ | Azure"
description: "本文提供站点恢复体系结构的概述"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 06/14/2017
ms.date: 07/10/2017
ms.author: v-yeche
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-azure-to-azure-architecture
ms.openlocfilehash: 6c10545ffdea431c2c846bfb3d12bec64c448fb7
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# Azure Site Recovery 如何作用于本地基础结构？
<a id="how-does-azure-site-recovery-work-for-on-premises-infrastructure" class="xliff"></a>

> [!div class="op_single_selector"]
> * [复制本地机](site-recovery-components.md)
<!-- Not Available [Replicate Azure virtual machines](site-recovery-azure-to-azure-architecture.md) -->
本文介绍 [Azure Site Recovery](site-recovery-overview.md) 服务的基础体系结构以及将工作负荷从本地复制到 Azure 所需的组件。

请将任何评论发布到本文底部，或者发布到 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)。

## 复制到 Azure
<a id="replicate-to-azure" class="xliff"></a>

可以将下述本地基础结构复制到 Azure 并对其进行保护：

- **VMware**：本地 VMware VM，运行在[支持的主机](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)上。 可以复制运行[支持的操作系统](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)的 VMware VM
- **Hyper-V**：本地 Hyper-V VM，运行在[支持的主机](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)上。
- **物理机**：本地物理服务器，在[支持的操作系统](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)上运行 Windows 或 Linux。 你可以复制运行 [Hyper-V 和 Azure](https://technet.microsoft.com/zh-cn/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows)支持的任何来宾操作系统的 Hyper-V VM。

##<a name="replicate-vmware-vmsphysical-servers-to-azure"></a><a name="vmware-to-azure"></a>VMware 复制到 Azure

下面是将 VMware VM 复制到 Azure 时所需的项。

区域 | 组件 | 详细信息
--- | --- | ---
**配置服务器** | 单个管理服务器 (VMWare VM) 运行所有本地组件 - 配置服务器、进程服务器、主目标服务器 | 配置服务器在本地与 Azure 之间协调通信并管理数据复制。
 **进程服务器**：  | 默认安装在配置服务器上。 | 充当复制网关。 接收复制数据，通过缓存、压缩和加密对其进行优化，然后将数据发送到 Azure 存储。<br/><br/> 进程服务器还处理用于保护计算机的移动服务的推送安装，并执行 VMware VM 的自动发现。<br/><br/> 随着部署扩大，可以添加作为进程服务器运行的独立专用进程服务器，目的仅在于处理更大的复制流量。
 **主目标服务器** | 默认安装在本地配置服务器上。 | 处理从 Azure 进行故障回复期间产生的复制数据。<br/><br/> 如果故障回复流量很大，可以单独部署一台主目标服务器用于故障回复。
**VMware 服务器** | VMware VM 托管在 vSphere ESXi 服务器上，我们建议使用 vCenter 服务器来管理主机。 | 可将 VMware 服务器添加到恢复服务保管库。<br/><br/>
**复制的计算机** | 移动服务将安装在要复制的每台 VMware VM 上。 可在每台计算机上手动安装，或者通过进程服务器进行推送安装。| -

**图 1：VMware 到 Azure 的组件**

![组件](./media/site-recovery-components/arch-enhanced.png)

### 复制过程
<a id="replication-process" class="xliff"></a>

1. 设置部署，包括 Azure 组件和恢复服务保管库。 在保管库中指定复制源和目标，设置配置服务器，添加 VMware 服务器，创建复制策略，部署移动服务，启用复制，并运行测试故障转移。
2.  计算机根据复制策略开始复制，初始数据副本将复制到 Azure 存储。
4. 完成初始复制后，开始将增量更改复制到 Azure。 计算机的受跟踪更改保存在 .hrl 文件中。
    - 复制计算机与 HTTPS 443 入站端口上的配置服务器通信，进行复制管理。
    - 复制计算机将复制数据发送到 HTTPS 9443 入站端口（可配置）上的进程服务器。
    - 配置服务器通过 HTTPS 443 出站端口来与 Azure 协调复制管理。
    - 进程服务器从源计算机接收数据、优化和加密数据，然后通过 443 出站端口将其发送到 Azure 存储。
    - 如果启用了多 VM 一致性，复制组中的计算机将通过端口 20004 相互通信。 如果将多台计算机分组到复制组，并且这些组在故障转移时共享崩溃一致且应用一致的恢复点，请使用多 VM 方案。 如果计算机运行相同的工作负荷并需要保持一致，这种做法非常有用。
5. 流量通过 Internet 复制到 Azure 存储公共终结点。 或者，可以使用 Azure ExpressRoute [公共对等互连](../expressroute/expressroute-circuit-peerings.md#public-peering)。 不支持通过站点到站点 VPN 将流量从本地站点复制到 Azure。

**图 2：VMware 到 Azure 的复制**

![增强版](./media/site-recovery-components/v2a-architecture-henry.png)

### 故障转移和故障回复
<a id="failover-and-failback" class="xliff"></a>

1. 在验证测试故障转移可以按预期工作后，你可以根据需要运行到 Azure 的计划外故障转移。 不支持计划内故障转移。
2. 可以故障转移单台计算机，或创建[恢复计划](site-recovery-create-recovery-plans.md)来故障转移多台 VM。
3. 运行故障转移时，将在 Azure 中创建副本 VM。 提交故障转移，开始从副本 Azure VM 访问工作负荷。
4. 当本地主站点再次可用时，便可以故障回复。 设置故障回复基础结构，开始将计算机从辅助站点复制到主站点，然后从辅助站点运行计划外故障转移。 提交此故障转移后，数据将回到本地，此时需要再次启用目标为 Azure 的复制。 
<!-- Not Available [Learn more](site-recovery-failback-azure-to-vmware.md) -->


**图 3：VMware/物理机故障回复**

![故障回复](./media/site-recovery-components/enhanced-failback.png)

## 物理机到 Azure
<a id="physical-to-azure" class="xliff"></a>

将物理本地服务器复制到 Azure 时，复制过程使用与 [从 VMware 到 Azure](#vmware-replication-to-azure)相同的组件和进程，但请注意以下差异：

- 可以使用物理服务器（而非 VMware VM）作为配置服务器
- 你需要具有用于故障回复的本地 VMware 基础结构。 无法故障回复到物理服务器。

## Hyper-V 到 Azure
<a id="hyper-v-to-azure" class="xliff"></a>

### 复制过程
<a id="replication-process" class="xliff"></a>

1. 设置 Azure 组件。 建议在开始部署 Site Recovery 之前设置存储和网络帐户。
2. 为 Site Recovery 创建复制服务保管库，并配置保管库设置，包括：
    - 源和目标设置。 如果不是在 VMM 云中管理 Hyper-V 主机，请为目标创建一个 Hyper-V 站点容器，并将 Hyper-V 主机添加到其中。 如果是在 VMM 云中管理 Hyper-V 主机，则源是 VMM 云。 目标是 Azure。
    - 安装 Azure Site Recovery 提供程序和 Azure 恢复服务代理。 如果使用 VMM，将在其上安装提供程序，在每台 Hyper-V 主机上安装代理。 如果未使用 VMM，提供程序和代理将安装在每台主机上。
    - 为 Hyper-V 站点或 VMM 云创建复制策略。 策略将应用到站点或云中主机上的所有 VM。
    - 为 Hyper-V VM 启用复制。 初始复制根据复制策略设置进行。
4. 数据更改将受到跟踪，完成初始复制后，开始将增量更改复制到 Azure。 项的受跟踪更改保存在 .hrl 文件中。
5. 运行测试故障转移，确保一切正常运行。

### 故障转移和故障回复过程
<a id="failover-and-failback-process" class="xliff"></a>

1. 可以运行从本地 Hyper-V VM 到 Azure 的计划内或计划外[故障转移](site-recovery-failover.md)。 如果运行计划内故障转移，则源 VM 将关闭以确保不会丢失数据。
2. 可以故障转移单台计算机，或创建[恢复计划](site-recovery-create-recovery-plans.md)来协调多台计算机的故障转移。
4. 运行故障转移后，应会在 Azure 中看到创建的副本 VM。 如果需要，可向 VM 分配公共 IP 地址。
5. 然后提交故障转移，开始从副本 Azure VM 访问工作负荷。
6. 当本地主站点再次可用时，便可以[故障回复](site-recovery-failback-from-azure-to-hyper-v.md)。 启动从 Azure 到主站点的计划内故障转移。 运行计划内故障转移时，可以选择故障回复到同一 VM 或备用位置，并同步 Azure 与本地之间的更改，确保不会丢失数据。 在本地创建 VM 时，请提交故障转移。

**图 4：从 Hyper-V 站点到 Azure 的复制**

![组件](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**图 5：从 VMM 云中的 Hyper-V 到 Azure 的复制**

![组件](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

## 复制到辅助站点
<a id="replicate-to-a-secondary-site" class="xliff"></a>

可以将以下各项复制到辅助站点：

- **VMware**：本地 VMware VM，运行在[支持的主机](site-recovery-support-matrix-to-sec-site.md#on-premises-servers)上。 可以复制运行[支持的操作系统](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)的 VMware VM
- **物理机**：本地物理服务器，在[支持的操作系统](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)上运行 Windows 或 Linux。
- **Hyper-V**：本地 Hyper-V VM，运行在[支持的 Hyper-V 主机](site-recovery-support-matrix-to-sec-site.md#on-premises-servers)上，后者在 VMM 云中管理。 [支持的主机](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)。 你可以复制运行 [Hyper-V 和 Azure](https://technet.microsoft.com/zh-cn/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows)支持的任何来宾操作系统的 Hyper-V VM。

## 从 VMware/物理服务器到辅助站点
<a id="vmwarephysical-to-a-secondary-site" class="xliff"></a>

可以使用 InMage Scout 将 VMware VM 或物理服务器复制到辅助站点。

### 组件
<a id="components" class="xliff"></a>

**区域** | **组件** | **详细信息**
--- | --- | ---
**进程服务器** | 位于主站点中 | 可以部署进程服务器来处理缓存、压缩和数据优化操作。<br/><br/> 它还可以将安装的统一代理推送到你要保护的计算机。
**配置服务器** | 位于辅助站点中 | 配置服务器使用管理网站或 vContinuum 控制台来管理、配置和监视你的部署。
**vContinuum 服务器** | 可选。 与配置服务器安装在同一位置。 | 它提供一个控制台用于管理和监视受保护的环境。
**主目标服务器** | 位于辅助站点中 | 主目标服务器保存复制的数据。 它从进程服务器接收数据，在辅助站点中创建副本地器，并保存数据保留点。<br/><br/> 需要的主目标服务器数目取决于要保护的计算机数目。<br/><br/> 如果希望故障转移到主站点，则主站点上也需要有一个主目标服务器。 在此服务器上安装统一代理。
**VMware ESX/ESXi 和 vCenter 服务器** |  VM 托管在 ESX/ESXi 主机上。 主机是通过 vCenter 服务器管理的 | 需要使用 VMware 基础结构来复制 VMware VM。
**VM/物理服务器** |  在你要复制的 VMware VM 和物理服务器上安装的统一代理。 | 该代理充当所有组件之间的通信提供程序。

### 复制过程
<a id="replication-process" class="xliff"></a>

1. 在每个站点（配置、进程、主目标）中设置组件服务器，并在要复制的计算机上安装统一代理。
2. 初始复制之后，每台计算机上的代理将增量复制更改发送到进程服务器。
3. 进程服务器将优化这些数据，并将其传输到辅助站点上的主目标服务器。 配置服务器将管理复制进程。

**图 6：从 VMware 到 VMware 的复制**

![VMware 到 VMware](./media/site-recovery-components/vmware-to-vmware.png)

## 从 Hyper-V 到辅助站点
<a id="hyper-v-to-a-secondary-site" class="xliff"></a>

下面是将 Hyper-V VM 复制到辅助站点时所需的项。

**区域** | **组件** | **详细信息**
--- | --- | ---
**VMM 服务器** | 我们建议在主站点和辅助站点中各部署一台 VMM 服务器 | 每个 VMM 服务器都应连接到 Internet。<br/><br/> 每台服务器应至少包含一个设置了 Hyper-V 功能配置文件的 VMM 私有云。<br/><br/> 在 VMM 服务器上安装 Azure Site Recovery 提供程序。 提供程序通过 Internet 使用 Site Recovery 服务协调和安排复制。 提供程序与 Azure 之间的通信是安全的且经过加密的。
**Hyper-V 服务器** |  主要和辅助 VMM 云中需要一台或多台 Hyper-V 主机服务器。<br/><br/> 服务器应已连接到 Internet。<br/><br/> 使用 Kerberos 或证书身份验证通过 LAN 或 VPN 在主要和辅助 Hyper-V 主机服务器之间复制数据。  
**Hyper-V VM** | 位于源 Hyper-V 主机服务器中。 | 源主机服务器应该至少有一个要复制的 VM。

### 复制过程
<a id="replication-process" class="xliff"></a>

1. 设置 Azure 帐户。
2. 为 Site Recovery 创建复制服务保管库，并配置保管库设置，包括：

    - 复制源和目标（主站点和辅助站点）。
    - 安装 Azure Site Recovery 提供程序和 Azure 恢复服务代理。 在 VMM 服务器上安装提供程序，在每台 Hyper-V 主机上安装代理。
    - 为源 VMM 云创建复制策略。 策略将应用到云中主机上的所有 VM。
    - 为 Hyper-V VM 启用复制。 初始复制根据复制策略设置进行。
4. 数据更改将受到跟踪，完成初始复制后，开始复制增量更改。 项的受跟踪更改保存在 .hrl 文件中。
5. 运行测试故障转移，确保一切正常运行。

**图 7：VMM 到 VMM 的复制**

![本地到本地](./media/site-recovery-components/arch-onprem-onprem.png)

### 故障转移和故障回复
<a id="failover-and-failback" class="xliff"></a>

1. 可以在本地站点之间运行计划内或计划外[故障转移](site-recovery-failover.md)。 如果运行计划内故障转移，则源 VM 将关闭以确保不会丢失数据。
2. 可以故障转移单台计算机，或创建[恢复计划](site-recovery-create-recovery-plans.md)来协调多台计算机的故障转移。
4. 如果执行了到辅助站点的计划外故障转移，在故障转移后，将不会为辅助位置中的计算机启用保护或复制。 如果运行了计划内故障转移，在故障转移后，辅助位置中的计算机将受保护。
5. 然后，你提交故障转移，开始从副本 VM 访问工作负荷。
6. 当主站点再次可用时，启动从辅助站点到主站点的反向复制。 反向复制会使虚拟机进入受保护状态，但辅助数据中心仍是活动位置。
7. 若要使主站点再次成为活动位置，可以启动从辅助站点到主站点的计划内故障转移，然后再次启动反向复制。

## 后续步骤
<a id="next-steps" class="xliff"></a>

<!-- Not Available [Learn more](site-recovery-hyper-v-azure-architecture.md) -->
- [检查先决条件](site-recovery-prereq.md)