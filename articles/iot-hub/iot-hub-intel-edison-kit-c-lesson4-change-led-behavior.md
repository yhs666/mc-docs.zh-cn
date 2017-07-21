---
title: "从 Azure IoT 中心更改消息的 LED 闪烁行为 | Azure"
description: "自定义这些消息，以便更改 LED 的开关行为。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "使用 arduino 控制 led"
ms.assetid: 9826c55a-0e24-4296-ae54-29b7fe66436a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 11/08/2016
ms.date: 05/08/2017
ms.author: v-yiso
ms.openlocfilehash: af22a8179143e4c0fcd2ad80768f58204025500c
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="change-the-on-and-off-behavior-of-the-led"></a>更改 LED 的开关行为
## <a name="what-you-will-do"></a>执行的操作
自定义这些消息，以便更改 LED 的开关行为。 如果有问题，可在 [故障排除页][troubleshooting]上查找解决方案。

## <a name="what-you-will-learn"></a>你要学习的知识
使用其他函数更改 LED 的开关行为。

## <a name="what-you-need"></a>需要什么
必须已成功完成 [在 Intel Edison 上运行示例应用程序，接收云到设备消息][receive-cloud-to-device-messages]这一操作。

## <a name="add-functions-to-mainc-and-gulpfilejs"></a>将函数添加到 main.c 和 gulpfile.js
1. 通过运行以下命令在 Visual Studio Code 中打开示例应用程序：

   ```bash
   cd Lesson4
   code .
   ```
2. 打开 `main.c` 文件，然后在 blinkLED() 函数之后添加以下函数：

   ```c
   static void turnOnLED()
   {
     mraa_gpio_write(context, 1);
   }

   static void turnOffLED()
   {
     mraa_gpio_write(context, 0);
   }
   ```

   ![添加了函数的 main.c 文件](./media/iot-hub-intel-edison-lessons/lesson4/updated_app_c.png)

3. 在 `receiveMessageCallback` 函数的 `else if` 块之前添加以下条件：

   ```c
   else if (0 == strcmp((const char*)value, "\"on\""))
   {
       turnOnLED();
   }
   else if (0 == strcmp((const char*)value, "\"off\""))
   {
       turnOffLED();
   }
   ```

   现在已将示例应用程序配置为通过消息响应更多指令。 “on”指令打开 LED，“off”指令关闭 LED。
4. 打开 gulpfile.js 文件，然后在函数 `sendMessage`之前添加新函数：

   ```javascript
   var buildCustomMessage = function (messageId) {
     if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
       return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
     } else if (messageId < MAX_MESSAGE_COUNT) {
       return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
     } else {
       return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
     }
   }
   ```

   ![增加了函数的 Gulpfile.js 文件][gulpfile]
5. 在 `sendMessage` 函数中，将 `var message = buildMessage(sentMessageCount);` 行替换为以下代码片段中显示的新行：

   ```javascript
   var message = buildCustomMessage(sentMessageCount);
   ```
6. 保存所有更改。

### <a name="deploy-and-run-the-sample-application"></a>部署并运行示例应用程序
运行以下命令，在 Edison 上部署并运行示例应用程序：

```bash
gulp deploy && gulp run
```

此时会看到 LED 开启两秒，然后关闭两秒。 最后一条为“停止”消息，停止示例应用程序运行。

![打开和关闭][on-and-off]

祝贺你！ 已成功自定义从 IoT 中心发送到 Edison 的消息。

### <a name="summary"></a>摘要
此可选部分介绍了如何自定义消息，使得示例应用程序能够以其他方式控制 LED 的开关行为。

<!-- Images and links -->

[troubleshooting]: ./iot-hub-intel-edison-kit-c-troubleshooting.md
[receive-cloud-to-device-messages]: ./iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md
[gulpfile]: ./media/iot-hub-intel-edison-lessons/lesson4/updated_gulpfile_c.png
[on-and-off]: ./media/iot-hub-intel-edison-lessons/lesson4/gulp_on_and_off_c.png