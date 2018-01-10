---
title: "在 Azure 区域之间迁移 Azure IaaS VM | Azure"
description: "使用 Azure Site Recovery 将 Azure IaaS 虚拟机从一个 Azure 区域迁移到另一个 Azure 区域。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/31/2017
ms.date: 01/01/2018
ms.author: v-yeche
ms.openlocfilehash: 428ed962f318c0a360b2ff9677dd1b5b17a2321b
ms.sourcegitcommit: 90e4b45b6c650affdf9d62aeefdd72c5a8a56793
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>使用 Azure Site Recovery 在 Azure 区域之间迁移 Azure IaaS 虚拟机
## <a name="overview"></a>概述
欢迎使用 Azure Site Recovery！ 如果想要在 Azure 区域之间迁移 Azure VM，请参阅此文章。
>[!NOTE]
>
> 要了解如何将 Azure VM 复制到其他区域以满足灾难恢复和迁移需求，请参阅[本文档](site-recovery-azure-to-azure.md)。 Azure 虚拟机的 Site Recovery 复制当前处于预览阶段。

在开始之前，请注意：

* Azure 提供两个不同的部署模型来创建和处理资源：Azure Resource Manager 模型和经典模型。 Azure 门户同时支持这两种部署模型。 无论是在 Resource Manager 中还是在经典模型中配置 Site Recovery，迁移的基本步骤都是相同的。 

<!-- Not Suitable [Azure Recovery Services Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr) -->

## <a name="prerequisites"></a>先决条件
以下是执行此部署所需的组件：

* **IaaS 虚拟机**：想要迁移的 VM。 将 VM 视为物理计算机进行迁移。

## <a name="deployment-steps"></a>部署步骤
本节介绍了 Azure 门户中的部署步骤。

1. [创建保管库](site-recovery-azure-to-azure.md#create-a-recovery-services-vault)。
2. 为需要迁移的 VM [启用复制](site-recovery-azure-to-azure.md)，并选择 Azure 作为源。 当前不支持使用托管磁盘来实现 Azure VM 的本机复制。 可使用[本文档](site-recovery-vmware-to-azure.md)中的“物理机到 Azure”选项，通过托管磁盘来迁移 VM。
3. [运行故障转移](site-recovery-failover.md)。 初始复制完成后，可以运行从一个 Azure 区域到另一个 Azure 区域的故障转移。 （可选）可以创建一个恢复计划并运行故障转移，在区域之间迁移多个虚拟机。 [详细了解](site-recovery-create-recovery-plans.md)恢复计划。

## <a name="next-steps"></a>后续步骤
若要详细了解其他复制方案，请参阅[什么是 Azure Site Recovery？](site-recovery-overview.md)

<!--Update_Description: update meta properties, wording update -->