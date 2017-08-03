---
title: "使用 Azure Site Recovery 设置保管库以将物理服务器复制到 Azure | Azure"
description: "总结使用 Azure Site Recovery 设置保管库以将物理服务器复制到 Azure 所需的步骤"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: d99f2605-f417-4995-be77-5323136b814f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/27/2017
ms.date: 07/31/2017
ms.author: v-yeche
ms.openlocfilehash: cdbe663d15f26e7958a35fe2ad89509393694505
ms.sourcegitcommit: 66db84041f1e6e77ef9534c2f99f1f5331a63316
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-to-azure"></a>步骤 6：设置保管库以将物理服务器复制到 Azure

本文介绍如何设置保管库。 可以在 Azure 门户中使用 [Azure Site Recovery](site-recovery-overview.md) 服务创建保管库，并指定要从本地位置复制到 Azure 的内容。



## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>选择保护目标

选择要复制的内容以及要复制到的位置。

1. 单击“恢复服务保管库”> 保管库。
2. 在“资源”菜单中，单击“Site Recovery” > “准备基础结构” > “保护目标”。
3. 在“保护目标”中，选择“到 Azure” > “未虚拟化/其他”。

## <a name="next-steps"></a>后续步骤

转到[步骤 7：设置源和目标](physical-walkthrough-source-target.md)

<!--Update_Description: new article about walkthrought create vault from physical to azure -->