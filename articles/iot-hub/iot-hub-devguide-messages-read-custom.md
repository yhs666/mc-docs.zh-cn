---
title: 了解 Azure IoT 中心自定义终结点 | Azure
description: 开发人员指南 - 使用路由查询将设备到云的消息路由到自定义终结点。
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
ms.date: 10/29/2018
ms.openlocfilehash: 96b79db44f914d72a4e80ee13e26f9c19efe7e3f
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49453783"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>对设备到云的消息使用消息路由和自定义终结点

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

借助 IoT 中心[消息路由](iot-hub-devguide-routing-query-syntax.md)，用户可将设备到云的消息路由到面向服务的终结点。 路由还提供了一种查询功能，让你先筛选数据，再将其路由到终结点。 所配置的每个路由查询包含以下属性：

| 属性      | 说明 |
| ------------- | ----------- |
| **名称**      | 用于标识查询的唯一名称。 |
| Source    | 要处理的数据流的来源。 例如，设备遥测。 |
| **条件** | 针对消息应用程序属性、系统属性、消息正文、设备孪生标记和设备孪生属性运行的路由查询的查询表达式，用于确定该查询是否是终结点的匹配项。 要详细了解如何构造查询，请参阅[消息路由查询语法](iot-hub-devguide-routing-query-syntax.md) |
| **终结点**  | IoT 中心将匹配查询的消息发送到的终结点的名称。 建议所选终结点与 IoT 中心位于同一区域。 |

一条消息可能与多个路由查询中的条件匹配，在这种情况下，IoT 中心会将该消息传递到与每个匹配查询关联的终结点。 IoT 中心还会自动删除重复的消息传递，因此如果消息与具有相同目标的多个查询匹配，仅将其写入该目标位置一次。

## <a name="endpoints-and-routing"></a>终结点和路由

IoT 中心有一个默认的[内置终结点](iot-hub-devguide-messages-read-builtin.md)。 通过将订阅中的其他服务链接到中心，可以创建要将消息路由到的自定义终结点。 IoT 中心目前支持将 Azure 存储容器、事件中心、服务总线队列和服务总线主题用作自定义终结点。

使用路由和自定义终结点时，如果消息不与任何查询匹配，则只将其传送到内置终结点。 若要将消息同时传送到内置终结点和自定义终结点，请添加一个可将消息发送到 **events** 终结点的路由。

> [!NOTE]
> IoT 中心仅支持将数据作为 blob 写入 Azure 存储容器。
>
>



> [!WARNING]
> 不支持将已启用**会话**或**重复项检测**的服务总线队列和主题用作自定义终结点。

有关在 IoT 中心创建自定义终结点的详细信息，请参阅 [IoT 中心终结点][lnk-devguide-endpoints]。

有关从自定义终结点读取数据的详细信息，请参阅：

* 从 [Azure 存储容器](../storage/blobs/storage-blobs-introduction.md)读取数据。

* 从[事件中心](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)读取数据。

* 从[服务总线队列](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)读取数据。

* 从[服务总线主题](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md)读取数据。

## <a name="next-steps"></a>后续步骤

有关 IoT 中心终结点的详细信息，请参阅 [IoT 中心终结点][lnk-devguide-endpoints]。

* 要详细了解用于定义路由查询的查询语言，请参阅[消息路由查询语法](iot-hub-devguide-routing-query-syntax.md)。

* [使用路由处理 IoT 中心设备到云的消息](tutorial-routing.md)教程介绍了如何使用路由查询和自定义终结点。

[lnk-built-in]: ./iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: ./iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: ./iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: ./iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: tutorial-routing.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[lnk-getstarted-storage]: ../storage/blobs/storage-blobs-introduction.md
