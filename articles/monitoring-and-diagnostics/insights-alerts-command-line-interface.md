---
title: 使用跨平台 Azure CLI 为 Azure 服务创建经典警报 | Microsoft Docs
description: 满足指定的条件时触发电子邮件或通知、或调用网站 URL (Webhook) 或自动化。
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 10/24/2016
ms.author: v-yiso
ms.date: 08/20/2018
ms.openlocfilehash: 78d63e73f028e9ed8ccc68ad3d9c9c1a6faaa5a9
ms.sourcegitcommit: 664584f55e0a01bb6558b8d3349d41d3f05ba4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/20/2018
ms.locfileid: "41704476"
---
# <a name="use-the-cross-platform-azure-cli-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>使用跨平台 Azure CLI 在 Azure Monitor 中为 Azure 服务创建经典指标警报 

> [!div class="op_single_selector"]
> * [Portal](./insights-alerts-portal.md)
> * [PowerShell](./insights-alerts-powershell.md)
> * [CLI](./insights-alerts-command-line-interface.md)
>
>

> [!NOTE]
> 本文介绍了如何创建旧式经典指标警报。 Azure Monitor 现在支持[较新、更好的指标警报](monitoring-near-real-time-metric-alerts.md)。 这些警报可监视多个指标，并允许对维度指标发出警报。 即将推出对新型指标警报的 Azure CLI 支持。
>
>

本文介绍了如何使用跨平台命令行接口 (Azure CLI) 设置 Azure 经典指标警报。

> [!NOTE]
> Azure Monitor 是 2016 年 9 月 25 日之前称为“Azure Insights”的新名称。 但是，这里所述的命名空间以及命令仍包含“insight”一词。

可以根据 Azure 服务的指标或基于 Azure 中发生的事件接收警报。

* **指标值**：当指定指标的值超过在任一方向分配的阈值时，将触发警报。 也就是说，当条件先是满足以及之后不再满足该条件时，警报都会触发。    

* **活动日志事件**：发生每个事件，或当出现特定事件时可触发警报。 若要详细了解活动日志，请参阅[创建活动日志警报（经典）](monitoring-activity-log-alerts.md)。 

可配置经典指标警报，使警报触发时执行以下操作：

* 向服务管理员和共同管理员发送电子邮件通知。 
* 将电子邮件发送到指定的电子邮件地址。
* 调用 Webhook。
* 开始执行 Azure Runbook（当前仅从 Azure 门户）。

可使用以下项配置和获取有关经典指标警报规则的信息： 

* [Azure 门户](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure CLI](insights-alerts-command-line-interface.md)
* [Azure 监视器 REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

还可以通过键入命令并在末尾键入 **-help** 来获取命令帮助。 下面是一个示例： 

```console
azure insights alerts -help
azure insights alerts actions email create -help
```

## <a name="create-alert-rules-by-using-azure-cli"></a>使用 Azure CLI 创建警报规则
1. 安装必备组件后，登录到 Azure。 有关开始使用所需的命令，请参阅 [Azure Monitor CLI 示例](insights-cli-samples.md)。 这些命令可帮助你进行登录，查看正在使用的订阅，并为运行 Azure Monitor 命令做好准备。

    ```console
    azure login
    azure account show
    azure config mode arm 

    ```

2. 若要列出资源组的现有规则，请使用以下格式：**azure insights alerts rule list** *[options] &lt;resourceGroup&gt;*

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. 若要创建一条规则，首先需要有以下几条重要信息。
    * 要为其设置警报的资源的资源 ID。
    * 可用于该资源的指标定义。

     获取资源 ID 的一种方法是使用 Azure 门户。 假设已创建该资源，请在门户中选中它。 然后，在下一个边栏选项卡的“设置”部分中，选择“属性”。 **资源 ID** 是下一个边栏选项卡中的字段。 
     以下是 Web 应用的示例资源 ID：

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     若要获取上一个资源示例中的可用指标及其单位的列表，请使用以下 Azure CLI 命令：  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M 
      ```

     PT1M 是可用度量的粒度（以 1 分钟为间隔）。 使用不同的粒度时，可获得不同的指标选项。
     
4. 若要创建基于指标的警报规则，请使用以下格式的命令：

    **azure insights alerts rule metric set** *[options] &lt;ruleName&gt; &lt;location&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operator&gt; &lt;threshold&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*

    以下示例设置了一个关于网站资源的警报。 当在 5 分钟内持续收到任何流量以及再次在 5 分钟内未收到任何流量时，警报触发。 

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. 若要在经典指标警报触发时创建 Webhook 或发送电子邮件，首先要创建电子邮件或 Webhook。 然后紧随其后创建规则。 不能将 Webhook 或电子邮件与已创建的规则相关联。

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. 可以通过查看各个规则来验证是否已正确创建了警报。

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. 若要删除规则，请使用以下格式的命令：

    **insights alerts rule delete** [options] &lt;resourceGroup&gt; &lt;ruleName&gt;

    这些命令将删除先前在本文中创建的规则。

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>后续步骤

* [获取 Azure 监视概述](./monitoring-overview.md)，包括可收集和监视的信息的类型。
* 了解[在警报中配置 Webhook](./insights-webhooks-alerts.md)的详细信息。
* 详细了解[针对活动日志事件配置警报](./monitoring-activity-log-alerts.md)。
* 了解关于 [Azure 自动化 Runbook](../automation/automation-starting-a-runbook.md) 的详细信息。
* 获取[收集诊断日志概述](monitoring-overview-of-diagnostic-logs.md)以收集有关服务的详细高频率指标。
* 
            [大致了解指标收集](insights-how-to-customize-monitoring.md)以确保服务可用且响应迅速。
