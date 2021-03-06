---
title: 如何将常见警报架构与逻辑应用集成
description: 了解如何创建一个逻辑应用，以利用常见警报架构来处理所有警报。
ms.service: azure-monitor
ms.subservice: alerts
ms.topic: conceptual
author: lingliw
origin.date: 05/27/2019
ms.date: 06/27/2019
ms.author: v-lingwu
ms.openlocfilehash: 0e3cf2400de6bf63109267b10f2e0bbc301ef032
ms.sourcegitcommit: 3a9c13eb4b4bcddd1eabca22507476fb34f89405
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2019
ms.locfileid: "74528424"
---
# <a name="how-to-integrate-the-common-alert-schema-with-logic-apps"></a>如何将常见警报架构与逻辑应用集成

本文介绍如何创建一个逻辑应用，以利用常见警报架构来处理所有警报。

## <a name="overview"></a>概述

[常见警报架构](https://aka.ms/commonAlertSchemaDocs)跨各种不同警报类型提供一种标准化的可扩展 JSON 架构。 在通过 Webhook、Runbook 和逻辑应用以编程方式利用时，常见警报架构最有用。 本文演示如何编写单个逻辑应用以处理所有警报。 对于其他编程方法，可以运用相同的原则。 本文中所述的逻辑应用为[“重要”字段](/azure-monitor/platform/alerts-common-schema-definitions#essentials-fields)创建了明确定义的变量，并且还描述了如何处理特定于[警报类型](/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields)的逻辑。


## <a name="prerequisites"></a>先决条件 

本文假设读者熟悉以下操作 
* 设置警报规则（[指标](/azure-monitor/platform/alerts-metric)、[日志](/azure-monitor/platform/alerts-log)、[活动日志](/azure-monitor/platform/alerts-activity-log)）
* 设置[操作组](/azure-monitor/platform/action-groups)
* 从操作组中启用[常见警报架构](/azure-monitor/platform/alerts-common-schema#how-do-i-enable-the-common-alert-schema)

    


## <a name="next-steps"></a>后续步骤

* [详细了解操作组](../../azure-monitor/platform/action-groups.md)。
* [详细了解常见警报架构](https://aka.ms/commonAlertSchemaDocs)。

