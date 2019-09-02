---
title: 关于使用 Azure Site Recovery 进行 Azure 到 Azure 灾难恢复的网络 | Azure
description: 概述了使用 Azure Site Recovery 复制 Azure 虚拟机的网络。
services: site-recovery
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: article
origin.date: 03/29/2019
ms.date: 08/26/2019
ms.author: v-yeche
ms.openlocfilehash: 014974fe3ed7770d1c3cf78781b1160971dada3c
ms.sourcegitcommit: 18a0d2561c8b60819671ca8e4ea8147fe9d41feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70134433"
---
# <a name="about-networking-in-azure-to-azure-replication"></a>关于 Azure 到 Azure 复制的网络

本文提供了使用 [Azure Site Recovery](site-recovery-overview.md) 在不同区域之间复制和恢复 Azure VM 的网络指南。

## <a name="before-you-start"></a>开始之前

了解 Site Recovery 如何为[此方案](azure-to-azure-architecture.md)提供灾难恢复。

## <a name="typical-network-infrastructure"></a>典型网络基础结构

下图描绘了 Azure VM 上运行的应用程序的典型 Azure 环境：

![客户环境](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

如果使用 Azure ExpressRoute 或从本地网络到 Azure 的 VPN 连接，则环境如下：

![客户环境](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

通常，网络使用防火墙和网络安全组 (NSG) 进行保护。 防火墙使用基于 URL 或 IP 的允许列表来控制网络连接。 NSG 提供使用 IP 地址范围控制网络连接的规则。

> [!IMPORTANT]
> Site Recovery 不支持使用经过身份验证的代理控制网络连接，并且无法启用复制。

## <a name="outbound-connectivity-for-urls"></a>URL 的出站连接

如果使用基于 URL 的防火墙代理来控制出站连接，请允许以下 Site Recovery URL：

**URL** | **详细信息**  
--- | ---
*.blob.core.chinacloudapi.cn | 必需，以便从 VM 将数据写入到源区域中的缓存存储帐户。 如果你知道 VM 的所有缓存存储帐户，则可以将特定存储帐户 URL（例如：cache1.blob.core.chinacloudapi.cn 和 cache2.blob.core.chinacloudapi.cn）而不是 *.blob.core.chinacloudapi.cn 加入允许列表
login.chinacloudapi.cn | 对于 Site Recovery 服务 URL 的授权和身份验证而言是必需的。
*.hypervrecoverymanager.windowsazure.cn | 必需，以便从 VM 进行 Site Recovery 服务通信。 如果防火墙代理支持 IP，则可以使用相应的“Site Recovery IP”。
*.servicebus.chinacloudapi.cn | 必需，以便从 VM 写入 Site Recovery 监视和诊断数据。 如果防火墙代理支持 IP，则可以使用相应的“Site Recovery 监视 IP”。

<a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>
## <a name="outbound-connectivity-for-ip-address-ranges"></a>IP 地址范围的出站连接

如果使用基于 IP 的防火墙代理或 NSG 规则来控制出站连接，需要允许这些 IP 地址范围。

- 对应于源区域中存储帐户的所有 IP 地址范围
    - 为源区域创建基于[存储服务标记](../virtual-network/security-overview.md#service-tags)的 NSG 规则。
    - 允许这些地址，才能从 VM 将数据写入到缓存存储帐户。
- 创建一个基于 [Azure Active Directory (AAD) 服务标记](../virtual-network/security-overview.md#service-tags)的 NSG 规则以允许访问与 AAD 对应的所有 IP 地址
    - 如果将来要向 Azure Active Directory (AAD) 添加新地址，则需要创建新的 NSG 规则。
- Site Recovery 服务终结点 IP 地址（在[中国的 Site Recovery 服务终结点](#site-recovery-ip-in-china)中提供），具体取决于目标位置。

    <!--MOONCAKE: CORRECT ON URL-->
    
- 在生产 NSG 中创建所需的 NSG 规则之前，建议先在测试 NSG 中创建这些规则，并确保没有任何问题。

Site Recovery IP 地址范围如下：

<!--MOONCAKE: CUSTOMIZE, UPDATE CAREFULLY-->
    
<a name="site-recovery-ip-in-china"></a>
**目标** | **Site Recovery IP** |  **Site Recovery 监视 IP**
--- | --- | ---
中国东部 | 42.159.205.45 | 42.159.132.40
中国北部 | 40.125.202.254 | 42.159.4.151
中国东部 2 | 40.73.118.52 | 40.73.100.125          
中国北部 2 | 40.73.35.193 | 40.73.33.230

<!--MOONCAKE: CUSTOMIZE, UPDATE CAREFULLY-->

## <a name="example-nsg-configuration"></a>NSG 配置示例

此示例演示如何为要复制的 VM 配置 NSG 规则。

- 如果使用 NSG 规则控制出站连接，请对所有必需的 IP 地址范围使用端口 443 的“允许 HTTPS 出站”规则。
- 此示例假设 VM 源位置是“中国东部”，目标位置是“中国中部”。

### <a name="nsg-rules---china-east"></a>NSG 规则 - 中国东部

<!--MOONCAKE: CORRECT ON Storage WITHOUT .ChinaEast-->

1. 在 NSG 上为“Storage”创建出站 HTTPS (443) 安全规则，如以下屏幕截图所示。

    <!--MOONCAKE: CORRECT ON Storage WITHOUT .ChinaEast-->
    
    ![storage-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. 基于 NSG 规则为“AzureActiveDirectory”创建出站 HTTPS (443) 安全规则，如以下屏幕截图所示。

    ![aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. 为对应于目标位置的 Site Recovery IP 创建出站 HTTPS (443) 规则：
    
    <!--MOONCAKE: CORRECT ON China North | 40.125.202.254 | 42.159.4.151 -->
    
    **Location** | **Site Recovery IP 地址** |  **Site Recovery 监视 IP 地址**
    --- | --- | ---
    中国北部 | 40.125.202.254 | 42.159.4.151
    
    <!--MOONCAKE: CORRECT ON China North | 40.125.202.254 | 42.159.4.151-->

### <a name="nsg-rules---china-north"></a>NSG 规则 - 中国北部

必须创建这些规则，才能在故障转移后启用从目标区域到源区域的复制：

<!--MOONCAKE: CORRECT ON Storage WITHOUT .chinanorth--> 

1. 在 NSG 上为“Storage.chinanorth”创建出站 HTTPS (443) 安全规则。
    
    <!--MOONCAKE: CORRECT ON Storage WITHOUT .chinanorth--> 
    
2. 基于 NSG 规则为“AzureActiveDirectory”创建出站 HTTPS (443) 安全规则。

3. 为对应于源位置的 Site Recovery IP 创建出站 HTTPS (443) 规则：
    
    <!--MOONCAKE: CORRECT ON China East | 42.159.205.45 | 42.159.132.40 -->
    
    **Location** | **Site Recovery IP 地址** |  **Site Recovery 监视 IP 地址**
    --- | --- | ---
    中国东部 | 42.159.205.45 | 42.159.132.40
    
    <!--MOONCAKE: CORRECT ON China East | 42.159.205.45 | 42.159.132.40 -->

<!--MOONCAKE: CUSTOMIZE, UPDATE CAREFULLY-->

## <a name="network-virtual-appliance-configuration"></a>网络虚拟设备配置

如果使用网络虚拟设备 (NVA) 控制来自 VM 的出站网络流量，则设备可能在所有复制流量通过 NVA 的情况下受到限制。 我们建议在虚拟网络中为“存储”创建一个网络服务终结点，这样复制流量就不会经过 NVA。

### <a name="create-network-service-endpoint-for-storage"></a>为存储创建网络服务终结点
可在虚拟网络中为“存储”创建一个网络服务终结点，这样复制流量就不会离开 Azure 边界。

- 选择 Azure 虚拟网络并单击“服务终结点”。

    ![storage-endpoint](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- 单击“添加”，“添加服务终结点”选项卡随即打开
- 选择“服务”下的“Microsoft.Storage”和“子网”字段下的所需子网，并单击“添加”。

> [!NOTE]
> 不限制虚拟网络对用于 ASR 的存储帐户的访问权限。 应允许来自“所有网络”的访问

### <a name="forced-tunneling"></a>强制隧道

对 0.0.0.0/0 地址前缀，可将 Azure 默认系统路由重写为[自定义路由](../virtual-network/virtual-networks-udr-overview.md#custom-routes)，并将 VM 流量转换为本地网络虚拟设备 (NVA)，但不建议对 Site Recovery 复制使用此配置。 如果使用自定义路由，则应在虚拟网络中为“存储”[创建一个虚拟网络服务终结点](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage)，这样复制流量就不会离开 Azure 边界。

## <a name="next-steps"></a>后续步骤
- [复制 Azure 虚拟机](site-recovery-azure-to-azure.md)，开始对工作负荷进行保护。
- 详细了解为 Azure 虚拟机故障转移[保留 IP 地址](site-recovery-retain-ip-azure-vm-failover.md)。
- 详细了解[使用 ExpressRoute 的 Azure 虚拟机](azure-vm-disaster-recovery-with-expressroute.md)的灾难恢复。

<!--Update_Description: update meta properties, wording update  -->
