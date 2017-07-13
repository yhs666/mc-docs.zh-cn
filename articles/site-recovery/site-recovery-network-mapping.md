---
title: "规划使用 Site Recovery 进行 Hyper-V VM 复制时的网络映射 | Azure"
description: "设置网络映射，以便进行从本地数据中心到 Azure 或者到辅助站点的 Hyper-V 虚拟机复制。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: fcaa2f52-489d-4c1c-865f-9e78e000b351
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 05/23/2017
ms.date: 07/10/2017
ms.author: v-yeche
ms.openlocfilehash: 55a2671ca279dbd0fcb6d575e07267f098df0dcf
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 规划使用 Site Recovery 进行 Hyper-V VM 复制时的网络映射
<a id="plan-network-mapping-for-hyper-v-vm-replication-with-site-recovery" class="xliff"></a>

本文帮助用户了解和规划使用 [Azure Site Recovery 服务](site-recovery-overview.md)将 Hyper-V VM 复制到 Azure 或辅助站点期间的网络映射。

## 复制到 Azure 时的网络映射
<a id="network-mapping-for-replication-to-azure" class="xliff"></a>

在将 HYPER-V VM（托管在 VMM 中）复制到 Azure 时，将使用网络映射。 网络映射将在源 VMM 服务器的 VM 网络与目标 Azure 网络之间映射。 映射将执行以下操作：

- 网络连接 - 确保复制的 Azure VM 连接到映射的网络。 在同一网络中进行故障转移的所有计算机可以相互连接，即使它们在不同的恢复计划中进行故障转移。
- 网关 - 如果在目标 Azure 网络上设置了网络网关，则 VM 可以连接到其他本地虚拟机。

请注意：

- 将源 VMM VM 网络映射到 Azure 虚拟网络。
- 故障转移后，源网络中的 Azure VM 将连接到映射的目标虚拟网络。
- 进行复制时，添加到源 VM 网络的新 VM 将连接到映射的 Azure 网络。
- 如果目标网络具有多个子网，并且其中一个子网与源虚拟机所在的子网同名，则在故障转移后副本虚拟机将连接到该目标子网。
- 如果没有具有匹配名称的目标子网，则虚拟机将连接到网络中的第一个子网。

## 复制到辅助数据中心时的网络映射
<a id="network-mapping-for-replication-to-a-secondary-datacenter" class="xliff"></a>

在将 Hyper-V VM（托管在 System Center Virtual Machine Manager (VMM) 中）复制到辅助数据中心时，将使用网络映射。 网络映射将在源 VMM 服务器的 VM 网络与目标VMM 服务器的 VM 网络之间映射。 映射将执行以下操作：

- 网络连接 - 在故障转移之后将 VM 连接到相应的网络。 副本 VM 将连接到已映射到源网络的目标网络。
- 最佳放置 - 以最佳方式将副本 VM 放置在 Hyper-V 主机服务器上。 副本 VM 放置在可访问映射 VM 网络的主机上。
- 无网络映射 - 如果未配置网络映射，则在故障转移后副本 VM 将不会连接到任何 VM 网络。

请注意：

- 可以在两个 VMM 服务器上的 VM 网络之间配置网络映射；如果两个站点由同一个服务器管理，则在可以在单个 VMM 服务器上的两个对应 VM 网络之间配置网络映射。
- 在正确地配置映射且启用复制后，位于主位置的 VM 将连接到网络，其位于目标位置的副本将连接到其映射网络。
-
- 如果已经在 VMM 中正确地设置了网络，则在网络映射期间选择目标 VM 网络时，将显示使用源 VM 网络的 VMM 源云以及用于保护的目标云上的可用目标 VM 网络。
- 如果目标网络具有多个子网，并且其中一个子网与源虚拟机所在的子网同名，则在故障转移后副本虚拟机将连接到该目标子网。 如果没有具有匹配名称的目标子网，虚拟机将连接到网络中的第一个子网。

