---
title: 为 Azure 应用服务中使用流量管理器进行负载均衡的 Web 应用配置自定义域名。
description: 为 Azure 应用服务中包含流量管理器（用于负载均衡）的 Web 应用使用自定义域名。
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/17/2017
ms.date: 12/04/2017
ms.author: v-yiso
ms.openlocfilehash: 51b1d7a72ff02925c3ef3079470d3f7cde152bab
ms.sourcegitcommit: 077e96d025927d61b7eeaff2a0a9854633565108
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2017
ms.locfileid: "25591887"
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>使用流量管理器为 Azure 应用服务中的 Web 应用配置自定义域名
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

本文提供了将自定义域名与一个[应用服务](app-service-web-overview.md)应用配合使用的一般说明，该应用已与[流量管理器](../traffic-manager/traffic-manager-overview.md)集成以进行负载均衡。

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>了解 DNS 记录
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>将 Web 应用配置为标准模式
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>为自定义域添加 DNS 记录
要将自定义域与 Azure 应用服务中的 Web 应用关联，必须在 DNS 表中为自定义域添加新条目。 请通过使用来自域提供商的管理工具执行此操作。

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

虽然各个域提供商的具体情况不同，但你都需从自定义域名（例如 **contoso.com**）映射到与 Web 应用集成的流量管理器域名 (**contoso.trafficmanager.net**)。
   
   > [!NOTE]
   > 如果某条记录已被使用并且需要提前将应用绑定到该记录，可以创建其他 CNAME 记录。 例如，要提前将 **www.contoso.com** 绑定到 Web 应用，请创建从 **awverify.www** 到 **contoso.trafficmanager.cn** 的 CNAME 记录。 然后可以将“www.contoso.com”添加到 Web 应用，而无需更改“www”CNAME 记录。
   > 
   > 
在域提供商处添加或修改完 DNS 记录后，请保存这些更改。

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>启用流量管理器
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>后续步骤
有关详细信息，请参阅 [Node.js 开发人员中心](/develop/nodejs/)。



<!-- URL List -->