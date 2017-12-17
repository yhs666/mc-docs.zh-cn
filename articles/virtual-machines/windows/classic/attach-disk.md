---
title: "将磁盘附加到经典 Azure VM | Azure"
description: "将数据磁盘附加到使用经典部署模型创建的 Windows 虚拟机并进行初始化。"
services: virtual-machines-windows, storage
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 02/21/2017
ms.date: 12/18/2017
ms.author: v-yeche
ms.openlocfilehash: f8b716ca6e6703bd364a9aa4120441c00e081a34
ms.sourcegitcommit: 408c328a2e933120eafb2b31dea8ad1b15dbcaac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2017
---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>将数据磁盘附加到使用经典部署模型创建的 Windows 虚拟机

本文介绍如何通过 Azure 门户将使用经典部署模型创建的新磁盘和现有磁盘附加到 Windows 虚拟机。

也可以[在 Azure 门户中将数据磁盘附加到 Linux VM](../../linux/attach-disk-portal.md)。

附加磁盘之前，请查看以下提示：

* 虚拟机的大小决定了可以附加多少个磁盘。 有关详细信息，请参阅[虚拟机大小](../../virtual-machines-windows-sizes.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。

* 若要使用高级存储，需要一个 DS 系列虚拟机。 可以从高级存储帐户和标准存储帐户通过这些虚拟机使用磁盘。 高级存储只在某些区域可用。 有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../premium-storage.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。
<!-- Not Available on GS-series virtual machine -->

* 对于新磁盘，不需要首先进行创建，因为 Azure 会在附加磁盘时创建该磁盘。

还可以[使用 Powershell 附加数据磁盘](../../virtual-machines-windows-attach-disk-ps.md)。

> [!IMPORTANT]
> Azure 提供两个不同的部署模型用于创建和处理资源：[Resource Manager 和经典模型](../../../resource-manager-deployment-model.md)。
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="find-the-virtual-machine"></a>查找虚拟机
1. 登录到 [Azure 门户](https://portal.azure.cn/)。
2. 从仪表板上列出的资源中选择虚拟机。
3. 在“设置”下的左窗格中，单击“磁盘”。

    ![打开磁盘设置](./media/attach-disk/virtualmachinedisks.png)

按照附加[新磁盘](#option-1-attach-a-new-disk)或[现有磁盘](#option-2-attach-an-existing-disk)的说明继续操作。

## <a name="option-1-attach-and-initialize-a-new-disk"></a>选项 1：附加并初始化新的磁盘

1. 在“磁盘”边栏选项卡上，单击“附加新磁盘”。
2. 检查默认设置，根据需要更新，并单击“确定” 。

    ![检查磁盘设置](./media/attach-disk/attach-new.png)

3. 在 Azure 创建磁盘并将磁盘附加到虚拟机之后，新磁盘出现在“数据磁盘” 下的虚拟机磁盘设置中。

### <a name="initialize-a-new-data-disk"></a>初始化新的数据磁盘

1. 连接到虚拟机。 有关说明，请参阅[如何连接并登录到运行 Windows 的 Azure 虚拟机](../../virtual-machines-windows-connect-logon.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。
2. 在登录虚拟机后，打开“服务器管理器” 。 在左窗格中，选择“文件和存储服务” 。

    ![打开服务器管理器](../media/attach-disk-portal/fileandstorageservices.png)

3. 选择“磁盘”。
4. “磁盘”部分会列出磁盘。 大多数情况下，虚拟机包含磁盘 0、磁盘 1 和磁盘 2。 磁盘 0 是操作系统磁盘，磁盘 1 是临时磁盘，磁盘 2 是新附加到虚拟机的数据磁盘。 数据磁盘将分区列为“未知”。

 右键单击磁盘，并选择“初始化” 。

5. 在初始化磁盘时，系统会通知用户所有的数据会被擦除。 单击“是”以确认警告并初始化磁盘  。 完成后，即会将分区列为“GPT” 。 再次右键单击磁盘，并选择“新建卷” 。

6. 使用默认值完成向导操作。 完成向导后，“卷”  部分列出新卷。 现在，磁盘处于联机状态并已准备好存储数据。

    ![已成功初始化卷](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a>选项 2：附加现有磁盘
1. 在“磁盘”边栏选项卡上，单击“附加现有磁盘”。
2. 在“附加现有磁盘”下，单击“位置”。

    ![附加现有磁盘](./media/attach-disk/attachexistingdisksettings.png)
3. 在“存储帐户” 下，选择帐户和容纳 .vhd 文件的容器。

    ![查找 VHD 位置](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. 选择 .vhd 文件
5. 在“附加现有磁盘”下，刚才选择的文件出现在“VHD 文件”中。 单击 **“确定”**。
6. 在 Azure 将磁盘附加到虚拟机之后，磁盘出现在“数据磁盘” 下的虚拟机磁盘设置中。

## <a name="use-trim-with-standard-storage"></a>将 TRIM 与标准存储配合使用

如果使用标准存储 (HDD)，应启用 TRIM。 TRIM 会放弃磁盘上未使用的块，以便仅对实际使用的存储进行收费。 使用 TRIM 可以节省成本，涉及删除大型文件时生成的未使用块。

可以运行此命令来检查 TRIM 设置。 在 Windows VM 上打开命令提示符，并键入：

```
fsutil behavior query DisableDeleteNotify
```

如果该命令返回 0，则表示正确启用了 TRIM。 如果返回 1，请运行以下命令启用 TRIM：
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a>后续步骤
如果应用程序需要使用 D: 盘存储数据，可以[更改 Windows 临时磁盘的驱动器号](../../virtual-machines-windows-change-drive-letter.md)。

## <a name="additional-resources"></a>其他资源
[关于虚拟机的磁盘和 VHD](../../virtual-machines-linux-about-disks-vhds.md)
<!--Update_Description: update meta properties, wording update -->