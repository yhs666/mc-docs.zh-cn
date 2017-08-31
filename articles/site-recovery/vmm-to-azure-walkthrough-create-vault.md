---
title: "为使用 Azure Site Recovery 执行 Hyper-V 到 Azure 的复制（使用 System Center VMM）设置保管库 | Azure"
description: "总结设置保管库以使用 Azure Site Recovery 执行 Hyper-V 到 Azure 的复制（使用 VMM）所需的步骤"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: b3cd6f03-c33c-406d-91d4-5cba69f79abd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 07/23/2017
ms.date: 08/28/2017
ms.author: v-yeche
ms.openlocfilehash: 8659e34fe912957192c96fe29cdbc900697d0405
ms.sourcegitcommit: 1ca439ddc22cb4d67e900e3f1757471b3878ca43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>步骤 7：为 Hyper-V 复制设置保管库

本文介绍如何设置保管库，并指定要从本地位置复制的内容，以便在 Azure 门户中使用 [Azure Site Recovery](site-recovery-overview.md) 服务将内容复制到 Azure。

请将评论和问题发布到这篇文章的底部，或者发布到 [Azure 恢复服务论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=hypervrecovmgr)。

## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>选择保护目标

选择要复制的内容以及要复制到的位置。

1. 单击“恢复服务保管库”> 保管库。
2. 在“资源”菜单中，单击“Site Recovery” > “准备基础结构” > “保护目标”。
3. 在“保护目标”中，选择“到 Azure” > “是，使用 Hyper-V”。 选择“是”，确认正在使用 VMM。 

     ![选择目标](./media/vmm-to-azure-walkthrough-create-vault/choose-goals.png)

## <a name="next-steps"></a>后续步骤

转到[步骤 8：设置源和目标](vmm-to-azure-walkthrough-source-target.md)

<!--Update_Description: new articles on site recovery vmm to azure to create vaults-->