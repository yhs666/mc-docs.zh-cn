---
title: 了解 Azure IoT SDK | Microsoft Docs
description: 开发人员指南 - 介绍了相关链接，其指向可用于构建设备应用和后端应用的各种 Azure IoT 设备和服务 SDK。
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
origin.date: 09/14/2018
ms.author: v-yiso
ms.custom: H1Hack27Feb2017
ms.date: 10/29/2018
ms.openlocfilehash: 8ff5881bcd4794ec6fb9cddedfdf28d4ecca839e
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49453848"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>了解和使用 Azure IoT 中心 SDK

有三种软件开发工具包 (SDK) 可以用于 IoT 中心：

* **设备 SDK** 用于使用设备客户端或模块客户端生成在 IoT 设备上运行的应用。 这些应用将遥测发送到 IoT 中心，并可以选择从 IoT 中心接收消息、作业、方法或孪生更新。  还可以使用模块客户端为 [Azure IoT Edge 运行时](../iot-edge/about-iot-edge.md)创建[模块](../iot-edge/iot-edge-modules.md)。

* **服务 SDK** 用于管理 IoT 中心，并且还可以选择用来发送消息、计划作业、调用直接方法，或者向 IoT 设备或模块发送所需的属性更新。


可以从[此处][lnk-benefits-blog]了解使用 Azure IoT SDK 进行开发的好处。

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Azure IoT 设备 SDK

Microsoft Azure IoT 设备 SDK 包含的代码可帮助构建连接到 Azure IoT 中心服务并由这些服务管理的设备和应用程序。

适用于 .NET 的 Azure IoT 中心设备 SDK： 

