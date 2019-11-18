---
title: Azure 资源日志概述 | Microsoft Docs
description: 了解 Azure 资源日志支持的服务和事件架构。
ms.service: azure-monitor
ms.subservice: logs
ms.topic: reference
author: lingliw
ms.author: v-lingwu
origin.date: 10/22/2019
ms.date: 11/05/2019
ms.openlocfilehash: 326b46f0435fc7c5cb8de8b618a3d2d492aba946
ms.sourcegitcommit: a89eb0007edd5b4558b98c1748b2bd67ca22f4c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2019
ms.locfileid: "73730335"
---
# <a name="azure-resource-logs-overview"></a>Azure 资源日志概述
Azure 资源日志是 Azure 资源发出的描述内部操作的[平台日志](platform-logs-overview.md)。 所有资源日志共享通用的顶级架构，且每个服务都能灵活地为各自的事件发出唯一属性。

> [!NOTE]
> 资源日志以前称为诊断日志。

## <a name="collecting-resource-logs"></a>收集资源日志
资源日志由支持的 Azure 资源自动生成，但除非你通过[诊断设置](diagnostic-settings.md)配置它们，否则系统不会收集它们。 请为每个 Azure 资源创建诊断设置，以便将这些日志转发到以下目标：

| 目标 | 方案 |
|:---|:---|:---|
| [Log Analytics 工作区](resource-logs-collect-workspace.md) | 借助其他监视数据对日志进行分析，并利用 Azure Monitor 功能（例如日志查询和日志警报）。 |
| [Azure 存储](resource-logs-collect-storage.md) | 将日志存档供审核或备份。 |
| [事件中心](resource-logs-stream-event-hubs.md) | 将日志流式传输到第三方日志记录和遥测系统。  |

## <a name="compute-resources"></a>计算资源
资源日志不同于 Azure 计算资源中的来宾 OS 级别日志。 计算资源需要通过代理从其来宾 OS 中收集日志和指标，包括事件日志、syslog 和性能计数器之类的数据。 请使用[诊断扩展](agents-overview.md#azure-diagnostic-extension)路由 Azure 虚拟机中的日志数据，使用 [Log Analytics 代理](agents-overview.md#log-analytics-agent)将 Azure 中的虚拟机、其他云中的虚拟机和本地的虚拟机中的日志和指标收集到 Log Analytics 工作区。 有关详细信息，请参阅 [Azure Monitor 的监视数据源](data-sources.md)。

## <a name="resource-logs-schema"></a>资源日志架构
有关资源日志架构和类别的详细信息，请参阅[资源日志架构](diagnostic-logs-schema.md)。 

## <a name="next-steps"></a>后续步骤

* [详细了解其他 Azure 平台日志](platform-logs-overview.md)，这些日志可以用来监视 Azure。
