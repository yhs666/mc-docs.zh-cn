---
title: 删除 Azure Service Fabric 执行组件 | Azure
description: 了解如何手动删除 Service Fabric Reliable Actors 及其状态。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 03/19/2018
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: 5c23eb3257c8a0f93f99b1d4d91e09a33bb07428
ms.sourcegitcommit: f1ecc209500946d4f185ed0d748615d14d4152a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463497"
---
# <a name="delete-reliable-actors-and-their-state"></a>删除 Reliable Actors 及其状态
对已停用的执行组件进行垃圾回收只会清除该执行组件对象，但是存储在执行组件的状态管理器中的数据不会被删除。 重新激活执行组件后，可通过状态管理器再次使用其数据。 如果执行组件将数据存储在状态管理器，并且已停用且始终不激活该执行组件，那么可能需要清理其数据。

[执行组件服务](service-fabric-reliable-actors-platform.md)提供了一个函数，用于从远程调用方删除执行组件：

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

删除一个执行组件将出现以下结果，具体取决于当前执行组件是否处于活动状态：

* **活动执行组件**
  * 执行组件从活动的执行组件列表中删除并停用。
  * 该执行组件的状态会被永久删除。
* **非活动执行组件**
  * 该执行组件的状态会被永久删除。

执行组件无法使用其任何一个执行组件方法对其自身调用删除，因为在执行组件调用时执行删除将无法删除该执行组件，在此过程中，运行时已获取该执行组件的锁，用于强制实施单线程访问。

有关 Reliable Actors 的详细信息，请阅读以下内容：
* [执行组件计时器和提醒](service-fabric-reliable-actors-timers-reminders.md)
* [执行组件事件](service-fabric-reliable-actors-events.md)
* [执行组件可重入性](service-fabric-reliable-actors-reentrancy.md)
* [执行组件诊断和性能监视](service-fabric-reliable-actors-diagnostics.md)
* [执行组件 API 参考文档](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# 示例代码](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java 代码示例](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png

<!-- Update_Description: update meta properties -->
