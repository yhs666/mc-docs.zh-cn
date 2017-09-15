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
origin.date: 05/31/2017
ms.date: 09/11/2017
ms.author: v-yeche
ms.openlocfilehash: 8aa86f0d9c41da48bd584339478946eb2c22fa92
ms.sourcegitcommit: 76a57f29b1d48d22bb4df7346722a96c5e2c9458
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="prepare-target-vmware-to-azure"></a>准备目标（VMware 到 Azure）
> [!div class="op_single_selector"]
> * [VMware 到 Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [物理机到 Azure](./site-recovery-prepare-target-physical-to-azure.md)

本文介绍如何准备 Azure 环境以将运行 Windows 或 Linux 的物理服务器 (x64) 复制到 Azure。

## <a name="prerequisites"></a>先决条件

本文的假设条件如下：
- 已创建恢复服务保管库来保护物理服务器。 可通过 [Azure 门户](http://portal.azure.cn "Azure 门户")创建恢复服务保管库。
- 已[设置本地环境](./site-recovery-set-up-physical-to-azure.md)，用于将物理服务器复制到 Azure。

## <a name="prepare-target"></a>准备目标

完成“步骤 1: 选择保护目标”和“步骤 2: 准备源”后，会转到“步骤 3: 目标”

![准备目标](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **订阅：**从下拉菜单中，选择要向其复制物理服务器的订阅。
2. **部署模型：**选择部署模型（经典或资源管理器）

根据选择的部署模型，系统会运行验证来确保目标订阅至少拥有一个兼容的存储帐户和虚拟网络，可供将物理服务器复制和故障转移到其中。

成功完成验证后，单击“确定”转到下一步。

如果没有（或想要添加更多）兼容的资源管理器存储帐户或虚拟网络，可通过单击边栏选项卡顶部的“+存储帐户”或“+网络”按钮实现。

## <a name="next-steps"></a>后续步骤
[配置复制设置](./site-recovery-setup-replication-settings-vmware.md)。

<!--Update_Description: new articles on prepare target physical to azure in site recovery -->