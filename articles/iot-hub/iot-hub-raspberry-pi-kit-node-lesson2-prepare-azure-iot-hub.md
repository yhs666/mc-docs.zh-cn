---
title: "创建 Azure IoT 中心并注册 Raspberry Pi 3 | Azure"
description: "使用 Azure CLI 创建资源组和 Azure IoT 中心，并在 IoT 中心标识注册表中注册 Pi。"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "raspberry pi 云, pi 云连接"
ms.assetid: 736215b6-e7e4-46f9-af30-0ded9ffa5204
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 11/28/2016
ms.date: 05/08/2017
ms.author: v-yiso
ms.openlocfilehash: 8ba02a50eb4396eb8c9e33291bbeef9a45468e9b
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>创建 IoT 中心并注册 Raspberry Pi 3
## <a name="what-you-will-do"></a>执行的操作
* 创建资源组。
* 在资源组中创建 Azure IoT 中心。
* 使用 Azure 命令行接口 (Azure CLI) 将 Raspberry Pi 3 添加到 Azure IoT 中心。

使用 Azure CLI 将 Pi 添加到 IoT 中心时，服务会为 Pi 生成一个密钥，以便通过服务进行身份验证。 如果有任何问题，请在[故障排除页面](./iot-hub-raspberry-pi-kit-node-troubleshooting.md)上查找解决方案。

## <a name="what-you-will-learn"></a>你要学习的知识
本文介绍：
* 如何使用 Azure CLI 创建 IoT 中心
* 如何在 IoT 中心为 Pi 创建设备标识

## <a name="what-you-need"></a>需要什么
* 一个 Azure 帐户
* 已安装 Azure CLI 的 Mac 或 Windows 计算机

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
> IoT 中心的名称必须全局唯一。 在 Azure 订阅下只能创建一个 F1 版的 Azure IoT 中心。

## <a name="register-pi-in-your-iot-hub"></a>在 IoT 中心注册 Pi
每个将消息发送到 IoT 中心并从 IoT 中心接收消息的设备都必须使用唯一 ID 注册。 你将使用 Azure CLI 注册 Pi，并创建自签名的 X.509 证书进行设备身份验证。

运行以下命令：

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started

# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

## <a name="summary"></a>摘要
用户已创建 IoT 中心并在 IoT 中心中使用设备标识注册 Pi。 用户已准备学习如何将消息从 Pi 发送到 IoT 中心。

## <a name="next-steps"></a>后续步骤
[创建 Azure 函数应用和 Azure 存储帐户，以便处理和存储 IoT 中心消息](./iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)