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
origin.date: 09/14/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.openlocfilehash: ab12e23b7cd4125149ae8191bee9b4dd30b8c529
ms.sourcegitcommit: 8a99d90ab1e883295aed43eb9ef2c9bc58456139
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2018
ms.locfileid: "48849127"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>如何使用 Azure Stack 中的监视数据

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用 Azure Monitor 管道可以在一个位置找到监视数据，就像中国 Azure 中的 Azure Monitor 一样。 但并非在中国 Azure 中找到的所有监视数据都可在 Azure Stack 中使用。 在本文中，可以找到可以编程方式从服务中提取监视数据的各种方法的摘要。
 
## <a name="options-for-data-consumption"></a>数据使用选项

| 数据类型 | 类别 | 支持的服务 | 访问方法 |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Azure Monitor 平台级别指标 | 指标 | [Azure Stack 上的 Azure Monitor 支持的指标](azure-stack-metrics-supported.md) | REST API |
| 计算来宾 OS 指标（例如，性能计数） | 指标 | Windows 和 Linux 虚拟机 | 存储表或 blob：<br>Windows 或 Linux Azure 诊断 <br>事件中心：<br>Windows Azure 诊断 |
| 存储度量值 | 指标 | Azure 存储 | 存储表：<br>存储分析 |
| 活动日志 | 事件 | 所有 Azure 服务 | REST API：<br>Azure Monitor 事件 API |
| 计算来宾 OS 日志（例如，IIS、ETW、syslog） | 事件 | Windows 和 Linux 虚拟机 | 存储表或 blob：<br>Windows 或 Linux Azure 诊断 <br>事件中心：<br>Windows Azure 诊断 |
| 存储日志 | 事件 | Azure 存储 | 存储表：<br>存储分析 |

## <a name="next-steps"></a>后续步骤

详细了解 [Azure Stack 上的 Azure Monitor](azure-stack-metrics-azure-data.md)。
