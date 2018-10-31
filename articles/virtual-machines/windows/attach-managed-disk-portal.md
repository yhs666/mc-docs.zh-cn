---
title: 将托管数据磁盘附加到 Windows VM - Azure | Azure
description: 如何使用 Azure 门户将托管数据磁盘附加到 Windows VM
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 10/08/2018
ms.date: 10/22/2018
ms.author: v-yeche
ms.openlocfilehash: 18581546baa5b2fa40f7892e7364d2a203a58468
ms.sourcegitcommit: c5529b45bd838791379d8f7fe90088828a1a67a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50034884"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>使用 Azure 门户将托管数据磁盘附加到 Windows VM

本文介绍如何使用 Azure 门户将新的托管数据磁盘附加到 Windows 虚拟机 (VM)。 VM 的大小决定了可以附加的数据磁盘数量。 有关详细信息，请参阅[虚拟机的大小](sizes.md)。

## <a name="add-a-data-disk"></a>添加数据磁盘

1. 在 [Azure 门户](https://portal.azure.cn)的左侧菜单中选择“虚拟机”。
2. 从列表中选择一个虚拟机。
3. 在“虚拟机”页上，选择“磁盘”。
4. 在“磁盘”页上，选择“添加数据磁盘”。
5. 在新磁盘的下拉列表中，选择“创建磁盘”。
6. 在“创建托管磁盘”页面中，键入磁盘名称，并根据需要调整其他设置。 完成操作后，选择“创建”。
7. 在“磁盘”页中，选择“保存”以保存 VM 的新磁盘配置。
8. 在 Azure 创建磁盘并将磁盘附加到虚拟机之后，新磁盘出现在“数据磁盘”下的虚拟机磁盘设置中。

## <a name="initialize-a-new-data-disk"></a>初始化新的数据磁盘

1. 连接到 VM。
1. 在正在运行的 VM 中选择 Windows“开始”菜单，然后在搜索框中输入 **diskmgmt.msc**。 “磁盘管理”控制台随即打开。
2. “磁盘管理”将识别出空的未初始化磁盘；同时会显示“初始化磁盘”窗口。
3. 确认已选择新磁盘，然后选择“确定”将其初始化。
4. 新磁盘显示为“未分配”。 右键单击磁盘上任意位置，选择“新建简单卷”。 “新建简单卷向导”窗口随即打开。
5. 完成向导中的每一步，保留所有默认值，完成后，选择“完成”。
6. 关闭“磁盘管理”。
7. 此时会显示一个弹出窗口，告知需要先格式化新磁盘，然后才能使用该磁盘。 选择“格式化磁盘”。
8. 在“格式化新磁盘”窗口中检查设置，然后选择“开始”。
9. 此时会显示一条警告，告知格式化磁盘会清除所有数据。 选择“确定” 。
10. 格式化完成后，选择“确定”。

## <a name="use-trim-with-standard-storage"></a>将 TRIM 与标准存储配合使用

如果使用标准存储 (HDD)，则应启用“TRIM”命令。 **TRIM** 命令会丢弃磁盘上未使用的块，因此，你只需为实际使用的存储付费。 使用 **TRIM** 时，如果你创建了较大的文件，后来已将其删除，则可以节省成本。 

若要检查 **TRIM** 设置，请在 Windows VM 上打开命令提示符，并输入以下命令：

```
fsutil behavior query DisableDeleteNotify
```

如果该命令返回 0，则表示正确启用了 **TRIM**。 如果返回 1，请运行以下命令启用 **TRIM**：

```
fsutil behavior set DisableDeleteNotify 0
```

从磁盘中删除数据后，可以使用 **TRIM** 运行碎片整理来确保 **TRIM** 操作刷新正常：

```
defrag.exe <volume:> -l
```

还可以格式化卷以确保修整整个卷。

## <a name="next-steps"></a>后续步骤

- 还可以[使用 PowerShell 附加数据磁盘](attach-disk-ps.md)。
- 如果应用程序需要使用 *D:* 盘存储数据，可以[更改 Windows 临时磁盘的驱动器号](change-drive-letter.md?toc=%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)。

<!-- Update_Description: update meta properties, wording update -->