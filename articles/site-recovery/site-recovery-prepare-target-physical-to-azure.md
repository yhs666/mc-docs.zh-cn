---
title: "准备目标（物理到 Azure）| Azure"
description: "本文介绍如何准备 Azure 环境，以便开始将运行 Windows 或 Linux 的物理服务器复制到 Azure 中。"
services: site-recovery
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
origin.date: 11/23/2017
ms.date: 01/01/2018
ms.author: v-yeche
ms.openlocfilehash: 0186e66e84d000c0f8cc85cb43e50eab80f47a29
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="prepare-target-vmware-to-azure"></a>准备目标（VMware 到 Azure）
> [!div class="op_single_selector"]
> * [VMware 到 Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [物理机到 Azure](./site-recovery-prepare-target-physical-to-azure.md)

本文介绍如何准备 Azure 环境以将运行 Windows 或 Linux 的物理服务器 (x64) 复制到 Azure。

## <a name="prerequisites"></a>先决条件

本文假设：
- 已创建恢复服务保管库来保护物理服务器。 可通过 [Azure 门户](http://portal.azure.cn)创建恢复服务保管库。
- 已[设置本地环境](./site-recovery-set-up-physical-to-azure.md)，用于将物理服务器复制到 Azure。

## <a name="prepare-target"></a>准备目标

完成“步骤 1: 选择保护目标”和“步骤 2: 准备源”后，会转到“步骤 3: 目标”

![准备目标](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. 订阅：从下拉菜单中，选择要将物理服务器复制到的“订阅”。
2. **部署模型：**选择部署模型（经典或资源管理器）

根据选择的部署模型，系统会运行验证来确保目标订阅至少拥有一个兼容的存储帐户和虚拟网络，可供将物理服务器复制和故障转移到其中。

成功完成验证后，单击“确定”转到下一步。

如果你没有兼容的资源管理器存储帐户或虚拟网络，则可单击页面顶部的“+ 存储帐户”或“+ 网络”按钮来创建一个。

## <a name="next-steps"></a>后续步骤
[配置复制设置](./site-recovery-setup-replication-settings-vmware.md)。

<!--Update_Description: update meta properties, wording udpate  -->