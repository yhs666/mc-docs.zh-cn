---
title: 教程 - 使用 Azure PowerShell 创建自定义 VM 映像 | Azure
description: 本教程介绍如何使用 Azure PowerShell 在 Azure 中创建自定义虚拟机映像
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 03/27/2017
ms.date: 06/04/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 13c9fcd5d31e480680c852ace691cfe4e249ceff
ms.sourcegitcommit: 00c8a6a07e6b98a2b6f2f0e8ca4090853bb34b14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38940143"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>教程：使用 Azure PowerShell 创建 Azure VM 的自定义映像

自定义映像类似于市场映像，不同的是自定义映像的创建者是自己。 自定义映像可用于启动配置，例如预加载应用程序、应用程序配置和其他 OS 配置。 在本教程中，你将创建自己的 Azure 虚拟机自定义映像。 你将学习如何：

> [!div class="checklist"]
> * 使用 Sysprep 通用化 VM
> * 创建自定义映像
> * 从自定义映像创建 VM
> * 列出订阅中的所有映像
> * 删除映像

## <a name="before-you-begin"></a>开始之前

下列步骤详细说明了如何将现有 VM 转换为可重用自定义映像，用于创建新的 VM 实例。

若要完成本教程中的示例，必须具备现有虚拟机。 如果需要，此[脚本示例](../scripts/virtual-machines-windows-powershell-sample-create-vm.md)可为你创建一个虚拟机。 按照教程进行操作时，请根据需要替换资源组和 VM 名称。

<!--[!INCLUDE [cloud-shell-try-it.md|cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]--> 如果选择在本地安装并使用 PowerShell，则本教程需要 AzureRM 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。

## <a name="prepare-vm"></a>准备 VM

若要创建虚拟机的映像，需通过以下方式准备 VM：通用化 VM、解除分配，然后在 Azure 中将源 VM 标记为通用化。

### <a name="generalize-the-windows-vm-using-sysprep"></a>使用 Sysprep 通用化 Windows VM

Sysprep 将删除所有个人帐户信息及其他某些数据，并准备好要用作映像的计算机。 有关 Sysprep 的详细信息，请参阅[如何使用 Sysprep：简介](http://technet.microsoft.com/library/bb457073.aspx)。

1. 连接到虚拟机。
2. 以管理员身份打开“命令提示符”窗口。 将目录切换到 *%windir%\system32\sysprep*，然后运行 *sysprep.exe*。
3. 在“系统准备工具”对话框中，选择“进入系统全新体验(OOBE)”，确保已选中“通用化”复选框。
4. 在“关机选项”中选择“关机”，然后单击“确定”。
5. Sysprep 在完成运行后会关闭虚拟机。 请勿重启 VM。

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>解除分配 VM 并将其标记为通用化

若要创建映像，需解除分配 VM，并在 Azure 中将其标记为通用化。

使用 [Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) 解除分配 VM。

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

使用 [Set-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvm) 将虚拟机的状态设置为 `-Generalized`。 

```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```

## <a name="create-the-image"></a>创建映像

现在，可以使用 [New-AzureRmImageConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimageconfig) 和 [New-AzureRmImage](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage) 来创建 VM 的映像。 以下示例从名为“myVM”的 VM 创建名为“myImage”的映像。

获取虚拟机。 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

创建映像配置。

```powershell
$image = New-AzureRmImageConfig -Location ChinaEast -SourceVirtualMachineId $vm.ID 
```

创建映像。

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

## <a name="create-vms-from-the-image"></a>从映像创建 VM

在已有映像之后，可以从该映像创建一个或多个新 VM。 从自定义映像创建 VM 与使用市场映像创建 VM 很相似。 如果使用市场映像，需提供有关映像、映像提供程序、产品/服务、SKU 和版本的信息。 使用为 [New-AzureRMVM]() cmdlet 设置的简化参数时，如果自定义映像位于同一资源组中，则只需提供该映像的名称。 

本示例从“myResourceGroup”中的“myImage”创建名为“myVMfromImage”的 VM。

```powershell
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "China East" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```

## <a name="image-management"></a>映像管理 

下面提供了一些常见的管理映像任务示例，并说明了如何使用 PowerShell 完成这些任务。

按名称列出所有映像。

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

删除映像。 此示例将从 myResourceGroup 中删除名为 myOldImage 的映像。

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本教程中，你已创建了一个自定义 VM 映像。 你已了解如何：

> [!div class="checklist"]
> * 使用 Sysprep 通用化 VM
> * 创建自定义映像
> * 从自定义映像创建 VM
> * 列出订阅中的所有映像
> * 删除映像

请转到下一教程，了解如何创建高度可用的虚拟机。

> [!div class="nextstepaction"]
> [创建高度可用的 VM](tutorial-availability-sets.md)

<!--Update_Description: update meta properties, wording update -->
