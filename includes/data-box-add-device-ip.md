---
author: WenJason
ms.service: databox
ms.topic: include
origin.date: 12/07/2018
ms.date: 06/10/2019
ms.author: v-jay
ms.openlocfilehash: 0a6268d1a7c4305fcc846d152f3bc6846f390056
ms.sourcegitcommit: 67a78cae1f34c2d19ef3eeeff2717aa0f78de38e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66726505"
---
1. 登录到 Data Box 设备。 请确保该设备已解锁。

    ![Data Box 仪表板](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. 转到“设置网络接口”  。 记下用来连接到客户端的网络接口的设备 IP 地址。

    ![Data Box 仪表板](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. 转到“连接和复制”  ，并单击“Rest”  。

    ![Data Box 仪表板](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. 从“访问存储帐户和上传数据”  对话框中，复制 **Blob 服务终结点**。

    ![Data Box 仪表板](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. 以管理员身份启动**记事本**，并打开位于 `C:\Windows\System32\Drivers\etc` 的 **hosts** 文件。
6. 将以下条目添加到 **hosts** 文件中：`<device IP address> <Blob service endpoint>`
7. 请使用以下图像作为参考。 保存 **hosts** 文件。

    ![Data Box 仪表板](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
