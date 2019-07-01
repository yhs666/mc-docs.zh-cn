---
title: Azure 中的管理解决方案的数据收集详细信息 | Azure Docs
description: Azure 中的管理解决方案是逻辑、可视化效果和数据采集规则的集合，提供围绕特定问题领域制定的指标。  本文提供了 Azure 提供的管理解决方案的列表以及有关其数据收集方法和频率的详细信息。
services: log-analytics
documentationcenter: ''
author: lingliw
manager: digimobile
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 6/4/2019
ms.author: v-lingwu
ms.openlocfilehash: c0e4439515c617e24454820a284e9917eb8b287b
ms.sourcegitcommit: 5fc46672ae90b6598130069f10efeeb634e9a5af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67236420"
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Azure 中的管理解决方案的数据收集详细信息
本文包括了 Azure 提供的[管理解决方案](solutions.md)的列表以及指向其详细文档的链接。  它还提供了这些解决方案将数据收集到 Azure Monitor 中时采用的方法和频率的相关信息。  可以使用本文中的信息来了解可用的各种解决方案，并了解各种管理解决方案的数据流和连接要求。 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="list-of-management-solutions"></a>管理解决方案的列表

下表列出了 Azure 中由 Microsoft 提供的[管理解决方案](solutions.md)的列表。 列中的条目意味着解决方案使用该方法将数据收集到 Azure Monitor 中。  如果解决方案没有选择任何列，则它将从另一个 Azure 服务直接写入到 Azure Monitor 中。 访问每个解决方案的链接可以查看其详细文档来了解更多信息。

各个列的说明如下：

- **Microsoft Monitoring Agent** - Windows 和 Linux 上用来运行来自 SCOM 的管理包和来自 Azure 的管理解决方案的代理。 在此配置中，代理直接连接到 Azure Monitor，无需连接到 Operations Manager 管理组。 
- **Operations Manager** - 与 Azure Monitoring Agent 相同的代理。
-  **Azure 存储** - 此解决方案从 Azure 存储帐户收集数据。 
- **是否需要 Operations Manager？** - 管理解决方案进行数据收集时需要一个已连接的 Operations Manager 管理组。 
- **通过管理组发送 Operations Manager 代理数据**
- **收集频率** - 指定管理解决方案收集数据时采用的频率。 



| **管理解决方案** | 平台  | **Microsoft Monitoring Agent** | **Operations Manager 代理** | **Azure 存储** | **是否需要 Operations Manager？** | **通过管理组发送 Operations Manager 代理数据** | **收集频率** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [活动日志分析](../../azure-monitor/platform/collect-activity-logs.md) | Azure | | | | | | 通知时 |
| [AD 评估](../../azure-monitor/insights/ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 天 |
| [AD 复制状态](../../azure-monitor/insights/ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 天 |
| [代理运行状况](solution-agenthealth.md) | Windows 和 Linux | &#8226; | &#8226; | | | &#8226; | 1 分钟 |
| [警报管理](../../azure-monitor/platform/alert-management-solution.md) (Nagios) |Linux |&#8226; | | | | |到达时 |
| [警报管理](../../azure-monitor/platform/alert-management-solution.md) (Zabbix) |Linux |&#8226; | | | | |1 分钟 |
| [警报管理](../../azure-monitor/platform/alert-management-solution.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 分钟 |
| [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) | Azure | | | | | | 不适用 |
| **管理解决方案** | 平台  | **Microsoft Monitoring Agent** | **Operations Manager 代理** | **Azure 存储** | **是否需要 Operations Manager？** | **通过管理组发送 Operations Manager 代理数据** | **收集频率** |
| [Azure SQL Analytics（预览版）](../../azure-monitor/insights/azure-sql.md) | Windows | | | | | | 1 分钟 |
| [备份](https://www.azure.cn/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | 通知时 |
| [容量和性能（预览版）](../../azure-monitor/insights/capacity-performance.md) |Windows |&#8226; |&#8226; | | |&#8226; |到达时 |
| [密钥保管库分析](../../azure-monitor/insights/azure-key-vault.md) |Windows | | | | | |通知时 |
| [网络性能监视器](../../azure-monitor/insights/network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | 每隔 5 秒钟进行 TCP 握手，每隔 3 分钟发送数据 |
| [Office 365 分析（预览版）](solution-office-365.md) |Windows | | | | | |通知时 |
| **管理解决方案** | 平台  | **Microsoft Monitoring Agent** | **Operations Manager 代理** | **Azure 存储** | **是否需要 Operations Manager？** | **通过管理组发送 Operations Manager 代理数据** | **收集频率** |
| [服务地图](../../azure-monitor/insights/service-map.md) | Windows 和 Linux | &#8226; | &#8226; |  |  |  | 15 秒 |
| [SQL 评估](../../azure-monitor/insights/sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 天 |
| [SurfaceHub](../../azure-monitor/insights/surface-hubs.md) |Windows |&#8226; | | | | |到达时 |
| [System Center Operations Manager 评估（预览版）](../../azure-monitor/insights/scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | 七天 |
| [升级准备情况](https://docs.microsoft.com/zh-cn/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 天 |
| [Wire Data 2.0（预览版）](../../azure-monitor/insights/wire-data.md) |Windows（2012 R2 / 8.1 或更高版本） |&#8226; |&#8226; | | | | 1 分钟 |




## <a name="next-steps"></a>后续步骤
* 了解如何[创建查询](../../azure-monitor/log-query/log-query-overview.md)来分析管理解决方案收集的数据。




