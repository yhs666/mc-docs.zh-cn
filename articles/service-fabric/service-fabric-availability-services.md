---
title: "Service Fabric 服务的可用性 | Azure"
description: "介绍服务的故障检测、故障转移和恢复"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 08/18/2017
ms.date: 09/11/2017
ms.author: v-yeche
ms.openlocfilehash: a1863437cd352fae3e6ab9dfdd8ef01da7881d43
ms.sourcegitcommit: 76a57f29b1d48d22bb4df7346722a96c5e2c9458
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="availability-of-service-fabric-services"></a>Service Fabric 服务的可用性
本文概述 Service Fabric 如何保持服务的可用性。

## <a name="availability-of-service-fabric-stateless-services"></a>Service Fabric 无状态服务的可用性
Azure Service Fabric 服务可以是有状态服务，也可以是无状态服务。 无状态服务是不具备任何[本地状态](service-fabric-concepts-state.md)、需要具有高可用性和可靠性的应用程序服务。

创建无状态服务需要定义 `InstanceCount`。 实例计数定义应在群集中运行的无状态服务应用程序逻辑的实例数。 建议通过增加实例数来扩大无状态服务。

无状态命名服务的实例发生故障时，会在群集中某个符合条件的节点上创建一个新实例。 例如，无状态服务实例可能会在 Node1 上发生故障，但可以在 Node5 上进行重建。

## <a name="availability-of-service-fabric-stateful-services"></a>Service Fabric 有状态服务的可用性
有状态服务具备一些与之关联的状态。 在 Service Fabric 中，有状态服务均建模为副本集。 每个副本是服务代码的运行实例，其中还包含该服务的状态副本。 在一个副本（以下称为“主副本”）上执行读取和写入操作。 因写入操作发生的状态更改会复制到副本集中的其他副本（以下称为活动次要副本），并进行应用。 

只能有一个主要副本，但可以有多个活动次要副本。 可以配置活动次要副本的数目，而副本数越多，可承受的并发软件和硬件故障数越多。

如果主要副本不可用，Service Fabric 会指定一个活动次要副本成为新的主要副本。 此活动辅助副本具有最新的状态（通过复制）并且可以继续处理更多的读取和写入操作。

主要副本和活动次要副本的这一概念称为副本角色。

### <a name="replica-roles"></a>副本角色
副本的角色用于管理受该副本所管理状态的生命周期。 具有主要副本角色的副本为读取请求提供服务。 主要副本还可通过更新其状态并复制更改来处理所有写入请求。 这些更改会应用于副本集中的活动次要副本。 活动次要副本的任务是接收主要副本已复制的状态更改并更新其状态视图。

> [!NOTE]
> 对开发人员而言，更高级别的编程模型（如 [Reliable Actors](service-fabric-reliable-actors-introduction.md) 和 [Reliable Services](service-fabric-reliable-services-introduction.md)）可以隐藏副本角色的概念。 在 Actors 中，角色的概念是不必要的，而在 Services 中它已针对大多数方案进行很大程度的简化。
>

## <a name="next-steps"></a>后续步骤
有关 Service Fabric 概念的详细信息，请参阅以下文章：

- [缩放 Service Fabric 服务](service-fabric-concepts-scalability.md)
- [Service Fabric 服务分区](service-fabric-concepts-partitioning.md)
- [定义和管理状态](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)

<!--Update_Description: wording update-->