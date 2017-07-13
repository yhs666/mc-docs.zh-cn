---
title: "创建卷"
description: "说明如何在 StorSimple 设备上添加卷。"
services: storsimple
documentationCenter: NA
authors: SharS
manager: adinah
editor: tysonn
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/01/15
ms.author: v-sharos
ms.openlocfilehash: 7c33d9f971bea4a5a9ed5b0f0d5c2062e4c87d33
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
### 创建卷
<a id="to-create-a-volume" class="xliff"></a>

1. 在设备“快速启动”页上，单击“添加卷”。 这将启动“添加卷向导”。

2. 在“添加卷”向导中的“基本设置”下：
   1. 键入卷的“名称”。
   2. 指定卷的“预配容量”。 卷容量必须介于 1 GB 和 64 TB 之间。
   3. 在下拉列表中，选择卷的“使用类型”  。 对于不经常访问的存档数据，请选择“存档卷”。 对于其他所有数据类型，请选择“主要卷”。
   4. 单击箭头图标 ![箭头图标](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) 转到下一页。

     ![添加卷](./media/storsimple-create-volume/HCS_AddVolume1M-include.png)

3. 在“其他设置”对话框中，添加新的访问控制记录 (ACR)：
   1. 为你的 ACR 提供“名称”。
   2. 在“iSCSI 发起程序名称”下，提供 Windows 主机的 iSCSI 限定名称 (IQN)。 如果没有 IQN，请按照[获取 Windows Server 主机的 IQN](#storsimple-get-iqn.md) 中所述步骤操作。
   3. 在“默认备份此卷?”下，选中“启用”复选框。 默认备份将创建于每天 22:30（设备时间）执行的策略，并创建此卷的云快照。

     > [!NOTE]
     > 在此处启用备份后，将无法还原。 需要编辑卷才能修改此设置。

     ![添加卷](./media/storsimple-create-volume/HCs_AddVolume2M-include.png)

4. 单击勾号图标 ![勾号图标](./media/storsimple-create-volume/HCS_CheckIcon-include.png)。 将使用指定设置创建卷。