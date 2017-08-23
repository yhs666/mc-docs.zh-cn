---
title: "接收有关服务通知的活动日志警报 | Azure"
description: "在 Azure 服务发生时，通过短信、电子邮件或 webhook 接收通知。"
author: anirudhcavale
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/31/2017
ms.author: v-yiso
ms.date: 08/21/2017
ms.openlocfilehash: 1df60304d2bafdd276e587de58a87e29dbd0f998
ms.sourcegitcommit: ffdf0916d06aa2c6f6e2af49fb49cafb381ace2c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>创建有关服务通知的活动日志警报
## <a name="overview"></a>概述
本文演示如何使用 Azure 门户为服务运行状况通知设置活动日志警报。  

当 Azure 将服务运行状况通知发送到 Azure 订阅时，可以收到警报。  
可以基于以下内容配置警报：
- 服务运行状况通知的类别（事件、维护、信息等）
- 受影响的服务
- 受影响的区域
- 通知的状态（活动与已解决）
- 通知的级别（信息、警告、错误）

还可以配置应将警报发送到的人员：
- 选择现有操作组
- 创建新操作组（可以在以后用于将来的警报）

可以使用以下方式配置和获取有关服务运行状况通知警报的信息
- [Azure 门户](./monitoring-activity-log-alerts-on-service-notifications.md)

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-with-the-azure-portal"></a>使用 Azure 门户为新操作组创建有关服务运行状况通知的警报
1.  在[门户](https://portal.azure.cn)中，导航到“监视”服务

    ![监视](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2.  单击“监视”选项打开“监视”边栏选项卡。 首次打开的是“活动日志”部分。

3.  现在单击“警报”部分。

    ![警报](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

4.  选择“添加活动日志警报”命令，并填写字段

    ![Add-Alert](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

5.  **命名**活动日志警报，并提供“说明”。

    ![Add-Alert-New-Action-Group](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

6.  **订阅**是在其中保存活动日志警报的订阅。 它会自动填充为当前用于操作的订阅。 这是警报资源将部署到并监视的订阅。

7.  在“订阅”中选择此警报将与之关联的“资源组”。

8.  在“事件类别”下，选择“服务运行状况”选项。 选择要接收的服务运行状况通知的“服务”、“区域”、“类型”、“状态”和“级别”。

9.  通过提供“名称”和“短名称”来“新建”操作组；在此警报触发时发送的通知中会引用短名称。

10. 然后，通过提供接收方来定义接收方的列表

    a. **名称：**接收方的名称、别名或标识符。

    b. **操作类型：**选择通过短信、电子邮件或 Webhook 联系接收方

    c. **详细信息：**根据选择的操作类型，提供电话号码、电子邮件地址或 Webhook URI。

11. 警报创建完成后，选择“确定”。

在几分钟后，警报将如前所述激活并触发。


>[!NOTE]
>这些步骤中定义的操作组将作为现有操作组重复用于所有未来的警报定义。
>
>

## <a name="create-an-alert-on-a-service-health-notification-using-an-existing-action-group-with-the-azure-portal"></a>通过 Azure 门户使用现有操作组创建有关服务运行状况通知的警报
1.  在[门户](https://portal.azure.cn)中，导航到“监视”服务

    ![监视](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)
2.  单击“监视”选项打开“监视”边栏选项卡。 首次打开的是“活动日志”部分。

3.  现在单击“警报”部分。

    ![警报](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)
4.  选择“添加活动日志警报”命令，并填写字段

    ![Add-Alert](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)
5.  为活动日志警报提供“名称”，并选择“说明”。

    ![Add-Alert-Existing-Action-Group](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-existing-action-group.png)
6.  **订阅**是在其中保存活动日志警报的订阅。 它会自动填充为当前用于操作的订阅。 这是警报资源将部署到并监视的订阅。

7.  在“订阅”中选择此警报将与之关联的“资源组”。

8.  在“事件类别”下，选择“服务运行状况”选项。 选择要接收的服务运行状况通知的“服务”、“区域”、“类型”、“状态”和“级别”。

9.  对“通知方式”选择“现有操作组”。 选择适当的操作组。

10. 警报创建完成后，选择“确定”。

在几分钟后，警报将如前所述激活并触发。

## <a name="managing-your-alerts"></a>管理警报

创建了警报之后，它会在“监视器”服务的“警报”部分中可见。 选择要管理的警报，将能够：
* **编辑**它。
* **删除**它。
* 如果要暂时停止或恢复接收警报的通知，可**禁用**或**启用**它。

## <a name="next-steps"></a>后续步骤：
 - 了解[服务运行状况通知](./monitoring-service-notifications.md)  


<!--Update_Description:update meta properties and wording-->