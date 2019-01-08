---
title: Azure PowerShell 脚本示例 - 对等互连两个虚拟网络
description: Azure PowerShell 脚本示例 - 对等互连两个虚拟网络
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 05/16/2017
ms.date: 01/07/2019
ms.author: v-biyu
ms.openlocfilehash: ac1d244530ce4acf6bf3409cf6f148497c76c141
ms.sourcegitcommit: a46f12240aea05f253fb4445b5e88564a2a2a120
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/26/2018
ms.locfileid: "53785288"
---
# <a name="peer-two-virtual-networks"></a>对等互连两个虚拟网络

此脚本通过 Azure 网络在同一区域创建并连接两个虚拟网络。 运行脚本后，会在两个虚拟网络之间创建对等互连。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)中的说明安装 Azure PowerShell，并运行 `Connect-AzureRmAccount` 创建与 Azure 的连接。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

# <a name="variables-for-common-values"></a>常见值的变量
$rgName='MyResourceGroup' $location='chinaeast'

# <a name="create-a-resource-group"></a>创建资源组。
New-AzureRmResourceGroup -Name $rgName -Location $location

# <a name="create-virtual-network-1"></a>创建虚拟网络 1。
$vnet1 = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name 'Vnet1' -AddressPrefix '10.0.0.0/16' -Location $location

# <a name="create-virtual-network-2"></a>创建虚拟网络 2。
$vnet2 = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name 'Vnet2' -AddressPrefix '10.1.0.0/16' -Location $location

# <a name="peer-vnet1-to-vnet2"></a>将 VNet1 对等互连到 VNet2。
Add-AzureRmVirtualNetworkPeering -Name 'LinkVnet1ToVnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

# <a name="peer-vnet2-to-vnet1"></a>将 VNet2 对等互连到 VNet1。
Add-AzureRmVirtualNetworkPeering -Name 'LinkVnet2ToVnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 | 
| [New-AzureRmVirtualNetwork](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/new-azurermvirtualnetwork)| 创建 Azure 虚拟网络和子网。 |
| [Add-AzureRmVirtualNetworkPeering](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | 创建两个虚拟网络之间的对等互连。  |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/zh-cn/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可在 [Azure 网络概述文档](../powershell-samples.md?toc=%2fnetworking%2ftoc.json)中找到其他网络 PowerShell 脚本示例。
