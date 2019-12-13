---
title: Azure IoT Central 中的设备连接 | Microsoft Docs
description: 本文介绍与 Azure IoT Central 中的设备连接相关的重要概念
author: dominicbetts
ms.author: v-yiso
origin.date: 08/22/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 0231fc8808205b865790b9ad2e062c576dbacedf
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883384"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Azure IoT Central 中的设备连接

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

本文介绍与 Microsoft Azure IoT Central 中的设备连接相关的重要概念。

Azure IoT Central 使用 [Azure IoT 中心设备预配服务 (DPS)](/iot-dps/about-iot-dps) 来管理所有设备注册和连接。

使用 DPS 可以：

- 使 IoT Central 能够支持大规模加入和连接设备。
- 离线生成设备凭据和配置设备，而无需通过 IoT Central UI 注册设备。
- 使设备能够使用共享访问签名 (SAS) 建立连接。
- 使设备能够使用行业标准的 X.509 证书建立连接。
- 使用自己的设备 ID 在 IoT Central 中注册设备。 使用自己的设备 ID 可以简化与现有后端办公系统的集成。
- 通过一种一致的方式将设备连接到 IoT Central。

本文介绍以下四个用例：

- [使用 SAS 快速连接单个设备](#connect-a-single-device)
- [使用 SAS 大规模连接设备](#connect-devices-at-scale-using-sas)
- [使用 X.509 证书大规模连接设备](#connect-devices-using-x509-certificates)：建议在生产环境中使用这种方法。
- [直接连接而无需首先注册设备](#connect-without-registering-devices)

## <a name="connect-a-single-device"></a>连接单个设备

在体验 IoT Central 或测试设备时，此方法非常有用。 可以使用 IoT Central 应用程序中的设备连接信息，通过设备预配服务 (DPS) 将设备连接到 IoT Central 应用程序。 可以找到适用于以下语言的示例 DPS 设备客户端代码：

- [C\#](./howto-connect-raspberry-pi-csharp.md)
- [Node.js](./howto-connect-nodejs.md)

## <a name="connect-devices-at-scale-using-sas"></a>使用 SAS 大规模连接设备

若要使用 SAS 大规模地将设备连接到 IoT Central，需要注册并设置这些设备：

### <a name="register-devices-in-bulk"></a>批量注册设备

若要将大量设备注册到 IoT Central 应用程序，请使用 CSV 文件[导入设备 ID 和设备名称](howto-manage-devices.md#import-devices)。

若要检索已导入的设备的连接信息，请[从 IoT Central 应用程序导出 CSV 文件](howto-manage-devices.md#export-devices)。

> [!NOTE]
> 若要了解如何在不事先将设备注册到 IoT Central 的情况下连接设备，请参阅[在不事先注册设备的情况下连接设备](#connect-without-registering-devices)。

### <a name="set-up-your-devices"></a>设置设备

在设备代码中使用导出文件中的连接信息，使设备能够连接到 IoT Central 应用程序并向其发送数据。 有关连接设备的详细信息，请参阅[后续步骤](#next-steps)。

## <a name="connect-devices-using-x509-certificates"></a>使用 X.509 证书连接设备

在生产环境中，建议使用 X.509 证书作为 IoT Central 的设备身份验证机制。 有关详细信息，请参阅[使用 X.509 CA 证书进行设备身份验证](../../iot-hub/iot-hub-x509ca-overview.md)。

以下步骤说明如何使用 X.509 证书将设备连接到 IoT Central：

1. 在 IoT Central 应用程序中，_添加并验证中间或根 X.509 证书_用于生成设备证书：

    - 导航到“管理”>“设备连接”>“证书(X.509)”，添加用于生成叶设备证书的 X.509 根证书或中间证书。 

      ![连接设置](media/concepts-connectivity/connection-settings.png)

      如果存在安全漏洞或者主要证书设置为会过期，请使用辅助证书来减少停机时间。 在更新主要证书时，可以继续使用辅助证书来预配设备。

    - 验证证书所有权可以确保证书的上传者拥有此证书的私钥。 若要验证证书：
        - 选择“验证码”旁边的按钮以生成代码。 
        - 使用在上一步骤中生成的验证码创建 X.509 验证证书。 将该证书保存为 .cer 文件。
        - 上传已签名的验证证书，然后选择“验证”。 

          ![连接设置](media/concepts-connectivity/verify-cert.png)

1. 使用 CSV 文件在 IoT Central 应用程序中_导入并注册设备_。

1. 设置设备。  使用上传的根证书生成叶证书。 将“设备 ID”用作叶证书中的 CNAME 值。  设备 ID 应全小写。 然后使用预配服务信息为设备编程。 首次打开某个设备时，它会从 DPS 检索 IoT Central 应用程序的连接信息。

### <a name="further-reference"></a>更多参考信息

- [RaspberryPi](https://aka.ms/iotcentral-docs-Raspi-releases) 的示例实现。

- [C 编写的示例设备客户端](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)

### <a name="for-testing-purposes-only"></a>仅用于测试目的

（仅限测试）可以使用这些实用工具来生成 CA 证书和设备证书。

- 如果使用 DevKit 设备，则此[命令行工具](https://aka.ms/iotcentral-docs-dicetool)会生成一个 CA 证书，可将其添加到 IoT Central 应用程序来验证证书。

- 使用此[命令行工具](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md )：
  - 创建证书链。 请遵循 GitHub 文章中的步骤 2。
  - 将证书保存为 .cer 文件，以上传到 IoT Central 应用程序。
  - 使用 IoT Central 应用程序中的验证码生成验证证书。 请遵循 GitHub 文章中的步骤 3。
  - 在工具中使用设备 ID 作为参数，为设备创建叶证书。 请遵循 GitHub 文章中的步骤 4。

## <a name="connect-without-registering-devices"></a>在不事先注册设备的情况下连接设备

IoT Central 支持的一个重要方案是，可让 OEM 大规模制造出可在不事先注册的情况下，连接到 IoT Central 应用程序的设备。 制造商必须生成合适的凭据，并在工厂中配置设备。 首次打开设备时，它会自动连接到 IoT Central 应用程序。 IoT Central 操作员必须先批准设备，然后设备才能开始发送数据。

下图大致描述了此流程：

![连接设置](media/concepts-connectivity/device-connection-flow1.png)

以下步骤更详细地描述了此过程。 步骤根据设备身份验证使用的是 SAS 证书还是 X.509 证书而略有不同：

1. 配置连接设置：

    - **X.509 证书：** [添加并验证根/中间证书](#connect-devices-using-x509-certificates)，并在后面的步骤中用它来生成设备证书。
    - **SAS：** 复制主密钥。 此密钥是 IoT Central 应用程序的组 SAS 密钥。 在后面的步骤中，将使用该密钥来生成设备 SAS 密钥。
    ![连接设置 SAS](media/concepts-connectivity/connection-settings-sas.png)

1. 生成设备凭据
    - **X.509 证书：** 使用已添加到 IoT Central 应用程序的根证书或中间证书为设备生成叶证书。 请确保使用小写的**设备 ID** 作为叶证书中的 CNAME。 （仅限测试）使用此[命令行工具](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md )生成设备证书。
    - **SAS：** 使用此[命令行工具](https://www.npmjs.com/package/dps-keygen)生成设备 SAS 密钥。 使用前面步骤中获取的组**主密钥**。 设备 ID 必须小写。

      若要安装[密钥生成器实用程序](https://github.com/Azure/dps-keygen)，请运行以下命令：

      ```cmd/sh
      npm i -g dps-keygen
      ```

      若要从组 SAS 主密钥生成设备密钥，请运行以下命令：

      ```cmd/sh
      dps-keygen -mk:<Primary_Key(GroupSAS)> -di:<device_id>
      ```

1. 若要设置设备，请使用**范围 ID**、**设备 ID**、**X.509 设备证书**或 **SAS 密钥**刷新每台设备。

1. 然后打开设备，使其连接到 IoT Central 应用程序。 打开设备时，它首先会连接到 DPS 以检索其 IoT Central 注册信息。

1. 连接的设备在“设备资源管理器”页上最初显示为“未关联的设备”。   设备预配状态为“已注册”  。 将设备**关联**到相应的设备模板，并批准设备连接到 IoT Central 应用程序。 然后，设备可以从 IoT 中心检索连接字符串并开始发送数据。 设备预配现已完成，预配状态为“已预配”。 

## <a name="provisioning-status"></a>预配状态

当真实设备连接到 IoT Central 应用程序时，其预配状态将按如下所述发生更改：

1. 设备预配状态最初为“已注册”。  此状态表示该设备已在 IoT Central 中创建，并具有设备 ID。 出现以下情况时，设备处于已注册状态：
    - 在“设备资源管理器”页上添加新的真实设备。 
    - 在“设备资源管理器”页上使用“导入”添加一组设备。  
    - 设备未在“设备资源管理器”页上手动注册，但已使用有效的凭据进行连接，并在“设备资源管理器”页上显示为“未关联”的设备。   

1. 当已使用有效凭据连接到 IoT Central 应用程序的设备完成预配步骤时，设备预配状态将更改为“已预配”。  在此步骤中，设备将从 IoT 中心检索连接字符串。 现在，该设备可以连接到 IoT 中心并开始发送数据。

1. 操作员可以阻止设备。 阻止设备时，它无法将数据发送到 IoT Central 应用程序。 已阻止的设备的预配状态为“已阻止”。  只有在操作员重置设备后，该设备才能继续发送数据。 当操作员取消阻止设备时，预配状态将恢复为以前的值：“已注册”或“已预配”。  

## <a name="sdk-support"></a>SDK 支持

Azure 设备 SDK 为实现设备代码提供最简便的方法。 以下设备 SDK 已发布：

- [适用于 C 的 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-c)
- [适用于 Python 的 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-python)
- [适用于 Node.js 的 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-node)
- [适用于 Java 的 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-java)
- [适用于 .NET 的 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-csharp)

### <a name="sdk-features-and-iot-hub-connectivity"></a>SDK 功能和 IoT 中心连接

与 IoT 中心进行的所有设备通信都使用以下 IoT 中心连接选项：

- [设备到云的消息传递](../../iot-hub/iot-hub-devguide-messages-d2c.md)
- [设备孪生](../../iot-hub/iot-hub-devguide-device-twins.md)

下表对 Azure IoT Central 设备功能映射到 IoT 中心功能的具体情况进行了汇总：

| Azure IoT Central | Azure IoT 中心 |
| ----------- | ------- |
| 度量：遥测 | 设备到云的消息传送 |
| 设备属性 | 设备孪生报告属性 |
| 设置 | 设备孪生所需的和报告的属性 |

若要详细了解如何使用设备 SDK，请查看下面的某篇文章以获取示例代码：

- [将泛型 Node.js 客户端连接到 Azure IoT Central 应用程序](howto-connect-nodejs.md)
- [将 Raspberry Pi 设备连接到 Azure IoT Central 应用程序](howto-connect-raspberry-pi-python.md)
- [将 DevDiv 工具包设备连接到 Azure IoT Central 应用程序](howto-connect-devkit.md)。

### <a name="protocols"></a>协议

设备 SDK 支持下述用于连接到 IoT 中心的网络协议：

- MQTT
- AMQP
- HTTPS

若要了解这些不同的协议以及选择协议的指南，请参阅[选择通信协议](../../iot-hub/iot-hub-devguide-protocols.md)。

如果设备无法使用这些受支持协议中的任何一种，可以使用 Azure IoT Edge 进行协议转换。 IoT Edge 支持其他边缘智能方案，可以将处理从 Azure IoT Central 应用程序卸载到边缘。

## <a name="security"></a>安全性

在设备与 Azure IoT Central 之间交换的所有数据都经过加密。 如果设备已连接到任何面向设备的 IoT 中心终结点，则 IoT 中心会对从该设备发出的所有请求进行身份验证。 为了避免通过网络交换凭据，设备使用签名的令牌进行身份验证。 有关详细信息，请参阅[控制对 IoT 中心的访问](../../iot-hub/iot-hub-devguide-security.md)。

## <a name="next-steps"></a>后续步骤

了解 Azure IoT Central 中的设备连接后，建议接下来执行以下步骤：

- [准备和连接 DevKit 设备](howto-connect-devkit.md)
- [准备和连接 Raspberry Pi](howto-connect-raspberry-pi-python.md)
- [将泛型 Node.js 客户端连接到 Azure IoT Central 应用程序](howto-connect-nodejs.md)
- [C SDK：预配设备客户端 SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)
