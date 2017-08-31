---
title: "准备 Azure 资源以便使用 Azure Site Recovery 将本地 VMware VM 复制到 Azure | Azure"
description: "介绍需要先在 Azure 中准备哪些资源，才能开始使用 Azure Site Recovery 将本地 VMware VM 复制到 Azure"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/27/2017
ms.date: 08/28/2017
ms.author: v-yeche
ms.openlocfilehash: 6c726c549dde47f6bf97d7668049e1c756af2da1
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="step-5-prepare-azure-resources-for-vmware-replication-to-azure"></a>步骤 5：准备 Azure 资源以便将 VMWare 复制到 Azure

请按照本文中的说明操作，准备 Azure 资源，以便可以使用 [Azure Site Recovery](site-recovery-overview.md) 服务将本地计算机复制到 Azure。

请将评论和问题发布到这篇文章的底部，或者发布到 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)。

## <a name="before-you-start"></a>开始之前

请确保已阅读[先决条件](vmware-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>设置 Azure 帐户

- 获取 [Azure 帐户](https://www.azure.cn/pricing/1rmb-trial-full/)。
- 可以从[试用版](https://www.azure.cn/pricing/1rmb-trial/)开始。
- 在 [Azure Site Recovery 定价详细信息](https://www.azure.cn/pricing/details/site-recovery/)中“支持的地理位置”下，查看 Site Recovery 支持哪些区域。
- 了解 [Site Recovery 定价](site-recovery-faq.md#pricing)，并获取[定价详细信息](https://www.azure.cn/pricing/details/site-recovery/)。

## <a name="set-up-an-azure-network"></a>设置 Azure 网络

- 设置 Azure 网络。 故障转移后创建 Azure VM 时，Azure VM 将置于此网络中。
- Azure 门户中的 Site Recovery 可以使用在 [Resource Manager](../resource-manager-deployment-model.md) 或经典模式下设置的网络。
- 该网络应位于恢复服务保管库所在的区域
- 了解[虚拟网络定价](https://www.azure.cn/pricing/details/networking/)。
- 详细了解故障转移后的 [Azure VM 连接性](site-recovery-network-design.md)。

## <a name="set-up-an-azure-storage-account"></a>设置 Azure 存储帐户

- Site Recovery 将本地计算机复制到 Azure 存储。 发生故障转移后，通过存储创建 Azure VM。
- 设置用于所复制数据的 [Azure 存储帐户](../storage/common/storage-create-storage-account.md#create-a-storage-account)。
- Azure 门户中的 Site Recovery 可以使用在 Resource Manager 或经典模式下设置的存储帐户。
- 存储帐户可以是标准帐户，也可以是[高级](../storage/common/storage-premium-storage.md)帐户。
- 如果设置高级帐户，还需要使用额外的标准帐户来记录数据。

## <a name="next-steps"></a>后续步骤

转到[步骤 6：准备 VMware 资源](vmware-walkthrough-prepare-vmware.md)

<!--Update_Description: new articles on site recovery prepare azure from vmware to azure-->