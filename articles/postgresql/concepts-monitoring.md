---
title: 在 Azure Database for PostgreSQL 中进行监视
description: 本文介绍用于对 Azure Database for PostgreSQL 进行监视并发出警报的指标，包括 CPU、存储和连接统计信息。
services: postgresql
author: WenJason
ms.author: v-jay
manager: digimobile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
origin.date: 02/28/2018
ms.date: 08/27/2018
ms.openlocfilehash: e9efe9fd2dc9306896e46d9c88c550e69f30ca88
ms.sourcegitcommit: 6dd65fba579a2ce25c63ac69ff3b71d814a9d256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2018
ms.locfileid: "42703826"
---
# <a name="monitoring-in-azure-database-for-postgresql"></a>在 Azure Database for PostgreSQL 中进行监视
监视服务器的相关数据有助于排查工作负荷故障及优化工作负荷。 Azure Database for PostgreSQL 提供了各种指标来帮助用户深入了解为 PostgreSQL 服务器提供支持的资源的行为。 

## <a name="metrics"></a>指标
所有 Azure 指标的频率都是一分钟，每个指标提供 30 天的历史记录。 其他任务包括设置自动操作、执行高级分析和存档历史记录。 有关详细信息，请参阅 [Azure 指标概述](../monitoring-and-diagnostics/monitoring-overview-metrics.md)。

### <a name="list-of-metrics"></a>指标列表
这些指标适用于 Azure Database for PostgreSQL：

|指标|指标显示名称|计价单位|说明|
|---|---|---|---|---|
|cpu_percent|CPU 百分比|百分比|使用的 CPU 百分比。|
|memory_percent|内存百分比|百分比|使用的内存百分比。|
|io_consumption_percent|IO 百分比|百分比|使用的 IO 百分比。|
|storage_percent|存储百分比|百分比|所用存储占服务器最大存储的百分比。|
|storage_used|已用的存储量|字节|使用的存储量。 服务使用的存储包括数据库文件、事务日志和服务器日志。|
|storage_limit|存储限制|字节|此服务器的最大存储。|
|active_connections|活动连接总数|计数|服务器的活动连接数。|
|connections_failed|失败连接总数|计数|服务器的失败连接数。|


## <a name="next-steps"></a>后续步骤
- 若要深入了解如何使用 Azure 门户、REST API 或 CLI 访问和导出指标，请参阅 [Azure 指标概述](../monitoring-and-diagnostics/monitoring-overview-metrics.md)。
