---
title: 通过 Visual Studio Code 注册新设备 - Azure IoT Edge | Microsoft Docs
description: 使用 Visual Studio Code 在 Azure IoT 中心中创建新的 IoT Edge 设备并检索连接字符串
author: kgremban
manager: philmea
ms.author: v-yiso
origin.date: 06/03/2019
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f8c397479e4b4f26771d787a3e5242f175a8d138
ms.sourcegitcommit: f4351979a313ac7b5700deab684d1153ae51d725
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67845228"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>通过 Visual Studio Code 注册新 Azure IoT Edge 设备

在 Azure IoT Edge 中使用 IoT 设备之前，需要在 IoT 中心中注册这些设备。 在注册设备后，会收到一个连接字符串，该字符串可用来针对 IoT Edge 工作负荷设置设备。

本文介绍如何使用 Visual Studio Code (VS Code) 注册新 IoT Edge 设备。 有多种方法可以执行 VS Code 中的大部分操作。 本文使用了资源管理器，但你也可使用命令面板来执行相关步骤。

## <a name="prerequisites"></a>先决条件

* Azure 订阅中的 [IoT 中心](../iot-hub/iot-hub-create-through-portal.md)
* [Visual Studio Code](https://code.visualstudio.com/)
* 适用于 Visual Studio Code 的 [Azure IoT 工具](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

## <a name="sign-in-to-access-your-iot-hub"></a>登录以访问 IoT 中心

可使用适用于 Visual Studio Code 的 Azure IoT 扩展来执行与 IoT 中心相关的操作。 为了使这些操作顺利执行，你需要登录到 Azure 帐户并选择 IoT 中心。

1. 在 Visual Studio Code 中打开“资源管理器”视图  。

1. 在资源管理器底部，展开“Azure IoT 中心”部分  。

   ![展开“Azure IoT 中心设备”部分](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

1. 单击“Azure IoT 中心”部分标题中的“...”   。 如果没有看到省略号，请单击标题或将鼠标指针悬停在标题上。

4. 选择“选择 IoT 中心”  。

1. 如果尚未登录到 Azure 帐户，请按照相关提示执行此操作。

6. 选择 Azure 订阅。 

7. 选择 IoT 中心。 

## <a name="create-a-device"></a>创建设备

1. 在 VS Code 资源管理器中，展开“Azure IoT 中心设备”部分  。 

2. 单击“Azure IoT 中心设备”部分标题中的“...”   。 如果没有看到省略号，请单击标题或将鼠标指针悬停在标题上。 

3. 选择“创建 IoT Edge 设备”  。 

4. 在打开的文本框中提供设备 ID。 

在输出屏幕中，可以看到命令的结果。 其中显示有设备信息，包括所提供的“deviceId”以及可用于将物理设备连接到 IoT 中心的“connectionString”   。 

## <a name="view-all-devices"></a>查看所有设备

Visual Studio Code 资源管理器的“Azure IoT 中心”部分中列出了连接到 IoT 中心的所有设备  。 IoT Edge 设备和非 Edge 设备可通过不同的图标加以区别。事实上， **$edgeAgent** 和 **$edgeHub** 模块会部署到每个 IoT Edge 设备。

   ![查看 IoT 中心中所有的 IoT Edge 设备](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>检索连接字符串

如果已准备好设置设备，则需要连接字符串，该字符串使用物理设备在 IoT 中心内的标识链接该设备。

1. 右键单击“Azure IoT 中心”部分中的设备 ID  。

1. 选择“复制设备连接字符串”  。

   连接字符串会复制到剪贴板。 

还可从右键菜单中选择“获取设备信息”，在输出窗口中查看包括连接字符串在内的所有设备信息  。 


## <a name="next-steps"></a>后续步骤

了解如何[使用 Visual Studio Code 将模块部署到设备](how-to-deploy-modules-vscode.md)。
