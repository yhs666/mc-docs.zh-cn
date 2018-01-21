---
title: "添加或删除 Azure 虚拟机的网络接口 | Azure"
description: "了解如何向虚拟机中添加网络接口或从中删除网络接口。"
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 12/15/2017
ms.date: 01/22/2018
ms.author: v-yeche
ms.openlocfilehash: 55ef0fd612d37958ec30b5578a3b46a2ce9f25fa
ms.sourcegitcommit: 020735d0e683791859d8e90381e9f8743a1af216
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>在虚拟机中添加或删除网络接口。

了解如何在创建 VM 时添加现有的网络接口，或者在处于“已停止”（“已解除分配”）状态的现有 VM 中添加或删除网络接口。 Azure 虚拟机 (VM) 通过网络接口与 Internet、Azure 及本地资源进行通信。 一台 VM 可以有一个或多个网络接口。 

如果需要为网络接口添加、更改或删除 IP 地址，请阅读[管理网络接口 IP 地址](virtual-network-network-interface-addresses.md)一文。 如果需要创建、更改或删除网络接口，请阅读[管理网络接口](virtual-network-network-interface.md)一文。

## <a name="before"></a>准备工作

在完成本文任何部分中的任何步骤之前，请完成以下任务：

- 使用 Azure 帐户登录到 Azure [门户](https://portal.azure.cn)、Azure 命令行接口 (CLI) 或 Azure PowerShell。 如果还没有 Azure 帐户，请注册[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。
- 如果使用 PowerShell 命令来完成本文中的任务，请[安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs?toc=%2fvirtual-network%2ftoc.json)。 确保已安装最新版本的 Azure PowerShell cmdlet。 若要获取 PowerShell 命令的帮助和示例，请键入 `get-help <command> -full`。
- 如果使用 Azure 命令行接口 (CLI) 命令来完成本文中的任务，请[安装和配置 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest)。 确保已安装最新版本的 Azure CLI。 若要获取 CLI 命令的帮助，请键入 `az <command> --help`。
<!--Not Available on Cloud Shell Introduction -->

## <a name="vm-create"></a>将现有网络接口添加到新 VM

通过门户创建 VM 时，门户会使用默认设置创建一个网络接口，并将其附加到 VM。 无法使用 Azure 门户将现有网络接口添加到新的 VM，或创建具有多个网络接口的 VM。 可以使用 CLI 或 PowerShell 执行这两项操作。 但是，在 PowerShell 或 CLI 中使用现有网络接口创建 VM 之前，请先熟悉[约束](#constraints)。 如果使用多个网络接口创建虚拟机，则还必须配置操作系统，以便在创建 VM 后能正常使用它。 有关详细信息，请参阅“为多个网络接口配置 [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) 或 [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) 操作系统”。

**命令**在创建 VM 之前，请使用[创建网络接口](virtual-network-network-interface.md#create-a-network-interface)中的步骤创建网络接口。

|工具|命令|
|---|---|
|CLI|[az vm create](https://docs.azure.cn/zh-cn/cli/vm?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#create)|
|PowerShell|[New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm?toc=%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>将网络接口添加到现有 VM

1. 登录到 Azure 门户。
2. 在门户顶部的搜索框中，搜索要将网络接口添加到的 VM 的名称，或依次单击“所有服务”、“虚拟机”来浏览该 VM。 找到该 VM 后，请单击它。 要将网络接口添加到的 VM 必须支持需添加的网络接口数。 若要了解每种 VM 大小支持的网络接口数量，请阅读有关 [Linux](../virtual-machines/linux/sizes.md?toc=%2fvirtual-network%2ftoc.json) 或 [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fvirtual-network%2ftoc.json) VM 大小的文章。  
3. 在“设置”下面单击“概述”。 单击“停止”，等待 VM 的“状态”更改为“已停止(已解除分配)”。 
4. 在“设置”下面单击“网络”。
5. 单击“附加网络接口”。 在当前尚未附加到其他 VM 的现有网络接口列表中，单击要附加的网络接口。 选择的网络接口不能已启用加速网络、不能分配有 IPv6 地址，并且必须与当前已附加到 VM 的网络接口位于同一个虚拟网络中。 如果没有现有的网络接口，必须先创建一个。 若要创建网络接口，请单击“创建网络接口”。 若要详细了解如何创建网络接口，请参阅[创建网络接口](virtual-network-network-interface.md#create-a-network-interface)。 若要详细了解将网络接口添加到虚拟机时的约束，请参阅[约束](#constraints)。
6. 单击 **“确定”**。
7. 在“设置”下面单击“概述”。 单击“启动”以启动虚拟机。
8. 配置 VM 操作系统以正常使用多个网络接口。 有关详细信息，请参阅“为多个网络接口配置 [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) 或 [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) 操作系统”。

|工具|命令|
|---|---|
|CLI|[az vm nic add](https://docs.azure.cn/zh-cn/cli/vm/nic?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#add)（引用）或[详细步骤](../virtual-machines/linux/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fvirtual-network%2ftoc.json)（引用）或[详细步骤](../virtual-machines/windows/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>查看 VM 的网络接口

可以查看当前附加到 VM 的网络接口，了解每个网络接口的配置，以及分配给每个网络接口的 IP 地址。 

1. 使用分配有订阅“所有者”、“参与者”或“网络参与者”角色的帐户登录到 [Azure 门户](https://portal.azure.cn)。 若要详细了解如何向帐户分配角色，请参阅[针对 Azure 基于角色的访问控制的内置角色](../active-directory/role-based-access-built-in-roles.md?toc=%2fvirtual-network%2ftoc.json#network-contributor)。
2. 在 Azure 门户顶部包含“搜索资源”文本的框中，键入“虚拟机”。 在搜索结果中出现“虚拟机”  时，单击该虚拟机。
3. 单击要查看其网络接口的 VM 的名称。
4. 在所选 VM 的“设置”部分，单击“网络”。 若要了解网络接口的设置及其更改方法，请参阅[管理网络接口](virtual-network-network-interface.md)。 若要了解如何添加、更改或删除分配给网络接口的 IP 地址，请阅读[管理 IP 地址](virtual-network-network-interface-addresses.md)。

命令

|工具|命令|
|---|---|
|CLI|[az vm show](https://docs.azure.cn/zh-cn/cli/vm?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#show)|
|PowerShell|[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm?toc=%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>从 VM 中删除网络接口

1. 登录到 Azure 门户。
2. 在门户顶部的搜索框中，搜索要从中删除（分离）网络接口的 VM 的名称，或依次单击“所有服务”、“虚拟机”来浏览该 VM。 找到该 VM 后，请单击它。
3. 在“设置”下面单击“概述”。 单击“停止”，等待 VM 的“状态”更改为“已停止(已解除分配)”。 
4. 在“设置”下面单击“网络”。
5. 单击“分离网络接口”。 在当前已附加到虚拟机的网络接口列表中，单击要分离的网络接口。 如果只列出了一个网络接口，则无法将它分离，因为虚拟机上必须始终至少附加一个网络接口。
6. 单击 **“确定”**。

命令

|工具|命令|
|---|---|
|CLI|[az vm nic remove](https://docs.azure.cn/zh-cn/cli/vm/nic?toc=%2fvirtual-network%2ftoc.json?view=azure-cli-latest#remove)（引用）或[详细步骤](../virtual-machines/linux/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fvirtual-network%2ftoc.json)（引用）或[详细步骤](../virtual-machines/windows/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>后续步骤
若要创建具有多个网络接口或 IP 地址的 VM，请阅读以下文章：

命令

|任务|工具|
|---|---|
|创建具有多个 NIC 的 VM|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json)、[PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fvirtual-network%2ftoc.json)|
|创建具有多个 IPv4 地址的单 NIC VM|[CLI](virtual-network-multiple-ip-addresses-cli.md)、[PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
<!-- Not Available on IPv6 -->
## <a name="constraints"></a>约束

- VM 上必须至少附加一个网络接口。
- VM 上附加的网络接口数量不能超过 VM 大小支持的数量。 若要详细了解每种 VM 大小支持的网络接口数量，请参阅有关 [Linux](../virtual-machines/linux/sizes.md?toc=%2fvirtual-network%2ftoc.json) 和 [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fvirtual-network%2ftoc.json) VM 大小的文章。 所有大小至少支持两个网络接口。
- 目前，添加到一个 VM 的网络接口不能附加到另一个 VM。 若要详细了解如何创建网络接口，请参阅[创建网络接口](virtual-network-network-interface.md#create-a-network-interface)。
- 过去，只能向支持多个网络接口且至少使用两个网络接口创建的 VM 中添加网络接口。 不能向使用一个网络接口创建的 VM 添加网络接口，即使 VM 大小支持多个网络接口也不可以。 相反，只能从至少有三个网络接口的 VM 中删除网络接口，因为使用至少两个网络接口创建的 VM 必须始终有至少两个网络接口。 这些约束均不再适用。 现在，可以创建包含任意数量（但不能超过 VM 大小支持的数量）的网络接口的 VM。
- 默认情况下，VM 上附加的第一个网络接口定义为主网络接口。 VM 中的所有其他网络接口为辅助网络接口。
- 尽管可以控制要将出站流量发送到的网络接口，但默认情况下，来自 VM 的所有出站流量都是通过分配给主网络接口的主 IP 配置的 IP 地址发出的。
- 过去，同一个可用性集中的所有 VM 都需要有一个或多个网络接口。 现在，同一个可用性集中可以存在具有任意数目的网络接口的 VM，只要 VM 大小支持该数目。 不过，只能在创建 VM 时将 VM 添加到可用性集。 若要详细了解可用性集，请阅读[在 Azure 中管理 VM 的可用性](../virtual-machines/windows/manage-availability.md?toc=%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy)一文。
- 尽管同一 VM 中的网络接口可以连接到 VNet 中的不同子网，但这些网络接口必须全部连接到同一个 VNet。
- 可将任何主要或辅助网络接口的任何 IP 配置的任何 IP 地址添加到 Azure 负载均衡器后端池。 过去，只能将主要网络接口的主要 IP 地址添加到后端池。 若要详细了解 IP 地址和配置，请阅读[添加、更改或删除 IP 地址](virtual-network-network-interface-addresses.md)一文。
- 删除 VM 不会删除附加到其中的网络接口。 删除 VM 时，网络接口会从此 VM 中分离。 可将网络接口添加到不同的 VM，也可将其删除。
- 如果网络接口分配有专用 IPv6 地址，则在创建 VM 时必须将其添加（附加）到 VM。 创建 VM 后，无法将分配有 IPv6 地址的网络接口添加到 VM。 如果在创建虚拟机时添加分配有专用 IPv6 地址的网络接口，则无论该 VM 大小支持多少个网络接口，仅可将该网络接口添加到虚拟机。 若要深入了解如何将 IP 地址分配给网络接口，请参阅[网络接口 IP 地址](virtual-network-network-interface-addresses.md)。

<!--Update_Description: update meta properties, wording udpate, add constraints content  -->