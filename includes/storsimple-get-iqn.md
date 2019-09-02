---
title: 获取 Windows 主机的 IQN
description: 说明如何获取运行 Windows Server 2012 的计算机的 iSCSI 限定名称 (IQN)。
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
ms.date: 04/01/2015
ms.author: v-sharos
ms.openlocfilehash: 8fedb1bbcc44668e89a89970a3293ed7c4cdec98
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
ms.locfileid: "20227652"
---
### <a name="to-get-the-iqn-of-a-windows-host"></a>获取 Windows 主机的 IQN

1. 在 Windows 主机上启动 Microsoft iSCSI 发起程序。

2. 在“iSCSI 发起程序属性”窗口中的“配置”选项卡上，选择并复制“发起程序名称”字段中的字符串。

    ![iSCSI 发起程序属性](./media/storsimple-get-iqn/HCS_iSCSIInitiatorPropertiesFigureIQN-include.png)

3. 保存此字符串。