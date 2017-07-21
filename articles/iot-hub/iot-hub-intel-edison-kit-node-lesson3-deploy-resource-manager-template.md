---
title: "创建 Azure Function App 和存储帐户 | Azure"
description: "Azure 函数应用可侦听 Azure IoT 中心事件、处理传入消息以及将其写入到 Azure 表存储。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "在云中存储数据, 云中存储的数据, iot 云服务"
ms.assetid: 37ee5962-95ce-40e8-8162-17e735eaec21
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 11/08/2016
ms.date: 05/08/2017
ms.author: v-yiso
ms.openlocfilehash: 01cd7e33dff643451b41a3be3df6a059c8f11036
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>创建 Azure Function App 和 Azure 存储帐户
Azure Functions 是一个用于轻松在云中运行函数（小段代码）的解决方案。 Azure Function App 主导函数在 Azure 中的执行。

## <a name="what-will-you-do"></a>执行的操作
使用 Azure Resource Manager 模板创建 Azure 函数应用和 Azure 存储帐户。 Azure 函数应用可侦听 Azure IoT 中心事件、处理传入消息以及将其写入到 Azure 表存储。 存储帐户用于从 Azure 表中读取消息的保存副本。 如果有问题，可在 [故障排除页][troubleshooting]上查找解决方案。

## <a name="what-will-you-learn"></a>学习的内容
本文介绍：
* 如何使用 [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) 部署 Azure 资源。
* 如何使用 Azure 函数应用处理 IoT 中心消息并将其写入到 Azure 表存储的表中。

## <a name="what-do-you-need"></a>所需条件
用户必须已成功完成：
- [开始使用 Intel Edison][get-started-with-your-intel-edison]
- [创建 Azure IoT 中心][create-your-azure-iot-hub]

## <a name="open-the-sample-app"></a>打开示例应用
通过运行以下命令在 Visual Studio Code 中打开示例项目：

```bash
cd Lesson3
code .
```

![存储库结构][repo-structure]

* `app` 子文件夹中的文件是重要的源文件。 此源文件包含的代码可将一条消息发送到 IoT 中心 20 次，并且在每次发送消息时使 LED 闪烁。
* `arm-template.json` 文件是 Azure Resource Manager 模板，其中包含一个 Azure 函数应用和一个 Azure 存储帐户。
* `arm-template-param.json` 文件是 Azure Resource Manager 模板使用的配置文件。
* `ReceiveDeviceMessages` 子文件夹包含用于 Azure 函数的 Node.js 代码。

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>在 Azure 中配置 Azure Resource Manager 模板并创建资源
在 Visual Studio Code 中更新 `arm-template-param.json` 文件。

![Azure Resource Manager 模板参数][arm-template-parameters]

* 将 [IoT 中心名称] 替换为 {我的中心名称}，后者在[创建 IoT 中心和注册 Intel Edison][created-your-iot-hub-and-registered-intel-edison] 时指定。
* 将 **[prefix string for new resources]** 替换为你需要的任何前缀。 前缀可以确保资源名称全局唯一以避免冲突。 请勿在前缀中以短划线或数字开头。

更新 `arm-template-param.json` 文件后，请运行以下命令，将资源部署到 Azure：

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

创建这些资源约需五分钟。 在创建这些资源时，用户可以阅读下一篇文章。

## <a name="summary"></a>摘要
用户已创建 Azure 函数应用，因此可以处理 IoT 中心消息并通过 Azure 存储帐户存储这些消息。 现在可部署和运行示例，在 Edison 上发送设备到云消息。

## <a name="next-steps"></a>后续步骤
[在 Intel Edison 上运行示例应用程序，发送设备到云消息][send-device-to-cloud-messages]。
<!-- Images and links -->

[troubleshooting]: ./iot-hub-intel-edison-kit-node-troubleshooting.md
[get-started-with-your-intel-edison]: ./iot-hub-intel-edison-kit-node-get-started.md
[create-your-azure-iot-hub]: ./iot-hub-intel-edison-kit-node-get-started.md
[repo-structure]: ./media/iot-hub-intel-edison-lessons/lesson3/repo_structure.png
[arm-template-parameters]: ./media/iot-hub-intel-edison-lessons/lesson3/arm_para.png
[created-your-iot-hub-and-registered-intel-edison]: ./iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: ./iot-hub-intel-edison-kit-node-lesson3-run-azure-blink.md