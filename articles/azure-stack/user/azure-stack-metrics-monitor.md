---
title: 使用 Azure Stack 中的监视数据 | Microsoft Docs
description: 了解有关使用 Azure Stack 中的监视数据的选项。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 04/15/2019
ms.date: 07/29/2019
ms.author: v-jay
ms.lastreviewed: 12/01/2018
ms.openlocfilehash: e0ef6a433dfd0a207d93cfe58eae0d28d448c69d
ms.sourcegitcommit: 4d34571d65d908124039b734ddc51091122fa2bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68513272"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>如何使用 Azure Stack 中的监视数据

*适用于：Azure Stack 集成系统*

使用 Azure Monitor 管道可以在单个位置找到监视数据，就像 Azure 中的 Azure Monitor 一样。 但并非在 Azure 中找到的所有监视数据都可在 Azure Stack 中使用。 在本文中，我们提供了在 Azure Stack 中使用监视数据的各种方法的摘要。
 
## <a name="options-for-data-consumption"></a>数据使用选项

| 数据类型 | Category | 支持的服务 | 访问方法 |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Azure Monitor 平台级别指标 | 指标 | [Azure Stack 上的 Azure Monitor 支持的指标](azure-stack-metrics-supported.md) | REST API |
| 计算来宾 OS 指标（例如，性能计数） | 指标 | Windows VM 和 Linux VM | 存储表或 blob：<br>Windows 或 Linux Azure 诊断 <br>事件中心：<br>Windows Azure 诊断 |
| 存储度量值 | 指标 | Azure 存储 | 存储表：<br>存储分析 |
| 活动日志 | 事件 | 所有 Azure 服务 | REST API：<br>Azure Monitor 事件 API |
| 计算来宾 OS 日志（例如，IIS、ETW、syslog） | 事件 | Windows VM 和 Linux VM | 存储表或 blob：<br>Windows 或 Linux Azure 诊断 <br>事件中心：<br>Windows Azure 诊断 |
| 存储日志 | 事件 | Azure 存储 | 存储表：<br>存储分析 |

## <a name="next-steps"></a>后续步骤

详细了解 [Azure Stack 上的 Azure Monitor](azure-stack-metrics-azure-data.md)。
