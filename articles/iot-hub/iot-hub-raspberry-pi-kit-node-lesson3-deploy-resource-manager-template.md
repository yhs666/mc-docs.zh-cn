---
title: "创建 Azure Function App 和 Azure 存储帐户 | Azure"
description: "Azure 函数应用可侦听 Azure IoT 中心事件、处理传入消息以及将其写入到 Azure 表存储。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "在云中存储数据, 云中存储的数据, iot 云服务"
ms.assetid: 6c58de85-c5c4-4989-bb5e-08c45c549966
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 11/28/2016
ms.date: 05/08/2017
ms.author: v-yiso
ms.openlocfilehash: 74478ab4fc12d2324197831752105eaae8cab01e
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>创建 Azure Function App 和 Azure 存储帐户
Azure Functions 是一个用于轻松在云中运行函数（小段代码）的解决方案。 Azure Function App 主导函数在 Azure 中的执行。

## <a name="what-you-will-do"></a>执行的操作
使用 Azure Resource Manager 模板创建 Azure 函数应用和 Azure 存储帐户。 Azure 函数应用可侦听 Azure IoT 中心事件、处理传入消息以及将其写入到 Azure 表存储。 如果有任何问题，请在[故障排除页面](./iot-hub-raspberry-pi-kit-node-troubleshooting.md)上查找解决方案。

## <a name="what-you-will-learn"></a>你要学习的知识
本文介绍：

* 如何使用 [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) 部署 Azure 资源。
* 如何使用 Azure 函数应用处理 IoT 中心消息并将其写入到 Azure 表存储的表中。

## <a name="what-you-need"></a>需要什么
用户必须已成功完成：
* [Raspberry Pi 3 入门](./iot-hub-raspberry-pi-kit-node-get-started.md)
* [创建 Azure IoT 中心](./iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="open-the-sample-app"></a>打开示例应用
通过运行以下命令在 Visual Studio Code 中打开示例项目：

```bash
cd Lesson3
code .
```

![存储库结构](./media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* `app` 子文件夹中的 `app.js` 文件是关键源文件。 此源文件包含的代码可将一条消息发送到 IoT 中心 20 次，并且在每次发送消息时使 LED 闪烁。
* `arm-template.json` 文件是 Azure Resource Manager 模板，其中包含一个 Azure 函数应用和一个 Azure 存储帐户。
* `arm-template-param.json` 文件是 Azure Resource Manager 模板使用的配置文件。
* `ReceiveDeviceMessages` 子文件夹包含用于 Azure 函数的 Node.js 代码。

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>在 Azure 中配置 Azure Resource Manager 模板并创建资源
在 Visual Studio Code 中更新 `arm-template-param.json` 文件。

![Azure Resource Manager 模板参数](./media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* 将 [your IoT Hub name] 替换为你在[创建 IoT 中心并注册 Raspberry Pi 3](./iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md) 时指定的 {my hub name}。
* 将 **[prefix string for new resources]** 替换为你需要的任何前缀。 前缀可以确保资源名称全局唯一以避免冲突。 请勿在前缀中以短划线或数字开头。

更新 `arm-template-param.json` 文件后，请运行以下命令，将资源部署到 Azure：

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

创建这些资源约需五分钟。 在创建这些资源时，用户可以阅读下一篇文章。

## <a name="summary"></a>摘要
用户已创建 Azure 函数应用，因此可以处理 IoT 中心消息并通过 Azure 存储帐户存储这些消息。 用户现在可以部署和运行示例，以便在 Pi 上发送从设备到云的消息。

## <a name="next-steps"></a>后续步骤
[在 Raspberry Pi 3 上运行示例应用程序，以便发送从设备到云的消息](./iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)