---
title: 在 Azure 门户中创建和管理器操作组
description: 了解如何在 Azure 门户中创建和管理操作组。
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 04/20/2018
ms.date: 06/18/2018
ms.author: v-yiso
ms.openlocfilehash: 5e623dff5bc1be2f75be6854ac53bfe1428e61eb
ms.sourcegitcommit: 794b9caca1147f1891513410dd61435708ef85ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "34855400"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>在 Azure 门户中创建和管理器操作组
## <a name="overview"></a>概述
本文演示如何在 Azure 门户中创建和管理操作组。

通过操作组可以配置操作列表。 之后，这些组可由你定义的每个警报使用，从而确保每次触发警报时采取相同的操作。

每个操作包含以下属性：

* 名称：操作组中的唯一标识符。  
* **操作类型**：发送语音呼叫或短信、发送电子邮件、调用 Webhook、将数据发送到 ITSM 工具、调用逻辑应用、向 Azure 应用发送推送通知，或者运行自动化 Runbook。
* **详细信息**：相应电话号码、电子邮件地址、webhook URI 或 ITSM 连接详细信息。

有关如何使用 Azure 资源管理器模板以配置操作组的信息，请参阅[操作组资源管理器模板](monitoring-create-action-group-with-resource-manager-template.md)。

## <a name="create-an-action-group-by-using-the-azure-portal"></a>使用 Azure 门户创建操作组
1. 在[门户](https://portal.azure.cn)中，选择“监视器”。 “监视器”边栏选项卡将所有监视设置和数据合并到一个视图中。

    ![“监视”服务](./media/monitoring-action-groups/home-monitor.png)
    
2. 在“设置”部分中，选择“操作组”。

    ![“操作组”选项卡](./media/monitoring-action-groups/action-groups-blade.png)
    
3. 选择“添加操作组”，并填写字段。

    ![“添加操作组”命令](./media/monitoring-action-groups/add-action-group.png)
    
4. 在“操作组名称”框中输入名称，然后在“短名称”框中输入名称。 使用此组发送通知时，短名称被用来代替完整的操作组名称。

    ![“添加操作组”对话框](./media/monitoring-action-groups/action-group-define.png)

5. “订阅”框会自动填充当前订阅。 此“订阅”是在其中保存操作组的订阅。

6. 选择在其中保存操作组的“资源组”。

7. 通过为每个操作提供以下项来定义操作列表：

    a. 名称：输入此操作的唯一标识符。

    b. **操作类型**：选择电子邮件、Webhook 或自动化 Runbook。

    c. **详细信息**：根据操作类型，输入电子邮件地址、Webhook URI 或自动化 Runbook。 

8. 选择“确定”创建操作组。

## <a name="action-specific-information"></a>特定于操作的信息
<dl>
<dt>电子邮件</dt>
<dd>一个操作组中最多可以有 50 个电子邮件操作</dd>
<dd>请参阅[速率限制信息](./monitoring-alerts-rate-limiting.md)一文</dd>
<dt>Runbook</dt>
<dd>一个操作组中最多可以有 10 个 Runbook 操作</dd>
<dt>Webhook</dt>
<dd>一个操作组中最多可以有 10 个 Webhook 操作
<dd>重试逻辑 - 响应的超时期限为 10 秒。 返回的 HTTP 状态代码为 408、429、503、504 时，或者 HTTP 终结点没有响应时，最多可以重试 2 次 Webhook 调用。 首次重试在 10 秒后发生。 第二次（也是最后一次）重试在 100 秒后发生。</dd>
</dl>
## 管理操作组 创建操作组后，它会在“监视器”**** 边栏选项卡的“操作组”**** 部分显示。 选择要管理的操作组：

* 添加、编辑或删除操作。
* 删除操作组。

## <a name="next-steps"></a>后续步骤 ##
* 详细了解[短信警报行为](monitoring-sms-alert-behavior.md)。  
* 获取[对活动日志警报 webhook 架构的了解](monitoring-activity-log-alerts-webhook.md)。  
* 详细了解有关警报的[速率限制](monitoring-alerts-rate-limiting.md)。 
* 获取[活动日志警报概述](monitoring-overview-alerts.md)，了解如何接收警报。  
* 了解如何[配置每次发布服务运行状况通知时的警报](monitoring-activity-log-alerts-on-service-notifications.md)。
