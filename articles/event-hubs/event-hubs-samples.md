---
title: Azure 事件中心示例 | Azure
description: Azure 事件中心示例
services: event-hubs
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 05/17/2018
ms.date: 08/06/2018
ms.author: v-yeche
ms.openlocfilehash: c419ce2da890efb1297900f922638fa3079eb8dd
ms.sourcegitcommit: c6205500afd23ac00f2829fe51858b51a622eaf1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2018
ms.locfileid: "39487812"
---
# <a name="event-hubs-samples"></a>事件中心示例 

Azure 事件中心示例集演示了 [Azure 事件中心](/event-hubs/)内的主要功能。 本文对可用示例进行了分类和介绍，每个示例均具有链接。

撰写本文时，事件中心示例位于多个不同位置：

- [MSDN 开发人员代码示例](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

有关 .NET Framework 的不同版本的详细信息，请参阅[框架和目标](https://docs.microsoft.com/dotnet/articles/standard/frameworks)。

后续会添加更多示例，因此请时常回访本网站获取更新。

## <a name="net-standard"></a>.NET Standard

下面的示例演示如何使用 [.NET Standard 库](https://docs.microsoft.com/dotnet/articles/standard/library)的[事件中心客户端](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md)发送和接收事件。

### <a name="send-events"></a>发送事件 

[发送入门](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender)示例演示如何编写将事件发送到事件中心的 .NET Core 控制台应用程序。

### <a name="receive-events"></a>接收事件 

[使用事件处理器主机接收入门](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver)示例是一个使用事件处理器主机从事件中心接收消息的 .NET Core 控制台应用程序。

## <a name="net-framework"></a>.NET framework   

这些示例针对 [.NET Framework 库](https://docs.microsoft.com/dotnet/framework/index?view=azure-dotnet)，演示 Azure 事件中心的各种其他功能。

### <a name="notify-users-of-events-received"></a>通知用户已收到事件

[AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) 示例通知用户已收到传感器或其他系统发出的数据。

### <a name="get-started-with-event-hubs"></a>事件中心入门 

[事件中心入门](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097)示例演示事件中心的基本功能，例如如何创建事件中心、将事件发送到事件中心，以及使用[事件处理器主机](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/)处理事件。

### <a name="scale-out-event-processing"></a>扩大事件处理 

[扩大事件处理](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3)示例演示如何使用[事件处理器主机](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/)分配事件中心流消耗工作负荷。 它演示如何实现用于管理事件流的 **EventProcessor** 和 **EventProcessorFactory** 对象。 

## <a name="next-steps"></a>后续步骤

访问以下链接，了解有关 .NET Framework 版本的详细信息：

- [框架和目标](https://docs.microsoft.com/dotnet/articles/standard/frameworks)
- [.NET Framework 4.6 和 4.5](https://docs.microsoft.com/dotnet/framework/index?view=azure-dotnet)

可在以下文章中了解有关事件中心的详细信息：

- [事件中心概述](event-hubs-what-is-event-hubs.md)
- [事件中心功能](event-hubs-features.md)
- [事件中心常见问题](event-hubs-faq.md)

<!--Update_Description: update meta properties, wording update, update reference link-->