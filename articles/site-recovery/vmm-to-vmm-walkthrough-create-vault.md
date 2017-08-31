---
title: "创建保管库以使用 Azure Site Recovery 将 Hyper-V 复制到辅助站点 | Azure"
description: "介绍如何在使用 Azure Site Recovery 将 Hyper-V VM 复制到辅助 System Center VMM 站点时创建保管库。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/30/2017
ms.date: 08/28/2017
ms.author: v-yeche
ms.openlocfilehash: 055cbba65eb7e478ca6fed5df4b97313c6700052
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a>步骤 5：创建保管库以将 Hyper-V 复制到辅助站点

为使用 [Azure Site Recovery](site-recovery-overview.md) 将 Hyper-V 复制到辅助站点准备本地 [System Center Virtual Machine Manager (VMM) 服务器和 Hyper-V 主机/群集](vmm-to-vmm-walkthrough-vmm-hyper-v.md)后，可创建恢复服务保管库并选择复制方案。

阅读本文后，请在底部发布评论，或者发布到 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)。

## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="choose-a-protection-goal"></a>选择保护目标

选择要复制的内容以及要复制到的位置。

1. 单击“Site Recovery” > “步骤 1: 准备基础结构” > “保护目标”。
2. 选择“到恢复站点”，然后选择“是，使用 Hyper-V”。
3. 选择“是”，指示将使用 VMM 管理 Hyper-V 主机。
4. 如果具有辅助 VMM 服务器，请选择“是”。 如果要在单个 VMM 服务器上的云之间部署复制，请单击“否”。 。

    ![选择目标](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)

## <a name="next-steps"></a>后续步骤

转到[步骤 6：设置复制源和目标](vmm-to-vmm-walkthrough-source-target.md)。

<!--Update_Description: new articles on creating site recovery vault from vmm to vmm-->