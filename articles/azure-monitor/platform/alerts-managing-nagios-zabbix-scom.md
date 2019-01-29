---
title: 在 Azure Monitor 中管理来自 SCOM、Zabbix 和 Nagios 的警报
description: 在 Azure Monitor 中管理来自 SCOM、Zabbix 和 Nagios 的警报
author: lingliw
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/21/19
ms.author: v-lingwu
ms.subservice: alerts
ms.openlocfilehash: 9a1282c3caabef43469de09506c87932075d9b47
ms.sourcegitcommit: 0cb57e97931b392d917b21753598e1bd97506038
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2019
ms.locfileid: "54906076"
---
# <a name="manage-alerts-from-scom-zabbix-and-nagios-in-azure-monitor"></a>在 Azure Monitor 中管理来自 SCOM、Zabbix 和 Nagios 的警报

现在可以在[统一警报体验](https://aka.ms/azure-alerts-overview)中查看来自 Nagios、Zabbix 和 System Center Operations Manager 的警报。 这些警报来自 Log Analytics 与 Nagios/Zabbix 服务器或 System Center Operations Manager 的集成。 

## <a name="prerequisites"></a>先决条件
Log Analytics 存储库中具有 Alert 类型的任何记录都将导入到统一警报体验中，因此必须执行收集这些记录所需的配置。



## <a name="view-your-alert-instances"></a>查看警报实例
将导入配置到 Log Analytics 中后，可以开始在[统一警报体验](https://aka.ms/azure-alerts-overview)中从这些监视服务查看警报实例。 这些警报实例出现在统一警报体验中后，你就可以[管理警报实例](https://aka.ms/managing-alert-instances)、[管理基于这些警报创建的智能组](https://aka.ms/managing-smart-groups)以及[更改警报和智能组的状态](https://aka.ms/managing-alert-smart-group-states)。

> [!NOTE]
>  1. 此解决方案仅允许在 Azure Monitor 中查看 SCOM/Zabbix/Nagios 触发的警报。 可以在相应的监视工具中查看/编辑警报规则配置。 
>  1. 所有已触发的警报实例都将同时显示在 Azure Monitor 和 Azure Log Analytics 中。 目前，无法在两者之间选择其一，也无法仅引入特定的已触发警报。
>  1. 来自 SCOM、Zabbix 和 Nagios 的所有警报的信号类型都是“未知”，因为未提供基础遥测类型。
>  1. Nagios 警报是无状态的 - 例如，警报的[监视条件](https://aka.ms/azure-alerts-overview)不会从“已触发”变为“已解决”， 而是，“已触发”和“已解决”都显示为单独的警报实例。 







