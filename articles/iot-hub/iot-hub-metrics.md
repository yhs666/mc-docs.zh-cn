---
title: 使用指标监视 Azure IoT 中心 | Azure
description: 如何使用 Azure IoT 中心度量值评估和监视 IoT 中心的总体运行状况。
author: nberdy
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
origin.date: 04/24/2019
ms.date: 11/11/2019
ms.author: v-yiso
ms.openlocfilehash: bad225dae6abb9244e71927bcdbfaf86710762d1
ms.sourcegitcommit: 642a4ad454db5631e4d4a43555abd9773cae8891
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425892"
---
# <a name="understand-iot-hub-metrics"></a>了解 IoT 中心指标
IoT 中心度量值提供更棒的数据，清晰显示 Azure 订阅中的 Azure IoT 资源状态。 通过 IoT 中心度量值，可评估 IoT 中心服务及其所连接的设备的总体运行状况。 面向用户的统计信息非常重要，因为它们可以帮助了解 IoT 中心的情况，并可以帮助在不联系 Azure 支持人员的情况下解决根本问题。

默认启用度量值。 可在 Azure 门户中查看 IoT 中心度量值。

## <a name="how-to-view-iot-hub-metrics"></a>如何查看 IoT 中心度量值

1. 创建 IoT 中心。 可以在[从设备向 IoT 中心发送遥测数据](quickstart-send-telemetry-dotnet.md)指南中找到有关如何创建 IoT 中心的说明。

2. 打开 IoT 中心的边栏选项卡。 在此处单击“度量值”  。
   
    ![显示指标选项在 IoT 中心资源页中位置的屏幕截图](./media/iot-hub-metrics/enable-metrics-1.png)

3. 在“度量值”边栏选项卡中，可查看 IoT 中心的度量值并创建度量值的自定义视图。 
   
    ![显示 IoT 中心的指标页的屏幕截图](./media/iot-hub-metrics/enable-metrics-2.png)
    
4. 可以选择将指标数据发送到事件中心终结点还是 Azure 存储帐户，方法是单击“诊断设置”  ，然后单击“添加诊断设置” 

   ![显示“诊断设置”按钮所在位置的屏幕截图](./media/iot-hub-metrics/enable-metrics-3.png)

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT 中心度量值及其用法
IoT 中心提供多个度量值，帮助你大致了解中心的运行状况以及所连接的设备总数。 可以结合多个度量值的信息，更清楚地了解 IoT 中心的状态。 下表描述了每个 IoT 中心所跟踪的度量值，以及每个度量值与 IoT 中心总体状态的关联。

