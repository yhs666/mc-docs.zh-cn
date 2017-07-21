---
title: "在 Azure 门户中进行导航的参考"
description: "了解用户在管理门户和 Azure 门户之间对应用服务 Web 的不同体验"
services: app-service
documentationcenter: 
author: jaime-espinosa
manager: erikre
editor: jimbe
ms.assetid: 0cc6a3cc-bd89-4a96-9177-d25f6fb737bb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/26/2016
ms.date: 09/26/2016
ms.author: v-dazen
ms.openlocfilehash: 115b352ddffa1717b336b1ef2dccb52d020c6191
ms.sourcegitcommit: f2f4389152bed7e17371546ddbe1e52c21c0686a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="reference-for-navigating-the-azure-portal"></a>在 Azure 门户中进行导航的参考
现在，Azure 网站称为[应用服务 Web 应用](/app-service-web/app-service-changes-existing-services)。 我们正在更新所有文档以反映此名称更改，并为 Azure 门户提供说明。 完成该过程之前，可以使用此文档作为在 Azure 门户中使用 Web 应用的指南。

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="the-future-of-the-azure-classic-management-portal"></a>将来的 Azure 经典管理门户
如果发现 Azure 经典管理门户中品牌打造改变，表示该门户正在替换为 Azure 门户。 由于正在逐步淘汰经典管理门户，因此新的开发工作重点将转移到 Azure 门户。 所有即将发布的 Web 应用新功能将出现在 Azure 门户中。 开始使用 Azure 门户，以使用 Web 应用提供的最新最好的功能。

## <a name="layout-differences-between-the-azure-classic-management-portal-and-azure-portal"></a>Azure 经典管理门户和 Azure 门户之间的布局差异
在经典管理门户中，所有 Azure 服务都列在左侧。 经典管理门户中的导航遵循树状结构，即从服务开始导航，然后再导航到各个元素。 此结构适用于管理独立组件。 但是，在 Azure 上构建的应用程序是互联服务的集合，而此树状结构并不适用于服务集合。 

Azure 门户则可以利用多个服务中的组件端对端地轻松构建应用程序。 门户以“旅程”的形式进行组织。 “旅程”是一系列“边栏选项卡”，而边栏选项卡是不同组件的容器。 例如，设置 Web 应用的自动缩放就是一段“旅程”，该“旅程”包含若干边栏选项卡，如下例所示：“网站”边栏选项卡（该边栏选项卡标题尚未更新至新术语）、“设置”边栏选项卡和“扩大”边栏选项卡。 在此示例中，自动缩放被设置为根据 CPU 使用情况而定，因此还具有一个“CPU 百分比”边栏选项卡。 边栏选项卡中的组件被称为“部件”，其外形像磁贴。 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>导航示例：创建 Web 应用
新建 Web 应用仍然像 1-2-3 一样简单。 下图并排显示了经典管理门户和新门户，以便说明创建和运行 Web 应用所需的步骤数并未发生太大变化。 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

创建 Web 应用时，可以在新门户中指定 URL、应用服务计划和位置，其操作与经典管理门户中的操作一样。 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

此外，还可以在新门户中定义其他通用设置。 例如，通过[资源组](../azure-resource-manager/resource-group-overview.md)可以轻松查看和管理相关 Azure 资源。 

## <a name="navigation-example-settings-and-features"></a>导航示例：设置和功能
现在所有设置和功能都有序地分组在单个边栏选项卡中，方便从中进行导航。

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

例如，可以通过单击“设置”边栏选项卡中的“自定义域和 SSL”来创建自定义域。

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

若要设置监视警报，请单击“请求和错误”，然后单击“添加警报”。

![](./media/app-service-web-app-azure-portal/Monitoring.png)

若要启用诊断，请单击“设置”边栏选项卡中的“诊断日志”。

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

若要配置应用程序设置，请单击“设置”边栏选项卡中的“应用程序设置”。 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

除品牌名称之外，已对门户中的一些内容进行了重命名或进行了不同的分组，以便更轻松地查找。 例如，下面是经典管理门户中应用设置（“配置”）相应页的屏幕截图。

![](./media/app-service-web-app-azure-portal/AppSettings.png)

[Azure Portal]: https://portal.azure.cn

## <a name="whats-changed"></a>发生的更改
* 有关从网站更改为应用服务的指南，请参阅 [Azure 应用服务及其对现有 Azure 服务的影响](/app-service-web/app-service-changes-existing-services)