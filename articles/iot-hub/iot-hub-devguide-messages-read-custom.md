---
title: 了解 Azure IoT 中心自定义终结点 | Azure
description: 开发人员指南 - 使用路由规则将设备到云的消息路由到自定义终结点。
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 04/09/2018
ms.author: v-yiso
ms.date: 05/07/2018
ms.openlocfilehash: 17ea56aa011ebd5dc505170e567ed35cbd4b614d
ms.sourcegitcommit: 0fedd16f5bb03a02811d6bbe58caa203155fd90e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2018
ms.locfileid: "32121073"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>对设备到云的消息使用消息路由和自定义终结点

IoT 中心允许基于消息属性将[设备到云的消息][lnk-device-to-cloud]路由到面向 IoT 中心服务的终结点。 使用路由规则可将消息灵活发送到所需目标位置，无需借助其他服务或自定义代码。 配置的每个路由规则都具有以下属性：

| 属性      | 说明 |
| ------------- | ----------- |
| **名称**      | 用于标识规则的唯一名称。 |
| **源**    | 要处理的数据流的来源。 例如，设备遥测。 |
| **条件** | 路由规则的查询表达式，针对消息的标头和正文运行，确定消息是否与终结点匹配。 有关构造路由条件的详细信息，请参阅 [参考 - 设备孪生和作业的查询语言][lnk-devguide-query-language]。 |
| **终结点**  | IoT 中心将匹配条件的消息发送到的终结点的名称。 终结点应与 IoT 中心位于同一区域，否则跨区域写入将产生费用。 |

一条消息可能与多个路由规则的条件匹配，在这种情况下，IoT 中心会将该消息传递到与每个匹配规则关联的终结点。 IoT 中心还会自动删除重复的消息传递，因此如果消息与具有相同目标的多个规则匹配，则仅会将其写入该目标位置一次。

## <a name="endpoints-and-routing"></a>终结点和路由

IoT 中心有一个默认的[内置终结点][lnk-built-in]。 通过将订阅中的其他服务链接到中心，可以创建要将消息路由到的自定义终结点。 IoT 中心目前支持将 Azure 存储容器、事件中心、服务总线队列和服务总线主题用作自定义终结点。

使用路由和自定义终结点时，如果消息不与任何规则匹配，则只会将其传送到内置终结点。 若要将消息同时传送到内置终结点和自定义终结点，请添加一个可将消息发送到 **events** 终结点的路由。

> [!NOTE]
> IoT 中心仅支持将数据作为 blob 写入 Azure 存储容器。
>
>



> [!WARNING]
> 不支持将已启用**会话**或**重复项检测**的服务总线队列和主题用作自定义终结点。

有关在 IoT 中心创建自定义终结点的详细信息，请参阅 [IoT 中心终结点][lnk-devguide-endpoints]。

有关从自定义终结点读取数据的详细信息，请参阅：

* 从 [Azure 存储容器][lnk-getstarted-storage]读取。
* 从[事件中心][lnk-getstarted-eh]读取数据。
* 从[服务总线队列][lnk-getstarted-queue]读取数据。
* 从[服务总线主题][lnk-getstarted-topic]读取数据。

## <a name="latency"></a>延迟

使用内置终结点路由设备到云遥测消息时，在创建第一个路由后，端到端延迟略微增大。

在大多数情况下，延迟的平均增大量小于一秒。 可以使用 **d2c.endpoints.latency.builtIn.events** [IoT 中心指标](./iot-hub-metrics.md)监视延迟。 在创建第一个路由后创建或删除任何路由不会影响端到端延迟。

### <a name="next-steps"></a>后续步骤

有关 IoT 中心终结点的详细信息，请参阅 [IoT 中心终结点][lnk-devguide-endpoints]。

有关用于定义路由规则的查询语言的详细信息，请参阅[用于设备孪生、作业和消息路由的 IoT 中心查询语言][lnk-devguide-query-language]。

[使用路由处理 IoT 中心设备到云的消息][lnk-d2c-tutorial]教程介绍了如何使用路由规则和自定义终结点。

[lnk-built-in]: ./iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: ./iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: ./iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: ./iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: ./iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[lnk-getstarted-storage]: ../storage/blobs/storage-blobs-introduction.md
