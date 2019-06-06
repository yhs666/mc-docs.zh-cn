---
title: Azure Cosmos DB 触发器日志
description: 了解如何向 Azure Functions 日志记录管道公开 Azure Cosmos DB 触发器日志
author: rockboyfor
ms.service: cosmos-db
ms.topic: sample
origin.date: 05/23/2019
ms.date: 06/03/2019
ms.author: v-yeche
ms.openlocfilehash: b7b188eb05eb801cc58c6314abd3f121cceffea8
ms.sourcegitcommit: 10458f9a72d4648fd5c9953136bb9581bb216015
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66424278"
---
# <a name="how-to-configure-and-read-the-azure-cosmos-db-trigger-logs"></a>如何配置和读取 Azure Cosmos DB 触发器日志

本文介绍如何配置 Azure Functions 环境，以将 Azure Cosmos DB 触发器日志发送到配置的监视解决方案。

<!--Not Available on [monitoring solution](../azure-functions/functions-monitoring.md)-->

## <a name="included-logs"></a>包含的日志

Azure Cosmos DB 触发器在内部使用[更改源处理器库](./change-feed-processor.md)，该库会生成一组运行状况日志，而这些日志又可用于监视内部操作以[进行故障排除](./troubleshoot-changefeed-functions.md)。

运行状况日志描述了在负载平衡方案或初始化期间尝试执行操作时，Azure Cosmos DB 触发器的行为。

## <a name="enabling-logging"></a>启用日志记录

若要启用 Azure Cosmos DB 触发器日志记录，请在 Azure Functions 项目或 Azure Functions 应用中找到 `host.json` 文件，并配置所需的日志记录级别。 必须按以下示例中所示为 `Host.Triggers.CosmosDB` 启用跟踪：

<!--Not Available on [configure the level of required logging](../azure-functions/functions-monitoring.md#log-configuration-in-hostjson)-->

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

使用更新的配置部署 Azure 函数后，将在跟踪中看到 Azure Cosmos DB 触发器日志。 可以在类别 `Host.Triggers.CosmosDB` 下查看所配置的日志记录提供程序中的日志。 

## <a name="query-the-logs"></a>查询日志

在 Azure Application Insights Analytics 中运行以下查询，以查询 Azure Cosmos DB 触发器生成的日志：

<!--Not Available on [Azure Application Insights' Analytics](../azure-monitor/app/analytics.md)-->

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>后续步骤

<!--Not Available on * [Enable monitoring](../azure-functions/functions-monitoring.md)-->

* 了解在 Azure Functions 中使用 Azure Cosmos DB 触发器时如何[诊断和排查常见问题](./troubleshoot-changefeed-functions.md)。

<!--Update_Description: new articles on how to configure cosmos db trigger logs -->
<!--ms.date: 06/03/2019-->
