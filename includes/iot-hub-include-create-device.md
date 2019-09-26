---
title: include 文件
description: include 文件
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: b563d91c431709c5456baff5ad1dec2c6e894d4c
ms.sourcegitcommit: 6a62dd239c60596006a74ab2333c50c4db5b62be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155936"
---
<!-- put the ## header in the file that includes this file -->

本部分在 IoT 中心的标识注册表中创建设备标识。 除非设备在标识注册表中具有条目，否则设备无法连接到中心。 有关详细信息，请参阅 [IoT 中心开发人员指南](../articles/iot-hub/iot-hub-devguide-identity-registry.md#identity-registry-operations)。

1. 在 IoT 中心导航菜单中，打开“IoT 设备”  ，然后选择“新建”  以在 IoT 中心中添加设备。

    ![在门户中创建设备标识](./media/iot-hub-include-create-device/create-identity-portal-vs2019.png)

1. 在“创建设备”  中，为新设备提供名称（例如 **myDeviceId**），然后选择“保存”  。 此操作会为 IoT 中心创建设备标识。

   ![添加新设备](./media/iot-hub-include-create-device/create-a-device-vs2019.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. 创建设备后，在“IoT 设备”窗格的列表中打开该设备  。 复制**主连接字符串**以便稍后使用。

    ![设备连接字符串](./media/iot-hub-include-create-device/device-details-vs2019.png)

> [!NOTE]
> IoT 中心标识注册表仅存储用于实现 IoT 中心安全访问的设备标识。 它存储设备 ID 和密钥作为安全凭据，以及启用/禁用标志让你禁用对单个设备的访问。 如果应用程序需要存储其他特定于设备的元数据，则应使用特定于应用程序的存储。 有关详细信息，请参阅 [IoT 中心开发人员指南](../articles/iot-hub/iot-hub-devguide-identity-registry.md)。
