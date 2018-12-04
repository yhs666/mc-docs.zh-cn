---
title: 将 Azure VM 复制到另一个 Azure 区域
description: 本快速入门提供将一个 Azure 区域中的 Azure VM 复制到其他区域所需的步骤。
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: quickstart
origin.date: 10/10/2018
ms.date: 11/19/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 733b3e15097d770c8bee879ad6340206e846ddb4
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52663557"
---
# <a name="replicate-an-azure-vm-to-another-azure-region"></a>将 Azure VM 复制到另一个 Azure 区域

[Azure Site Recovery](site-recovery-overview.md) 服务通过在计划内和计划外中断期间使商业应用程序保持启动和运行状态，有助于实施业务连续性和灾难恢复 (BCDR) 策略。 Site Recovery 管理并安排本地计算机和 Azure 虚拟机 (VM) 的灾难恢复，包括复制、故障转移和恢复。

本快速入门介绍如何将 Azure VM 复制到不同的 Azure 区域。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="log-in-to-azure"></a>登录 Azure

在 http://portal.azure.cn 登录 Azure 门户。

## <a name="enable-replication-for-the-azure-vm"></a>为 Azure VM 启用复制

1. 在 Azure 门户中，单击“虚拟机”，并选择要复制的 VM。

2. 在“操作”中，单击“灾难恢复”。
3. 在“配置灾难恢复” > “目标区域”中，选择要复制到的目标区域。
4. 在本快速入门中，接受其他默认设置。
5. 单击“启用复制”。 这将启动用于为 VM 启用复制的作业。

    ![启用复制](media/azure-to-azure-quickstart/enable-replication1.png)

## <a name="verify-settings"></a>验证设置

复制作业完成后，可以检查复制状态、修改复制设置和测试部署。

1. 在 VM 菜单中，单击“灾难恢复”。
2. 可以验证复制运行状况、已创建的恢复点以及映射中的源和目标区域。

   ![复制状态](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>清理资源

对主要区域中的 VM 禁用复制时，该 VM 会停止复制：

- 将自动清除源复制设置。
- 对 VM 的 Site Recovery 计费也会停止。

请按如下所述停止复制：

1. 选择 VM。
2. 在“灾难恢复”中，单击“禁用复制”。

   ![禁用复制](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>后续步骤

在本快速入门中，将单个 VM 复制到了次要区域。

> [!div class="nextstepaction"]
> [为 Azure VM 配置灾难恢复](azure-to-azure-tutorial-enable-replication.md)

<!-- Update_Description: update meta properties -->