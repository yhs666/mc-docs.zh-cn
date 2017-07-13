---
title: "什么是 Site Recovery？ | Azure"
description: "简要介绍 Azure Site Recovery 服务并概述讲解部署方案。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 03/14/2017
ms.date: 07/10/2017
ms.author: v-yeche
ms.openlocfilehash: 1eb6492d9ac2a69553def2cd9b7cc0e63740d312
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 什么是 Site Recovery？
<a id="what-is-site-recovery" class="xliff"></a>

欢迎使用 Azure Site Recovery 服务！ 本文提供了该服务的快速概述。

自然事件和操作失败会导致服务中断。 组织需制定业务连续性和灾难恢复 (BCDR) 策略，借此保证计划内和计划外停机期间的数据安全和应用可用性，并尽快恢复业务的正常运营状态。

Azure 恢复服务是你的 BCDR 策略的一部分。 [Azure 备份](https://www.azure.cn/home/features/back-up/)服务保证数据安全性和可恢复性。 Site Recovery 将对工作负荷进行复制、故障转移和恢复，以便它们在发生故障时保持可用。

## Site Recovery 的功能是什么？
<a id="what-does-site-recovery-provide" class="xliff"></a>

- 在云中进行灾难恢复 - 可以使用 Azure 到 Azure 灾难恢复解决方案，对在 Azure 上运行的 VM 进行复制和保护。 可将 VM 和物理服务器上运行的工作负荷复制到 Azure，而不是复制到辅助站点。 这就消除了维护辅助数据中心时面临的成本和复杂性。
- 在混合环境中进行灵活的复制 - 可复制在 Azure VM、本地 Hyper-V VM、VMware VM 和 Windows/Linux 物理服务器上支持的任何工作负荷。
- 迁移 - 可使用 Site Recovery 将本地实例和 AWS 实例迁移到 Azure VM，或在 Azure 区域之间迁移 Azure VM。
- **简化的 BCDR**- 你可以从 Azure 门户中的单个位置部署复制。  你可以针对单台计算机和多台计算机运行简单的故障转移和故障回复。
- **恢复能力**- Site Recovery 在不截获应用程序数据的情况下安排复制和故障转移。 复制的数据存储在 Azure 存储中，具有 Azure 存储提供的恢复能力。 发生故障转移时，将基于复制的数据创建 Azure VM。
- 复制性能 - Site Recovery 为 Azure VM 和 VMware VM 提供持续复制，为 Hyper-V 提供低至 30 秒的复制频率。 可以通过 Site Recovery 的自动恢复过程以及与 [Azure 流量管理器](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/)的集成缩短恢复时间目标 (RTO)
- 应用程序一致性 - 可以为恢复点配置应用程序一致性快照。 除了捕获磁盘数据以外，应用程序一致的快照还捕获内存中的所有数据以及流程中的所有事务。
- 无中断测试 - 可轻松运行测试性故障转移，在支持灾难恢复演练的同时，不会影响生产环境和正在进行的复制。
- 灵活的故障转移和恢复 - 可针对预期中断运行计划的故障转移，且不会出现数据丢失，或针对意外灾难运行计划外的故障转移，尽量减少数据丢失（取决于复制频率）。 当主站点重新可用时，可以轻松地故障回复到主站点。
- **自定义恢复计划**- 使用恢复计划可以创建模型，自定义故障转移和恢复分散在多个 VM 中的多层应用程序。 你可以对计划内的组进行排序，以及添加脚本和手动操作。 恢复计划可以与 Azure 自动化 runbook 进行集成。
- **多层应用**- 你可以创建恢复计划来针对多层应用进行有序的故障转移和恢复。 可在恢复计划中对不同层（如数据库、Web 和应用）的计算机进行分组，并自定义每个组的故障转移和启动方式。
* **与现有 BCDR 技术的集成**- Site Recovery 与其他 BCDR 技术集成。 例如，可以使用 Site Recovery 保护企业工作负荷的 SQL Server 后端，包括本机支持 SQL Server AlwaysOn 以便管理可用性组的故障转移。
* **与自动化库的集成**- 丰富的 Azure 自动化库，提供特定于应用程序的生产就绪型脚本，可以下载并与 Site Recovery 集成。
* **简单的网络管理**— Site Recovery 和 Azure 中的高级网络管理简化了应用程序网络要求，包括保留 IP 地址、配置负载均衡器，以及集成 Azure 流量管理器来提高网络切换的效率。

## 支持的功能
<a id="whats-supported" class="xliff"></a>

**支持** | **详细信息**
--- | ---
**Site Recovery 支持哪些区域？** | [支持的区域](https://www.azure.cn/support/service-dashboard/) |
**我可以复制哪些内容？** | Azure VM（预览版）、本地 VMware VM、Hyper-V VM、Windows 和 Linux 物理服务器。
**复制的计算机需要什么操作系统？** | Azure VM [支持的操作系统](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> 对于 Hyper-V VM，支持 Azure 和 Hyper-V 所支持的任何[来宾 OS](https://technet.microsoft.com/zh-cn/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows)。<br/><br/> 适用于物理服务器的[操作系统](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**可以复制到何处？** | 对于 Azure VM，可以复制其他 Azure 区域。 <br></br> 对于本地计算机，可以复制到 Azure 存储或辅助数据中心。<br/><br/> 
>[!NOTE]
>
> 对于 Hyper-V，只有 System Center VMM 云中托管的 Hyper-V 主机上的 VM 可以复制到辅助数据中心。
**我需要什么 VMware 服务器/主机？** | 可通过[支持的 vSphere 主机/vCenter 服务器](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)托管要复制的 VMware VM
可以复制哪些工作负荷**** | 可复制支持的复制计算机上运行的任何工作负荷。 另外，Site Recovery 团队已针对[多个应用](site-recovery-workload.md#workload-summary)执行了特定于应用的测试。

## 哪一种 Azure 门户？
<a id="which-azure-portal" class="xliff"></a>

* 可在 [Azure 门户](https://portal.azure.cn)中部署 Site Recovery。
* 在 Azure 经典管理门户中，可使用经典服务管理模型管理 Site Recovery。
* 经典管理门户应仅用于维护现有的 Site Recovery 部署。 不能在经典管理门户中创建新的保管库。

## 后续步骤
<a id="next-steps" class="xliff"></a>
<!-- Not Available site-recovery-azure-to-azure.md -->
* 阅读有关[工作负荷支持](site-recovery-workload.md)的更多内容
* 深入了解 [Site Recovery 体系结构和组件](site-recovery-components.md)