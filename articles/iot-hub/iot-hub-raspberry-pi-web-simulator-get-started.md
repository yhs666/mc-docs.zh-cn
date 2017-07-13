---
title: "连接到云的模拟 Raspberry Pi (Node.js) - 将 Raspberry Pi Web 模拟器连接到 Azure IoT 中心 | Azure"
description: "将 Raspberry Pi Web 模拟器连接到 Azure IoT 中心，以供 Raspberry Pi 将数据发送到 Azure 云。"
services: iot-hub
documentationcenter: 
author: Derek1101
manager: timtl
tags: 
keywords: "raspberry pi 模拟器, azure iot raspberry Pi, raspberry pi iot 中心, raspberry pi 将数据发送到云, 连接到云的 raspberry pi"
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 5/27/2017
ms.author: v-yiso
ms.date: 07/03/2017
ms.openlocfilehash: 385b5020f61ae17878ad01c72e847c74ad9b5e07
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 将 Raspberry Pi 联机模拟器连接到 Azure IoT 中心 (Node.js)
<a id="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs" class="xliff"></a>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

在本教程中，首先学习有关使用 Raspberry Pi 联机模拟器的基础知识。 然后将学习如何使用 [Azure IoT 中心](./iot-hub-what-is-iot-hub.md)将 Pi 模拟器无缝连接到云。 

如有物理设备，请访问[将 Raspberry Pi 连接到 Azure IoT 中心](./iot-hub-raspberry-pi-kit-node-get-started.md)。 

## 准备工作
<a id="what-you-do" class="xliff"></a>

* 了解 Raspberry Pi 联机模拟器的基础知识。
* 创建 IoT 中心。
* 在 IoT 中心内为 Pi 注册设备。
* 在 Pi 上运行示例应用程序，将模拟传感器数据发送到 IoT 中心。

将模拟 Raspberry Pi 连接到所创建的 IoT 中心。 然后使用模拟器运行示例应用程序，生成传感器数据。 最后，将传感器数据发送到 IoT 中心。

## 学习内容
<a id="what-you-learn" class="xliff"></a>

* 如何创建 Azure IoT 中心以及如何获取新的设备连接字符串。
* 如何使用 Raspberry Pi 联机模拟器。
* 如何将传感器数据发送到 IoT 中心。

## Raspberry Pi Web 模拟器概述
<a id="overview-of-raspberry-pi-web-simulator" class="xliff"></a>

单击按钮，启动 Raspberry Pi 联机模拟器。

> [!div class="button"]
[启动 Raspberry Pi 模拟器](https://azure-samples.github.io/raspberry-pi-web-simulator/build/index.html)

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


## 在 Pi Web 模拟器上运行示例应用程序
<a id="run-a-sample-application-on-pi-web-simulator" class="xliff"></a>

1. 在编码区中，请确保正在使用默认示例应用程序。 将 15 行中的占位符替换为 Azure IoT 中心设备的连接字符串。
   ![替换设备连接字符串](./media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. 单击“运行”，或键入 `npm start`，即可运行应用程序。


应看到以下输出，其中显示传感器数据以及发送至 IoT 中心的消息![输出 - 从 Raspberry Pi 发送到 IoT 中心的传感器数据](./media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## 后续步骤
<a id="next-steps" class="xliff"></a>

此时已运行示例应用程序，收集传感器数据并将其发送到 IoT 中心。

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
