---
title: "创建手动备份"
description: "介绍如何启动手动按需备份作业。"
services: storsimple
documentationCenter: NA
authors: SharS
manager: adinah
edito**r: tysonn
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/01/2015
ms.author: v-sharos
ms.openlocfilehash: 572a7ceb0e38f02af8131c56f44c37462ddd8055
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
### <a name="to-create-a-manual-backup"></a>创建手动备份

1. 在“设备”页上，转到“备份策略”选项卡。 此选项卡会以表格形式列出所有备份策略，包括要备份的卷的策略。

2. 通过单击相应行中的任意位置（第一列除外）选择策略。 在页面底部，单击“执行备份”。 按钮将展开，显示备份选项：本地快照和云快照。 

3. 选择其中任一选项时，系统将提示你进行确认。 单击“是”。 

    ![创建手动备份 1](./media/storsimple-create-manual-backup/HCS_CreateManualBackup1-include.png)

    这将启动一个创建快照的作业。 成功创建作业后，将看到页面底部的通知。

4. 若要监视该作业，请在通知区域中单击“查看作业”（位于页面底部）。 

    ![创建手动备份 2](./media/storsimple-create-manual-backup/HCS_CreateManualBackup2-include.png)

5. 备份作业完成后，请转到“备份目录”选项卡。

6. 将筛选器选择设置为相应的设备、备份策略和时间范围。 设置筛选器后，单击勾号图标 ![勾号图标](./media/storsimple-create-manual-backup/HCS_CheckIcon-include.png) 。

  备份应显示在目录中所示的备份集列表中。