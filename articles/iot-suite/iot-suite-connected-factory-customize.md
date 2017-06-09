---
title: "自定义 Azure IoT 套件连接工厂 | Azure"
description: "介绍如何自定义连接工厂预配置解决方案的行为。"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: v-yiso
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 57a967f655a2aec24ad749d1d31eafdd96201031
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017


---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>自定义连接工厂解决方案显示来自 OPC UA 服务器的数据的方式

## <a name="introduction"></a>介绍

连接工厂解决方案可以聚合并显示来自与该解决方案相连接的 OPC UA 服务器的数据。 可以在解决方案中浏览命令并将命令发送到 OPC UA 服务器。

解决方案中的聚合数据示例包括总体设备效率 (OEE) 和关键绩效指标 (KPI)，可以在工厂、生产线和工位级别的仪表板中查看这些数据。 以下屏幕截图显示“慕尼黑”工厂“生产线 1”中“装配”工位的 OEE 和 KPI 值：

![解决方案中的 OEE 和 KPI 值示例][img-oee-kpi]

使用该解决方案可以查看来自 OPC UA 服务器（称为*工位*）的特定数据项中的详细信息。 以下屏幕截图显示特定工位的制造项数的图形：

![制造项数图形][img-manufactured-items]

如果单击某个图形，可以使用 Time Series Insights (TSI) 进一步浏览数据：

![使用 Time Series Insights 浏览数据][img-tsi]

本文将介绍：

- 如何在解决方案的各种视图中显示数据。
- 如何自定义解决方案显示数据的方式。

## <a name="data-sources"></a>数据源

连接工厂解决方案可以显示来自与该解决方案相连接的 OPC UA 服务器的数据。 默认安装中包括多台运行工厂仿真的 OPC UA 服务器。 你可以添加自己的、[通过网关连接][lnk-connect-cf]到解决方案的 OPC UA 服务器。

可在仪表板中浏览已连接的 OPC UA 服务器可发送到解决方案的数据项：

1. 导航到“选择 OPC UA 服务器”视图：

    ![导航到“选择 OPC UA 服务器”视图][img-select-server]

1. 选择一台服务器，然后单击“连接”。 出现安全警告时，请单击“继续”。

    > [!NOTE]
    > 此警告只会针对每台服务器显示一次，可解决方案仪表板与服务器之间建立信任关系。

1. 现在，可以浏览服务器可发送到解决方案的数据项。 要发送到解决方案的项带有一个绿色复选标记：

    ![发布的项][img-published]

1. 如果你是解决方案中的*管理员*，可以选择发布数据项，以便在连接工厂解决方案中显示。 管理员还可以更改数据项的值，并在 OPC UA 服务器中调用方法。

## <a name="map-the-data"></a>映射数据

连接工厂解决方案会将来自 OPC UA 服务器的已发布数据项映射并聚合到解决方案中的各种视图。 预配解决方案时，连接工厂解决方案将部署到你的 Azure 帐户。 Visual Studio 连接工厂解决方案中的 JSON 文件将存储此映射信息。 可以在连接工厂 Visual Studio 解决方案中查看和修改此 JSON 配置文件并将它重新部署。

使用此配置文件可以：

- 编辑现有的仿真工厂、生产线和工位。
- 将连接的实际 OPC UA 服务器中的数据映射到解决方案。

若要克隆连接工厂 Visual Studio 解决方案的副本，请使用以下 git 命令：

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

文件 **ContosoTopologyDescription.json** 定义从 OPC UA 服务器数据项到连接工厂解决方案仪表板中视图的映射。 可以在 Visual Studio 解决方案的 **WebApp** 项目中的 **Contoso\Topology** 文件夹内找到此配置文件。

此 JSON 文件的内容已组织成工厂、生产线和工位节点的层次结构。 此层次结构定义连接工厂仪表板中的导航层次结构。 层次结构中每个节点上的值确定了仪表板中显示的信息。 例如，JSON 文件包含慕尼黑工厂的以下值：

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

名称、说明和位置显示在仪表板中的此视图上：

![仪表板中的慕尼黑数据][img-munich]

每个工厂、生产线和工位都有一个图像属性。 可以在 **WebApp** 项目中的 **Content\img** 文件夹内找到这些 JPEG 文件。 这些图像文件显示在连接工厂仪表板中。

每个工位包含多个详细属性用于定义来源为 OPC UA 数据项的映射。 以下部分将介绍这些属性：

### <a name="opcuri"></a>OpcUri

**OpcUri** 值是唯一标识 OPC UA 服务器的 OPC UA 应用程序 URI。 例如，慕尼黑工厂生产线 1 装配工位的 **OpcUri** 值如下所示：**urn:scada2194:ua:munich:productionline0:assemblystation**。

可以在解决方案仪表板中查看连接的 OPC UA 服务器的 URI：

![查看 OPC UA 服务器 URI][img-server-uris]

### <a name="simulation"></a>仿真

“仿真”节点中的信息特定于在默认预配的 OPC UA 服务器中运行的 OPC UA 仿真。 这些信息不用于实际的 OPC UA 服务器。

### <a name="kpi1-and-kpi2"></a>Kpi1 和 Kpi2

这些节点描述工位中的数据如何影响仪表板中的两个 KPI 值。 在默认部署中，这些 KPI 值为每小时单位数和每小时 kWh 数。 解决方案将在工位级别计算 KPI 值，并在生产线和工厂级别对其进行聚合。

每个 KPI 都有最小、最大和目标值。 每个 KPI 值还可以定义连接工厂解决方案要执行的警报操作。 以下代码片段显示慕尼黑工厂生产线 1 装配工位的 KPI 定义：

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

以下屏幕截图显示仪表板中的 KPI 数据。

![仪表板中的 KPI 信息][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

**OpcNodes** 节点标识来自 OPC UA 服务器的已发布数据项，并指定如何处理该数据。

**NodeId** 值标识来自 OPC UA 服务器的特定 OPC UA NodeID。 慕尼黑工厂生产线 1 装配工位中第一个节点包含 **ns=2;i=385** 值。 **NodeId** 值指定要从 OPC UA 服务器读取的数据项，**SymbolicName** 提供要在仪表板中对该数据使用的用户友好名称。

下表汇总了与每个节点关联的其他值：

| 值 | 说明 |
| ----- | ----------- |
| 相关性  | 此数据影响的 KPI 和 OEE 值。 |
| OpCode     | 如何聚合数据。 |
| Units      | 要在仪表板中使用的单位。  |
| Visible    | 是否在仪表板中显示此值。 某些值用于计算，但不会显示。  |
| 最大值    | 在仪表板中触发警报的最大值。 |
| MaximumAlertActions | 为响应警报而采取的操作。 例如，向工位发送命令。 |
| ConstValue | 在计算中使用的常量值。 |

## <a name="deploy-the-changes"></a>部署更改

完成对 **ContosoTopologyDescription.json** 文件的更改后，必须将连接工厂解决方案重新部署到 Azure 帐户。

**azure-iot-connected-factory** 存储库包含一个可用来重新生成和部署解决方案的 **build.ps1** PowerShell 脚本。

## <a name="next-steps"></a>后续步骤

请阅读以下文章，详细了解连接工厂预配置解决方案：

* [连接工厂预配置解决方案演练][lnk-rm-walkthrough]
* [将设备连接到连接工厂预配置解决方案][lnk-connect-cf]
* [azureiotsuite.cn 站点权限][lnk-permissions]
* [常见问题解答][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: ./iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-cf]: ./iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: ./iot-suite-permissions.md
[lnk-faq]: ./iot-suite-faq.md
