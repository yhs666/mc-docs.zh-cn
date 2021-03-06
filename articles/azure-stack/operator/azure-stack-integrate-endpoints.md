---
title: 在数据中心发布 Azure Stack 服务 | Microsoft Docs
description: 了解如何在数据中心发布 Azure Stack 服务。
services: azure-stack
author: WenJason
manager: digimobile
ms.service: azure-stack
ms.topic: article
origin.date: 09/09/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: wamota
ms.lastreviewed: 09/09/2019
ms.openlocfilehash: f5352a7301667f47d454594fb01ade98210eb532
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020232"
---
# <a name="publish-azure-stack-services-in-your-datacenter"></a>在数据中心发布 Azure Stack 服务 

Azure Stack 为其基础结构角色设置虚拟 IP 地址 (VIP)。 这些 VIP 是从公共 IP 地址池分配的。 每个 VIP 受软件定义的网络层中的访问控制列表 (ACL) 保护。 还可以在物理交换机（TOR 和 BMC）之间使用 ACL 来进一步强化解决方案。 将会根据部署时的指定，针对外部 DNS 区域中的每个终结点创建一个 DNS 条目。 例如，将为用户门户分配 DNS 主机条目 portal. *&lt;region>.&lt;fqdn>* 。

以下体系结构图显示了不同的网络层和 ACL：

