---
title: Azure Stack 数据中心集成 - 发布终结点 | Microsoft Docs
description: 了解如何在数据中心发布 Azure Stack 终结点
services: azure-stack
author: WenJason
manager: digimobile
ms.service: azure-stack
ms.topic: article
origin.date: 09/13/2018
ms.date: 12/17/2018
ms.author: v-jay
ms.reviewer: wamota
keywords: ''
ms.openlocfilehash: 125b4323c5b572f8cc4cb5b8fd23ebf5d3138666
ms.sourcegitcommit: 98142af6eb83f036d72e26ebcea00e2fceb673af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2018
ms.locfileid: "53396150"
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure Stack 数据中心集成 - 发布终结点

Azure Stack 为其基础结构角色设置虚拟 IP 地址 (VIP)。 这些 VIP 是从公共 IP 地址池分配的。 每个 VIP 受软件定义的网络层中的访问控制列表 (ACL) 保护。 还可以在物理交换机（TOR 和 BMC）之间使用 ACL 来进一步强化解决方案。 将会根据部署时的指定，针对外部 DNS 区域中的每个终结点创建一个 DNS 条目。


以下体系结构图显示了不同的网络层和 ACL：

![结构化图片](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>端口和协议（入站）

将 Azure Stack 终结点发布到外部网络需要一组基础结构 VIP。 “终结点 (VIP)”表显示了每个终结点、所需的端口和协议。 请参阅特定资源提供程序部署文档，了解需要其他资源提供程序（例如 SQL 资源提供程序）的终结点。

此处未列出内部基础结构 VIP，因为发布 Azure Stack 时不需要这些 VIP。

> [!Note]  
> 用户 VIP 是动态的，由用户自己定义，而不受 Azure Stack 操作员的控制。

|终结点 (VIP)|DNS 主机 A 记录|协议|端口|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|门户（管理员）|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13012<br>13020<br>13021<br>13026<br>30015|
|Adminhosting | *.adminhosting.\<region>.\<fqdn> | HTTPS | 443 |
|Azure 资源管理器（管理员）|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|门户（用户）|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13012<br>13020<br>13021<br>30015<br>13003|
|Azure 资源管理器（用户）|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Graph|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|证书吊销列表|Crl.*&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP 和 UDP|53|
|Hosting | *.hosting.\<region>.\<fqdn> | HTTPS | 443 |
|Key Vault（用户）|&#42;.vault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Key Vault（管理员）|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|存储队列|&#42;.queue.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|存储表|&#42;.table.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|存储 Blob|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|SQL 资源提供程序|sqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|MySQL 资源提供程序|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|应用服务|&#42;.appservice.*&lt;region>.&lt;fqdn>*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)|
|  |api.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)<br>44300（Azure 资源管理器）|
|  |ftp.appservice.*&lt;region>.&lt;fqdn>*|TCP、UDP|21、1021、10001-10100 (FTP)<br>990 (FTPS)|
|VPN 网关|     |     |[请参阅 VPN 网关常见问题解答](/vpn-gateway/vpn-gateway-vpn-faq#can-i-traverse-proxies-and-firewalls-using-point-to-site-capability)。|
|     |     |     |     |

## <a name="ports-and-urls-outbound"></a>端口和 URL（出站）

Azure Stack 仅支持透明代理服务器。 如果部署中的透明代理上行链接到传统的代理服务器，则必须允许以下端口和 URL，以便能够进行出站通信：

> [!Note]  
> Azure Stack 不支持使用 Express Route 访问下表中列出的 Azure 服务。

|目的|URL|协议|端口|
|---------|---------|---------|---------|
|标识|login.chinacloudapi.cn<br>login.partner.microsoftonline.cn<br>graph.chinacloudapi.cn<br>https://secure.aadcdn.microsoftonline-p.com<br>office.com|HTTP<br>HTTPS|80<br>443|
|市场联合|https://management.chinacloudapi.cn<br>https://&#42;.blob.core.chinacloudapi.cn<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|修补程序和更新|https://&#42;.azureedge.net|HTTPS|443|
|注册|https://management.chinacloudapi.cn|HTTPS|443|
|使用情况|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.cn|HTTPS|443|
|Windows Defender|.wdcp.microsoft.com<br>.wdcpalt.microsoft.com<br>*.updates.microsoft.com<br>*.download.microsoft.com<br>https://msdl.microsoft.com/download/symbols<br>http://www.microsoft.com/pkiops/crl<br>http://www.microsoft.com/pkiops/certs<br>http://crl.microsoft.com/pki/crl/products<br>http://www.microsoft.com/pki/certs<br>https://secure.aadcdn.microsoftonline-p.com<br>|HTTPS|80<br>443|
|NTP|（为部署提供的 NTP 服务器的 IP）|UDP|123|
|DNS|（为部署提供的 DNS 服务器的 IP）|TCP<br>UDP|53|
|CRL|（证书上的 CRL 分发点下的 URL）|HTTP|80|
|     |     |     |     |

> [!Note]  
> 使用 Azure 流量管理器对出站 URL 进行负载均衡，以根据地理位置提供尽可能最佳的连接。 使用负载均衡 URL，Azure 可以更新和更改后端终结点，而不会影响客户。 Azure 不共享负载均衡 URL 的 IP 地址列表。 应使用支持按 URL 而不是按 IP 筛选的设备。

## <a name="next-steps"></a>后续步骤

[Azure Stack PKI 要求](azure-stack-pki-certs.md)
<!-- Update_Description: wording update -->