---
title: 将 Azure 门户 URL 加入安全列表 | Azure
description: 将这些 URL 添加到代理服务器旁路，以便与 Azure 门户及其服务通信
services: azure-portal
keywords: ''
author: kfollis
ms.author: v-tawe
origin.date: 09/13/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 72935de82273fbab2965ed0be6aee417a24bc339
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335798"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>在防火墙或代理服务器上将 Azure 门户 URL 加入安全列表

若要在局域网或广域网和 Azure 云之间实现良好的性能和连接性，请配置本地安全设备，绕过针对 Azure 门户 URL 的安全限制。 网络管理员经常会部署代理服务器、防火墙或其他设备，以确保用户访问 Internet 时的安全性并控制其访问方式。 但是，旨在保护用户的规则有时候可能会阻止合法的与业务相关的 Internet 流量（包括你与 Azure 之间的通信）或降低其速度。 为了优化你的网络与 Azure 门户及其服务之间的连接，建议将 Azure 门户 URL 添加到安全列表。

## <a name="azure-portal-urls-for-proxy-bypass"></a>用于跳过代理的 Azure 门户 URL

Azure 门户的安全列表的 URL 终结点特定于部署组织时所在的 Azure 云。 选择云，然后将 URL 列表添加到代理服务器或防火墙，使发往这些终结点的网络流量可以绕过限制。

#### <a name="public-cloudtabpublic-cloud"></a>[公有云](#tab/public-cloud)
```
*.aadcdn.microsoftonline-p.com
*.aka.ms
*.applicationinsights.io
*.azure.com
*.azure.net
*.azureafd.net
*.azure-api.net
*.azuredatalakestore.net
*.azureedge.net
*.loganalytics.io
*.microsoft.com
*.microsoftonline.com
*.microsoftonline-p.com
*.msauth.net
*.msftauth.net
*.trafficmanager.net
*.visualstudio.com
*.windows.net
*.windows-int.net
```

#### <a name="us-government-cloudtabus-government-cloud"></a>[美国政府云](#tab/us-government-cloud)
```
*.azure.us
*.loganalytics.us
*.microsoft.us
*.microsoftonline.us
*.msauth.net
*.usgovcloudapi.net
*.usgovtrafficmanager.net
*.windowsazure.us
```

#### <a name="china-government-cloudtabchina-government-cloud"></a>[中国政府云](#tab/china-government-cloud)
```
*.azure.cn
*.microsoft.cn
*.microsoftonline.cn
*.chinacloudapi.cn
*.trafficmanager.cn
*.chinacloudsites.cn
*.windowsazure.cn
```
---

> [!NOTE]
> 发往这些终结点的流量使用标准的 TCP 端口：80 (HTTP) 和 443 (HTTPS)。
>
>
## <a name="next-steps"></a>后续步骤

需要将 IP 地址加入安全列表？ 下载适合你的云的 Microsoft Azure 数据中心 IP 范围的列表：

* [全球](https://www.microsoft.com/download/details.aspx?id=56519)
* [美国政府](https://www.microsoft.com/download/details.aspx?id=57063)
* [德国](https://www.microsoft.com/download/details.aspx?id=57064)
* [中国](https://www.microsoft.com/download/details.aspx?id=57062)

其他 Microsoft 服务使用其他 URL 和 IP 地址进行连接。 若要优化 Microsoft 365 服务的网络连接，请参阅[为 Office 365 设置网络](https://docs.microsoft.com/office365/enterprise/set-up-network-for-office-365)。
