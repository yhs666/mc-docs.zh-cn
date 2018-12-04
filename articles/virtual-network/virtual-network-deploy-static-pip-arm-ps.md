---
title: 创建具有静态公共 IP 地址的 VM - PowerShell | Azure
description: 了解如何使用 PowerShell 创建具有静态公共 IP 地址的 VM。
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 08/08/2018
ms.date: 09/10/2018
ms.author: v-yeche
ms.openlocfilehash: 741825b1dfe2cea1190edf955804b82273b245a4
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647628"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-powershell"></a>使用 PowerShell 创建具有静态公用 IP 地址的虚拟机

可以创建具有静态公用 IP 地址的虚拟机。 使用公共 IP 地址可以通过 Internet 来与虚拟机通信。 分配静态公共 IP 地址而非动态地址可以确保地址永远不会改变。 详细了解[静态公共 IP 地址](virtual-network-ip-addresses-overview-arm.md#allocation-method)。 若要将分配给现有虚拟机的公共 IP 地址从动态更改为静态，或者要使用专用 IP 地址，请参阅[添加、更改或删除 IP 地址](virtual-network-network-interface-addresses.md)。 公共 IP 地址会产生[少许费用](https://www.azure.cn/pricing/details/reserved-ip-addresses/)，可为每个订阅使用的公共 IP 地址数有[限制](../azure-subscription-service-limits.md?toc=%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)。

## <a name="create-a-virtual-machine"></a>创建虚拟机

可以从本地计算机完成以下步骤。 若要使用本地计算机，请确保[安装了 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fvirtual-network%2ftoc.json)。
<!-- Not Available on Cloud Shell-->

1. 打开命令会话并使用 `Connect-AzureRmAccount -Environment AzureChinaCloud` 登录到 Azure。
2. 使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 命令创建资源组。 以下示例在“中国东部”Azure 区域中创建一个资源组：

    ```PowerShell
    New-AzureRmResourceGroup -Name myResourceGroup -Location ChinaEast
    ```

3. 使用 [New-AzureRmVM](https://docs.microsoft.com/powershell/module/AzureRM.Compute/New-AzureRmVM) 命令创建虚拟机。 `-AllocationMethod "Static"` 选项向虚拟机分配静态公共 IP 地址。 以下示例使用名为 *myPublicIpAddress* 的静态公共 IP 地址创建 Windows Server 虚拟机。 出现提示时，提供要用作虚拟机的登录凭据的用户名和密码：<!-- Not Available on basic SKU -->
    ```PowerShell
    New-AzureRmVm `
     -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "China East" `
     -PublicIpAddressName "myPublicIpAddress" `
     -AllocationMethod "Static"
    ```

    <!-- Not Available on Standard SKU -->
4. 使用 [Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) 查看分配的公用 IP 地址并确认它创建为静态地址：

    ```PowerShell
    Get-AzureRmPublicIpAddress `
     -ResourceGroupName "myResourceGroup" `
     -Name "myPublicIpAddress" `
     | Select "IpAddress", "PublicIpAllocationMethod" `
     | Format-Table
    ```

   Azure 从你在其中创建虚拟机的区域使用的地址中分配了一个公共 IP 地址。 对于 Azure [公有](https://www.microsoft.com/download/details.aspx?id=56519)云、[美国政府](https://www.microsoft.com/download/details.aspx?id=57063)云、[中国](https://www.microsoft.com/download/details.aspx?id=57062)云和[德国](https://www.microsoft.com/download/details.aspx?id=57064)云，可以下载范围（前缀）的列表。

> [!WARNING]
> 不要修改虚拟机操作系统中的 IP 地址设置。 操作系统不知道 Azure 公共 IP 地址。 虽然可以向操作系统添加专用 IP 地址设置，但除非必要，否则我们建议不要这样做，而只能阅读[向操作系统添加专用 IP 地址](virtual-network-network-interface-addresses.md#private)之后才执行此操作。

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组及其包含的所有资源，请使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 将其删除：

```PowerShell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>后续步骤

- 详细了解 Azure 中的[公共 IP 地址](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- 详细了解所有[公共 IP 地址设置](virtual-network-public-ip-address.md#create-a-public-ip-address)
- 详细了解[专用 IP 地址](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses)以及如何为 Azure 虚拟机分配[静态公共 IP 地址](virtual-network-network-interface-addresses.md#add-ip-addresses)
- 详细了解如何创建 [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fvirtual-network%2ftoc.json) 和 [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fvirtual-network%2ftoc.json) 虚拟机

<!-- Update_Description: wording update, update link -->