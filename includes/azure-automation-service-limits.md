---
title: include 文件
description: include 文件
services: automation
author: WenJason
ms.service: automation
ms.topic: include
origin.date: 12/13/2018
ms.date: 01/14/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 20d4d40af67165fe9ef84cb9a95ea7e3bc4ae172
ms.sourcegitcommit: 5eff40f2a66e71da3f8966289ab0161b059d0263
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193314"
---
#### <a name="process-automation"></a>流程自动化

| 资源 | 最大限制 |说明|
| --- | --- |---|
| 每个自动化帐户每 30 秒可以提交的新作业的最大数量（非计划的作业） |100 |达到此限制时，后续作业创建请求会失败。 客户端会收到错误响应。|
| 每个自动化帐户相同时间实例并发运行的作业的最大数量（非计划的作业） |200 |达到此限制时，后续作业创建请求会失败。 客户端会收到错误响应。|
| 30 天滚动期内作业元数据的最大存储大小。 | 10GB（约 400 万个作业）|达到此限制时，后续作业创建请求会失败。 |
| 每个自动化帐户每 30 秒可以导入的模块的最大数量 |5 ||
| 一个模块的最大大小 |100 MB ||
| 作业运行时间 - 免费层 |每个订阅每个日历月 500 分钟 ||
| 每个沙盒允许的最大磁盘空间 **<sup>1</sup>** |1 GB |仅适用于 Azure 沙盒|
| 给定到沙盒的最大内存量 **<sup>1</sup>** |400 MB |仅适用于 Azure 沙盒|
| 每个沙盒允许的最大网络套接字数量 **<sup>1</sup>** |1000 |仅适用于 Azure 沙盒|
| 每个 runbook 允许的最大运行时 **<sup>1</sup>** |3 小时 |仅适用于 Azure 沙盒|
| 订阅中自动化帐户的最大数目 |无限制 ||
| Runbook 作业参数大小上限   | 512 kb||
| Runbook 参数数量上限   | 50|可以将 JSON 或 XML 字符串传递给参数，并在达到 50 个参数的限制时使用 Runbook 对其进行分析|
| Webhook 有效负载大小上限 |  512 kb|
| 保留作业数据的最大天数|30 天|
| PowerShell 工作流状态大小上限 |5 MB| 执行检查点工作流时适用于 PowerShell 工作流 runbook。|

<sup>1</sup> 沙盒是可由多个作业使用的共享环境，使用相同沙盒的作业受到该沙盒的资源限制的约束。

