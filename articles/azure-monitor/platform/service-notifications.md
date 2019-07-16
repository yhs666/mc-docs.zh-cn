---
title: 什么是 Azure 服务运行状况通知？
description: 借助服务运行状况通知，可以查看由世纪互联 Azure 发布的服务运行状况消息。
author: lingliw
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/21/19
ms.author: v-lingwu
ms.component: logs
ms.openlocfilehash: 3d9f1fbc8768f64035e564e66fdef6dd28caafc5
ms.sourcegitcommit: fd927ef42e8e7c5829d7c73dc9864e26f2a11aaa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/04/2019
ms.locfileid: "67562277"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>使用 Azure 门户查看服务运行状况通知

服务运行状况通知由 Azure 基础结构发布。 这些通知包含有关订阅下的资源的信息。 这些通知是活动日志事件的一个子类别，因此也可在活动日志中找到。 服务运行状况通知可能仅提供信息，也可能提示执行某个操作，具体取决于类别。

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>在 Azure 门户中查看服务运行状况通知
1.  在 [Azure 门户](https://portal.azure.cn)中，选择“监视”  。

    ![Azure 门户菜单的屏幕快照，其中“监视器”处于选中状态](./media/service-notifications/home-monitor.png)

    Azure Monitor 将所有监视设置和数据汇聚到一个合并视图中。 首次打开的是“活动日志”  部分。

3.  选择“**警报**”。

    ![监视活动日志的屏幕快照，其中“警报”处于选中状态](./media/service-notifications/service-health-summary.png)
4. 选择“+添加活动日志警报”  并设置警报，以确保以后可收到服务通知。 有关详细信息，请参阅[创建有关服务通知的活动日志警报](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)。

## <a name="next-steps"></a>后续步骤
每当[发布服务运行状况通知时接收警报通知](../../azure-monitor/platform/alerts-activity-log-service-notifications.md)。  
详细了解[活动日志警报](../../azure-monitor/platform/activity-log-alerts.md)。
