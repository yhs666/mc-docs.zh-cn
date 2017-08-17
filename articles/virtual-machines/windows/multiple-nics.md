---
title: "在 Azure 中创建并管理使用多个 NIC 的 Windows VM | Azure"
description: "了解如何使用 Azure PowerShell 或资源管理器模板创建并管理附有多个 NIC 的 Windows VM。"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 07/05/2017
ms.date: 08/14/2017
ms.author: v-dazen
ms.openlocfilehash: 93a4a7e262d7397b147beef38a0d9926e3ec7df7
ms.sourcegitcommit: f858adac6a7a32df67bcd5c43946bba5b8ec6afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>创建并管理具有多个 NIC 的 Windows 虚拟机
Azure 中的虚拟机 (VM) 可附有多个虚拟网络接口卡 (NIC)。 一种常见方案是为前端和后端连接使用不同子网，或为监视或备份解决方案使用一个专用网络。 本文详述了如何创建附有多个 NIC 的 VM。 还可以了解如何从现有 VM 中添加或删除 NIC。 不同的 [VM 大小](sizes.md)支持不同数目的 NIC，因此请相应地调整 VM 的大小。

有关详细信息，包括如何在自己的 PowerShell 脚本中创建多个 NIC，请参阅[部署具有多个 NIC 的 VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md)。

## <a name="prerequisites"></a>先决条件
确保[已安装并配置最新版本的 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。

在以下示例中，请将示例参数名称替换成自己的值。 示例参数名称包括 *myResourceGroup*、*myVnet* 和 *myVM*。

## <a name="create-a-vm-with-multiple-nics"></a>创建具有多个 NIC 的 VM
首先创建一个资源组。 以下示例在 *EastUs* 位置创建名为 *myResourceGroup* 的资源组：

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "ChinaEast"
```

### <a name="create-virtual-network-and-subnets"></a>创建虚拟网络和子网
虚拟网络的一种常见方案是具有两个或多个子网。 一个子网可能用于前端流量，另一个用于后端流量。 若要连接两个子网，可在 VM 上使用多个 NIC。

1. 通过 [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) 定义两个虚拟网络子网。 以下示例分别定义 *mySubnetFrontEnd* 和 *mySubnetBackEnd* 的子网：

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. 通过 [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) 创建虚拟网络和子网。 以下示例创建一个名为 *myVnet* 的虚拟网络：

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```

### <a name="create-multiple-nics"></a>创建多个 NIC
通过 [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) 创建两个 NIC。 将其中一个 NIC 附加到前端子网，将另一个 NIC 附加到后端子网。 以下示例创建名为 *myNic1* 和 *myNic2* 的 NIC：

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

通常，还会创建[网络安全组](../../virtual-network/virtual-networks-nsg.md)或[负载均衡器](../../load-balancer/load-balancer-overview.md)来帮助管理流量以及跨 VM 分布流量。 [更详细的多 NIC VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) 一文逐步讲解了如何创建网络安全组和分配 NIC。

### <a name="create-the-virtual-machine"></a>创建虚拟机
立即开始构建 VM 配置。 每种 VM 大小限制了可添加到 VM 的 NIC 数目。 有关详细信息，请参阅 [Windows VM 大小](sizes.md)。

1. 将 VM 凭据设置为 `$cred` 变量，如下所示：

    ```powershell
    $cred = Get-Credential
    ```

2. 通过 [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) 定义 VM。 以下示例定义名为 *myVM* 的 VM，并使用支持两个以上 NIC 的 VM 大小(*Standard_DS3_v2*)：

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. 通过 [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) 和 [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) 创建 VM 配置的其余部分。 以下示例创建一个 Windows Server 2016 VM：

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. 通过 [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) 附加两个之前创建的 NIC：

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. 最后，通过 [New-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) 创建 VM：

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-to-an-existing-vm"></a>向现有 VM 添加 NIC
若要向现有 VM 添加虚拟 NIC，解除分配 VM，添加虚拟 NIC，并启动 VM。

1. 通过 [Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) 解除分配 VM。 以下示例解除分配 *myResourceGroup* 中名为 *myVM* 的 VM：

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. 通过 [Get-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) 获取 VM 的现有配置。 以下示例从 *myResourceGroup* 中获取名为 *myVM* 的 VM 的信息：

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. 以下示例通过 [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) 创建附加到 *mySubnetBackEnd* 的名为 *myNic3* 的虚拟 NIC。 然后，通过 [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) 将虚拟 NIC 附加到 *myResourceGroup* 中名为 *myVM* 的 VM：

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>主虚拟 NIC
    具有多个 NIC 的 VM 上其中一个需为主 NIC。 如果 VM 上现有虚拟 NIC 之一已设置为主 NIC，则可跳过此步骤。 以下示例假设 VM 上现在存在两个虚拟NIC，并且想要将第一个 NIC (`[0]`) 添加为主 NIC：

    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces

    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false

    # Update the VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. 通过 [Start-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) 启动 VM：

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a>从现有 VM 中删除 NIC
若要从现有 VM 中删除虚拟 NIC，解除分配 VM，删除虚拟 NIC，并启动 VM。

1. 通过 [Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) 解除分配 VM。 以下示例解除分配 *myResourceGroup* 中名为 *myVM* 的 VM：

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. 通过 [Get-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) 获取 VM 的现有配置。 以下示例从 *myResourceGroup* 中获取名为 *myVM* 的 VM 的信息：

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. 通过 [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterface) 获取有关删除 NIC 的信息。 以下示例获取有关“myNic3”的信息：

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. 通过 [Remove-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) 删除 NIC，并通过 [Update-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) 更新 VM。 以下示例删除上一步中由 `$nicId` 获得的“myNic3”：

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. 通过 [Start-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) 启动 VM：

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>使用模板创建多个 NIC
使用 Azure 资源管理器模板可在部署期间创建资源的多个实例，例如，创建多个 NIC。 资源管理器模板使用声明性 JSON 文件来定义环境。 有关详细信息，请参阅 [Azure 资源管理器概述](../../azure-resource-manager/resource-group-overview.md)。 使用“copy”指定要创建的实例数：

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

有关详细信息，请参阅[使用“copy”创建多个实例](../../resource-group-create-multiple.md)。 

还可以使用 `copyIndex()` 在资源名称后面追加一个数字。 然后可创建“myNic1”、“MyNic2”等。 以下代码演示了追加索引值的示例：

```json
"name": "[concat('myNic', copyIndex())]", 
```

可以阅读有关[使用 Resource Manager 模板创建多个 NIC](../../virtual-network/virtual-network-deploy-multinic-arm-template.md) 的完整示例。

## <a name="next-steps"></a>后续步骤
尝试创建具有多个 NIC 的 VM 时，请查看 [Windows VM 大小](sizes.md)。 注意每个 VM 大小支持的 NIC 数目上限。

<!--Update_Description: wording update-->