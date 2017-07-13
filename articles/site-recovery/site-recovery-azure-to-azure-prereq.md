---
title: "使用 Azure Site Recovery 复制到 Azure 时所要满足的先决条件 | Azure"
description: "本文汇总了使用 Azure Site Recovery 服务将 VM 和物理机复制到 Azure 所要满足的先决条件。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 03/27/2017
ms.date: 07/10/2017
ms.author: v-yeche
ms.openlocfilehash: 714c6d559ff3fcafd8b3becdf23d7353697a8ebf
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
#  使用 Azure Site Recovery 将 Azure 虚拟机复制到另一区域的先决条件
<a id="prerequisites-for-replicating-azure-virtual-machines-to-another-region-using-azure-site-recovery" class="xliff"></a>

> [!div class="op_single_selector"]
> * [从 Azure 复制到 Azure](site-recovery-azure-to-azure-prereq.md)
> * [从本地复制到 Azure](site-recovery-prereq.md)

Azure Site Recovery 服务可协调 Azure 虚拟机到另一 Azure 区域和本地物理服务器以及虚拟机到云 (Azure) 或辅助数据中心的复制，进而促进业务连续性和灾难恢复 (BCDR) 策略。 当主要位置发生故障时，可以故障转移到辅助位置，使应用和工作负荷保持可用。 当主要位置恢复正常时，可以故障回复到主要位置。 有关 Site Recovery 的详细信息，请参阅[什么是 Site Recovery？](site-recovery-overview.md)。

本文汇总了开始使用 Site Recovery 从本地复制到 Azure 时所要满足的先决条件。

欢迎在本文底部发表任何看法，或者在 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)上咨询技术问题。

## Azure 要求
<a id="azure-requirements" class="xliff"></a>

**要求** | **详细信息**
--- | ---
**Azure 帐户** | 一个 [Azure](http://azure.microsoft.com/) 帐户。<br/><br/> 可以从[试用版](https://www.azure.cn/pricing/1rmb-trial/)开始。
**Site Recovery 服务** | 有关 Site Recovery 定价的详细信息，请参阅[站点恢复定价](https://www.azure.cn/pricing/details/site-recovery/)。 建议在想要用作灾难恢复位置的目标 Azure 区域中创建恢复服务保管库。 例如，如果想要将在中国东部运行的源 VM 复制到“中国北部”，建议在“中国北部”创建保管库。|
Azure 配额 | 对于想要用作灾难恢复位置的目标 Azure 区域，订阅中需要具有充足的虚拟机、存储帐户和网络组件配额。 可以通过联系支持人员来添加足够的配额。
存储指南 | 请确保按照源 Azure 虚拟机的[存储指南](../storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)进行操作，避免出现任何性能问题。 如果使用默认设置，Site Recovery 将基于源配置创建所需的存储帐户。 如果自定义并选择自己的设置，请确保使用 (.../ storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) 作为源 VM。
网络指南 | 需要将 Azure VM 到特定 URL 或 IP 范围的出站连接加入允许列表。 有关更多详细信息，请参阅 [networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md)（有关复制 Azure 虚拟机的网络指南）一文。
Azure VM | 确保 Windows 或 Linux VM 上存在所有最新的根证书。 由于安全约束，如果没有最新的根证书，无法将 VM 注册到 Site Recovery。

>[!NOTE]
>若要深入了解对特定配置的支持，请阅读[支持矩阵](site-recovery-support-matrix-azure-to-azure.md)。

## 后续步骤
<a id="next-steps" class="xliff"></a>
- 详细了解 [networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md)（有关复制 Azure 虚拟机的网络指南）。
<!-- Not Available [replicating Azure virtual machines.](site-recovery-azure-to-azure.md) -->