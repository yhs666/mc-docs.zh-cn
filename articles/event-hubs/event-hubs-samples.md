---
title: "Azure 事件中心示例 | Azure"
description: "Azure 事件中心示例"
services: event-hubs
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 05/01/2017
ms.date: 07/03/2017
ms.author: v-yeche
ms.openlocfilehash: 72f28d60fd28a9524bb7e9b4ab84de2a87241596
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 事件中心示例
<a id="event-hubs-samples" class="xliff"></a> 

Azure 事件中心示例演示了 [Azure 事件中心](/event-hubs/)内的主要功能。 本文对可用示例进行了分类和介绍，每个示例均具有链接。

撰写本文时，事件中心示例位于多个不同位置：

- [MSDN 开发人员代码示例](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs)
<!-- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/) is correct-->

若要深入了解 .NET Framework 的不同版本，请参阅 [框架和目标](https://docs.microsoft.com/dotnet/articles/standard/frameworks)。

后续将添加更多示例，因此请时常回访本网站获取更新。

## .NET Standard
<a id="net-standard" class="xliff"></a>

下面的示例演示如何使用 [.NET Standard 库](https://docs.microsoft.com/dotnet/articles/standard/library)的[事件中心客户端](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md)发送和接收事件。

### 发送事件
<a id="send-events" class="xliff"></a> 

[发送入门](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender)示例演示如何编写将事件发送到事件中心的 .NET Core 控制台应用程序。

### 接收事件
<a id="receive-events" class="xliff"></a> 

[使用事件处理器主机接收入门](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver)示例是一个使用 `Event Processor Host` 从事件中心接收消息的 .NET Core 控制台应用程序。

## .NET framework
<a id="net-framework" class="xliff"></a>   

这些示例针对 [.NET Framework 库](https://msdn.microsoft.com/library/w0x726c2.aspx)，演示 Azure 事件中心的各种其他功能。

### 通知用户已收到事件
<a id="notify-users-of-events-received" class="xliff"></a>

[AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) 示例通知用户已收到传感器或其他系统发出的数据。

### 事件中心入门
<a id="get-started-with-event-hubs" class="xliff"></a> 

[事件中心入门](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097)示例演示事件中心的基本功能，例如如何创建事件中心、将事件发送到事件中心，以及使用[事件处理器主机](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/)处理事件。

### 扩大事件处理
<a id="scale-out-event-processing" class="xliff"></a> 

[扩大事件处理](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3)示例演示如何使用[事件处理器主机](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/)分配事件中心流消耗工作负荷。 它演示如何实现用于管理事件流的 **EventProcessor** 和 **EventProcessorFactory** 对象。 

###  将数据从 SQL 提取到事件中心
<a id="pull-data-from-sql-into-an-event-hub" class="xliff"></a>

[提取 SQL 数据](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql)示例演示如何从 SQL 表中提取数据并将其推送到事件中心，以用作下游分析应用程序的输入。

### 将 Web 数据提取到事件中心
<a id="pull-web-data-into-an-event-hub" class="xliff"></a> 

[从 Web 导入数据](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb)示例演示如何从公共源（例如运输部的流量信息源）提取数据，并将其推送到事件中心。

## 后续步骤
<a id="next-steps" class="xliff"></a>

访问以下链接，了解有关 .NET Framework 版本的详细信息：

- [框架和目标](https://docs.microsoft.com/dotnet/articles/standard/frameworks)
- [.NET Framework 4.6 和 4.5](https://msdn.microsoft.com/library/w0x726c2.aspx)

可在以下文章中了解有关事件中心的详细信息：

- [事件中心概述](event-hubs-what-is-event-hubs.md)
- [创建事件中心](event-hubs-create.md)
- [事件中心常见问题](event-hubs-faq.md)