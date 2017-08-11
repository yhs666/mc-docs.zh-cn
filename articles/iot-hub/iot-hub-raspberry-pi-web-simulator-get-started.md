---
title: "连接到云的模拟 Raspberry Pi (Node.js) - 将 Raspberry Pi Web 模拟器连接到 Azure IoT 中心 | Azure"
description: "了解如何设置模拟 Raspberry Pi 并将其连接到 Azure IoT 中心，使其能够将数据发送到 Azure 云平台。 在本教程中，不需要使用物理板。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "raspberry pi 模拟器, azure iot raspberry Pi, raspberry pi iot 中心, raspberry pi 将数据发送到云, 连接到云的 raspberry pi"
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 07/07/2017
ms.author: v-yiso
ms.date: 08/14/2017
ms.openlocfilehash: 2094d808189fcddc8567fef12ae32d448beb0100
ms.sourcegitcommit: cd0f14ddb0bf91c312d5ced9f38217cfaf0667f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a>将 Raspberry Pi 联机模拟器连接到 Azure IoT 中心 (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

在本教程中，首先学习有关使用 Raspberry Pi 联机模拟器的基础知识。 然后将学习如何使用 [Azure IoT 中心](./iot-hub-what-is-iot-hub.md)将 Pi 模拟器无缝连接到云。 

如有物理设备，请访问[将 Raspberry Pi 连接到 Azure IoT 中心](./iot-hub-raspberry-pi-kit-node-get-started.md)。 

## <a name="what-you-do"></a>准备工作

* 了解 Raspberry Pi 联机模拟器的基础知识。
* 创建 IoT 中心。
* 在 IoT 中心内为 Pi 注册设备。
* 在 Pi 上运行示例应用程序，将模拟传感器数据发送到 IoT 中心。

将模拟 Raspberry Pi 连接到所创建的 IoT 中心。 然后使用模拟器运行示例应用程序，生成传感器数据。 最后，将传感器数据发送到 IoT 中心。

## <a name="what-you-learn"></a>学习内容

* 如何创建 Azure IoT 中心以及如何获取新的设备连接字符串。 如果没有 Azure 帐户，只需花费几分钟就能[创建一个 Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。
* 如何使用 Raspberry Pi 联机模拟器。
* 如何将传感器数据发送到 IoT 中心。

## <a name="overview-of-raspberry-pi-web-simulator"></a>Raspberry Pi Web 模拟器概述

单击按钮，启动 Raspberry Pi 联机模拟器。

> [!div class="button"]
[启动 Raspberry Pi 模拟器](https://azure-samples.github.io/raspberry-pi-web-simulator/)

Web 模拟器中有三个区域。
* 装配区 - 默认电路是 Pi 与 BME280 传感器和 LED 连接。 在预览版中该区域是锁定的，因此当前无法执行自定义操作。
* 编码区 - 一个联机代码编辑器，用于使用 Raspberry Pi 进行编码。 默认示例应用程序有助于从 BME280 传感器收集传感器数据，并发送到 Azure IoT 中心。 应用程序与实际 Pi 设备是完全兼容的。 
* 集成控制台窗口 - 会显示代码的输出。 此窗口顶部有三个按钮。
   * 运行 - 在编码区中运行应用程序。
   * 重置 - 将编码区重置为默认示例应用程序。
   * 折叠/展开 - 按钮位于右侧，用于折叠/展开控制台窗口。

> [!NOTE] 
Raspberry Pi Web 模拟器现于预览版中提供。 我们希望通过 [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator) 听到你的建议。 [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator) 上公开了源代码。

![Pi 联机模拟器概述](./media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a>在 Pi Web 模拟器上运行示例应用程序

1. 在编码区中，请确保正在使用默认示例应用程序。 将 15 行中的占位符替换为 Azure IoT 中心设备的连接字符串。
   ![替换设备连接字符串](./media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. 单击“运行”，或键入 `npm start`，即可运行应用程序。


应看到以下输出，其中显示传感器数据以及发送至 IoT 中心的消息![输出 - 从 Raspberry Pi 发送到 IoT 中心的传感器数据](./media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## <a name="next-steps"></a>后续步骤

此时已运行示例应用程序，收集传感器数据并将其发送到 IoT 中心。

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!--Update_Description:update meta properties and wording-->