![显示不同网络层和 ACL 的图表](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

### <a name="ports-and-urls"></a>端口和 URL
要使 Azure Stack 服务（例如门户、Azure 资源管理器、DNS 等）可供外部网络使用，必须允许特定 URL、端口和协议的入站流量发往这些终结点。
 
在到传统代理服务器或防火墙的透明代理上行链路正在保护解决方案的部署中，必须允许特定的端口和 URL，以便进行[入站](azure-stack-integrate-endpoints.md#ports-and-protocols-inbound)和[出站](azure-stack-integrate-endpoints.md#ports-and-urls-outbound)通信。 这包括用于标识、市场、修补和更新、注册和使用情况数据的端口与 URL。

## <a name="ports-and-protocols-inbound"></a>端口和协议（入站）

将 Azure Stack 终结点发布到外部网络需要一组基础结构 VIP。  “终结点 (VIP)”表显示了每个终结点、所需的端口和协议。 请参阅特定资源提供程序部署文档，了解需要其他资源提供程序（例如 SQL 资源提供程序）的终结点。

此处未列出内部基础结构 VIP，因为发布 Azure Stack 时不需要这些 VIP。 用户 VIP 是动态的，由用户自己定义，而不受 Azure Stack 操作员的控制。

> [!Note]  
> IKEv2 VPN 是一个基于标准的 IPsec VPN 解决方案，它使用 UDP 端口 500 和 4500 以及 TCP 端口 50。 防火墙并非始终打开这些端口，因此，IKEv2 VPN 可能无法穿过代理和防火墙。

添加[扩展主机](azure-stack-extension-host-prepare.md)后，不需要 12495-30015 范围内的端口。

|终结点 (VIP)|DNS 主机 A 记录|协议|端口|
|---------|---------|---------|---------|
|AD FS|Adfs. *&lt;region>.&lt;fqdn>*|HTTPS|443|
|门户（管理员）|Adminportal. *&lt;region>.&lt;fqdn>*|HTTPS|443|
|Adminhosting | *.adminhosting.\<region>.\<fqdn> | HTTPS | 443 |
|Azure 资源管理器（管理员）|Adminmanagement. *&lt;region>.&lt;fqdn>*|HTTPS|443|
|门户（用户）|Portal. *&lt;region>.&lt;fqdn>*|HTTPS|443|
|Azure 资源管理器（用户）|Management. *&lt;region>.&lt;fqdn>*|HTTPS|443|
|Graph|Graph. *&lt;region>.&lt;fqdn>*|HTTPS|443|
|证书吊销列表|Crl. *&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;. *&lt;region>.&lt;fqdn>*|TCP 和 UDP|53|
|Hosting | *.hosting.\<region>.\<fqdn> | HTTPS | 443 |
|Key Vault（用户）|&#42;.vault. *&lt;region>.&lt;fqdn>*|HTTPS|443|
|Key Vault（管理员）|&#42;.adminvault. *&lt;region>.&lt;fqdn>*|HTTPS|443|
|存储队列|&#42;.queue. *&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|存储表|&#42;.table. *&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|存储 Blob|&#42;.blob. *&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|SQL 资源提供程序|sqladapter.dbadapter. *&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|MySQL 资源提供程序|mysqladapter.dbadapter. *&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|应用服务|&#42;.appservice. *&lt;region>.&lt;fqdn>*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;.scm.appservice. *&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)|
|  |api.appservice. *&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)<br>44300（Azure 资源管理器）|
|  |ftp.appservice. *&lt;region>.&lt;fqdn>*|TCP、UDP|21、1021、10001-10100 (FTP)<br>990 (FTPS)|
|VPN 网关|     |     |[请参阅 VPN 网关常见问题解答](/vpn-gateway/vpn-gateway-vpn-faq#can-i-traverse-proxies-and-firewalls-using-point-to-site-capability)。|
|     |     |     |     |

## <a name="ports-and-urls-outbound"></a>端口和 URL（出站）

Azure Stack 仅支持透明代理服务器。 在使用到传统代理服务器的透明代理上行链路的部署中，必须允许下表中的端口和 URL，以便进行出站通信。

> [!Note]  
> Azure Stack 不支持使用 ExpressRoute 访问下表中列出的 Azure 服务，因为 ExpressRoute 可能无法将流量路由到所有终结点。

|目的|目标 URL|协议|端口|源网络|
|---------|---------|---------|---------|---------|
|标识|**Azure 中国世纪互联**<br>https:\//login.chinacloudapi.cn/<br>https:\//graph.chinacloudapi.cn/<br>|HTTP<br>HTTPS|80<br>443|公共 VIP - /27<br>公共基础结构网络|
|市场联合|**Azure 中国世纪互联**<br>https:\//management.chinacloudapi.cn/<br>http://&#42;.blob.core.chinacloudapi.cn/|HTTPS|443|公共 VIP - /27|
|修补程序和更新|https://&#42;.azureedge.net<br>https:\//aka.ms/azurestackautomaticupdate|HTTPS|443|公共 VIP - /27|
|注册|**Azure 中国世纪互联**<br>https:\//management.chinacloudapi.cn/|HTTPS|443|公共 VIP - /27|
|使用情况|**Azure 中国世纪互联**<br>https://&#42;.trafficmanager.cn|HTTPS|443|公共 VIP - /27|
|Windows Defender|&#42;.wdcp.microsoft.com<br>&#42;.wdcpalt.microsoft.com<br>&#42;.wd.microsoft.com<br>&#42;.update.microsoft.com<br>&#42;.download.microsoft.com<br>https:\//www.microsoft.com/pkiops/crl<br>https:\//www.microsoft.com/pkiops/certs<br>https:\//crl.microsoft.com/pki/crl/products<br>https:\//www.microsoft.com/pki/certs<br>https:\//secure.aadcdn.microsoftonline-p.com<br>|HTTPS|80<br>443|公共 VIP - /27<br>公共基础结构网络|
|NTP|（为部署提供的 NTP 服务器的 IP）|UDP|123|公共 VIP - /27|
|DNS|（为部署提供的 DNS 服务器的 IP）|TCP<br>UDP|53|公共 VIP - /27|
|CRL|（证书上的 CRL 分发点下的 URL）|HTTP|80|公共 VIP - /27|
|LDAP|为 Graph 集成提供的 Active Directory 林|TCP<br>UDP|389|公共 VIP - /27|
|LDAP SSL|为 Graph 集成提供的 Active Directory 林|TCP|636|公共 VIP - /27|
|LDAP GC|为 Graph 集成提供的 Active Directory 林|TCP|3268|公共 VIP - /27|
|LDAP GC SSL|为 Graph 集成提供的 Active Directory 林|TCP|3269|公共 VIP - /27|
|AD FS|为 AD FS 集成提供的 AD FS 元数据终结点|TCP|443|公共 VIP - /27|
|诊断日志收集服务|Azure 存储提供的 Blob SAS URL|HTTPS|443|公共 VIP - /27|
|     |     |     |     |     |

使用 Azure 流量管理器对出站 URL 进行负载均衡，以根据地理位置提供尽可能最佳的连接。 使用负载均衡 URL，Azure 可以更新和更改后端终结点，而不会影响客户。 Azure 不共享负载均衡 URL 的 IP 地址列表。 使用支持按 URL 而不是按 IP 筛选的设备。

任何时候都需要出站 DNS，不同的是查询外部 DNS 的源以及选择了哪种类型的标识集成。 在联网场景的部署过程中，位于 BMC 网络上的 DVM 需要出站访问权限。 但在部署后，DNS 服务会移到通过公共 VIP 发送查询的内部组件。 此时，可以删除通过 BMC 网络的出站 DNS 访问权限，但是必须保留对该 DNS 服务器的公共 VIP 访问权限，否则身份验证将失败。

## <a name="next-steps"></a>后续步骤

[Azure Stack PKI 要求](azure-stack-pki-certs.md)
