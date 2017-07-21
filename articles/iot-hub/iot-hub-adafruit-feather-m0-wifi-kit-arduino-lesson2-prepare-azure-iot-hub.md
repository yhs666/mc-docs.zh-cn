---
title: "创建 Azure IoT 中心并注册 Adafruit Feather M0 WiFi | Azure"
description: "使用 Azure CLI 创建资源组、创建 Azure IoT 中心，以及在 Azure IoT 中心注册 Adafruit Feather M0 WiFi。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "将 arduino 连接到云, azure iot 中心, 物联网云, azure iot 中心创建设备, arduino 云"
ms.assetid: 5edc690b-7a1d-4ebc-b011-ff27bfffe6e8
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 11/13/2016
ms.date: 05/08/2017
ms.author: v-yiso
ms.openlocfilehash: f656ba41b2c636dda3ee72b32e0e7b9d12a15a4b
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a>创建 IoT 中心并注册 Adafruit Feather M0 WiFi Arduino 开发板

## <a name="what-you-will-do"></a>执行的操作
* 创建资源组。
* 在资源组中创建 Azure IoT 中心。
* 使用 Azure 命令行接口 (Azure CLI) 将 Arduino 开发板添加到 Azure IoT 中心。

使用 Azure CLI 将 Arduino 开发板添加到 IoT 中心时，服务会为 Arduino 开发板生成一个密钥，用于向服务进行身份验证。 如果有问题，可在 [故障排除页][troubleshoot]上查找解决方案。

## <a name="what-you-will-learn"></a>你要学习的知识
本文介绍：
* 如何使用 Azure CLI 创建 IoT 中心。
* 如何在 IoT 中心为 Arduino 开发板创建设备标识。

## <a name="what-you-need"></a>需要什么
* 一个 Azure 帐户
* 已安装 Azure CLI 的计算机

## <a name="create-your-iot-hub"></a>创建 IoT 中心
Azure IoT 中心用于连接、监视并管理数百万 IoT 资产。 若要创建 IoT 中心，请执行以下步骤：

1. 通过运行以下命令登录到 Azure 帐户：

   ```bash
   az login
   ```

   成功登录后，会列出所有可用的订阅。

2. 运行以下命令，设置想要使用的默认订阅：

   ```bash
   az account set --subscription {subscription id or name}
   ```

   可以在 `az login` 或 `az account list` 命令的输出中找到 `subscription ID or name`。

3. 运行以下命令，注册提供程序。 资源提供程序是指为应用程序提供资源的服务。 必须先注册提供程序，然后才能部署该提供程序提供的 Azure 资源。

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. 运行以下命令，在“美国西部”区域创建名为 iot-sample 的资源组：

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus` 是创建资源组所在的位置。 如果想要使用其他位置，可运行 `az account list-locations -o table` 来查看 Azure 支持的所有位置。

5. 运行以下命令，在 iot-sample 资源组中创建 IoT 中心：

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

默认情况下，该工具在免费定价层中创建 IoT 中心。 有关详细信息，请参阅 [Azure IoT 中心定价](https://www.azure.cn/pricing/details/iot-hub/)。

> [!NOTE]
> IoT 中心的名称必须全局唯一。
> 在 Azure 订阅下只能创建一个 F1 版的 Azure IoT 中心。

## <a name="register-your-arduino-board-in-your-iot-hub"></a>在 IoT 中心注册 Arduino 开发板
每个将消息发送到 IoT 中心并从 IoT 中心接收消息的设备都必须使用唯一 ID 注册。

运行以下命令，在 IoT 中心注册 Arduino 开发板：

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a>摘要
已创建 IoT 中心并在 IoT 中心使用设备标识注册 Arduino 开发板。 已准备好学习如何将消息从 Arduino 开发板发送到 IoT 中心。

## <a name="next-steps"></a>后续步骤
[创建 Azure Function App 和 Azure 存储帐户，以便处理和存储 IoT 中心消息][process-and-store-iot-hub-messages]。

<!-- Images and links -->

[troubleshoot]: ./iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: ./iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md