* 通过 [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/) 安装
* [源代码](https://github.com/Azure/azure-iot-sdk-csharp)
* [API 参考](/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [模块参考](/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

适用于 C 的 Azure IoT 中心设备 SDK，采用 ANSI C (C99) 编写，具有可移植性和广泛的平台兼容性：

* 通过 [apt-get、MBED、Arduino IDE 或 Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md) 安装
* [源代码](https://github.com/Azure/azure-iot-sdk-c)
* [API 参考](https://azure.github.io/azure-iot-sdk-c/index.html)
* [模块参考](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_module_client.h)

适用于 Java 的 Azure IoT 中心设备 SDK： 

* 添加到 [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk) 项目
* [源代码](https://github.com/Azure/azure-iot-sdk-java)
* [API 参考](/java/api/com.microsoft.azure.sdk.iot.device)
* [模块参考](/java/api/com.microsoft.azure.sdk.iot.device._module_client?view=azure-java-stable)

适用于 Node.js 的 Azure IoT 中心设备 SDK： 

* 通过 [npm](https://www.npmjs.com/package/azure-iot-device) 安装
* [源代码](https://github.com/Azure/azure-iot-sdk-node)
* [API 参考](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [模块参考](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

适用于 Python 的 Azure IoT 中心设备 SDK： 

* 通过 [pip](https://pypi.python.org/pypi/azure-iothub-device-client/) 安装
* [源代码](https://github.com/Azure/azure-iot-sdk-python)
* API 参考：请参阅 [C API 参考](https://azure.github.io/azure-iot-sdk-c/index.html)

适用于 iOS 的 Azure IoT 中心设备 SDK： 

* 通过 [CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient) 安装
* [示例](https://github.com/Azure-Samples/azure-iot-samples-ios)
* API 参考：请参阅 [C API 参考](https://azure.github.io/azure-iot-sdk-c/index.html)

> [!NOTE]
> 有关使用语言和平台特定的程序包管理器在开发计算机上安装二进制文件和依赖项的信息，请参阅 GitHub 存储库中的自述文件。
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>操作系统平台和硬件兼容性

可以在 [Azure IoT SDK 平台支持](iot-hub-device-sdk-platform-support.md)中找到支持的 SDK 平台。

有关与特定硬件设备的 SDK 兼容性的详细信息，请参阅 [Azure IoT 认证设备目录](https://catalog.azureiotsuite.com/)或单个存储库。

## <a name="azure-iot-service-sdks"></a>Azure IoT 服务 SDK

Azure IoT 服务 SDK 包含的代码可帮助生成直接与 IoT 中心进行交互以管理设备和安全性的应用程序。

适用于 .NET 的 Azure IoT 中心服务 SDK：

* 通过 [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices/) 下载
* [源代码](https://github.com/Azure/azure-iot-sdk-csharp)
* [API 参考](/dotnet/api/microsoft.azure.devices)

适用于 Java 的 Azure IoT 中心服务 SDK： 

* 添加到 [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
) 项目
* [源代码](https://github.com/Azure/azure-iot-sdk-java)
* [API 参考](/java/api/com.microsoft.azure.sdk.iot.service)

适用于 Node.js 的 Azure IoT 中心服务 SDK： 

* 通过 [npm](https://www.npmjs.com/package/azure-iothub) 下载
* [源代码](https://github.com/Azure/azure-iot-sdk-node)
* [API 参考](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

适用于 Python 的 Azure IoT 中心服务 SDK： 

* 通过 [pip](https://pypi.python.org/pypi/azure-iothub-service-client/) 下载
* [源代码](https://github.com/Azure/azure-iot-sdk-python)

适用于 C 的 Azure IoT 中心服务 SDK： 

* 通过 [apt-get、MBED、Arduino IDE 或 Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md) 下载
* [源代码](https://github.com/Azure/azure-iot-sdk-c)

适用于 iOS 的 Azure IoT 中心服务 SDK： 

* 通过 [CocoaPod](https://cocoapods.org/pods/AzureIoTHubServiceClient) 安装
* [示例](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> 有关使用语言和平台特定的程序包管理器在开发计算机上安装二进制文件和依赖项的信息，请参阅 GitHub 存储库中的自述文件。

## <a name="device-provisioning-sdks"></a>设备预配 SDK

**Microsoft Azure 预配 SDK** 使你可以使用[设备预配服务](../iot-dps/about-iot-dps.md)将设备预配到 IoT 中心。

适用于 C# 的 Azure 预配设备和服务 SDK：
* [预配设备客户端 SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/device)
* [预配服务客户端 SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/service)

适用于 Java 的 Azure 预配设备和服务 SDK：

* [预配设备客户端 SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-device-client)
* [预配服务客户端 SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-service-client)

适用于 Node.js 的 Azure 预配设备和服务 SDK：
* [预配设备客户端 SDK](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/device)
* [预配服务客户端 SDK](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service)

适用于 Python 的 Azure 预配设备和服务 SDK：
* [预配设备客户端 SDK](https://github.com/Azure/azure-iot-sdk-python/blob/master/provisioning_device_client)
* [预配服务客户端 SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client)

适用于 C 的 Azure 预配设备和服务 SDK：
* [预配设备客户端 SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [预配服务客户端 SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_service_client)

## <a name="next-steps"></a>后续步骤

Azure IoT SDK 还提供了一组工具来帮助开发：
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics)：一种跨平台命令行工具，用于帮助诊断与 IoT 中心连接相关的问题。
* [设备资源管理器](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer)：一个 Windows 桌面应用程序，用于连接到 IoT 中心。

此 IoT 中心开发人员指南中的其他参考主题包括：

* [IoT 中心终结点][lnk-devguide-endpoints]
* [用于设备孪生、作业和消息路由的 IoT 中心查询语言][lnk-devguide-query]
* [配额和限制][lnk-devguide-quotas]
* [IoT 中心 MQTT 支持][lnk-devguide-mqtt]
* [IoT 中心 REST API 参考][lnk-rest-ref]
* [Azure IoT SDK 平台支持](iot-hub-device-sdk-platform-support.md)

<!-- Links and images -->

[lnk-c-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-dotnet-sdk]: https://github.com/Azure/azure-iot-sdk-csharp
[lnk-java-sdk]: https://github.com/Azure/azure-iot-sdk-java
[lnk-node-sdk]: https://github.com/Azure/azure-iot-sdk-node
[lnk-python-sdk]: https://github.com/Azure/azure-iot-sdk-python
[lnk-certified]: https://catalog.azureiotsuite.com/

[lnk-dotnet-ref]: /dotnet/api/microsoft.azure.devices?view=azure-dotnet
[lnk-dotnet-service-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service
[lnk-node-service-ref]: https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest

[lnk-maven-device]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk
[lnk-maven-service]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
[lnk-npm-device]: https://www.npmjs.com/package/azure-iot-device
[lnk-npm-service]: https://www.npmjs.com/package/azure-iothub
[lnk-nuget-csharp-device]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-csharp-service]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-c-package]: https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md
[lnk-pip-device]: https://pypi.python.org/pypi/azure-iothub-device-client/
[lnk-pip-service]: https://pypi.python.org/pypi/azure-iothub-service-client/

[lnk-devguide-endpoints]: ./iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: ./iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: ./iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: ./iot-hub-mqtt-support.md
[lnk-benefits-blog]: https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/
[lnk-cocoa-device]: https://cocoapods.org/pods/AzureIoTHubClient
[lnk-ios-sample]: https://github.com/Azure-Samples/azure-iot-samples-ios
[lnk-cocoa-service]: https://cocoapods.org/pods/AzureIoTHubServiceClient