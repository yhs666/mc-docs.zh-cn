---
title: "增加 Azure 中的应用 | Azure"
description: "了解如何增加 Azure 应用服务中的应用以增加容量和功能。"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/05/2016
ms.date: 12/05/2016
ms.author: v-dazen
ms.openlocfilehash: 1f47976858f5b2ce4fc4c120acaa931304f65829
ms.sourcegitcommit: 86616434c782424b2a592eed97fa89711a2a091c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# 增加 Azure 中的应用
<a id="scale-up-an-app-in-azure" class="xliff"></a>
本文介绍如何在 Azure 应用服务中缩放应用。 缩放的工作流有两种：向上缩放和向外缩放；本文介绍向上缩放工作流。

* [向上缩放](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling)：获取更多 CPU、内存、磁盘空间和额外功能，例如专用虚拟机 (VMs)、自定义域和证书、暂存槽、自动缩放以及更多功能。 可以通过更改应用所属的应用服务计划的定价层来向上缩放。
* [向外缩放](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling)：增加用于运行应用的 VM 实例数。
  根据定价层，最多可以向外缩放到 20 个实例。 有关向外缩放的详细信息，请参阅[手动或自动缩放实例计数](../monitoring-and-diagnostics/insights-how-to-scale.md)。 可以在该文中了解如何使用自动缩放，即根据预定义的规则和计划自动缩放实例计数。

缩放设置仅需几秒即可应用，并且会影响[应用服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)中的所有应用。
在此过程中，不需要更改代码或重新部署应用程序。

有关各个应用服务计划的定价和功能的信息，请参阅[应用服务定价详细信息](https://www.azure.cn/pricing/details/app-service/)。  

> [!NOTE]
> 在从**免费**层切换应用服务计划之前，必须首先删除对 Azure 订阅施加的[支出限制](https://www.azure.cn/pricing/spending-limits/)。 若要查看或更改 Azure 应用服务订阅的选项，请参阅 [Azure Subscriptions][azuresubscriptions]（Azure 订阅）。
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## 增加定价层
<a id="scale-up-your-pricing-tier" class="xliff"></a>
1. 在浏览器中，打开 [Azure 门户][portal]。
2. 在应用的边栏选项卡中，单击“所有设置”，然后单击“向上缩放”。

    ![导航到向上缩放 Azure 应用。][ChooseWHP]
3. 选择层，然后单击“选择”。

    在操作完成后，“通知”选项卡上将闪现绿色的**成功**字样。

<a name="ScalingSQLServer"></a>

## 与缩放相关的资源
<a id="scale-related-resources" class="xliff"></a>
如果应用依赖于其他服务，如 Azure SQL 数据库或 Azure 存储，则还可以按需增加这些资源。 不应将这些资源与应用服务计划一起缩放，而必须单独进行缩放。

1. 在“软件包”中，单击“资源组”链接。

    ![与向上缩放 Azure 应用相关的资源](./media/web-sites-scale/RGEssentialsLink.png)
2. 在“资源组”边栏选项卡的“摘要”部分中，单击要缩放的某个资源。 以下屏幕截图显示了 SQL 数据库资源和 Azure 存储资源。

    ![导航到资源组边栏选项卡以向上缩放 Azure 应用](./media/web-sites-scale/ResourceGroup.png)
3. 对于 SQL 数据库资源，请单击“设置” > “定价层”以缩放定价层。

    ![向上缩放 Azure 应用的 SQL 数据库后端](./media/web-sites-scale/ScaleDatabase.png)

    还可以为 SQL 数据库实例启用[异地复制](../sql-database/sql-database-geo-replication-overview.md)。

    对于 Azure 存储资源，请单击“设置” > “配置”以向上缩放存储选项。

    ![增加 Azure 应用使用的 Azure 存储帐户](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## 了解开发人员功能
<a id="learn-about-developer-features" class="xliff"></a>
我们根据定价层提供以下面向开发人员的功能：

### 位数
<a id="bitness" class="xliff"></a>
* **基本**、**标准**和**高级**层支持 64 位和 32 位应用程序。
* **免费**和**共享**计划层仅支持 32 位应用程序。

### 调试器支持
<a id="debugger-support" class="xliff"></a>
* 对于**免费**、**共享**和**基本**模式，按每个应用服务计划一个连接的形式提供调试器支持。
* 对于**标准**和**高级**模式，按每个应用服务计划五个并发连接的形式提供调试器支持。

<a name="OtherFeatures"></a>

## 了解其他功能
<a id="learn-about-other-features" class="xliff"></a>
* 有关应用服务计划中的所有其余功能的详细信息，包括定价和所有用户（包括开发人员）感兴趣的功能，请参阅[应用服务定价详细信息](https://www.azure.cn/pricing/details/app-service/)。

<a name="Next Steps"></a>

## 后续步骤
<a id="next-steps" class="xliff"></a>
* 若要开始使用 Azure，请参阅 [Azure 试用版](https://www.azure.cn/pricing/1rmb-trial/)。
* 有关定价、支持和 SLA 的信息，请访问以下链接。

    [数据传输定价详细信息](https://www.azure.cn/pricing/details/data-transfer/)

    [Azure 支持计划](https://www.azure.cn/support/plans/)

    [服务级别协议](https://www.azure.cn/support/legal/sla/)

    [SQL 数据库定价详细信息](https://www.azure.cn/pricing/details/sql-database/)

    [Azure 的虚拟机和云服务大小][vmsizes]

    [应用服务定价详细信息](https://www.azure.cn/pricing/details/app-service/)

    [应用服务定价详细信息 - SSL 连接](https://www.azure.cn/pricing/details/app-service/)

<!-- LINKS -->
[vmsizes]:https://www.azure.cn/pricing/details/app-service/
[SQLaccountsbilling]:https://www.azure.cn/pricing/details/sql-database/
[azuresubscriptions]:http://account.windowsazure.cn/subscriptions
[portal]: https://portal.azure.cn/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png