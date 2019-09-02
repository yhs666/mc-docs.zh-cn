---
title: 可选：针对服务配置新的存储帐户
description: 说明如何为 StorSimple 服务配置存储帐户。
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
ms.openlocfilehash: 2acf6720981154d3236723166f0a83aa27109a66
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
ms.locfileid: "20227668"
---
### <a name="to-configure-a-new-storage-account"></a>配置新的存储帐户

1. 在 StorSimple Manager 服务登陆页上，选择服务并双击它。 这将转到“快速启动”页。 选择“配置”页。

2. 单击“添加/编辑存储帐户”。

3. 在“添加/编辑存储帐户”对话框中，执行以下操作：

  1. 单击“新增”。
  2. 为存储帐户提供名称。
  3. 为 Microsoft Azure 存储帐户提供主“访问密钥”。
  4. 选择“启用 SSL 模式”，创建用于在设备和云之间进行网络通信的安全通道。 仅当在私有云内执行操作时才清除“启用 SSL 模式”复选框。
  5. 单击勾号图标 ![勾号图标](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png)。 成功创建存储帐户后，你将收到通知。

    ![添加存储帐户](./media/storsimple-configure-new-storage-account/HCS_AddStorageAccount-include.png)

4. 新建的存储帐户将显示在“存储帐户”下的“配置”页上。 单击“保存” ，保存新建的存储帐户。 出现确认提示时单击“确定”。