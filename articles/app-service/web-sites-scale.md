---
title: 增加 Azure 中的应用
description: 了解如何增加 Azure 应用服务中的应用以增加容量和功能。
services: app-service
documentationcenter: ''
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
ms.date: 10/08/2018
ms.author: v-yiso
ms.openlocfilehash: ca8b354e1b7503e511f7bcc609201150180b8c4d
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52649066"
---
# <a name="scale-up-an-app-in-azure"></a>增加 Azure 中的应用
本文介绍如何在 Azure 应用服务中缩放应用。 缩放的工作流有两种：向上缩放和向外缩放；本文介绍向上缩放工作流。

* [向上缩放](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling)：获取更多 CPU、内存、磁盘空间和额外功能，例如专用虚拟机 (VMs)、自定义域和证书、暂存槽、自动缩放以及更多功能。 可以通过更改应用所属的应用服务计划的定价层来向上缩放。
* [向外缩放](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling)：增加用于运行应用的 VM 实例数。
  根据定价层，最多可以向外缩放到 20 个实例。 
  有关向外缩放的详细信息，请参阅[手动或自动缩放实例计数](../monitoring-and-diagnostics/insights-how-to-scale.md)。 可在该文中了解如何使用自动缩放，即根据预定义的规则和计划自动缩放实例计数。

缩放设置仅需几秒即可应用，并且会影响[应用服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)中的所有应用。
缩放设置不需要更改代码或重新部署应用程序。

有关各个应用服务计划的定价和功能的信息，请参阅[应用服务定价详细信息](https://www.azure.cn/pricing/details/app-service/)。  

> [!NOTE]
> 在从**免费**层切换应用服务计划之前，必须首先删除对 Azure 订阅施加的[支出限制](https://www.azure.cn/pricing/spending-limits/)。 若要查看或更改 Azure 应用服务订阅的选项，请参阅 [Azure Subscriptions][azuresubscriptions]（Azure 订阅）。
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>增加定价层
1. 在浏览器中，打开 [Azure 门户][portal]。
2. 在应用服务应用页面，单击“所有设置”，然后单击“纵向扩展”。
   
    ![导航到向上缩放 Azure 应用。][ChooseWHP]
3. 选择层，并单击“应用”。
   
    在操作完成后，“通知”选项卡上将闪现绿色的**成功**字样。

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>与缩放相关的资源
如果应用依赖于其他服务，如 Azure SQL 数据库或 Azure 存储，则可单独对这些资源进行纵向扩展。 这些资源不由应用服务计划管理。

1. 在“软件包”中，单击“资源组”链接。

    ![与向上缩放 Azure 应用相关的资源](./media/web-sites-scale/RGEssentialsLink.png)
2. 在“资源组”页的“摘要”部分，单击希望缩放的资源。 以下屏幕截图显示了 SQL 数据库资源和 Azure 存储资源。
   
    ![导航到资源组页面对 Azure 应用进行纵向扩展](./media/web-sites-scale/ResourceGroup.png)
3. 对于 SQL 数据库资源，请单击“设置” > “定价层”以缩放定价层。

    ![向上缩放 Azure 应用的 SQL 数据库后端](./media/web-sites-scale/ScaleDatabase.png)

    还可以为 SQL 数据库实例启用[异地复制](../sql-database/sql-database-geo-replication-overview.md)。

    对于 Azure 存储资源，请单击“设置” > “配置”以向上缩放存储选项。

    ![增加 Azure 应用使用的 Azure 存储帐户](./media/web-sites-scale/ScaleStorage.png)

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>比较定价层
有关详细信息（例如每个定价层的 VM 大小），请参阅[应用服务定价详细信息](https://www.azure.cn/pricing/details/web-sites/)。

有关服务限制、配额和约束的表以及每个层级所支持的功能，请参阅[应用服务限制](../azure-subscription-service-limits.md#app-service-limits)。

<a name="Next Steps"></a>

## <a name="next-steps"></a>后续步骤
* 有关定价、支持和 SLA 的信息，请访问以下链接。

    [数据传输定价详细信息](https://www.azure.cn/pricing/details/data-transfer/)

    [Azure 支持计划](https://www.azure.cn/support/plans/)

    [服务级别协议](https://www.azure.cn/support/legal/sla/)

    [SQL 数据库定价详细信息](https://www.azure.cn/pricing/details/sql-database/)

    [Azure 的虚拟机和云服务大小][vmsizes]

* 有关 Azure 应用服务最佳实践（包括构建可缩放、有弹性的体系结构）的信息，请参阅 [Best Practices: Azure 应用服务 Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx)（最佳实践：Azure 应用服务 Web 应用）。

<!-- LINKS -->
[vmsizes]:https://www.azure.cn/pricing/details/app-service/
[SQLaccountsbilling]:https://www.azure.cn/pricing/details/sql-database/
[azuresubscriptions]:http://account.windowsazure.cn/subscriptions
[portal]: https://portal.azure.cn/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png