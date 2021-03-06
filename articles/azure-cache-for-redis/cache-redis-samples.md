---
title: Azure Redis 缓存示例 | Microsoft Docs
description: 了解如何使用 Azure Redis 缓存
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 1f8d210c-ee09-4fe2-b63f-1e69246a27d8
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: multiple
ms.topic: article
origin.date: 01/23/2017
ms.date: 02/27/2019
ms.author: v-junlch
ms.openlocfilehash: 66954af7a1903e785e35cff2cde0259239d29484
ms.sourcegitcommit: 1e5ca29cde225ce7bc8ff55275d82382bf957413
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "56903217"
---
# <a name="azure-cache-for-redis-samples"></a>“用于 Redis 的 Azure 缓存”示例
本主题提供 Azure Redis 缓存示例列表，包括连接到缓存、从缓存读取数据和将数据写入缓存，以及使用 ASP.NET Azure Redis 缓存提供程序等方案。 有些示例是可下载的项目，有些示例提供了分步指南并包含代码片段但没有链接到可下载的项目。

## <a name="hello-world-samples"></a>Hello world 示例
本部分中的示例显示了使用各种语言和 Redis 客户端连接到 Azure Redis 缓存实例、从缓存中读取数据以及向其中写入数据方面的基础知识。

[Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) 示例展示了如何使用 [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET 客户端执行不同的缓存操作。

此示例演示如何：

- 使用不同的连接选项
- 使用同步和异步操作与缓存相互读取和写入对象
- 使用 Redis MGET/MSET 命令返回指定键的值
- 执行 Redis 事务操作
- 处理 Redis 列表和排序集
- 使用 JsonConvert 序列化程序存储.NET 对象
- 使用 Redis 集实现标记
- 使用 Redis 群集

有关详细信息，请参阅 GitHub 上的 [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis)；有关更多的使用方案，请参阅 [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/tests) 单元测试。

[如何将 Azure Redis 缓存与 Python 配合使用](cache-python-get-started.md)展示了如何使用 Python 和 [redis-py](https://github.com/andymccurdy/redis-py) 客户端开始使用 Azure Redis 缓存。

[在缓存中处理 .NET 对象](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)演示了如何对 .NET 对象进行序列化，以便可以将它写入到 Azure Redis 缓存实例以及从中读取它。 

## <a name="use-azure-cache-for-redis-as-a-scale-out-backplane-for-aspnet-signalr"></a>将 Azure Redis 缓存用作 ASP.NET SignalR 的横向扩展基架
[将 Azure Redis 缓存用作 ASP.NET SignalR 的横向扩展基架](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane)示例演示如何将 Azure Redis 缓存用作 SignalR 基架。 有关基架的更多信息，请参阅 [采用 Redis 的 SignalR 扩展](https://www.asp.net/signalr/overview/performance/scaleout-with-redis)。

## <a name="azure-cache-for-redis-customer-query-sample"></a>Azure Redis 缓存客户查询示例
此示例对从缓存访问数据与从持久存储访问数据时的性能进行了比较。 此示例有两个项目。

- [展示 Azure Redis 缓存如何通过对数据进行缓存提高性能](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
- [为进行展示创立数据库和缓存](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET 会话状态和输出缓存
[使用 Azure Redis 缓存存储 ASP.NET SessionState 和 OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching)示例演示如何使用 Azure Redis 缓存通过为 Redis 使用 SessionState 和 OutputCache 提供程序来存储 ASP.NET 会话和输出缓存。

## <a name="manage-azure-cache-for-redis-with-maml"></a>使用 MAML 管理 Azure Redis 缓存
[使用 Azure Management Libraries 管理 Azure Redis 缓存](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML)示例展示如何使用 Azure Management Libraries 来管理（创建/更新/删除）缓存。 

## <a name="custom-monitoring-sample"></a>自定义监视示例
[访问 Azure Redis 缓存监视数据](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring)示例演示了如何在 Azure 门户外访问 Azure Redis 缓存的监视数据。

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>使用 PHP 和 Redis 编写的 Twitter 式克隆
[Retwis](https://github.com/SyntaxC4-MSFT/retwis) 示例是 Redis Hello World。 它是最小的 Twitter 样式的社交网络克隆使用 Redis 和 PHP 编写使用 [Predis](https://github.com/nrk/predis) 客户端。 源代码旨在是非常简单，并且在同一时间以显示其他 Redis 数据结构。

## <a name="bandwidth-monitor"></a>带宽监视器
[带宽监视器](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) 示例允许监视客户端上使用的带宽。 要测量带宽、请在缓存客户端计算机上运行该示例，对缓存执行调用，并观察带宽监视器示例报告的带宽。

<!-- Update_Description: update metedata properties -->

