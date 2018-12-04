---
title: Azure 服务总线消息处理体系结构概述
description: 介绍 Azure 服务总线的消息处理体系结构。
services: service-bus
documentationCenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: baf94c2d-0e58-4d5d-a588-767f996ccf7f
ms.service: service-bus
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 12/21/2017
ms.author: v-yiso
ms.date: 02/05/2018
ms.openlocfilehash: 6dcc0a9f2926bb5d0040100e7a7e7477eb70129a
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52649057"
---
# <a name="service-bus-architecture"></a>服务总线体系结构

本文介绍 Azure 服务总线的消息处理体系结构。

## <a name="service-bus-scale-units"></a>服务总线缩放单元

服务总线按*缩放单元*进行组织。 缩放单元是部署单元，包含运行服务所需的全部组件。 每个区域部署一个或多个服务总线缩放单元。

一个服务总线命名空间映射到一个缩放单元。 缩放单元负责处理各种类型的服务总线实体（队列、主题、订阅）。 服务总线缩放单元由以下组件构成：

* **一组网关节点。** 网关节点对传入请求进行身份验证。 每个网关节点都有一个公共 IP 地址。

- **一组消息代理节点。** 消息代理节点处理有关消息实体的请求。

- **一个网关存储。** 网关存储保存此缩放单元中定义的每个实体的数据。 网关存储在 SQL Azure 数据库顶层实施。

- **多个消息存储。** 消息存储保存此缩放单元中定义的所有队列、主题和订阅的消息。 它还包含所有订阅数据。 除非启用了[分区消息实体](service-bus-partitioning.md)，否则队列或主题将映射到一个消息存储。 订阅是存储在与其父主题相同的消息存储中。 除了服务总线 [高级消息传送](service-bus-premium-messaging.md)外，消息存储在 [SQL 数据库](/sql-database/)实例基础上实施。

## <a name="containers"></a>容器
为每个消息实体分配特定的容器。 容器是一种逻辑构造，它使用一个消息存储来存储此容器的所有相关数据。 为每个容器分配一个消息代理节点。 通常，容器比消息代理节点要多。 因此，每个消息代理节点将加载多个容器。 消息代理节点的容器分配经过适当的组织，可以均衡加载所有消息代理节点。 如果负载模式发生更改（例如其中一个容器变得十分繁忙），或消息代理节点暂时不可用，则会在消息代理节点之间重新分配容器。

## <a name="processing-of-incoming-messaging-requests"></a>处理传入消息请求

当客户端向服务总线发送请求时，Azure 负载均衡器将其路由到任何一个网关节点。 网关节点为请求授权。 如果请求涉及某个消息传送实体（队列、主题、订阅），网关节点将在网关存储中查找该实体，并确定该实体位于哪个消息存储中。 然后，它将查询哪些消息代理节点目前正在为此容器提供服务，并将请求发送到该消息代理节点。 消息代理节点将处理请求并更新容器存储中的实体状态。 然后，消息代理节点向网关节点发送响应，而网关节点向发出原始请求的客户端发送相应的响应。

![处理传入消息请求](./media/service-bus-architecture/IC690644.png)

## <a name="next-steps"></a>后续步骤
现在，已概要了解服务总线体系结构，请访问以下链接了解详细信息：

- [服务总线消息传送概述](./service-bus-messaging-overview.md)
* [服务总线队列入门](service-bus-dotnet-get-started-with-queues.md)
