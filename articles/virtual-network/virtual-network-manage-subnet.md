---
title: "添加、更改或删除 Azure 虚拟网络子网 | Azure"
description: "了解如何在 Azure 中添加、更改或删除虚拟网络子网。"
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 05/10/2017
ms.date: 09/04/2017
ms.author: v-yeche
ms.openlocfilehash: 3fe81b424dfb6cafa70c9447d53bb18d75c552bf
ms.sourcegitcommit: 095c229b538d9d2fc51e007abe5fde8e46296b4f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2017
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>添加、更改或删除虚拟网络子网

了解如何添加、更改或删除虚拟网络子网。 

如果不熟悉虚拟网络，建议在添加、更改或删除子网之前阅读 [Azure 虚拟网络概述](virtual-networks-overview.md)和[创建、更改或删除虚拟网络](virtual-network-manage-network.md)。 部署到虚拟网络的所有 Azure 资源都将部署到虚拟网络内的子网中。 在虚拟网络中创建多个子网通常是出于以下目的：
- **筛选子网之间的流量**。 可将网络安全组应用于子网，筛选虚拟网络中所有资源（如虚拟机）的入站和出站网络流量。 若要详细了解如何创建网络安全组，请参阅[创建网络安全组](virtual-networks-create-nsg-arm-pportal.md)。
- **控制子网之间的路由**。 Azure 创建默认路由，以便在子网之间自动路由流量。 可以通过创建用户定义的路由来替代默认的 Azure 路由。 若要详细了解用户定义的路由，请参阅[创建用户定义的路由](virtual-network-create-udr-arm-ps.md)。 

本文介绍如何为使用 Azure 资源管理器部署模型创建的虚拟网络添加、更改和删除子网。

## <a name="before"></a>准备工作

在开始执行本文所述的任务之前，需满足以下先决条件：

- 如果不熟悉虚拟网络的用法，建议你查看[创建你的第一个 Azure 虚拟网络](virtual-network-get-started-vnet-subnet.md)中的练习。 此练习可帮助你熟悉虚拟网络。
- 若要了解虚拟网络的限制，请查看 [Azure 限制](../azure-subscription-service-limits.md?toc=%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)。
- 使用 Azure 帐户登录到 Azure 门户、Azure 命令行工具 (Azure CLI) 或 Azure PowerShell。 如果没有 Azure 帐户，请注册[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。
- 如果你打算使用 PowerShell 命令来完成本文所述的任务，必须先[安装并配置 Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json)。 确保安装最新版本的 Azure PowerShell cmdlet。 若要获取示例中 PowerShell 命令的帮助，请输入 `get-help <command> -full`。
- 如果打算使用 Azure CLI 命令来完成本文所述的任务，必须：[安装并配置 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)。 确保安装最新版本的 Azure CLI。 若要获取 Azure CLI 命令的帮助，请输入 `az <command> --help`。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]
<!-- Not Available Azure Cloud Shell-->

## <a name="create-subnet"></a>添加子网

添加子网：

