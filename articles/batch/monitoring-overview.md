---
title: 监视 Azure Batch | Azure Docs
description: 了解 Azure 监视服务、指标、诊断日志以及 Azure Batch 的其他监视功能。
services: batch
author: lingliw
manager: digimobile
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 04/12/19
ms.author: v-lingwu
ms.openlocfilehash: 98d4bd6ba33a2132d5811ee68a0ee7ed7f8d8236
ms.sourcegitcommit: f9d082d429c46cee3611a78682b2fc30e1220c87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566280"
---
# <a name="monitor-batch-solutions"></a>监视 Batch 解决方案

Azure 和 Batch 服务提供了一系列服务、工具和 API 来监视 Batch 解决方案。 本概述文章可帮助你选择适合需求的监视方法。

有关可用来监视 Azure 资源的 Azure 组件和服务的概述，请参阅[监视 Azure 应用程序和资源](../monitoring-and-diagnostics/monitoring-overview.md)。

## <a name="subscription-level-monitoring"></a>订阅级监视

在订阅级别（包括 Batch 帐户），[Azure 活动日志](../azure-monitor/platform/activity-logs-overview.md)将操作事件数据收集到[几个类别](../azure-monitor/platform/activity-logs-overview.md#categories-in-the-activity-log)中。

对于 Batch 帐户，具体而言，活动日志收集与帐户创建和删除以及密钥管理相关的事件。

从活动日志中检索事件的一种方法是使用 Azure 门户。 单击“所有服务” > “活动日志”。 或者，使用 Azure CLI、PowerShell cmdlet 或 Azure Monitor REST API 来查询事件。 

## <a name="batch-account-level-monitoring"></a>Batch 帐户级监视

使用 [Azure Monitor](../azure-monitor/overview.md) 的各项功能监视每个 Batch 帐户。 Azure Monitor 针对 Batch 帐户级别范围内的资源（例如池、作业和任务）选择性地收集[诊断日志](../azure-monitor/platform/diagnostic-logs-overview.md)。 可以手动或以编程方式收集并使用此数据来监视 Batch 帐户中的活动以及对问题进行诊断。 有关详细信息，请参阅[用于诊断评估和监视的 Batch 指标、警报和日志](batch-diagnostics.md)。
 
> [!NOTE]
> 指标默认情况下在 Batch 帐户中可用，不需要进行额外配置，它们具有为期 30 天的滚动历史记录。 必须为 Batch 帐户启用诊断日志记录，并且，若要存储或处理诊断日志数据，可能会产生其他成本。 

## <a name="batch-resource-monitoring"></a>Batch 资源监视

在 Batch 应用程序中，可以使用 Batch API 来监视或查询资源（包括作业、任务、节点和池）的状态。 例如：

* [按状态对任务和计算节点计数](batch-get-resource-counts.md)
* [创建可高效列出 Batch 资源的查询](batch-efficient-list-queries.md)
* [创建任务依赖项](batch-task-dependencies.md)
* 使用[作业管理器任务](https://docs.microsoft.com/rest/api/batchservice/job/add#jobmanagertask)
* 监视[任务状态](https://docs.microsoft.com/rest/api/batchservice/task/list#taskstate)
* 监视[节点状态](https://docs.microsoft.com/rest/api/batchservice/computenode/list#computenodestate)
* 监视[池状态](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate)
* 监视[帐户中的池使用情况](https://docs.microsoft.com/rest/api/batchservice/pool/listusagemetrics)
* [按状态对池节点进行计数](https://docs.microsoft.com/rest/api/batchservice/account/listpoolnodecounts)

## <a name="next-steps"></a>后续步骤

* 了解适用于生成批处理解决方案的[批处理 API 和工具](batch-apis-tools.md)。
* 详细了解使用 Batch [实现诊断日志记录](batch-diagnostics.md)。

<!-- Update_Description: wording update -->