### 示例
<a id="example" class="xliff"></a>

以下示例演示了此机制。 我们来考察一个具有两个位置（北京和上海）的组织。

**位置** | **VMM 服务器** | **VM 网络** | **映射到**
---|---|---|---
北京 | VMM-Beijing| VMNetwork1-Beijing | 映射到 VMNetwork1-Shanghai
 |  | VMNetwork2-Beijing | 未映射
上海 | VMM-Shanghai| VMNetwork1-Shanghai | 映射到 VMNetwork1-Beijing
 | | VMNetwork1-Shanghai | 未映射

在本示例中：

- 当为任何连接到 VMNetwork1-Beijing 的虚拟机创建副本虚拟机时，副本虚拟机将连接到 VMNetwork1-Shanghai。
- 当为 VMNetwork2-Beijing 或 VMNetwork2-Shanghai 创建副本虚拟机时，副本虚拟机将不会连接到任何网络。

这是本示例组织中 VMM 云的设置方式，以及逻辑网络与云关联的方式。

#### 云保护设置
<a id="cloud-protection-settings" class="xliff"></a>

**受保护的云** | **提供保护的云** | 逻辑网络（北京）  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>不可用</p><p></p> | <p>LogicalNetwork1-Beijing</p><p>LogicalNetwork1-Shanghai</p>
SilverCloud2 | <p>不可用</p><p></p> | <p>LogicalNetwork1-Beijing</p><p>LogicalNetwork1-Shanghai</p>

#### 逻辑和 VM 网络设置
<a id="logical-and-vm-network-settings" class="xliff"></a>

**位置** | **逻辑网络** | **关联的 VM 网络**
---|---|---
北京 | LogicalNetwork1-Beijing | VMNetwork1-Beijing
上海 | LogicalNetwork1-Shanghai | VMNetwork1-Shanghai
 | LogicalNetwork2Shanghai | VMNetwork2-Shanghai

#### 目标网络设置
<a id="target-network-settings" class="xliff"></a>

根据这些设置，在选择目标 VM 网络时，下表显示将可用的选项。

**选择** | **受保护的云** | **提供保护的云** | **可用目标网络**
---|---|---|---
VMNetwork1-Shanghai | SilverCloud1 | SilverCloud2 | 可用
 | GoldCloud1 | GoldCloud2 | 可用
VMNetwork2-Shanghai | SilverCloud1 | SilverCloud2 | 不可用
 | GoldCloud1 | GoldCloud2 | 可用

如果目标网络具有多个子网，并且其中一个子网与源虚拟机所在的子网同名，则在故障转移后副本虚拟机将连接到该目标子网。 如果没有具有匹配名称的目标子网，虚拟机将连接到网络中的第一个子网。

#### 故障回复行为
<a id="failback-behavior" class="xliff"></a>

若要了解在故障回复（反向复制）情况下会发生什么，让我们假设 VMNetwork1-Beijing 已映射到VMNetwork1-Shanghai，设置如下。

**虚拟机** | **连接到 VM 网络**
---|---
VM1 | VMNetwork1-Network
VM2（VM1 的副本） | VMNetwork1-Shanghai

在采用这些设置的条件下，我们看看在一些可能的方案中会发生什么情况。

**方案** | **结果**
---|---
在故障转移后，VM-2 的网络属性没有发生任何变化。 | VM-1 保持连接到源网络。
故障转移且断开连接后，VM-2 的网络属性已更改。 | VM-1 已断开连接。
VM-2 的网络属性在故障转移后发生更改并连接到 VMNetwork2-Shanghai。 | 如果未映射 VMNetwork2-Shanghai，则 VM-1 将断开连接。
VMNetwork1-Shanghai 的网络映射已更改。 | VM-1 将连接到现已映射到 VMNetwork1-Shanghai 的网络。

## 后续步骤
<a id="next-steps" class="xliff"></a>

了解[规划网络基础结构](site-recovery-network-design.md)。