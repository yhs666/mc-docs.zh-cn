---
title: 关于使用 Azure Site Recovery 进行 Azure 到 Azure 灾难恢复的网络 | Azure
description: 概述了使用 Azure Site Recovery 复制 Azure 虚拟机的网络。
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: article
origin.date: 02/08/2018
ms.date: 03/05/2018
ms.author: v-yeche
ms.openlocfilehash: e87da25395d8da3689e09b964171231c908ecb36
ms.sourcegitcommit: 966200f9807bfbe4986fa67dd34662d5361be221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2018
---
# <a name="about-networking-in-azure-to-azure-replication"></a>关于 Azure 到 Azure 复制的网络

> [!NOTE]
> Azure 虚拟机的 Site Recovery 复制当前处于预览阶段。

本文提供了使用 [Azure Site Recovery](site-recovery-overview.md) 在不同区域之间复制和恢复 Azure VM 的网络指南。 

## <a name="before-you-start"></a>开始之前

了解 Site Recovery 如何为[此方案](azure-to-azure-architecture.md)提供灾难恢复。

## <a name="typical-network-infrastructure"></a>典型网络基础结构

下图描绘了 Azure VM 上运行的应用程序的典型 Azure 环境：

![客户环境](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

如果使用 Azure ExpressRoute 或从本地网络到 Azure 的 VPN 连接，则环境如下所示：

![客户环境](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

通常，网络使用防火墙和网络安全组 (NSG) 进行保护。 防火墙使用基于 URL 或 IP 的允许列表来控制网络连接。 NSG 提供使用 IP 地址范围控制网络连接的规则。

> [!IMPORTANT]
> Site Recovery 不支持使用经过身份验证的代理控制网络连接，并且无法启用复制。

## <a name="outbound-connectivity-for-urls"></a>URL 的出站连接

如果使用基于 URL 的防火墙代理来控制出站连接，请允许以下 Site Recovery URL：

**URL** | **详细信息**  
--- | ---
*.blob.core.chinacloudapi.cn | 必需，以便从 VM 将数据写入到源区域中的缓存存储帐户。
login.chinacloudapi.cn | 对于 Site Recovery 服务 URL 的授权和身份验证而言是必需的。
*.hypervrecoverymanager.windowsazure.cn | 必需，以便从 VM 进行 Site Recovery 服务通信。
*.servicebus.chinacloudapi.cn | 必需，以便从 VM 写入 Site Recovery 监视和诊断数据。

<a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>
## <a name="outbound-connectivity-for-ip-address-ranges"></a>IP 地址范围的出站连接

如果使用基于 IP 的防火墙代理或 NSG 规则来控制出站连接，需要允许这些 IP 地址范围。

- 对应于源位置的所有 IP 地址范围。
    - 可以下载 [IP 地址范围](https://www.microsoft.com/download/details.aspx?id=42064)。
    - 需要允许这些地址，才能从 VM 将数据写入到缓存存储帐户。
- 对应于 Office 365 [身份验证和标识 IP V4 终结点](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)的所有 IP 地址范围。
    - 如果将来向 Office 365 范围添加新地址，则需要创建新的 NSG 规则。
- Site Recovery 服务终结点 IP 地址。 这些地址可在 [XML 文件](https://aka.ms/site-recovery-public-ips)中找到，并取决于目标位置。
-  可以[下载并使用此脚本](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702)，以在 NSG 上自动创建所需的规则。 
- 在生产 NSG 中创建所需的 NSG 规则之前，建议先在测试 NSG 中创建这些规则，并确保没有任何问题。
- 若要创建所需的 NSG 规则数，请确保订阅已列入允许列表。 联系 Azure 支持人员，以提高订阅中的 NSG 规则限制。
<!-- Not Available
Waiting for the PM reply
## Example NSG configuration

This example shows how to configure NSG rules for a VM to replicate. 

- If you're using NSG rules to control outbound connectivity, use "Allow HTTPS outbound" rules for all the required IP address ranges.
- The example presumes that the VM source location is "China East" and the target location is "China North.


* Create rules that correspond to [China East IP ranges](https://www.microsoft.com/download/details.aspx?id=42064). This is required so that data can be written to the cache storage account from the VM.

* Create rules for all IP ranges that correspond to Office 365 [authentication and identity IP V4 endpoints](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Create rules that correspond to the target location:

   **Location** | **Site Recovery service IPs** |  **Site Recovery monitoring IP**
    --- | --- | ---
   China North | 40.69.144.231 | 52.165.34.144

### NSG rules on the China North network security group

These rules are required so that replication can be enabled from the target region to the source region post-failover:

* Rules that correspond to [China North IP ranges](https://www.microsoft.com/download/details.aspx?id=42064). These are required so that data can be written to the cache storage account from the VM.

* Rules for all IP ranges that correspond to Office 365 [authentication and identity IP V4 endpoints](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Rules that correspond to the source location:

   **Location** | **Site Recovery service IPs** |  **Site Recovery monitoring IP**
    --- | --- | ---
   China East | 13.82.88.226 | 104.45.147.24

-->

## <a name="expressroutevpn"></a>ExpressRoute/VPN 

如果在本地和 Azure 位置之间建立了 ExpressRoute 或 VPN 连接，请遵从本部分中的准则。

### <a name="forced-tunneling"></a>强制隧道

通常，会定义默认路由 (0.0.0.0/0) 以强制出站 Internet 流量流经本地位置。 但并不建议这样做。 复制流量和 Site Recovery 服务通信不应离开 Azure 边界。 若要解决此问题，可以为[这些 IP 范围](#outbound-connectivity-for-azure-site-recovery-ip-ranges)添加用户定义路由 (UDR)，使复制流量不流向本地。

### <a name="connectivity"></a>连接 

目标位置与本地位置之间的连接遵循以下准则：
- 如果应用程序需要连接到本地计算机，或者有通过 VPN/ExpressRoute 从本地连接到应用程序的客户端，请确保在目标 Azure 区域和本地数据中心之间至少有一个[站点到站点连接](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)。

- 如果预计有大量流量在目标 Azure 区域与本地数据中心之间流动，则应在目标 Azure 区域与本地数据中心之间再创建一个 [ExpressRoute 连接](../expressroute/expressroute-introduction.md)。

- 如果想在故障转移后保留虚拟机的 IP，则将目标区域的站点到站点/ExpressRoute 连接保留为断开状态。 这样做可确保源区域的 IP 范围和目标区域的 IP 范围之间没有范围冲突。

### <a name="expressroute-configuration"></a>ExpressRoute 配置
请遵从以下 ExpressRoute 配置最佳做法：

- 需要在源区域和目标区域中分别创建 ExpressRoute 线路。 然后需要在以下对象之间创建连接：
  - 源虚拟网络和 ExpressRoute 线路。
  - 目标虚拟网络和 ExpressRoute 线路。

- ExpressRoute 标准规定，可以在同一地缘政治区域创建线路。 若要在不同的地缘政治区域创建 ExpressRoute 线路，则需使用 Azure ExpressRoute 高级版，这会增加成本。 （如果已在使用 ExpressRoute 高级版，则不必支付额外费用。）有关更多详细信息，请参阅 [ExpressRoute 位置文档](../expressroute/expressroute-locations.md)和 [ExpressRoute 定价](https://www.azure.cn/pricing/details/expressroute/)。
<!-- Archor is not Exist on #azure-regions-to-expressroute-locations-within-a-geopolitical-region -->

- 建议在源区域和目标区域中使用不同的 IP 范围。 ExpressRoute 线路无法同时连接两个使用相同 IP 范围的 Azure 虚拟网络。

- 可以在这两个区域创建使用相同 IP 范围的虚拟网络，然后在这两个区域分别创建 ExpressRoute 线路。 发生故障转移时，断开源虚拟网络中的线路，连接目标虚拟网络中的线路。

 >[!IMPORTANT]
 > 如果主要区域完全关闭，断开连接操作可能会失败， 导致目标虚拟网络无法获取 ExpressRoute 连接。

## <a name="next-steps"></a>后续步骤
[复制 Azure 虚拟机](site-recovery-azure-to-azure.md)，开始对工作负荷进行保护。

<!--Update_Description: wording update, update link -->
