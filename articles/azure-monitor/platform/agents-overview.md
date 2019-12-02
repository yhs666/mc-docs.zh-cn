---
title: Azure 监视代理概述| Azure Docs
description: 本文提供可用于在 Azure 或混合环境中支持监视虚拟机的 Azure 代理的详细概述。
services: azure-monitor
author: lingliw
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
origin.date: 11/14/2018
ms.date: 01/21/2019
ms.author: v-lingwu
ms.openlocfilehash: 5c97a365eca25c0e9697a83126eb98f7f76a3ff4
ms.sourcegitcommit: 3a9c13eb4b4bcddd1eabca22507476fb34f89405
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2019
ms.locfileid: "74527903"
---
# <a name="overview-of-the-azure-monitor-agents"></a>Azure Monitor 代理概述 
即使是对于计算资源，也需要监视其来宾操作系统和工作负荷。 从资源内部收集此类监视数据需要用到一个代理。 本文介绍 Azure Monitor 使用的代理，并帮助你确定需要满足特定环境的哪些要求。

## <a name="summary-of-agents"></a>代理摘要

> [!NOTE]
> 由于 Azure Monitor 和 Log Analytics 最近已合并到一起，Azure Monitor 目前有多个代理。 这两个代理具有一些相同的功能，还有一些功能是特定代理的独特功能。 根据具体的要求，你可能需要使用其中的一个或两个代理。 

Azure Monitor 有三个代理，其中每个代理提供特定的功能。 根据具体的要求，可以在虚拟机和其他计算资源上安装一个或多个代理。

* [Azure 诊断扩展](#azure-diagnostic-extension)
* [Log Analytics 代理](#log-analytics-agent)
* [依赖项代理](#dependency-agent)

下表提供了不同代理的快速比较。 有关每个代理的详细信息，请参阅本文的余下内容。

| | Azure 诊断扩展 | Log Analytics 代理 | 依赖关系代理 |
|:---|:---|:---|:---|
| 支持的环境 | Azure | Azure<br>其他云<br>本地 | Azure<br>其他云<br>本地 |
| 操作系统 | Windows<br>Linux | Windows<br>Linux | Windows<br>Linux
| 代理依赖项  | 无 | 无 | 需要 Log Analytics 代理 |
| 收集的数据 | 事件日志<br>ETW 事件<br>Syslog<br>性能<br>IIS 日志<br>.NET 应用程序跟踪输出日志<br>故障转储 | 事件日志<br>Syslog<br>性能<br>IIS 日志<br>自定义日志<br>解决方案中的数据 | 进程详细信息和依赖项<br>网络连接指标 |
| 数据发送目标 | Azure 存储<br>Azure Monitor 指标<br>事件中心 | Azure Monitor 日志 | Azure Monitor 日志 |



## <a name="azure-diagnostic-extension"></a>Azure 诊断扩展
[Azure 诊断扩展](../../azure-monitor/platform/diagnostics-extension-overview.md)从 Azure 计算资源的来宾操作系统和工作负荷收集监视数据。 它主要将数据收集到 Azure 存储中。 可将 Azure Monitor 配置为将数据从存储复制到 Log Analytics 工作区。 还可以将来宾性能数据收集到 Azure Monitor 指标中。

Azure 诊断扩展通常称作 Windows Azure 诊断 (WAD) 或 Linux Azure 诊断 (LAD) 扩展。


### <a name="scenarios-supported"></a>支持的方案

Azure 诊断扩展支持的方案包括：

* 从 Azure 计算资源收集日志和性能数据。
* 将来宾操作系统中的日志和性能数据存档到 Azure 存储。
* 使用 [Azure 存储资源管理器](../../vs-azure-tools-storage-manage-with-storage-explorer.md)等工具查看存储中的监视数据。
* 收集指标数据库中的性能数据，以利用 [Azure Monitor 指标](data-platform-metrics.md)支持的功能，例如，近实时[指标警报](../../azure-monitor/platform/alerts-metric-overview.md)和[自动缩放](autoscale-overview.md)。 
* 将存储中的监视数据收集到 [Log Analytics 工作区](azure-storage-iis-table.md)，以利用 [Azure Monitor 日志](data-platform-logs.md#what-can-you-do-with-azure-monitor-logs)支持的功能，例如[日志查询](../log-query/log-query-overview.md)。
* 使用 [Azure 事件中心](diagnostics-extension-stream-event-hubs.md)将监视数据发送到第三方工具。
* 使用[启动诊断](../../virtual-machines/troubleshooting/boot-diagnostics.md)调查 VM 启动问题。
* 将数据从 VM 中运行的应用程序复制到 [Application Insights](../../azure-monitor/overview.md)，以便与其他应用程序监视相集成。

## <a name="log-analytics-agent"></a>Log Analytics 代理
[Log Analytics 代理](log-analytics-agent.md)从 Azure 中的虚拟机、其他云提供商和本地的来宾操作系统与工作负荷收集监视数据。 它将数据收集到 Log Analytics 工作区中。

Log Analytics 代理是 System Center Operations Manager 使用的同一个代理，多宿主代理计算机同时与管理组和 Azure Monitor 通信。 Azure Monitor 中的某些解决方案也需要此代理。

适用于 Windows 的 Log Analytics 代理通常称作 Microsoft 管理代理 (MMA)。 适用于 Linux 的 Log Analytics 代理通常称作 OMS 代理。


### <a name="scenarios-supported"></a>支持的方案

Log Analytics 代理支持的方案包括：

* 从 Azure、其他云提供商和本地资源中的各种源收集数据。  


以前，*Operations Management Suite* 由多个 Azure 服务捆绑而成，因此，Log Analytics 代理在 Azure 安全中心和 Azure 自动化等服务之间共享。  此套件包含这些服务提供的整套功能，可在 Azure VM 的整个生命周期内为其提供全面的管理。  这些功能的示例包括：

* 使用 [Azure 自动化 Desired State Configuration](../../automation/automation-dsc-overview.md) 来保持一致的配置状态。
* Azure 服务（例如 [Application Insights](/azure-monitor/) 和 [Azure 安全中心](/security-center/)）原生就会将其数据直接存储在 Log Analytics 中。  

## <a name="dependency-agent"></a>依赖关系代理
Dependency Agent 是作为服务映射解决方案的一部分开发的，而后者最初不是由 Microsoft 开发的。 [服务映射](../insights/service-map.md)要求在 Windows 和 Linux 虚拟机上安装 Dependency Agent，并与 Log Analytics 代理集成，以收集有关虚拟机上运行的进程和外部进程依赖项的发现数据。 它将此数据存储在 Log Analytics 工作区中，并将发现的互连组件可视化。

可能需要以某种形式组合使用这些代理才能监视 VM。 这些代理可与 Azure 扩展并列安装，但在 Linux 上，必须先安装 Log Analytics 代理，否则安装将会失败。  

### <a name="considerations"></a>注意事项

- 依赖项代理要求在同一个虚拟机上安装 Log Analytics 代理。
- 在 Linux VM 上，必须先安装 Log Analytics 代理，然后再安装 Azure 诊断扩展。


## <a name="next-steps"></a>后续步骤

- 请参阅 [Log Analytics 代理概述](../../azure-monitor/platform/log-analytics-agent.md)，了解将代理部署到 Auzre、数据中心或其他云环境中托管的计算机所要满足的要求和支持的方法。





