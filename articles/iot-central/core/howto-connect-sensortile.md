---
title: 将 SensorTile.box 设备连接到 Azure IoT Central 应用 | Microsoft Docs
description: 了解如何以设备开发人员的身份将 SensorTile.box 设备连接到 Azure IoT Central 应用程序。
author: sarahhubbard
ms.author: sahubbar
origin.date: 08/24/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: sandeep.pujar
ms.openlocfilehash: 3e14d227f274c206f529bdd685728dc9272c2e8d
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883176"
---
# <a name="connect-sensortilebox-device-to-your-azure-iot-central-application"></a>将 SensorTile.box 设备连接到 Azure IoT Central 应用程序

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

本文介绍如何以设备开发人员的身份将 SensorTile.box 设备连接到 Microsoft Azure IoT Central 应用程序。

## <a name="before-you-begin"></a>准备阶段

若要完成本文中的步骤，需要准备好以下资源：

* 一台 SensorTile.box 设备。 有关详细信息，请参阅 [SensorTile.box](https://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mems-motion-sensor-eval-boards/steval-mksbox1v1.html)。
* 安装在 Android 设备上的 ST BLE 传感器应用，可[从此处下载](https://play.google.com/store/apps/details?id=com.st.bluems)。 有关详细信息，请访问：[ST BLE 传感器](https://www.st.com/stblesensor)
* 从“DevKits”应用程序模板创建的 Azure IoT Central 应用程序。  有关详细信息，请参阅[创建应用程序快速入门](quick-deploy-iot-central.md)。
* 访问“设备模板”页，单击“+ 新建”，然后选择“SensorTile.box”模板，将“SensorTile.box”设备模板添加到 IoT Central 应用程序。    

### <a name="get-your-device-connection-details"></a>获取设备连接详细信息

在 Azure IoT Central 应用程序中，从“SensorTile.box”设备模板添加真实设备，并记下设备连接详细信息：  “范围 ID”、“设备 ID”和“主密钥”：   

1. 在“设备”中添加设备。 选择“+ 新建”>“真实设备”以添加真实设备。 

    * 输入小写的**设备 ID**，或使用建议的**设备 ID**。
    * 输入**设备名称**，或使用建议的名称

    ![添加设备](media/howto-connect-sensortile/real-device.png)

1. 若要获取设备连接详细信息、**范围 ID**、**设备 ID** 和**主密钥**，请在设备页上选择“连接”。 

    ![连接详细信息](media/howto-connect-sensortile/connect-device.png)

1. 记下连接详细信息。 在下一步骤中准备 DevKit 设备时，会暂时断开 Internet 连接。

## <a name="set-up-the-sensortilebox-with-the-mobile-application"></a>在移动应用程序中设置 SensorTile.box

本部分介绍如何将应用程序固件推送到设备。 然后，介绍如何使用低功耗蓝牙 (BLE) 连接，通过 ST BLE 传感器移动应用将设备数据发送到 IoT Central。

1. 打开 ST BLE 传感器应用，并按“创建新应用”按钮。 

    ![创建应用](media/howto-connect-sensortile/create-app.png)

1. 选择“气压计”应用程序。 
1. 按上传按钮。

    ![气压计数据上传](media/howto-connect-sensortile/barometer-upload.png)

1. 按下与 SensorTile.box 关联的播放按钮。
1. 完成此过程后，SensorTile.box 将通过 BLE 流式传输温度、压力和湿度数据。

## <a name="connect-the-sensortilebox-to-the-cloud"></a>将 SensorTile.box 连接到云

本部分介绍如何将 SensorTile.box 连接到移动应用程序，然后将移动应用程序连接到云。

1. 在左侧窗格中选择“云日志记录”按钮。 

    ![云日志记录](media/howto-connect-sensortile/cloud-logging.png)

1. 选择“Azure IoT Central”作为云提供程序。 
1. 插入前面记下的“设备 ID”和“范围 ID”。

    ![凭据](media/howto-connect-sensortile/credentials.png)

1. 选中“应用程序密钥”单选按钮。 
1. 单击“连接”，然后选择要上传的遥测数据。 
1. 几秒钟后，数据将显示在 IoT Central 应用程序仪表板上。

## <a name="sensortilebox-device-template-details"></a>SensorTile.box 设备模板详细信息

从 SensorTile.box 设备模板创建的具有以下特征的应用程序：

### <a name="telemetry"></a>遥测

| 字段名称     | 单位  | 最小值 | 最大值 | 小数位数 |
| -------------- | ------ | ------- | ------- | -------------- |
| 湿度       | %      | 30       | 90     | 1              |
| temp           | °C     | 0     | 40     | 1              |
| 压力       | mbar    | 900     | 1100    | 2              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | mg     | -2000   | 2000    | 0              |
| accelerometerY | mg     | -2000   | 2000    | 0              |
| accelerometerZ | mg     | -2000   | 2000    | 0              |
| gyroscopeX     | dps   | -3276   | 3276    | 1              |
| gyroscopeY     | dps   | -3276   | 3276    | 1              |
| gyroscopeZ     | dps   | -3276   | 3276    | 1              |
| FFT_X     |    |    |     |               |
| FFT_Y     |    |    |     |               |
| FFT_Z     |    |    |     |               |

## <a name="next-steps"></a>后续步骤

了解如何将 SensorTile.box 设备连接到 Azure IoT Central 应用程序后，建议接下来了解如何为自己的 IoT 设备[设置自定义设备模板](howto-set-up-template.md)。
