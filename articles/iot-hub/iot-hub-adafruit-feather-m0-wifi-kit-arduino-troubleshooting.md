---
title: "Adafruit Feather M0 WiFi Azure IoT 初学者工具包故障排除 | Azure"
description: "Adafruit Feather M0 WiFi Arduino 体验的故障排除页"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino 故障排除"
ms.assetid: fdcc56ff-4420-463c-8a0e-5a1d215a874f
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 11/08/2016
ms.date: 05/08/2017
ms.author: v-yiso
ms.openlocfilehash: fc60babc1efbb41c9db07ac213be3ebe137b5e30
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="troubleshooting"></a>故障排除
## <a name="hardware-issues"></a>硬件问题
若要了解如何解决 Adafruit Feather M0 WiFi Arduino 开发板的常见问题，请参阅 [官方故障排除页](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq)。

## <a name="nodejs-package-issues"></a>Node.js 包问题
### <a name="no-response-during-gulp-tasks"></a>在 Gulp 任务期间没有响应
如果在运行 gulp 任务时遇到问题，可添加 `--verbose` 选项进行调试。 请尝试使用 `Ctrl + C`终止当前 gulp 任务，然后在控制台窗口中运行以下命令，以便查看调试消息。 可以在控制台输出中查看详细的错误消息。

```bash
gulp --verbose
```

或者，可以添加 `--listen` 打开输出设备日志信息的串行端口。

```bash
gulp --listen
``` 

### <a name="npm-issues"></a>NPM 问题
请尝试使用以下命令更新 NPM 包：

```bash
npm install -g npm
```

如果问题仍然存在，请在本文末尾留下你的评论，或者在 [示例存储库][sample-repository]中创建一个 GitHub 问题。

## <a name="azure-cli-issues"></a>Azure-CLI 问题
Azure 命令行接口 (Azure CLI) 是一个预览版本。 在 [预览版安装指南](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) 中寻求解决方案。 如果命令不按预期工作，请尝试升级到最新版本的 Azure-cli。

如果使用工具时遇到任何 bug，请在 GitHub 存储库的“问题”部分中记录一个[问题](https://github.com/Azure/azure-cli/issues)。

如需常见问题的疑难解答帮助，请查看 [自述文件](https://github.com/Azure/azure-cli/blob/master/README.rst)。

如果遇到“找不到满足需求的版本”，请运行以下命令，将 pip 升级到最新版本。

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Python 安装问题
### <a name="legacy-installation-issues-macos"></a>旧安装问题 (macOS)
安装 pip 时，如果使用 su 权限安装的包较旧，则会引发权限错误。 之所以发生此情况是因为未完全卸载以前使用 brew (macOS) 安装的 Python。 之前安装的某些 **pip** 包由根权限创建，会导致权限错误。 删除这些通过根权限安装的包即可解决问题。 通过以下步骤完成该任务：

1. 转到：/usr/local/lib/python2.7/site-packages
2. 列出由根权限创建的包： `ls -l | grep root`
3. 卸载步骤 2 中的包： `sudo rm -rf {package name}`
4. 重新安装 Python。

## <a name="azure-iot-hub-issues"></a>Azure IoT 中心问题
如果已通过 `azure-cli`成功预配 Azure IoT 中心，且需使用工具管理连接到 IoT 中心的设备，可尝试以下工具：

### <a name="device-explorer"></a>设备资源管理器
[设备资源管理器](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) 在 Windows 本地计算机上运行，并连接到 Azure 中的 IoT 中心。 它与以下 [IoT 中心终结点](./iot-hub-devguide.md)进行通信：

* *设备标识管理* ：用于预配和管理注册到 IoT 中心的设备。
* *接收从设备到云的消息* ：用于监视从设备发送到 IoT 中心的消息。
* *发送从云到设备的消息* ：用于将消息从 IoT 中心发送到设备。

在此工具中配置 `IoT hub connection string` ，以便使用其所有功能。

### <a name="iot-hub-explorer"></a>IoT 中心资源管理器
[IoT 中心资源管理器](https://github.com/Azure/iothub-explorer) 是示例多平台 CLI 工具，可用于管理设备客户端。 可以使用该工具在标识注册表中管理设备、监视从设备到云的消息，以及发送从云到设备的命令。

若要安装 iothub-explorer 工具的最新（预发行）版本，请在命令行环境中运行以下命令：

```bash
npm install -g iothub-explorer@latest
```

可以使用以下命令获取所有 iothub-explorer 命令及其参数的更多帮助：

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure 门户
完整的 CLI 体验有助于用户创建和管理其所有 Azure 资源。 可能还需要使用 [Azure 门户](../azure-portal-overview.md)来帮助预配、管理和调试 Azure 资源。

## <a name="azure-storage-issues"></a>Azure 存储问题
[Microsoft Azure 存储资源管理器（预览版）](http://storageexplorer.com)是 Microsoft 提供的一款独立应用，可用于在 Windows、macOS 和 Linux 上处理 Azure 存储数据。 可以使用此工具连接到表并查看其中的数据。 可以使用此工具排查 Azure 存储问题。

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md