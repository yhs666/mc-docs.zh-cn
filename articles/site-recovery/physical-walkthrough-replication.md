---
title: "设置复制策略以便使用 Azure Site Recovery 将物理服务器复制到 Azure | Azure"
description: "总结了设置复制策略以便使用 Azure Site Recovery 服务将本地物理服务器复制到 Azure 存储的必要步骤"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/27/2017
ms.date: 07/31/2017
ms.author: v-yeche
ms.openlocfilehash: 7e88377d4660f8c5ea755cc1e727da0abc0db0e1
ms.sourcegitcommit: 66db84041f1e6e77ef9534c2f99f1f5331a63316
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-to-azure"></a>步骤 8：设置复制策略以便将物理服务器复制到 Azure

本文介绍如何设置复制策略，以便在 Azure 门户中使用 [Azure Site Recovery](site-recovery-overview.md) 服务将 Windows/Linux 物理服务器复制到 Azure。



## <a name="configure-a-policy"></a>配置策略

1. 依次单击“Site Recovery 基础结构” > “复制策略” > “+复制策略”。
2. 在“创建复制策略”中指定策略名称。
3. 在“RPO 阈值”中：指定 RPO 限制。 此值指示创建数据恢复点的频率。 如果连续复制超出此限制，则会生成警报。
4. 在“恢复点保留期”中，针对每个恢复点指定保留期窗口的长度（以小时为单位）。 可将复制的虚拟机恢复到窗口中的任何点。 复制到高级存储的计算机最多支持 24 小时的保留期，复制到标准存储的计算机最多支持 72 小时的保留期。
5. 在“应用一致性快照频率”中，指定创建包含应用程序一致性快照的恢复点的频率（以分钟为单位）。 单击“确定”创建该策略。

    ![复制策略](./media/physical-walkthrough-replication/gs-replication2.png)
8. 当你创建新策略时，该策略将自动与配置服务器关联。 默认情况下将自动创建一个匹配策略用于故障回复。 例如，如果复制策略是 rep-policy，则故障回复策略将是 rep-policy-failback。 从 Azure 启动故障回复之前，不会使用此策略。

## <a name="next-steps"></a>后续步骤

转到[步骤 9：安装移动服务](physical-walkthrough-install-mobility.md)

<!--Update_Description: new article about walkthrought replication from physical to azure -->