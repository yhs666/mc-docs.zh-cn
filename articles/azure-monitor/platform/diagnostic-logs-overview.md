---
title: Azure 诊断日志概述
description: 了解什么是 Azure 诊断日志，以及如何使用该诊断日志了解发生在 Azure 资源内的事件。
author: lingliw
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: v-lingwu
ms.component: logs
ms.openlocfilehash: 2ec85ca1d24fdf0c207df76bb2879ddfaa6111e1
ms.sourcegitcommit: 461c7b2e798d0c6f1fe9c43043464080fb8e8246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68818388"
---
# <a name="overview-of-azure-diagnostic-logs"></a>Azure 诊断日志概述

**诊断日志**提供有关 Azure 资源操作的丰富、频繁的数据。 Azure Monitor 提供两种类型的诊断日志：

* **租户日志** - 这些日志来自 Azure 订阅之外存在的租户级服务，例如 Azure Active Directory 日志。
* **资源日志** - 这些日志来自在 Azure 订阅中部署资源的 Azure 服务，例如网络安全组或存储帐户。

    ![资源诊断日志与其他类型的日志 ](./media/diagnostic-logs-overview/Diagnostics_Logs_vs_other_logs_v5.png)

这些日志的内容因 Azure 服务和资源类型而异。 例如，网络安全组规则计数器和 Key Vault 审核是两种类型的诊断日志。

这些日志与[活动日志](activity-logs-overview.md)不同。 通过活动日志，可以深入了解使用资源管理器在订阅中的资源上执行的操作，例如创建虚拟机或删除逻辑应用。 活动日志是一个订阅级别的日志。 通过资源级诊断日志深入了解在资源本身内执行的操作，例如，从 Key Vault 获取机密。

这些日志也与来宾 OS 级诊断日志不同。 来宾 OS 级诊断日志是由在虚拟机内部或其他受支持的资源类型中运行的代理收集的日志。 资源级诊断日志不需要代理并从 Azure 平台本身捕获特定于资源的数据，而来宾 OS 级诊断日志从操作系统和在虚拟机上运行的应用程序捕获数据。

并非所有服务都支持此处所述的诊断日志。 [本文包含的一个部分列出了哪些服务支持诊断日志](../../azure-monitor/platform/diagnostic-logs-schema.md)。

## <a name="what-you-can-do-with-diagnostic-logs"></a>可以对诊断日志执行的操作
可以对诊断日志执行的部分操作如下：

![诊断日志的逻辑位置](./media/diagnostic-logs-overview/Diagnostics_Logs_Actions.png)

* 将诊断日志保存到[**存储帐户**](../../azure-monitor/platform/archive-diagnostic-logs.md)进行审核或手动检查。 可以使用**资源诊断设置**指定保留时间（天）。
* [将诊断日志流式传输到**事件中心**](diagnostic-logs-stream-event-hubs.md)，方便第三方服务或自定义分析解决方案（例如 PowerBI）引入。
* 使用 [Azure Monitor](../../azure-monitor/platform/collect-azure-metrics-logs.md) 对其进行分析时，其中的数据将立即写入到 Azure Monitor，而无需先将数据写入到存储。  

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

可以使用与发出日志的订阅不同的订阅中的存储帐户或事件中心命名空间。 配置设置的用户必须对这两个订阅具有相应的 RBAC 访问权限。

> [!NOTE]
>  当前无法将网络流日志存档到安全虚拟网络后的存储帐户。

## <a name="diagnostic-settings"></a>诊断设置

使用资源诊断设置配置资源诊断日志。 使用租户诊断设置配置租户诊断日志。 用于服务控制的**诊断设置**：

* 将诊断日志和指标发送到的位置（存储帐户、事件中心和/或 Azure Monitor）。
* 发送哪些日志类别，是否也会发送指标数据。
* 应该将每个日志类别保留在存储帐户中多长时间。
    - 保留期为 0 天表示永久保留日志。 如果不需永久保留，则可将该值设置为 1 到 365 之间的任意天数。
    - 如果设置了保留策略，但禁止将日志存储在存储帐户中（例如，如果仅选择了“事件中心”或“Log Analytics”选项），则保留策略无效。
    - 保留策略按天应用，因此在一天结束时 (UTC)，会删除当天已超过保留策略期限的日志。 例如，假设保留策略的期限为一天，则在今天开始时，会删除前天的日志。 删除过程从午夜 (UTC) 开始，但请注意，可能最多需要 24 小时才能将日志从存储帐户中删除。

这些设置可以通过门户中的诊断设置、Azure PowerShell 和 CLI 命令或 [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/) 进行配置。

> [!NOTE]
> 当前不支持通过诊断设置发送多维指标。 多维指标将按平展后的单维指标导出，并跨维值聚合。
>
> 例如：  可以基于每个队列级别浏览和绘制事件中心上的“传入消息”指标。 但是，当通过诊断设置导出时，该指标将表示为事件中心的所有队列中的所有传入消息。
>
>

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>诊断日志支持的服务、类别和架构

[参阅此文](../../azure-monitor/platform/diagnostic-logs-schema.md)获取受支持服务的完整列表，以及这些服务使用的日志类别和架构。

## <a name="next-steps"></a>后续步骤

* [将资源诊断日志流式传输到事件中心  ](diagnostic-logs-stream-event-hubs.md)
* [使用 Azure Monitor REST API 更改资源诊断设置](https://msdn.microsoft.com/library/azure/dn931931.aspx)
