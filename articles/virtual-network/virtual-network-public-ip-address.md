---
title: 创建、更改或删除 Azure 公共 IP 地址 | Azure
description: 了解如何创建、更改或删除公共 IP 地址。
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 09/25/2017
ms.date: 03/12/2018
ms.author: v-yeche
ms.openlocfilehash: 5bbc9fe60e77cffb1eda35061ef1339f086e0889
ms.sourcegitcommit: 9b4669fe42e0dd7e3b463423ae4f58143af2b111
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2018
---
# <a name="create-change-or-delete-a-public-ip-address"></a>创建、更改或删除公共 IP 地址

了解公共 IP 地址，以及如何创建、更改和删除此类地址。 公共 IP 地址是具有自身可配置设置的资源。 通过将公共 IP 地址分配给其他 Azure 资源，可以：
- 建立与 Azure 虚拟机、Azure 虚拟机规模集、Azure VPN 网关、应用程序网关和面向 Internet 的 Azure 负载均衡器等资源的入站 Internet 连接。 若未分配公共 IP 地址，Azure 资源无法接收来自 Internet 的入站通信。 虽然本质上可通过公共 IP 地址访问某些 Azure 资源，但其他资源必须分配有公共 IP 地址才可从 Internet 访问它们。
- 使用可预测的 IP 地址与 Internet 建立出站连接。 例如，如果某虚拟机未分配有公共 IP 地址，但其地址由 Azure 网络地址转换为可预测的公共地址，则该虚拟机可与 Internet 建立出站通信。 通过将公共 IP 地址分配给资源，可了解哪个 IP 地址用于出站连接。 尽管可预测，但地址可根据所选分配方法进行更改。 有关详细信息，请参阅[创建公共 IP 地址](#create-a-public-ip-address)。 有关从 Azure 资源建立出站连接的详细信息，请阅读 [Understand outbound connections](../load-balancer/load-balancer-outbound-connections.md?toc=%2fvirtual-network%2ftoc.json)（了解出站连接）一文。

<a name="before"></a>
## <a name="before-you-begin"></a>准备阶段

在完成本文任何部分中的任何步骤之前，请完成以下任务：

- 查看 [Azure 限制](../azure-subscription-service-limits.md?toc=%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)一文，了解对公共 IP 地址的限制。
- 使用 Azure 帐户登录到 Azure [门户](https://portal.azure.cn)、Azure 命令行接口 (CLI) 或 Azure PowerShell。 如果还没有 Azure 帐户，请注册[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。
- 如果使用 PowerShell 命令来完成本文中的任务，请[安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs?toc=%2fvirtual-network%2ftoc.json)。 确保已安装最新版本的 Azure PowerShell cmdlet。 若要获取 PowerShell 命令的帮助和示例，请键入 `get-help <command> -full`。
- 如果使用 Azure 命令行接口 (CLI) 命令来完成本文中的任务，请[安装和配置 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest)。 确保已安装最新版本的 Azure CLI。 若要获取 CLI 命令的帮助，请键入 `az <command> --help`。
<!-- Not Available on Azure Cloud Shell -->

公共 IP 地址会产生少许费用。 若要查看定价，请参阅 [IP 地址定价](https://www.azure.cn/pricing/details/reserved-ip-addresses/)页。 

## <a name="create-a-public-ip-address"></a>创建公共 IP 地址

1. 使用已分配订阅的“网络参与者”角色权限（最低权限）的帐户登录到 [Azure 门户](https://portal.azure.cn)。 请参阅[用于 Azure 基于角色的访问控制的内置角色](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)一文，详细了解如何将角色和权限分配给帐户。
2. 在 Azure 门户顶部包含“搜索资源”文本的框中，键入“公共 IP 地址”。 在搜索结果中出现 **公共 IP 地址** 时，单击该地址。
3. 在出现的“公共 IP 地址”边栏选项卡中单击“+ 添加”。
4. 在出现的“创建公共 IP 地址”边栏选项卡中，为以下设置输入或选择值，然后单击“创建”：

    |设置|必需？|详细信息|
    |---|---|---|
    |SKU|是|引入 SKU 之前创建的所有公共 IP 地址均为基本 SKU 公共 IP 地址。  创建公共 IP 地址后，无法更改此 SKU。 独立虚拟机、可用性集内的虚拟机或虚拟机规模集可使用基本 SKU 或标准 SKU。  不允许在可用性集或规模集内的虚拟机之间混用 SKU。 基本 SKU：如果要在支持可用性区域的区域内创建公共 IP 地址，“可用性区域”设置默认设为“无”。 可选择一个可用性区域，保证公共 IP 地址具有一个特定区域。 标准 SKU：标准 SKU 公共 IP 可关联到虚拟机或负载均衡器前端。 如果要在支持可用性区域的区域内创建公共 IP 地址，“可用性区域”设置默认设为“区域冗余”。 有关可用性区域的详细信息，请参阅“可用性区域”设置。 将地址关联到标准负载均衡器时需使用标准 SKU。  标准 SKU 目前为预览版本。 创建标准 SKU 公共 IP 地址之前，必须先完成“注册标准 SKU 预览版”中的步骤，然后在支持的位置（区域）中创建该公共 IP 地址。 有关支持位置的列表，请密切关注 [Azure 虚拟网络更新](https://www.azure.cn/what-is-new/)页面，了解其他的区域支持信息。 将标准 SKU 公共 IP 地址分配到虚拟机的网络接口时，必须使用[网络安全组](security-overview.md#network-security-groups)显式允许预期流量。 创建并关联网络安全组且显式允许所需流量之后，才可与资源通信。|
    <!-- Not Avaialbe on Load Balancer Standard -->
    |名称|是|该名称必须在所选资源组中唯一。| |IP 版本|是| 选择 IPv4。  虽然可以将公共 IPv4 地址分配给多个 Azure 资源，
    |IP 地址分配|是|**动态：**仅在将公共 IP 地址与附加到虚拟机的网络接口相关联并首次启动该虚拟机后，才分配动态地址。 如果网络接口所附加到的虚拟机停止（解除分配），动态地址可能发生更改。 如果虚拟机重启或停止（但未解除分配），该地址将保持不变。 **静态：** 创建公共 IP 地址时，将分配静态地址。 即使虚拟机处于停止（解除分配）状态，静态地址也不改变。 仅当删除网络接口时才释放该地址。 创建网络接口后，可更改分配方法。 如果选择“标准”作为 SKU，则分配方法为“静态”。| |空闲超时（分钟）|否|在不依赖于客户端发送 keep-alive 消息的情况下，将 TCP 或 HTTP 连接保持打开的分钟数。| |DNS 名称标签|否|必须在创建该名称的 Azure 位置（所有订阅和所有客户位置）中保持唯一。 Azure 会在其 DNS 中自动注册该名称和 IP 地址，使你能够连接到使用该名称的资源。 Azure 会将类似于 *location.cloudapp.chinacloudapi.cn*（其中 location 是所选的位置）的默认子网追加到提供的名称后面，以创建完全限定的 DNS 名称。 Azure 的默认 DNS 包含 IPv4 A 名称记录，并在查找 DNS 名称时响应该记录。 客户端选择要与哪个 (IPv4) 地址通信。 除了使用带有默认后缀的 DNS 名称标签以外，还可以改用 Azure DNS 服务来配置带有自定义后缀（可解析为公共 IP 地址）的 DNS 名称。| |订阅|是|必须与要将公共 IP 地址关联到的资源位于同一[订阅](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#subscription)中。| |资源组|是|可与要将公共 IP 地址关联到的资源位于相同或不同的[资源组](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#resource-group)中。| |位置|是|必须与要将公共 IP 地址关联到的资源位于同一[位置](https://azure.microsoft.com/regions)（也称为“区域”）。|
<!-- Line 49 Not Available on [Azure load balancer standard SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fvirtual-network%2ftoc.json) -->
<!-- Not Available on Available zone -->

命令

<!-- Not Available on two version for IPv4 and IPv6 -->
|工具|命令|
|---|---|
|CLI|[az network public-ip create](https://docs.azure.cn/zh-cn/cli/network/public-ip?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#az_network_public_ip_create)|
|PowerShell|[New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress)|

<a name="change"></a>
## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>查看、删除公共 IP 地址或更改其设置

1. 使用已分配订阅的“网络参与者”角色权限（最低权限）的帐户登录到 [Azure 门户](https://portal.azure.cn)。 请参阅[用于 Azure 基于角色的访问控制的内置角色](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)一文，详细了解如何将角色和权限分配给帐户。
2. 在 Azure 门户顶部包含“搜索资源”文本的框中，键入“公共 IP 地址”。 在搜索结果中出现 **公共 IP 地址** 时，单击该地址。
3. 在出现的“公共 IP 地址”边栏选项卡中，单击要查看、更改其设置或要删除的公共 IP 地址的名称。
4. 在出现的公共 IP 地址边栏选项卡中，根据是要查看、删除还是更改公共 IP 地址，完成以下选项之一。
    - **查看**：边栏选项卡的“概述”部分显示公共 IP 地址的主要设置，例如与之关联的网络接口（如果地址与某个网络接口关联）。
    - **删除**：若要删除公共 IP 地址，请在边栏选项卡的“概述”部分中单击“删除”。 如果该地址当前与 IP 配置关联，则无法删除。 如果地址当前与配置相关联，请单击“取消关联”  ，取消该地址与 IP 配置之间的关联。
    - **更改**：单击“配置”。 使用本文 [创建公共 IP 地址](#create-a-public-ip-address) 部分的步骤 4 中的信息更改设置。 要将 IPv4 地址的分配方法从静态更改为动态，必须先从该公共 IPv4 地址关联的 IP 配置中取消关联该地址。 然后，可将分配方法更改为动态，并单击“关联”  将 IP 地址关联到相同的 IP 配置或其他配置，还可将其保留为取消关联状态。 若要取消关联公共 IP 地址，请在“概述”部分中单击“取消关联”。

>[!WARNING]
>将分配方法从静态更改为动态时，将丢失分配给公共 IP 地址的 IP 地址。 尽管 Azure 公共 DNS 服务器会保留静态或动态地址与任何 DNS 名称标签（若已定义）之间的映射，但如果虚拟机在处于停止（解除分配）状态之后启动，动态 IP 地址可能更改。 为防止地址变化，请分配静态 IP 地址。

命令

|工具|命令|
|---|---|
|CLI|[az network public-ip-list](https://docs.azure.cn/zh-cn/cli/network/public-ip?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#az_network_public_ip_list) 用于列出公共 IP 地址；[az network public-ip-show](https://docs.azure.cn/zh-cn/cli/network/public-ip?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#az_network_public_ip_show) 用于显示设置；[az network public-ip update](https://docs.azure.cn/zh-cn/cli/network/public-ip?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#az_network_public_ip_update) 用于更新；[az network public-ip delete](https://docs.azure.cn/zh-cn/cli/network/public-ip?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#az_network_public_ip_delete) 用于删除|
|PowerShell|[Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fvirtual-network%2ftoc.json) 用于检索公共 IP 地址对象并查看其设置；[Set-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fvirtual-network%2ftoc.json) 用于更新设置；[Remove-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermpublicipaddress) 用于删除|

<!-- Not Available on 20171206 ## Register for the standard SKU preview -->
## <a name="next-steps"></a>后续步骤
创建以下 Azure 资源时，将分配公共 IP 地址：

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fvirtual-network%2ftoc.json) 或 [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fvirtual-network%2ftoc.json) 虚拟机
- [面向 Internet 的 Azure 负载均衡器](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fvirtual-network%2ftoc.json)
- [Azure 应用程序网关](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fvirtual-network%2ftoc.json)
- [使用 Azure VPN 网关建立站点到站点连接](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fvirtual-network%2ftoc.json)
- [Azure 虚拟机规模集](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fvirtual-network%2ftoc.json)

<!--Update_Description: wording update, update reference link, update link -->