---
title: 用于虚拟机规模集的邻近放置组预览版 | Microsoft Docs
description: 了解如何在 Azure 中创建和使用用于 Windows 虚拟机规模集的邻近放置组。
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
origin.date: 07/01/2019
ms.date: 08/06/2019
ms.author: v-junlch
ms.openlocfilehash: 90245dcfd5c8faf0ec6a142bfcb7061970969a57
ms.sourcegitcommit: 17cd5461e7d99f40b9b1fc5f1d579f82b2e27be9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819540"
---
# <a name="preview-creating-and-using-proximity-placement-groups-using-powershell"></a>预览版：通过 PowerShell 创建和使用邻近放置组

若要让 VM 尽可能靠近，将延迟尽可能降至最低，应将规模集部署到一个[邻近放置组](co-location.md#preview-proximity-placement-groups)中。

邻近放置组是一种逻辑分组，用于确保 Azure 计算资源在物理上彼此靠近。 邻近放置组用于要求低延迟的工作负荷。

> [!IMPORTANT]
> 邻近放置组目前为公开预览版。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。 有关详细信息，请参阅[适用于 Azure 预览版的补充使用条款](https://www.azure.cn/support/legal/)。
>

## <a name="create-a-proximity-placement-group"></a>创建邻近放置组
使用 [New-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/new-azproximityplacementgroup) cmdlet 创建邻近放置组。 

```azurepowershell
$resourceGroup = "myPPGResourceGroup"
$location = "chinanorth"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>列出邻近放置组

可以使用 [Get-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/get-azproximityplacementgroup) cmdlet 列出所有邻近放置组。

```azurepowershell
Get-AzProximityPlacementGroup
```


## <a name="create-a-scale-set"></a>创建规模集

通过 [New-AzVMSS](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) 创建规模集时，请在邻近放置组中创建规模集，使用 `-ProximityPlacementGroup $ppg.Id` 引用邻近放置组 ID。

```azurepowershell
$scalesetName = "myVM"

New-AzVmss `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -VMScaleSetName $scalesetName `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ProximityPlacementGroup $ppg.Id
```

可以使用 [Get-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/get-azproximityplacementgroup) 查看放置组中的实例。

```azurepowershell
  Get-AzProximityPlacementGroup `
   -ResourceId $ppg.Id | Format-Table `
   -Wrap `
   -Property VirtualMachineScaleSets
```

## <a name="next-steps"></a>后续步骤

也可使用 [Azure CLI](../virtual-machines/linux/proximity-placement-groups.md) 创建邻近放置组。

