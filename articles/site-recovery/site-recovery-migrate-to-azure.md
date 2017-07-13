---
title: "使用 Site Recovery 迁移到 Azure | Azure"
description: "本文概述如何使用 Azure Site Recovery 将 VM 和物理服务器迁移到 Azure"
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
origin.date: 04/05/2017
ms.date: 07/10/2017
ms.author: v-yeche
ms.openlocfilehash: cf904234f250cf68c2db91c62979d8b5ddee6df4
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 使用 Site Recovery 迁移到 Azure
<a id="migrate-to-azure-with-site-recovery" class="xliff"></a>

本文概述如何使用 Azure Site Recovery 服务迁移虚拟机和物理服务器。

Site Recovery 是一项 Azure 服务，可以通过协调从本地物理服务器和虚拟机到云 (Azure) 或辅助数据中心的复制，来为 BCDR 策略提供辅助。 主要位置发生故障时，可故障转移到辅助位置，使应用和工作负荷保持可用。 当主要位置恢复正常时，你可以故障回复到主要位置。 有关详细信息，请参阅[什么是 Site Recovery？](site-recovery-overview.md) 还可使用 Site Recovery 将现有本地工作负荷迁移到 Azure，加快云进程并增益 Azure 提供的大量功能。

本文介绍 [Azure 门户](https://portal.azure.cn)中的部署。 使用 [Azure 经典管理门户](https://manage.windowsazure.cn/)可以维护现有的 Site Recovery 保管库，但无法创建新保管库。

## 迁移的意思是什么？
<a id="what-do-we-mean-by-migration" class="xliff"></a>

可以部署 Site Recovery，实现将本地 VM 和物理服务器复制到 Azure 或辅助站点。 可以复制计算机，在发生中断时将其从主站点进行故障转移，然后在主站点恢复时再将其故障回复到主站点。 除此之外，还可以使用 Site Recovery 将 VM 和物理服务器迁移到 Azure，以便用户可将其作为 Azure VM 进行访问。 迁移涉及到复制、从主站点故障转移到 Azure，以及完整的迁移动作。

## Site Recovery 可以迁移哪些工作负荷？
<a id="what-can-site-recovery-migrate" class="xliff"></a>

可执行以下操作：

- 迁移本地 Hyper-V VM、VMware VM 和物理服务器上运行的工作负荷，使其在 Azure VM 上运行。 在此方案中，还可以执行完整复制和故障回复。
- 在 Azure 区域之间迁移 [Azure IaaS VM](site-recovery-migrate-azure-to-azure.md)。 目前只有此方案才支持迁移，这意味着，故障回复不受支持。
- 将 [AWS Windows 实例](site-recovery-migrate-aws-to-azure.md)迁移到 Azure IaaS VM。 目前只有此方案才支持迁移，这意味着，故障回复不受支持。

## 迁移本地 VM 和物理服务器
<a id="migrate-on-premises-vms-and-physical-servers" class="xliff"></a>

若要迁移本地 Hyper-V VM、VMware VM 和物理服务器，遵循的步骤大致与普通复制相同。

1. 设置恢复服务保管库
2. 配置所需的管理服务器（VMware、VMM、Hyper-V - 具体取决于想要迁移哪些工作负荷），将这些服务器添加到保管库，然后指定复制设置。
3. 为需要迁移的虚拟机启用复制
4. 初始迁移之后，请运行一个快速的测试性故障转移，确保一切正常。
5. 确认复制环境正常工作后，根据方案[支持的操作](site-recovery-failover.md)使用计划内或计划外故障转移。 建议尽可能使用计划内故障转移。
6. 迁移时，不需要提交故障转移，也不需要将其删除。 对于想要迁移的每台计算机，可以选择“完成迁移”选项。
     - 在“复制的项”中，右键单击 VM，然后单击“完成迁移”。 单击“确定”完成相关操作。 若要在 VM 属性中跟踪进度，可以在“Site Recovery 作业”中监视“完成迁移”作业。
     - “完成迁移”操作会完成迁移过程，删除计算机复制设置，使计算机不再产生 Site Recovery 费用。

![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## 在 Azure 区域之间迁移
<a id="migrate-between-azure-regions" class="xliff"></a>

可以使用 Site Recovery 在区域之间迁移 Azure VM。 此方案仅支持迁移。 换而言之，可以复制 Azure VM 并将其故障转移到另一区域，但无法故障回复。 在此方案中，需要设置一个恢复服务保管库，部署本地配置服务器用于管理复制，将该服务器添加到保管库，然后指定复制设置。 为想要迁移的计算机启用复制，运行快速测试故障转移。 然后，使用“完成迁移”选项运行计划外故障转移。

## 将 AWS 迁移到 Azure
<a id="migrate-aws-to-azure" class="xliff"></a>

可将 AWS 实例迁移到 Azure VM。 此方案仅支持迁移。 换而言之，可以复制 AWS 实例并将其故障转移到 Azure，但无法故障回复。 在迁移过程中，AWS 实例的处理方式与物理服务器相同。 需要设置一个恢复服务保管库，部署本地配置服务器用于管理复制，将该服务器添加到保管库，然后指定复制设置。 为想要迁移的计算机启用复制，运行快速测试故障转移。 然后，使用“完成迁移”选项运行计划外故障转移。

## 后续步骤
<a id="next-steps" class="xliff"></a>

- [将 VMware VM 迁移到 Azure](site-recovery-vmware-to-azure.md)
- [将 VMM 云中的 Hyper-V VM 迁移到 Azure](site-recovery-vmm-to-azure.md)
- [将不包含 VMM 的 Hyper-V VM 迁移到 Azure](site-recovery-hyper-v-site-to-azure.md)
- [在 Azure 区域之间迁移 Azure VM](site-recovery-migrate-azure-to-azure.md)
- [将 AWS 实例迁移到 Azure](site-recovery-migrate-aws-to-azure.md)
