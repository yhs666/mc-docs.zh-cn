---
title: "使用 Azure 门户创建 SQL 数据库警报 | Azure"
description: "使用 Azure 门户创建 SQL 数据库警报，该警报可在满足指定的条件时触发通知或自动化操作。"
author: Hayley244
manager: digimobile
editor: 
services: sql-database
documentationcenter: 
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/01/2017
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 89df1d6e2ef2c611d373dcdd6ca1a82fe4cadcc1
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 使用 Azure 门户为 Azure SQL 数据库创建警报
<a id="use-azure-portal-to-create-alerts-for-azure-sql-database" class="xliff"></a>

## 概述
<a id="overview" class="xliff"></a>
本文将展示如何使用 Azure 门户设置 Azure SQL 数据库警报。 本文还提供了值和阈值的最佳实践。    

可以根据监视指标或事件接收 Azure 服务的警报。

* **指标值** - 当指定指标的值在任一方向越过了指定的阈值时警报将触发。 也就是说，当条件先是满足以及之后不再满足该条件时，警报都会触发。    
* **活动日志事件** - 警报可以在发生 *每个* 事件时都触发，也可以仅在发生特定数量的事件时触发。

可以配置警报以在其触发时执行以下操作：

* 向服务管理员和共同管理员发送电子邮件通知
* 向指定的其他电子邮件地址发送电子邮件。
* 调用 Webhook
* 开始执行 Azure Runbook（仅在 Azure 门户中可行）

可使用以下项配置并获取有关警报规则的信息

