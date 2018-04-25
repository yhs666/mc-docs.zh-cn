---
title: include 文件
description: include 文件
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
origin.date: 04/06/2018
ms.date: 04/10/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 30ba5d1901eb9055cf71c02ca046995a0febacf5
ms.sourcegitcommit: 6e80951b96588cab32eaff723fe9f240ba25206e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2018
---
在本地开发函数时，可以使用 Azure Functions Core Tools 从终端或从命令提示符安装所需的扩展。 以下 `func extensions install` 命令安装 Azure Cosmos DB 绑定扩展：

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

将 `<taget_version>` 替换为特定包版本。 在 [NuGet.org](https://nuget.org) 上的单个包页上列出了有效版本。