|指标|指标显示名称|计价单位|聚合类型|说明|维度|
|---|---|---|---|---|---|
|d2c<br>.telemetry<br>.ingress.<br>allProtocol|遥测消息发送尝试次数|计数|总计|尝试发送到 IoT 中心的、设备到云的遥测消息数|无维度|
|d2c<br>.telemetry<br>.ingress<br>.success|发送的遥测消息数|计数|总计|成功发送到 IoT 中心的、设备到云的遥测消息数|无维度|
|c2d<br>.commands<br>.egress<br>.complete<br>.success|完成的命令数|计数|总计|设备已成功完成的云到设备命令数目|无维度|
|c2d<br>.commands<br>.egress<br>.abandon<br>.success|放弃的命令数|计数|总计|设备放弃的云到设备命令数目|无维度|
|c2d<br>.commands<br>.egress<br>.reject<br>.success|拒绝的命令数|计数|总计|设备拒绝的云到设备命令数目|无维度|
|devices<br>.totalDevices|设备总数（已弃用）|计数|总计|已注册到 IoT 中心的设备数目|无维度|
|devices<br>.connectedDevices<br>.allProtocol|连接的设备数（已弃用） |计数|总计|已连接到 IoT 中心的设备数目|无维度|
|d2c<br>.telemetry<br>.egress<br>.success|路由：遥测消息传送次数|计数|总计|使用 IoT 中心路由将消息成功传送到所有终结点的次数。 如果某条消息已路由到多个终结点，则每成功传送一次，此值就会加 1。 如果某条消息多次路由到同一终结点，则每成功传送一次，此值就会加 1。|无维度|
|d2c<br>.telemetry<br>.egress<br>.dropped|路由：遥测消息删除次数 |计数|总计|由于终结点消亡，IoT 中心路由删除消息的次数。 此值不会统计已传送到回退路由的消息，因为已删除的消息不会传送到回退路由。|无维度|
|d2c<br>.telemetry<br>.egress<br>.orphaned|路由：遥测消息孤立次数 |计数|总计|消息由于与任何路由规则（包括回退规则）都不匹配而被 IoT 中心路由孤立的次数。 |无维度|
|d2c<br>.telemetry<br>.egress<br>.invalid|路由：遥测消息不兼容|计数|总计|消息由于与终结点不兼容而无法由 IoT 中心路由传送的次数。 此值不包括重试次数。|无维度|
|d2c<br>.telemetry<br>.egress<br>.fallback|路由：消息传送到回退路由的次数|计数|总计|IoT 中心路由将消息传送到与回退路由关联的终结点的次数。|无维度|
|d2c<br>.endpoints<br>.egress<br>.eventHubs|路由：消息传送到事件中心的次数|计数|总计|IoT 中心路由成功将消息传送到事件中心终结点的次数。|无维度|
|d2c<br>.endpoints<br>.latency<br>.eventHubs|路由：事件中心的消息延迟|毫秒|平均值|消息进入 IoT 中心与进入事件中心终结点之间的平均延迟（毫秒）|无维度|
|d2c<br>.endpoints<br>.egress<br>.serviceBusQueues|路由：消息传送到服务总线队列的次数|计数|总计|IoT 中心路由成功将消息传送到服务总线队列终结点的次数。|无维度|
|d2c<br>.endpoints<br>.latency<br>.serviceBusQueues|路由：服务总线队列的消息延迟|毫秒|平均值|消息进入 IoT 中心与遥测消息进入服务总线队列终结点之间的平均延迟（毫秒）|无维度|
|d2c<br>.endpoints<br>.egress<br>.serviceBusTopics|路由：消息传送到服务总线主题的次数|计数|总计|IoT 中心路由成功将消息传送到服务总线主题终结点的次数。|无维度|
|d2c<br>.endpoints<br>.latency<br>.serviceBusTopics|路由：服务总线主题的消息延迟|毫秒|平均值|消息进入 IoT 中心与遥测消息进入服务总线主题终结点之间的平均延迟（毫秒）|无维度|
|d2c<br>.endpoints<br>.egress<br>.builtIn<br>.events|路由：消息传送到消息/事件的次数|计数|总计|IoT 中心路由成功将消息传送到内置终结点（消息/事件）的次数。 此指标仅在已为 IoT 中心启用路由 (https://aka.ms/iotrouting) 时开始工作。|无维度|
|d2c<br>.endpoints<br>.latency<br>.builtIn.events|路由：消息/事件的消息延迟|毫秒|平均值|消息进入 IoT 中心与遥测消息进入内置终结点（消息/事件）之间的平均延迟（毫秒） 此指标仅在已为 IoT 中心启用路由 (https://aka.ms/iotrouting) 时开始工作。|无维度|
|d2c<br>.endpoints<br>.egress<br>.storage|路由：消息传送到存储的次数|计数|总计|IoT 中心路由成功将消息传送到存储终结点的次数。|无维度|
|d2c<br>.endpoints<br>.latency<br>.storage|路由：存储的消息延迟|毫秒|平均值|消息进入 IoT 中心与遥测消息进入存储终结点之间的平均延迟（毫秒）。|无维度|
|d2c<br>.endpoints<br>.egress<br>.storage<br>.bytes|路由：传送到存储的数据量|字节|总计|IoT 中心路由传送到存储终结点的数据量（字节）。|无维度|
|d2c<br>.endpoints<br>.egress<br>.storage<br>.blobs|路由：将 Blob 传送到存储的次数|计数|总计|IoT 中心路由将 Blob 传送到存储终结点的次数。|无维度|
|EventGridDeliveries|事件网格传送（预览版）|计数|总计|发布到事件网格的 IoT 中心事件的数量。 使用 Result 维度表示成功和失败请求的数量。 EventType 维度显示事件的类型 (https://aka.ms/ioteventgrid) 。 若要查看请求来自何处，请使用 EventType 维度。|Result、EventType|
|EventGridLatency|事件网格延迟（预览）|毫秒|平均值|从生成 IoT 中心事件到将事件发布到事件网格的平均延迟（毫秒）。 此数值是所有事件类型的平均。 若要查看特定事件类型的延迟，请使用 EventType 维度。|EventType|
|d2c<br>.twin<br>.read<br>.success|设备的成功孪生读取数|计数|总计|由设备发起的所有成功孪生读取的计数。|无维度|
|d2c<br>.twin<br>.read<br>.failure|设备的失败孪生读取数|计数|总计|由设备发起的所有失败孪生读取的计数。|无维度|
|d2c<br>.twin<br>.read<br>.size|设备的孪生读取的响应大小|字节|平均值|由设备发起的所有成功的孪生读取的平均、最小和最大大小。|无维度|
|d2c<br>.twin<br>.update<br>.success|设备的成功孪生更新数|计数|总计|由设备发起的所有成功的孪生更新的计数。|无维度|
|d2c<br>.twin<br>.update<br>.failure|设备的失败孪生更新数|计数|总计|由设备发起的所有失败的孪生更新的计数。|无维度|
|d2c<br>.twin<br>.update<br>.size|设备的孪生更新的大小|字节|平均值|由设备发起的所有成功孪生更新的平均、最小和最大大小。|无维度|
|c2d<br>.methods<br>.success|成功的直接方法调用数|计数|总计|所有成功的直接方法调用的计数。|无维度|
|c2d<br>.methods<br>.failure|失败的直接方法调用数|计数|总计|所有失败直接方法调用的计数。|无维度|
|c2d<br>.methods<br>.requestSize|直接方法调用的请求大小|字节|平均值|所有成功直接方法请求的平均、最小和最大大小。|无维度|
|c2d<br>.methods<br>.responseSize|直接方法调用的响应大小|字节|平均值|所有成功直接方法响应的平均、最小和最大大小。|无维度|
|c2d<br>.twin<br>.read<br>.success|后端的成功孪生读取数|计数|总计|由后端发起的所有成功孪生读取的计数。 此计数不包括由孪生查询发起的孪生读取操作。|无维度|
|c2d<br>.twin<br>.read<br>.failure|后端的失败孪生读取数|计数|总计|由后端发起的所有失败孪生读取的计数。|无维度|
|c2d<br>.twin<br>.read<br>.size|后端的孪生读取的响应大小|字节|平均值|由后端发起的所有成功的孪生读取的平均、最小和最大大小。|无维度|
|c2d<br>.twin<br>.update<br>.success|后端的成功孪生更新数|计数|总计|由后端发起的所有成功孪生更新的计数。|无维度|
|c2d<br>.twin<br>.update<br>.failure|后端的失败孪生更新数|计数|总计|由后端发起的所有失败孪生更新的计数。|无维度|
|c2d<br>.twin<br>.update<br>.size|后端的失败孪生更新大小|字节|平均值|由后端发起的所有成功孪生更新的平均、最小和最大大小。|无维度|
|twinQueries<br>.success|成功的孪生查询数|计数|总计|所有成功孪生查询的计数。|无维度|
|twinQueries<br>.failure|失败的孪生查询数|计数|总计|所有失败孪生查询的计数。|无维度|
|twinQueries<br>.resultSize|孪生查询结果大小|字节|平均值|所有成功孪生查询的结果大小的平均值、最小值和最大值。|无维度|
|jobs<br>.createTwinUpdateJob<br>.success|孪生更新作业创建成功数|计数|总计|孪生更新作业创建成功的所有次数。|无维度|
|jobs<br>.createTwinUpdateJob<br>.failure|孪生更新作业创建失败数|计数|总计|孪生更新作业创建失败的所有次数。|无维度|
|jobs<br>.createDirectMethodJob<br>.success|方法调用作业的创建成功数|计数|总计|直接方法调用作业创建成功的所有次数。|无维度|
|jobs<br>.createDirectMethodJob<br>.failure|方法调用作业的创建失败数|计数|总计|直接方法调用作业创建失败的所有次数。|无维度|
|jobs<br>.listJobs<br>.success|对列出作业的成功调用数|计数|总计|对列出作业的所有成功调用的计数。|无维度|
|jobs<br>.listJobs<br>.failure|对列出作业的失败调用数|计数|总计|对列出作业的所有失败调用的计数。|无维度|
|jobs<br>.cancelJob<br>.success|成功的作业取消数|计数|总计|用来取消作业的调用成功的次数。|无维度|
|jobs<br>.cancelJob<br>.failure|失败的作业取消数|计数|总计|用来取消作业的调用失败的次数。|无维度|
|jobs<br>.queryJobs<br>.success|成功的作业查询数|计数|总计|对查询作业的所有成功调用的计数。|无维度|
|jobs<br>.queryJobs<br>.failure|失败的作业查询数|计数|总计|对查询作业的所有失败调用的计数。|无维度|
|jobs<br>.completed|已完成的作业|计数|总计|所有已完成的作业的计数。|无维度|
|jobs<br>.failed|失败的作业数|计数|总计|所有失败的作业的计数。|无维度|
|d2c<br>.telemetry<br>.ingress<br>.sendThrottle|限制错误数|计数|总计|由于设备吞吐量限制而导致的限制错误数|无维度|
|dailyMessage<br>QuotaUsed|已使用的消息总数|计数|平均值|今天使用的消息总数。 这是累积值，每日 00:00 UTC 重置为零。|无维度|
|deviceDataUsage|设备数据用量总计|字节|总计|从与 IotHub 相连的任意设备传出的字节，以及传入到与 IotHub 相连的任意设备的字节|无维度|
|totalDeviceCount|设备总数（预览）|计数|平均值|已注册到 IoT 中心的设备数目|无维度|
|已连接<br>设备数|连接设备数（预览）|计数|平均值|已连接到 IoT 中心的设备数目|无维度|
|配置|配置指标|计数|总计|配置操作的指标|无维度|

## <a name="next-steps"></a>后续步骤

现已大致了解了 IoT 中心度量值，请单击此链接，深入了解如何管理 Azure IoT 中心：

* [操作监视](iot-hub-operations-monitoring.md)

若要进一步探索 IoT 中心的功能，请参阅：

* [IoT 中心开发人员指南](iot-hub-devguide.md)

* [使用 Azure IoT Edge 将 AI 部署到边缘设备](../iot-edge/quickstart-linux.md)
