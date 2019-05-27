---
title: 在 Azure 本地 Shell 中使用简化的 New-AzVM cmdlet 创建 Windows VM | Azure
description: 快速了解如何在 Azure 本地 Shell 中使用简化的 New-AzVM cmdlet 创建 Windows 虚拟机。
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 12/12/2017
ms.date: 05/20/2019
ms.author: v-yeche
ROBOTS: NOINDEX
ms.openlocfilehash: 3c635046c6892cdacd8d703ea07e603ec7911abb
ms.sourcegitcommit: 878a2d65e042b466c083d3ede1ab0988916eaa3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835823"
---
<!--Verify Successfully-->
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azvm-cmdlet-in-cloud-shell"></a>在 Cloud Shell 中使用简化的 New-AzVM cmdlet 创建 Windows 虚拟机 

[New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet 已添加了一组简化的参数，用于使用 PowerShell 创建新的 VM。 本主题演示如何通过 Azure 本地 Shell 中的 PowerShell，使用预安装的最新版本 New-AzureVM cmdlet 创建新的 VM。 我们将使用简化的参数集，以使用智能默认设置自动创建所有必需的资源。 

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

<!--[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]-->

## <a name="create-the-vm"></a>创建 VM

可以使用 [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet 创建具有智能默认设置（包括使用 Azure 市场中的 Windows Server 2016 Datacenter 映像）的 VM。 可以将 New-AzVM 仅与 **-Name** 参数配合使用，它会将该值用于所有资源名称。 在本示例中，我们将设置 **-Name** 参数设置为 *myVM*。 

请确保在 Cloud Shell 中选择 **PowerShell** 并键入：

```powershell
New-AzVm -Name myVM
```

系统会要求为 VM 创建用户名和密码，稍后在本主题中连接到该 VM 时需要用到这些凭据。 密码长度必须为 12 到 123 个字符，并且必须满足以下 4 个复杂性要求的其中 3 个：1 个小写字符、1 个大写字符、1 个数字和 1 个特殊字符。

创建 VM 和关联的资源需要一分钟时间。 完成后，可以使用 [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) cmdlet 查看创建的所有资源。

```powershell
Get-AzResource `
    -ResourceGroupName myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>连接到 VM

在部署完成后，创建到虚拟机的远程桌面连接。

使用 [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) 命令返回虚拟机的公共 IP 地址。 请记下此 IP 地址。

```powershell
Get-AzPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

在本地计算机上打开命令提示符，并使用 **mstsc** 命令开始与新 VM 建立远程桌面会话。 请将 &lt;publicIPAddress&gt; 替换为虚拟机的 IP 地址。 出现提示时，请输入创建 VM 时指定的用户名和密码。

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>指定不同的资源名称

还可以为资源提供更具描述性的名称，并仍将它们自动创建。 下面是一个示例，其中为新的 VM 命名了多个资源，包括新的资源组。

```powershell
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "China East" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要资源组、VM 和所有相关的资源，可以使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 命令将其删除。

```powershell
Remove-AzResourceGroup -Name myVM
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

本主题使用 New-AzVM 部署了一个简单的虚拟机，然后通过 RDP 连接到该虚拟机。 若要深入了解 Azure 虚拟机，请继续学习 Windows VM 教程。

> [!div class="nextstepaction"]
> [Azure Windows 虚拟机教程](./tutorial-manage-vm.md)

<!-- Update_Description: new article about new azvm demo-->
<!--ms.date: 05/20/2019-->