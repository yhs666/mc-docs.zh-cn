---
title: "为使用 Azure Site Recovery 执行 Hyper-V 到 Azure 的复制（不使用 System Center VMM）设置保管库 | Azure"
description: "总结为使用 Azure Site Recovery 执行 Hyper-V 到 Azure 的复制设置保管库所需的步骤"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: a936b3e2-0dbe-47ac-a98e-5285d96dc786
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
origin.date: 06/22/2017
ms.date: 07/31/2017
ms.author: v-yeche
ms.openlocfilehash: 5cd6b4941f38a58944cf187dfefd061872131c21
ms.sourcegitcommit: 66db84041f1e6e77ef9534c2f99f1f5331a63316
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>步骤 7：为 Hyper-V 复制设置保管库

本文介绍如何设置保管库，并指定要从本地位置复制的内容，以便在 Azure 门户中使用 [Azure Site Recovery](site-recovery-overview.md) 服务将内容复制到 Azure。



## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>选择保护目标

选择要复制的内容以及要复制到的位置。

1. 单击“恢复服务保管库”> 保管库。
2. 在“资源”菜单中，单击“Site Recovery” > “准备基础结构” > “保护目标”。
3. 在“保护目标”中，选择“到 Azure” > “是，使用 Hyper-V”。 选择“否”，确认未使用 VMM。 

    ![选择目标](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)

## <a name="next-steps"></a>后续步骤

转到[步骤 8：设置源和目标](hyper-v-site-walkthrough-source-target.md)

<!--Update_Description: new article about walkthrought create vault from hyper-v to azure  -->