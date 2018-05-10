---
title: Azure 监视器入门
description: 开始使用 Azure 监视器来洞察资源操作并根据数据采取措施。
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/25/2018
ms.date: 05/14/2018
ms.author: johnkem
ms.openlocfilehash: 7f558f428c0f96ad69c23eddfb04454f1cb73c39
ms.sourcegitcommit: 0b63440e7722942ee1cdabf5245ca78759012500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
---
# <a name="get-started-with-azure-monitor"></a>Azure 监视器入门
Azure Monitor 是一项平台服务，可提供单个源用于监视 Azure 资源。 通过 Azure Monitor，可直观显示、查询、路由和存档来自 Azure 内部资源的指标和日志并对其采取其他操作。 可以通过使用 Azure 门户、[监视器 PowerShell Cmdlet](insights-powershell-samples.md)、[跨平台 CLI](insights-cli-samples.md) 或 [Azure 监视器 REST API](https://msdn.microsoft.com/library/dn931943.aspx) 来处理此数据。 本文使用该门户进行演示，演练 Azure Monitor 的几个重要组件。

## <a name="walkthrough"></a>演练
1. 在门户中，导航到“所有服务”并找到“监视器”选项。 单击星型图标将此选项添加到收藏夹列表中，以便可始终从左侧导航栏轻松访问此选项。

    ![在服务列表中监视](./media/monitoring-get-started/monitor-more-services.png)
2. 单击“监视”选项以打开“监视”页面。 此页面将所有监视设置和数据汇聚到一个合并视图中。 它首先打开的是“概述”部分。 “概述”显示与订阅中的资源相关的所有监视警报、错误和服务运行状况公告的汇总。  

    ![监视器导航](./media/monitoring-get-started/monitor-blade-nav.png)

    Azure 监视器有 3 个基本类别的监视数据：**活动日志**、**度量**和**诊断日志**。
3. 单击“活动日志”，确保显示活动日志部分。

    [活动日志](monitoring-overview-activity-logs.md)描述了对订阅资源执行的所有操作。 通过活动日志，可确定对于订阅资源上的任何创建、更新或删除操作，“谁何时执行什么操作”。 例如，活动日志可以告知何时停止了 Web 应用，以及谁停止了 Web 应用。 活动日志事件在平台中存储 90 天，在此期间内可供查询。

    ![活动日志](./media/monitoring-get-started/monitor-act-log-blade.png)

    可以为常用的筛选器创建并保存查询，并将最重要的查询固定到门户仪表板，这样，只要发生符合条件的事件，就能收到通知。
4. 将视图筛选范围定为上周的特定资源组，然后单击“保存”按钮。 为查询指定名称。

    ![保存活动日志查询](./media/monitoring-get-started/monitor-act-log-save.png)
5. 现在，单击“大头针”按钮。

    ![单击活动日志的“大头针”按钮](./media/monitoring-get-started/monitor-act-log-pin.png)

    此演练中的大多数视图都可固定到仪表板。 这有助于创建单一信息源来显示有关服务的操作数据。
6. 返回到仪表板。 现在可看到仪表板中显示了查询（及结果数）。 如果想快速查看订阅中最近发生的任何重大操作（例如，分配了新角色或删除了 VM），这非常有用。

    ![固定到仪表板的活动日志](./media/monitoring-get-started/monitor-act-log-db.png)
7. 返回到“监视器”磁贴并单击“度量”部分。 首先需要使用页面顶部的下拉选项进行筛选和选择，选择一个资源。

    ![筛选资源的指标](./media/monitoring-get-started/monitor-met-filter.png)

    所有 Azure 资源都会发出[**度量**](monitoring-overview-metrics.md)。 此视图将所有指标汇集到了一个控制台中，以便轻松地了解资源的工作原理。 此外，请单击“指标(预览版)”选项卡，查看我们[全新的指标图表体验](https://aka.ms/azuremonitor/new-metrics-charts)。
8. 选中某项资源后，页面左侧会立即显示所有可用度量。 可以选择指标并修改图形类型和时间范围，一次性绘制多个指标的图表。 此外，还可以查看针对此资源设置的所有指标警报。

    ![“度量值”分页](./media/monitoring-get-started/monitor-metric-blade.png)

9. 如果图表令你满意，可使用“大头针”按钮将其固定到仪表板。
10. 返回到“监视器”并单击“诊断日志”。

    ![“诊断日志”边栏选项卡](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [诊断日志](monitoring-overview-of-diagnostic-logs.md)是资源发出的日志，提供与该特定资源的操作相关的数据。 例如，“网络安全组规则计数器”和“逻辑应用工作流日志”就属于诊断日志。 这些日志可存储在存储帐户中，流式传输到事件中心，并/或可发送到 [Log Analytics](../log-analytics/log-analytics-overview.md)。 Log Analytics 是 Microsoft 提供的操作智能产品，用于高级搜索和警报。

    在门户中，可以查看和筛选订阅中所有资源的列表，确定它们是否已启用诊断日志。
    > [!NOTE]
    > 当前不支持通过诊断设置发送多维指标。 多维指标将按平展后的单维指标导出，并跨维值聚合。
    >
    > 例如：可以基于每个队列级别浏览和绘制事件中心上的“传入消息”指标。 但是，当通过诊断设置导出时，该指标将表示为事件中心的所有队列中的所有传入消息。
    >
    >

11. 单击诊断日志页面中的某个资源。 如果诊断日志存储在存储帐户中，则会显示一个可直接下载的每小时日志列表。

    ![一个资源的诊断日志](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    还可单击“诊断设置”，由此设置或修改到存储帐户的存档设置、流式处理到事件中心，或者发送到 Log Analytics 工作区。

    ![启用诊断日志](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    如果已将诊断日志设置发到 Log Analytics，则可在门户的“日志搜索”部分中搜索它们。
12. 导航到“监视器”页的“警报(经典)”部分。

    ![面向公众的警报边栏选项卡](./media/monitoring-get-started/monitor-alerts-nopp.png)

    可在此处管理 Azure 资源上的所有[**经典警报**](monitoring-overview-alerts.md)。 这些警报包括与指标、活动日志事件、Application Insights Web 测试（位置）和 Application Insights 主动诊断相关的警报。 警报将连接到操作组。 [操作组](monitoring-action-groups.md)提供了在触发警报时通知相关人员或执行特定操作的方法。

13. 单击“添加度量警报”以创建警报。

    ![添加指标警报](./media/monitoring-get-started/monitor-alerts-add.png)

    然后，可将警报固定到仪表板，随时轻松查看其状态。

    Azure Monitor 现在还具有[**新型警报**](https://aka.ms/azuremonitor/near-real-time-alerts)，可按低至每分钟的频率进行评估。
可以通过执行以下步骤并将所有相关磁贴固定到仪表板，创建应用程序与基础结构的完整视图，如下所示：

![Azure 监视器仪表板](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>后续步骤
* 阅读[所有 Azure 监视工具的概述](monitoring-overview.md)，了解 Azure Monitor 与它们配合使用的方式。
