---
title: "使用 Intel NUC 将网关连接到 Azure IoT 套件 | Azure"
description: "使用 Microsoft IoT 商业网关工具包和远程监视预配置解决方案。 使用 Azure IoT Edge 网关连接到远程监视解决方案，将模拟遥测数据发送到云，并响应从解决方案仪表板调用的方法。"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 07/25/2017
ms.author: v-yiso
ms.date: 09/25/2017
ms.openlocfilehash: 0709760732b9c8fe138b057355db1074d645c5a9
ms.sourcegitcommit: 9d3011bb050f232095f24e34f290730b33dff5e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>将 Azure IoT Edge 网关连接到远程监视预配置解决方案，并发送模拟遥测数据

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

本教程演示如何使用 Azure IoT Edge 来模拟要发送到远程监视预配置解决方案的温度和湿度数据。 本教程使用：

- Azure IoT Edge 实现示例网关。
- IoT 套件远程监视预配置解决方案作为基于云的后端。

## <a name="overview"></a>概述

本教程会完成以下步骤：

- 将远程监视预配置解决方案的实例部署到 Azure 订阅。 此步骤会自动部署并配置多个 Azure 服务。
- 将 Intel NUC 网关设备设置为与计算机和远程监视解决方案通信。
- 配置 IoT Edge 网关，以便发送可在解决方案仪表板上查看的模拟遥测数据。

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> 远程监视解决方案在 Azure 订阅中预配一组 Azure 服务。 部署反映实际企业体系结构。 要避免产生不必要的 Azure 使用费用，请在使用完预配置解决方案的实例后，在 azureiotsuite.com 上将其删除。 如果再次需要预配置解决方案，可以轻松地重新创建它。 若要详细了解如何在远程监视解决方案运行时减少消耗，请参阅[出于演示目的配置 Azure IoT 套件预配置解决方案][lnk-demo-config]。

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

重复前面的步骤使用设备 ID（如 **device02**）添加第二个设备。 该示例从网关中的两台模拟设备向远程监视解决方案发送数据。

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>生成自定义 IoT Edge 模块

现在可以生成自定义 IoT Edge 模块，以便网关将消息发送到远程监视解决方案。 若要详细了解如何配置网关和 IoT Edge 模块，请参阅 [Azure IoT Edge 概念][lnk-gateway-concepts]。

使用以下命令，从 GitHub 下载自定义 IoT Edge 模块的源代码：

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

使用以下命令，生成自定义 IoT Edge 模块：

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

生成脚本将 libsimulator.so 自定义 IoT Edge 模块置于生成文件夹中。

## <a name="configure-and-run-the-iot-edge-gateway"></a>配置和运行 IoT Edge 网关

现在可以配置 IoT Edge 网关，将模拟遥测数据发送到远程监视仪表板。 若要详细了解如何配置网关和 IoT Edge 模块，请参阅 [Azure IoT Edge 概念][lnk-gateway-concepts]。

> [!TIP]
> 本教程使用 Intel NUC 上的标准 `vi` 文本编辑器。 如果以前未使用过 `vi`，应完成简介教程（例如 [Unix - vi 编辑器教程][lnk-vi-tutorial]）来熟悉此编辑器。 或者，可以使用命令 `smart install nano -y` 安装更友好的 [nano](https://www.nano-editor.org/) 编辑器。

使用以下命令在 **vi** 编辑器中打开示例配置文件：

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
```

在 IoTHub 模块的配置中找到以下行：

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

将占位符值替换为在本教程开始时创建并保存的 IoT 中心信息。 IoTHubName 的值类似于 **yourrmsolution37e08**，而 IoTSuffix 的值通常为 **azure-devices.cn**。

在映射模块的配置中找到以下行：

```json
args": [
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

将 **deviceID** 和 **deviceKey** 占位符替换为此前在远程监视解决方案中创建的两个设备的 ID 和密钥。

保存所做更改。

现在可使用以下命令运行 IoT Edge 网关：

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

网关在 Intel NUC 上启动，并将模拟遥测数据发送到远程监视解决方案：

![IoT Edge 网关生成模拟遥测数据][img-simulated telemetry]

随时都可按 **Ctrl-C** 退出程序。

## <a name="view-the-telemetry"></a>查看遥测数据

IoT Edge 网关现在正将模拟遥测数据发送到远程监视解决方案。 可以在解决方案仪表板上查看遥测数据。

- 导航到解决方案仪表板。
- 从“要查看的设备”下拉列表中选择在网关中配置的两个设备之一。
- 来自网关的遥测数据显示在仪表板上。

![显示来自模拟网关设备的遥测数据][img-telemetry-display]

> [!WARNING]
> 如果让远程监视解决方案在 Azure 帐户中保持运行状态，系统会按其运行时间计费。 若要详细了解如何在远程监视解决方案运行时减少消耗，请参阅[出于演示目的配置 Azure IoT 套件预配置解决方案][lnk-demo-config]。 请在用完后从 Azure 帐户中删除预配置的解决方案。

## <a name="next-steps"></a>后续步骤

有关 Azure IoT 的更多示例和文档，请访问 [Azure IoT 开发人员中心](https://www.azure.cn/develop/iot/)。

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: ../iot-hub/iot-hub-linux-iot-edge-get-started.md