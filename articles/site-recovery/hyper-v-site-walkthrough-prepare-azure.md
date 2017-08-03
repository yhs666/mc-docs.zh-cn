---
title: "使用 Azure Site Recovery 准备 Azure 资源以将 Hyper-V VM（不带 System Center VMM）复制到 Azure | Azure"
description: "介绍开始使用 Azure Site Recovery 将 Hyper-V VM（不带 VMM）复制到 Azure 之前需在 Azure 中准备好的资源"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 06/21/2017
ms.date: 07/31/2017
ms.author: v-yeche
ms.openlocfilehash: d2169b38b43d4a3a22495e3a5bca1d1b721c9b06
ms.sourcegitcommit: 66db84041f1e6e77ef9534c2f99f1f5331a63316
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-to-azure"></a>步骤 5：准备 Azure 资源以将 Hyper-V 复制到 Azure

使用本文中的说明准备 Azure 资源，以便可以使用 [Azure Site Recovery](site-recovery-overview.md) 服务将本地 Hyper-V VM（不带 System Center VMM）复制到 Azure。

阅读本文后，欢迎在页面底部发表任何看法，或者在 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)上咨询技术问题。

## <a name="before-you-start"></a>开始之前

请确保已阅读[先决条件](hyper-v-site-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>设置 Azure 帐户

- 获取 [Azure 帐户](http://azure.microsoft.com/)。
- 可以从[试用版](https://www.azure.cn/pricing/1rmb-trial/)开始。
- 在 [Azure Site Recovery 定价详细信息](https://www.azure.cn/pricing/details/site-recovery/)中“支持的地理位置”下，查看 Site Recovery 支持哪些区域。
- 了解 [Site Recovery 定价](site-recovery-faq.md#pricing)，并获取[定价详细信息](https://www.azure.cn/pricing/details/site-recovery/)。

## <a name="set-up-an-azure-network"></a>设置 Azure 网络

- 设置 Azure 网络。 在故障转移后创建 Azure VM 时，Azure VM 将置于此网络中。
- 该网络应位于恢复服务保管库所在的区域
- Azure 门户中的 Site Recovery 可以使用在 [Resource Manager](../resource-manager-deployment-model.md) 或经典模式下设置的网络。
- 建议在开始之前先设置网络。 否则，需在Site Recovery 部署期间执行此操作。
- 了解[虚拟网络定价](https://www.azure.cn/pricing/details/networking/)。

## <a name="set-up-an-azure-storage-account"></a>设置 Azure 存储帐户

- Site Recovery 将本地计算机复制到 Azure 存储。 发生故障转移后，通过存储创建 Azure VM。
- 设置标准/高级 [Azure 存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)来保存复制到 Azure 的数据。
- [高级存储](../storage/storage-premium-storage.md)通常用于 IO 性能一贯较高且延迟一贯较低、托管 IO 密集型工作负荷的虚拟机。
- 如果要将高级帐户用于存储复制的数据，则还需要创建一个标准存储帐户来存储复制日志，这些日志将捕获本地数据正在发生的更改。
- 根据要用于故障转移 Azure VM 的资源模型，需在 [Resource Manager 模式](../storage/storage-create-storage-account.md)或[经典模式](../storage/storage-create-storage-account-classic-portal.md)下设置帐户。
- 建议在开始之前先设置存储帐户。 否则，需要在 Site Recovery 部署期间执行此操作。 这些帐户必须位于与恢复服务保管库相同的区域中。
- 无法跨资源组移动 Site Recovery 所使用的存储帐户，不管是在同一订阅内，还是跨不同的订阅。

## <a name="next-steps"></a>后续步骤

转到[步骤 6：准备 Hyper-V 资源](hyper-v-site-walkthrough-prepare-hyper-v.md)

<!--Update_Description: new article about walkthrought prepare azure from hyper-v to azure  -->