---
title: 新建 StorSimple Manager 服务
description: 说明如何创建 StorSimple Manager 服务的新实例。
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
ms.openlocfilehash: 2d4c981a0c4065df462c99b6dc5906e9378d5ccf
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
ms.locfileid: "20227688"
---
### <a name="to-create-a-new-service"></a>新建服务

1. 使用 Microsoft 帐户凭据登录到此处的 Microsoft Azure 管理门户：[Azure 管理门户](https://manage.windowsazure.com/)。

2. 在管理门户中，依次单击“新建” > “数据服务” > “StorSimple Manager” > “快速创建”。

3. 在显示的表单中，执行以下操作：
  1. 为服务提供唯一“名称”。 这是可用于标识该服务的友好名称。 名称可以为 2 到 50 个字符，包括字母、数字和连字符。 名称必须以字母或数字开头和结尾。
  2. 为服务提供“位置”。 位置是指要部署设备的地理区域。
  3. 从下拉列表中选择一个“订阅”。 该订阅将链接到计费帐户。 如果只有一个订阅，此字段不存在。
  4. 选择“新建存储帐户”，自动通过该服务创建存储帐户。 此存储帐户将具有特殊名称，例如“storsimplebwv8c6dcnf”。
  5. 单击“创建 StorSimple Manager”创建服务。

       ![创建服务](./media/storsimple-create-new-service/HCS_CreateAService-include.png)

     随后将定向到“服务”登陆页。 创建服务需要几分钟时间。 成功创建该服务后，用户将收到相应通知，服务状态将更改为“活动”。

       ![服务创建](./media/storsimple-create-new-service/HCS_StorSimpleManagerServicePage-include.png)