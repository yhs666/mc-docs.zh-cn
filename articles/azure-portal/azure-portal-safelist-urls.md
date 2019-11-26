---
title: 将 Azure 门户 URL 加入安全列表 | Azure
description: 将这些 URL 添加到代理服务器旁路，以便与 Azure 门户及其服务通信
services: azure-portal
keywords: ''
author: kfollis
ms.author: v-tawe
origin.date: 09/13/2019
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 0040dadc30995b309f4211c22b65ce4b2a746547
ms.sourcegitcommit: c863b31d8ead7e5023671cf9b58415542d9fec9c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020884"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>在防火墙或代理服务器上将 Azure 门户 URL 加入安全列表

若要在局域网或广域网和 Azure 云之间实现良好的性能和连接性，请配置本地安全设备，绕过针对 Azure 门户 URL 的安全限制。 网络管理员经常会部署代理服务器、防火墙或其他设备，以确保用户访问 Internet 时的安全性并控制其访问方式。 但是，旨在保护用户的规则有时候可能会阻止合法的与业务相关的 Internet 流量（包括你与 Azure 之间的通信）或降低其速度。 为了优化你的网络与 Azure 门户及其服务之间的连接，建议将 Azure 门户 URL 添加到安全列表。

## <a name="azure-portal-urls-for-proxy-bypass"></a>用于跳过代理的 Azure 门户 URL

Azure 门户的安全列表的 URL 终结点特定于部署组织时所在的 Azure 云。 选择云，然后将 URL 列表添加到代理服务器或防火墙，使发往这些终结点的网络流量可以绕过限制。

#### <a name="china-government-cloud"></a>中国政府云
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

* [中国](https://www.microsoft.com/download/details.aspx?id=57062)

其他 Microsoft 服务使用其他 URL 和 IP 地址进行连接。 若要优化 Microsoft 365 服务的网络连接，请参阅[为 Office 365 设置网络](https://docs.microsoft.com/office365/enterprise/set-up-network-for-office-365)。
