---
title: 将 DevKit 设备连接到 Azure IoT Central 应用程序 | Microsoft Docs
description: 了解如何使用 IoT 即插即用以设备开发人员的身份将 MXChip IoT DevKit 设备连接到 Azure IoT Central 应用程序。
author: liydu
ms.author: v-yiso
origin.date: 08/17/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: jeffya
ms.openlocfilehash: 734e5544d4b7a5fb7c7038bfae243d29338e4fed
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883063"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application-preview-features"></a>将 MXChip IoT DevKit 设备连接到 Azure IoT Central 应用程序（预览版功能）

本文介绍如何将 MXChip IoT DevKit (DevKit) 设备连接到 Azure IoT Central 应用程序。 该设备使用 DevKit 设备的已认证 IoT 即插即用模型来配置与 IoT Central 的连接。

在本操作指南文章中，你将：

- 从 IoT Central 应用程序获取连接详细信息。
- 准备设备并将其连接到 IoT Central 应用程序。
- 在 IoT Central 中查看来自设备的遥测数据和属性。

## <a name="prerequisites"></a>先决条件

若要完成本文中的步骤，需要准备好以下资源：

- 一个 [DevKit 设备](https://aka.ms/iot-devkit-purchase)。
- 从“预览版应用程序”模板创建的 IoT Central 应用程序。  可以遵循[创建 IoT 即插即用应用程序](./quick-deploy-iot-central.md)中的步骤。

## <a name="get-device-connection-details"></a>获取设备连接详细信息

在 Azure IoT Central 应用程序中选择“管理”选项卡，然后选择“设备连接”。   请记下“范围 ID”和“主密钥”（在“查看密钥”链接中）。    确保已启用“自动批准”。 

![设备组连接详细信息](media/howto-connect-devkit/device-group-connection-details.png)

## <a name="prepare-the-device"></a>准备设备

1. 从 GitHub 下载 DevKit 设备的最新[预建 Azure IoT Central 即插即用固件](https://github.com/MXCHIP/IoTDevKit/raw/master/pnp/iotc_devkit/bin/iotc_devkit.bin)。

1. 使用 USB 线缆将 DevKit 设备连接到开发计算机。 在 Windows 中，已映射到 DevKit 设备上的存储的驱动器中会打开一个文件资源管理器窗口。 例如，该驱动器可能名为 **AZ3166 (D:)** 。

1. 将 **iotc_devkit.bin** 文件拖放到驱动器窗口。 复制完成后，设备会使用新固件重新启动。

    > [!NOTE]
    > 如果屏幕上出现类似于“无 Wi-Fi”的错误，原因是 DevKit 尚未连接到 WiFi。 

1. 在 DevKit 上，按住**按钮 B** 不放，按下再松开“重置”按钮，然后松开**按钮 B**。  现在，设备处于接入点模式。 如果屏幕上显示“IoT DevKit - AP”和配置门户 IP 地址，则可以确认处于此模式。

1. 在计算机或平板电脑上，连接到设备屏幕上显示的 WiFi 网络名称。 WiFi 网络以 **AZ-** 开头，后接 MAC 地址。 连接到此网络时，不需要通过 Internet 访问。 此状态符合预期，只是在配置设备时才要与此网络保持短时间的连接。

1. 打开 Web 浏览器并导航到 [http://192.168.0.1/](http://192.168.0.1/)。 将显示以下网页：

    ![配置 UI](media/howto-connect-devkit/config-ui.png)

    在网页上输入：

    - WiFi 网络的名称 (SSID)。
    - WiFi 网络密码。
    - 连接详细信息：可以自行选择的“设备 ID”，以及前面记下的“范围 ID”和“组 SAS 主密钥”。   

    > [!NOTE]
    > IoT DevKit 目前只能连接到 2.4 GHz Wi-Fi，由于硬件限制，它不支持 5 GHz。

1. 选择“配置设备”，然后 DevKit 设备将重新启动并运行应用程序： 

    ![重新启动 UI](media/howto-connect-devkit/reboot-ui.png)

    DevKit 屏幕会显示一条确认消息，指出应用程序正在运行：

    ![DevKit 正在运行](media/howto-connect-devkit/devkit-running.png)

DevKit 首先在 IoT Central 应用程序中注册新设备，然后开始发送数据。

## <a name="view-the-telemetry"></a>查看遥测数据

遵循此步骤在 Azure IoT Central 应用程序中查看遥测数据。

在 IoT Central 应用程序中选择“设备”选项卡，然后选择添加的设备。  在“概述”选项卡中，可以查看来自 DevKit 设备的遥测数据： 

![IoT Central 设备概述](media/howto-connect-devkit/mxchip-overview-page.png)

## <a name="review-the-code"></a>查看代码

若要查看代码，或者要修改并编译代码，请访问 [MXChip IoT DevKit 示例代码 GitHub 存储库](https://github.com/MXCHIP/IoTDevKit/tree/master/pnp)。

## <a name="next-steps"></a>后续步骤

了解如何将 DevKit 设备连接到 Azure IoT Central 应用程序后，建议接下来了解如何为自己的 IoT 设备[设置自定义设备模板](./howto-set-up-template.md)。
