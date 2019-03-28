---
title: include 文件
description: include 文件
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
origin.date: 02/21/2018
ms.date: 03/20/2019
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: a83f0a2f3540abdb3a06a6a7f71a572b6125996c
ms.sourcegitcommit: 5c73061b924d06efa98d562b5296c862ce737cc7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58256384"
---
## <a name="timeout"></a>函数应用超时持续时间 

函数应用的超时持续时间可通过 [host.json](../articles/azure-functions/functions-host-json.md#functiontimeout) 项目文件中的 functionTimeout 属性定义。 下表显示两种计划和两种运行时版本的默认值和最大值。

| 计划 | 运行时版本 | 默认 | 最大值 |
|------|---------|---------|---------|
| 消耗 | 1.x | 5 | 10 个 |
| 消耗 | 2.x | 5 | 10 个 |
| 应用服务 | 1.x | 无限制 | 无限制 |
| 应用服务 | 2.x | 30 | 无限制 |

> [!NOTE] 
> 不管函数应用超时设置如何，230 秒是 HTTP 触发的函数在响应请求时需要的最长时间。 这起因于 [Azure 负载均衡器的默认空闲超时](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds)。 对于处理时间较长的情况，考虑使用 [Durable Functions 异步模式](../articles/azure-functions/durable/durable-functions-concepts.md#async-http)或[延迟实际工作并返回即时响应](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions)。

