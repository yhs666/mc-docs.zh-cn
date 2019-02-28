---
title: Azure Monitor 日志查询 | Azure Docs
description: 有关如何在 Azure Monitor 中编写日志查询的资源参考。
services: log-analytics
documentationcenter: ''
author: lingliw
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/21/19
ms.author: v-lingwu
ms.openlocfilehash: 17de51e32f3a7c0989305b6f9bca1ca859de4dd4
ms.sourcegitcommit: 7e25a709734f03f46418ebda2c22e029e22d2c64
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2019
ms.locfileid: "56440746"
---
# <a name="azure-monitor-log-queries"></a>Azure Monitor 日志查询
Azure Monitor 日志构建在 Azure 数据资源管理器的基础之上，Azure Monitor 日志查询使用同一 Kusto 查询语言的某个版本。 [Azure 数据资源管理器查询语言文档](/azure/kusto/query)提供了该语言的完整详细信息，在编写 Azure Monitor 日志查询时，应将此文档用作主要参考资源。 本页提供了用于学习编写查询，以及该语言的 Azure Monitor 实现差异的其他资源的链接。

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="getting-started"></a>入门

- [Azure Monitor 日志分析入门](get-started-portal.md)课程介绍了如何在 Azure 门户中编写查询和处理结果。
- [Azure Monitor 日志查询入门](get-started-queries.md)是一门介绍如何使用 Azure Monitor 日志数据编写查询的课程。

## <a name="concepts"></a>概念
- [在 Azure Monitor 中分析日志数据](../../azure-monitor/log-query/log-query-overview.md)简要概述了日志查询以及如何构建 Azure Monitor 日志数据。
- [查看和分析 Azure Monitor 中的日志数据](../../azure-monitor/log-query/portals.md)介绍了可在其中创建和运行日志查询的门户。

## <a name="reference"></a>参考

- [查询语言参考](/azure/kusto/query)是 Kusto 查询语言的完整语言参考。
- [在 Azure Monitor 中执行跨资源日志查询](../../azure-monitor/log-query/cross-workspace-query.md)介绍了如何编写使用多个 Log Analytics 工作区和 Application Insights 应用程序中的数据的日志查询。


## <a name="examples"></a>示例

- Azure Monitor 日志查询示例提供了使用 Azure Monitor 日志数据的示例查询。



## <a name="lessons"></a>课程

- [在 Azure Monitor 日志查询中使用字符串](string-operations.md)介绍了如何使用字符串数据。
- “在 Azure Monitor 日志查询中使用日期时间值”介绍了如何使用日期和时间数据。 
- “Azure Monitor 日志查询中的聚合”和“Azure Monitor 日志查询中的高级聚合”介绍了如何聚合和汇总数据。
- [Azure Monitor 日志查询中的联接](joins.md)介绍了如何联接多个表中的数据。
- [在 Azure Monitor 日志查询中使用 JSON 和数据结构](json-data-structures.md)介绍了如何分析 JSON 数据。
- [在 Azure Monitor 中编写高级日志查询](advanced-query-writing.md)介绍了创建复杂查询和重用代码的策略。
- “通过 Azure Monitor 日志查询创建图表和关系图”介绍了如何将日志查询中的数据可视化。

## <a name="cheatsheets"></a>速查表

-  [从 SQL 到 Azure Monitor 日志查询](sql-cheatsheet.md)可为已经熟悉 SQL 的用户提供帮助。
-  [从 Splunk 到 Azure Monitor 日志查询](sql-cheatsheet.md)可为已经熟悉 Splunk 的用户提供帮助。
 
## <a name="next-steps"></a>后续步骤

- 访问完整的 [Kusto 查询语言参考文档](/azure/kusto/query/)。



