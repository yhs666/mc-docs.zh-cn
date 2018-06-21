---
title: Azure App Service 及其对现有 Azure 服务的影响
description: 介绍了新的 Azure App Service 及其功能对 Azure 中现有服务的影响。
services: app-service
documentationcenter: ''
author: yochay
manager: nirma
editor: ''
ms.assetid: 86c6a292-3c33-49f4-890c-89cc0321b397
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/12/2016
ms.date: 09/26/2016
ms.author: v-dazen
ms.openlocfilehash: bd9f93e3e82348171adf71f79a5a4315b24f0ee2
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
ms.locfileid: "20184247"
---
# <a name="azure-app-service-and-existing-azure-services"></a>Azure App Service 和现有的 Azure 服务
本文概括介绍了对现有 Azure 服务的更改，这是将多个 Azure 服务组合为 [Azure App Service](https://www.azure.cn/home/features/app-service/)（新的集成产品/服务）更改的一部分。

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>概述
[Azure App Service](https://www.azure.cn/home/features/app-service/) 是一种全新独特的云服务，开发人员能够使用它来创建适用于任何平台和任何设备的 Web 应用和移动应用。 应用服务是一个集成的解决方案，旨在简化重复编码函数、与企业和 SaaS 系统集成并自动执行业务流程，同时满足你对安全性、可靠性和可伸缩性的需要。

应用服务将[网站](https://www.azure.cn/home/features/app-service/web-apps/)和[移动服务](https://www.azure.cn/home/features/mobile-services/)这两个现有 Azure 服务融合为单个综合服务，同时添加强大的新功能。  使用应用服务可以托管以下应用类型：

* Web 应用
* Mobile Apps
* API Apps

下表介绍了现有的 Azure 服务如何映射到应用服务，以及应用服务中的可用应用类型。

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">现有的应用服务</th>
<th align="left", style="width:10%">Azure 应用服务</th>
<th align="left", style="width:80%">更改内容</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure 网站</td>
<td align="left">Web 应用</td>
<td align="left"><li>对于 Azure 网站，对应用服务进行了严格限制，必须将名称“网站”更改为“Web 应用”。
<p><li>现在，网站的所有现有实例均已改为应用服务中的 Web 应用。</p>
<p><li>可以通过 <a href="/app-service-web/app-service-web-app-azure-portal">Azure 门户</a>访问现有网站，该门户的 <em>应用服务</em>下列出了所有现有站点。</p>
<p><li>Web 托管计划现在改为了应用服务计划<em></em><em></em>。 应用服务计划可以托管任何应用类型的应用服务，例如 Web 应用、移动应用或 API 应用<em></em>。</p>
<p><li>Azure App Service Web 应用现已公开上市。</p>
<p><li><a href="https://www.azure.cn/home/features/app-service/web-apps/">了解有关 Web 应用的详细信息</a>。</p></td>
</tr>
<tr class="even">
<td align="left">Azure 移动服务</td>
<td align="left">Mobile Apps</td>
<td align="left"><p><li>移动服务继续用作独立服务，并仍然受完全支持。</p>
<p><li>移动应用是应用服务中的应用类型，它集成了移动服务和其他服务的所有功能。</p>
<p><li>可以轻松地<a href="/app-service-mobile/app-service-mobile-migrating-from-mobile-services">从移动服务迁移到移动应用</a>。</p>
<p><li>作为应用服务的一部分，移动应用的功能远胜于移动服务，例如，与本地和 SaaS 系统集成、暂存槽、Web 作业、更好的缩放选项等。</p>
<p><li><a href="https://www.azure.cn/home/features/app-service/mobile-apps/">了解有关移动应用的详细信息</a>。</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API Apps</td>
<td align="left">
<p><li>API 应用是应用服务中的全新应用类型，使用它可以轻松地构建和使用云中的 API。</p>
<p><li><a href="https://www.azure.cn/home/features/app-service/api-apps/">了解有关 API 应用的详细信息</a>。</p></td>
</tr>
</tbody>
</table>

若要了解详细信息，请访问[应用服务文档](/app-service/)。