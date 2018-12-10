---
title: 在 Azure Database for MySQL 中进行监视
description: 本文介绍了用于对 Azure Database for MySQL 进行监视并发出警报的指标，包括 CPU、存储和连接统计信息。
services: mysql
author: WenJason
ms.author: v-jay
manager: digimobile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
origin.date: 11/05/2018
ms.date: 12/03/2018
ms.openlocfilehash: 704d1025d001818180a7a4fd2fcbd3b37942ec78
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672526"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>在 Azure Database for MySQL 中进行监视

> [!NOTE] 
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql-database-on-azure/)。

监视服务器的相关数据有助于排查工作负荷故障及优化工作负荷。 Azure Database for MySQL 提供了各种指标来帮助用户深入了解服务器的行为。

## <a name="metrics"></a>指标
所有 Azure 指标的频率都是一分钟，每个指标提供 30 天的历史记录。 其他任务包括设置自动操作、执行高级分析和存档历史记录。 有关详细信息，请参阅 [Azure 指标概述](../monitoring-and-diagnostics/monitoring-overview-metrics.md)。

### <a name="list-of-metrics"></a>指标列表
这些指标适用于 Azure Database for MySQL：

|指标|指标显示名称|计价单位|说明|
|---|---|---|---|---|
|cpu_percent|CPU 百分比|百分比|使用的 CPU 百分比。|
|memory_percent|内存百分比|百分比|使用的内存百分比。|
|io_consumption_percent|IO 百分比|百分比|使用的 IO 百分比。|
|storage_percent|存储百分比|百分比|所用存储占服务器最大存储的百分比。|
|storage_used|已用的存储量|字节|使用的存储量。 服务使用的存储可能包括数据库文件、事务日志和服务器日志。|
|serverlog_storage_percent|服务器日志存储空间百分比|百分比|所用服务器日志存储占服务器最大服务器日志存储的百分比。|
|serverlog_storage_usage|服务器日志已用的存储量|字节|使用的服务器日志存储量。|
|serverlog_storage_limit|服务器存储空间上限|字节|此服务器的最大服务器日志存储。|
|storage_limit|存储限制|字节|此服务器的最大存储。|
|active_connections|活动连接数|计数|服务器的活动连接数。|
|connections_failed|失败的连接数|计数|服务器的失败连接数。|
|seconds_behind_master|复制延迟（秒）|计数|副本服务器滞后于主服务器的秒数。|
|network_bytes_egress|网络传出|字节|跨活动连接的网络传出。|
|network_bytes_ingress|网络传入|字节|跨活动连接的网络传入。|
|backup_storage_used|使用的备份存储|字节|已使用的备份存储量。|

## <a name="server-logs"></a>服务器日志
可以在服务器上启用慢查询日志记录。 这些日志也可通过事件中心和存储帐户获得。 若要了解有关日志记录的详细信息，请访问[服务器日志](concepts-server-logs.md)页。

## <a name="next-steps"></a>后续步骤
- 若要深入了解如何使用 Azure 门户、REST API 或 CLI 访问和导出指标，请参阅 [Azure 指标概述](../monitoring-and-diagnostics/monitoring-overview-metrics.md)。
