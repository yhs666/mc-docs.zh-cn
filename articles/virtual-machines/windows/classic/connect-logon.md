---
title: "登录到经典 Azure VM | Azure"
description: "使用 Azure 门户登录到使用经典部署模型创建的 Windows 虚拟机。"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 05/30/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: 5de137f21786c0cd029ca02fcb0a9cd48cebffa4
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 Azure 门户登录到 Windows 虚拟机
<a id="log-on-to-a-windows-virtual-machine-using-the-azure-portal" class="xliff"></a>
在 Azure 门户中，使用“连接”按钮启动远程桌面会话，然后登录到 Windows VM。

是否要连接到 Linux VM？ 请参阅[如何登录到运行 Linux 的虚拟机](../../linux/mac-create-ssh-keys.md)。

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../connect-logon.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> Azure 提供两个不同的部署模型用于创建和处理资源：[Resource Manager 和经典模型](../../../resource-manager-deployment-model.md)。 本文介绍如何使用经典部署模型。 Azure 建议大多数新部署使用 Resource Manager 模型。 有关如何使用 Resource Manager 模型登录到 VM 的信息，请参阅[此处](../connect-logon.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。

## 连接到虚拟机
<a id="connect-to-the-virtual-machine" class="xliff"></a>
1. 登录到 Azure 门户。
2. 单击要访问的虚拟机。 其名称在“所有资源”窗格中列出。

    ![Virtual-machine-locations](./media/connect-logon/azureportaldashboard.png)

3. 单击虚拟机仪表板顶部的命令栏上的“连接”。

    ![虚拟机的连接图标](./media/connect-logon/virtualmachine_dashboard_connect.png)

<!-- Don't know if this still applies
     I think we can zap this.
> [!TIP]
> If the **Connect** button isn't available, see the troubleshooting tips at the end of this article.
>
>
-->

## 登录到虚拟机
<a id="log-on-to-the-virtual-machine" class="xliff"></a>
[!INCLUDE [virtual-machines-log-on-win-server](../../../../includes/virtual-machines-log-on-win-server.md)]

## 后续步骤
<a id="next-steps" class="xliff"></a>
* 如果“连接”按钮处于非活动状态或者在使用远程桌面连接时遇到其他问题，请尝试重置配置。 在虚拟机仪表板中单击“重置远程访问”。

    ![Reset-remote-access](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* 如果密码出现问题，可尝试重置密码。 在虚拟机仪表板左侧，“支持 + 故障排除”下面单击“重置密码”。

    ![Reset-password](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

如果这些提示不起作用或不是所需的内容，请参阅[对与基于 Windows 的 Azure 虚拟机的远程桌面连接进行故障排除](../troubleshoot-rdp-connection.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。 此文将指导你完成诊断和解决常见问题。