1. 使用已分配订阅的“网络参与者”角色权限（最低权限）的帐户登录到[门户](https://portal.azure.cn)。 若要详细了解如何将角色和权限分配给帐户，请参阅[用于 Azure 基于角色的访问控制的内置角色](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)。
2. 在门户的搜索框中，输入“虚拟网络”。 在搜索结果中，单击“虚拟网络”。
3. 在“虚拟网络”边栏选项卡中，单击要向其添加子网的虚拟网络。
4. 在“虚拟网络”边栏选项卡中，单击“子网”。
5. 单击“+ 子网”。
6. 在“添加子网”边栏选项卡中，输入以下参数的值：
    - **名称**：此名称在虚拟网络中必须唯一。
    - **地址范围**：此范围在虚拟网络的地址空间中必须唯一。 此范围不能与虚拟网络中的其他子网地址范围重叠。 必须使用无类域间路由 (CIDR) 表示法指定地址空间。 例如，在地址空间为 10.0.0.0/16 的虚拟网络中，可将子网地址空间定义为 10.0.0.0/24。 可以指定的最小范围为 /29，为子网提供八个 IP 地址。 Azure 保留每个子网中的第一个地址和最后一个地址，以确保协议一致性。 此外还会保留三个地址供 Azure 服务使用。 因此，使用 /29 地址范围定义子网时，子网中会有三个可用 IP 地址。 如果你打算将虚拟网络连接到 VPN 网关，则必须创建网关子网。 详细了解[网关子网地址范围具体考虑事项](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fvirtual-network%2ftoc.json#gwsub)。 在特定条件下，可在添加子网后更改地址范围。 若要了解如何更改子网地址范围，请参阅本文的[更改子网设置](#change-subnet)部分。
    - **网络安全组**：（可选）可将现有网络安全组关联到子网，控制子网的入站和出站网络流量筛选。 网络安全组必须与虚拟网络位于同一订阅和位置中。 此外，必须使用 Resource Manager 部署模型创建它。 若要详细了解如何创建网络安全组，请参阅[网络安全组](virtual-networks-create-nsg-arm-pportal.md)。
    - **路由表**：（可选）可将现有的路由表关联到子网，控制目标为其他网络的网络流量路由。 路由表必须与虚拟网络位于同一订阅和位置中。 此外，必须使用 Resource Manager 部署模型创建它。 若要详细了解如何创建路由表，请参阅[用户定义的路由](virtual-network-create-udr-arm-ps.md)。
    - **用户**：可以使用内置角色或自己的自定义角色控制对子网的访问。 若要详细了解如何分配访问子网的角色和用户，请参阅[使用角色分配管理对 Azure 资源的访问权限](../active-directory/role-based-access-control-configure.md?toc=%2fvirtual-network%2ftoc.json#add-access)。
7. 若要将子网添加到所选的虚拟网络，请单击“确定”。

命令

|工具|命令|
|---|---|
|Azure CLI|[az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?toc=%2fvirtual-network%2ftoc.json)New-AzureRmVirtualNetworkSubnetConfig、[](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?toc=%2fvirtual-network%2ftoc.json)Add-AzureRmVirtualNetworkSubnetConfig|

## <a name="change-subnet"></a>更改子网设置

可通过管理子网中的资源，更改网络安全组、路由表和对子网的用户访问权限。 若要了解这些设置，请参阅[添加子网](#create-subnet)中的步骤 6。 若要更改子网的地址空间，必须首先删除此子网中的任何资源。 删除资源所采取的步骤因资源而异。 若要了解如何删除子网中的资源，请阅读针对要删除的每种资源类型的相关文档。 若要更改子网的地址范围，请执行以下操作：

1. 使用已分配订阅的“网络参与者”角色权限（最低权限）的帐户登录到[门户](https://portal.azure.cn)。 若要详细了解如何将角色和权限分配给帐户，请参阅[用于 Azure 基于角色的访问控制的内置角色](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)。
2. 在门户的搜索框中，输入“虚拟网络”。 在搜索结果中，单击“虚拟网络”。
3. 在“虚拟网络”边栏选项卡中，单击要更改其子网地址范围的虚拟网络。
4. 单击要更改其地址范围的子网。
5. 在“子网”边栏选项卡的“地址范围”框中，输入新的地址范围。 此范围在虚拟网络的地址空间中必须唯一。 此范围不能与虚拟网络中的其他子网地址范围重叠。 必须使用 CIDR 表示法指定地址空间。 例如，在地址空间为 10.0.0.0/16 的虚拟网络中，可将子网地址空间定义为 10.0.0.0/24。 可以指定的最小范围为 /29，为子网提供八个 IP 地址。 Azure 保留每个子网中的第一个地址和最后一个地址，以确保协议一致性。 此外还会保留三个地址供 Azure 服务使用。 因此，地址范围为 /29 的子网有三个可用 IP 地址。 如果你打算将虚拟网络连接到 VPN 网关，则必须创建网关子网。 详细了解[网关子网地址范围具体考虑事项](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fvirtual-network%2ftoc.json#gwsub)。 在特定条件下，可以在子网创建后更改地址范围。 若要了解如何更改子网地址范围，请参阅本文的[更改子网设置](#change-subnet)部分。
6. 单击“保存” 。

**命令**

|工具|命令|
|---|---|
|Azure CLI|[az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?toc=%2fvirtual-network%2ftoc.json)|

## <a name="delete-subnet"></a>删除子网

仅当子网中无任何资源时，才可删除该子网。 如果子网中存在资源，则必须先删除子网中的资源，然后才能删除该子网。 删除资源所采取的步骤因资源而异。 若要了解如何删除子网中的资源，请阅读针对要删除的每种资源类型的相关文档。 若要删除子网，请执行以下操作：

1. 使用已分配订阅的“网络参与者”角色权限（最低权限）的帐户登录到[门户](https://portal.azure.cn)。 若要详细了解如何将角色和权限分配给帐户，请参阅[用于 Azure 基于角色的访问控制的内置角色](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)。
2. 在门户的搜索框中，输入“虚拟网络”。 在搜索结果中，单击“虚拟网络”。
3. 在“虚拟网络”边栏选项卡中，单击要从中删除子网的虚拟网络。
4. 在“虚拟网络”边栏选项卡中的“设置”下，单击“子网”。
5. 在“子网”边栏选项卡上显示的子网列表中，右键单击要删除的子网，然后依次单击“删除”、“是”删除该子网。

命令

|工具|命令|
|---|---|
|Azure CLI|[az network vnet delete](https://docs.microsoft.com/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>后续步骤

若要在子网中创建虚拟机，请参阅[在子网中创建虚拟网络并部署 VM](virtual-network-get-started-vnet-subnet.md#create-vms)。

<!--Update_Description: wording update, update reference link-->