---
title: "面向物联网的 Azure 解决方案 | Azure"
description: "概括介绍了示例 IoT 解决方案体系结构，以及其如何与设备、Azure IoT 服务、Azure IoT 设备 SDK、Azure IoT 服务 SDK 和其他 Azure 服务进行关联"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/16/2017
ms.author: v-yiso
ms.date: 08/14/2017
ms.openlocfilehash: a9b5d708dbfd5f5265519b6c81b540cdab110f53
ms.sourcegitcommit: cd0f14ddb0bf91c312d5ced9f38217cfaf0667f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>后续步骤
Azure IoT 中心是一项 Azure 服务，可在解决方案后端和数百万台设备之间实现安全可靠的双向通信。 由此，解决方案后端可以：

- 从设备大规模接收遥测。
- 将数据从设备路由到流事件处理器。
- 从设备接收文件上传。
- 将云到设备的消息发送到特定设备。

可以使用 IoT 中心来实现自己的解决方案后端。 此外，IoT 中心还包含标识注册表，可用于预配设备、其安全凭据以及其连接到 IoT 中心的权限。 若要了解有关 IoT 中心的详细信息，请参阅 [IoT 中心是什么？][lnk-iot-hub]。

若要了解 Azure IoT 中心如何实现基于标准的设备管理，从而远程管理、配置和更新设备，请参阅 [IoT 中心设备管理概述][lnk-device-management]。

若要在各种设备硬件平台和操作系统上实现客户端应用程序，可使用 Azure IoT 设备 SDK。 设备 SDK 包含一些库，有助于将遥测数据发送到 IoT 中心和接收云到设备的消息。 使用设备 SDK 时，多个网络协议可选择用于与 IoT 中心通信。 若要了解详细信息，请参阅 [设备 SDK 的相关信息][lnk-device-sdks]。

若要开始编写一些代码并运行一些示例，请参阅 [《IoT 中心入门》][lnk-getstarted] 教程。

也可能会对 [Azure IoT 套件][lnk-iot-suite]有兴趣，这是一套预配置的解决方案。 使用 IoT 套件可快速入门和扩展 IoT 项目，以应对常见的 IoT 情形，例如远程监视、资产管理和预测性维护。

[lnk-getstarted]: ./iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: ./iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: /iot-suite/
[lnk-iotdev]: https://www.azure.cn/develop/iot/
[lnk-device-management]: ./iot-hub-device-management-overview.md

<!--Update_Description:update meta properties and link references-->