* [Azure 门户](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [命令行接口 (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure 监视器 REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## 使用 Azure 门户创建指标的警报规则
<a id="create-an-alert-rule-on-a-metric-with-the-azure-portal" class="xliff"></a>
1. 在此[门户](https://portal.azure.cn/)，查找想要监视的资源并选中它。
2. 在“监视”部分下，选择“警报”或“警报规则”。 对于不同的资源，文本和图标可能会略有不同。  

    ![监视](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
3. 选择“添加通知”命令，并填写字段。

    ![添加警报](../monitoring-and-diagnostics/media/insights-alerts-portal/AddAlertOnlyParamsPage.png)
4. **命名**警报规则，并选择也在通知电子邮件中显示的“说明”。
5. 选择想要监视的“指标”为该指标选择一个“条件”和“阈值”。 还选择了触发警报前指标规则必须满足的时间**段**。 例如，如果使用了时间“PT5M”，并且警报监视使用率高于  80% 的 CPU，则当 CPU 的使用率持续高于 80% 达 5 分钟时，该警报将触发。 在发生第一次触发后，当 CPU 使用率保持低于 80% 达到 5 分钟时，该触发器将再次触发。 每 1 分钟对 CPU 进行一次测量。   
6. 如果触发警报时希望向管理员和共同管理员发送电子邮件，则选择“向所有者发送电子邮件...”。
7. 触发警报时，如果希望其他电子邮件收到通知，请将其添加到“其他管理员电子邮件”字段下。 用分号隔开多个电子邮件 - *email@contoso.com;email2@contoso.com*
8. 触发警报时，如果希望调用有效的 URI，请将其放入“Webhook”字段。
9. 如果使用 Azure 自动化时，则触发警报时可选择要运行的 Runbook。
10. 警报创建完成后，选择“确定”。   

几分钟后，警报将处于活动状态，并按前面所述进行触发。

## 管理警报
<a id="managing-your-alerts" class="xliff"></a>
在创建警报后，可以选择警报并且：

* 查看其中显示了指标阈值和前一天的实际值的图形。
* 编辑或删除其。
* 如果想要暂时停止或恢复接收该警报的通知，可**禁用**或**启用**它。

## SQL 数据库警报值和阈值
<a id="sql-database-alert-values-and-thresholds" class="xliff"></a>

| 资源类型 | 指标名称 | 友好名称 | 聚合类型 | 最小警报时间范围|
| --- | --- | --- | --- | --- |
| SQL 数据库 | cpu_percent | CPU 百分比 | 平均值 | 5 分钟 |
| SQL 数据库 | physical_data_read_percent | 数据 IO 百分比 | 平均值 | 5 分钟 |
| SQL 数据库 | log_write_percent | 日志 IO 百分比 | 平均值 | 5 分钟 |
| SQL 数据库 | dtu_consumption_percent | DTU 百分比 | 平均值 | 5 分钟 |
| SQL 数据库 | storage | 数据库总大小 | 最大值 | 30 分钟 |
| SQL 数据库 | connection_successful | 成功的连接数 | 总计 | 10 分钟 |
| SQL 数据库 | connection_failed | 失败的连接数 | 总计 | 10 分钟 |
| SQL 数据库 | blocked_by_firewall | 被防火墙阻止 | 总计 | 10 分钟 |
| SQL 数据库 | deadlock | 死锁数 | 总计 | 10 分钟 |
| SQL 数据库 | storage_percent | 数据库大小百分比 | 最大值 | 30 分钟 |
| SQL 数据库 | xtp_storage_percent | In-Memory OLTP 存储百分比（预览） | 平均值 | 5 分钟 |
| SQL 数据库 | workers_percent | 辅助角色百分比 | 平均值 | 5 分钟 |
| SQL 数据库 | sessions_percent | 会话百分比 | 平均值 | 5 分钟 |
| SQL 数据库 | dtu_limit | DTU 限制 | 平均值 | 5 分钟 |
| SQL 数据库 | dtu_used | 已用的 DTU | 平均值 | 5 分钟 |
||||||           
| SQL 数据仓库 | cpu_percent | CPU 百分比 | 平均值 | 10 分钟 |
| SQL 数据仓库 | physical_data_read_percent | 数据 IO 百分比 | 平均值 | 10 分钟 |
| SQL 数据仓库 | storage | 数据库总大小 | 最大值 | 10 分钟 |
| SQL 数据仓库 | connection_successful | 成功的连接数 | 总计 | 10 分钟 |
| SQL 数据仓库 | connection_failed | 失败的连接数 | 总计 | 10 分钟 |
| SQL 数据仓库 | blocked_by_firewall | 被防火墙阻止 | 总计 | 10 分钟 |
| SQL 数据仓库 | service_level_objective | 数据库的服务级别目标 | 总计 | 10 分钟 |
| SQL 数据仓库 | dwu_limit | dwu 限制 | 最大值 | 10 分钟 |
| SQL 数据仓库 | dwu_consumption_percent | DWU 百分比 | 平均值 | 10 分钟 |
| SQL 数据仓库 | dwu_used | 已用的 DWU | 平均值 | 10 分钟 |
||||||               
| 弹性池 | cpu_percent | CPU 百分比 | 平均值 | 5 分钟 |
| 弹性池 | physical_data_read_percent | 数据 IO 百分比 | 平均值 | 5 分钟 |
| 弹性池 | log_write_percent | 日志 IO 百分比 | 平均值 | 5 分钟 |
| 弹性池 | dtu_consumption_percent | DTU 百分比 | 平均值 | 5 分钟 |
| 弹性池 | storage_percent | 存储百分比 | 平均值 | 5 分钟 |
| 弹性池 | workers_percent | 辅助角色百分比 | 平均值 | 5 分钟 |
| 弹性池 | eDTU_limit | eDTU 限制 | 平均值 | 5 分钟 |
| 弹性池 | storage_limit | 存储限制 | 平均值 | 5 分钟 |
| 弹性池 | eDTU_used | 已用的 eDTU | 平均值 | 5 分钟 |
| 弹性池 | storage_used | 已用的存储量 | 平均值 | 5 分钟 |
||||||

## 后续步骤
<a id="next-steps" class="xliff"></a>
* [获取 Azure 监视概述](../monitoring-and-diagnostics/monitoring-overview.md)，包括可收集和监视的信息的类型。
* 了解[在警报中配置 Webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md)的详细信息。
* 了解关于 [Azure 自动化 Runbook](../automation/automation-starting-a-runbook.md) 的详细信息。
* 获取[指标集合概述](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)以确保你的服务可用且响应迅速。