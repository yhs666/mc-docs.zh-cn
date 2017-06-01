<!-- not suitbale for Mooncake -->

---
title: 从 Azure 中的托管 VM 映像创建 VM | Azure
description: 在 Resource Manager 部署模型中，使用 Azure PowerShell 从通用托管 VM 映像创建 Windows 虚拟机。
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager

ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/7/2017
wacn.date: 03/20/2017
ms.author: cynthn
---

# 从通用托管 VM 映像创建 VM

可以从 Azure 中托管的 VM 映像创建多个 VM。托管 VM 映像包含创建 VM 所需的信息，包括 OS 和数据磁盘。构成映像的 VHD（包括 OS 磁盘和任何数据磁盘）存储为托管磁盘。

通用 VM 已使用 [Sysprep](../virtual-machines-windows-generalize-vhd.md) 删除所有个人帐户信息。若要创建通用 VM，可在本地 VM 上运行 Sysprep，然后[将 VHD 上载到 Azure](../virtual-machines-windows-upload-image.md)；或者在现有 Azure VM 上运行 Sysprep，然后[捕获 VM 的映射](../virtual-machines-windows-capture-image-resource.md)。

## 先决条件

需要已[创建托管 VM 映像](../virtual-machines-windows-capture-image-resource.md)以用于创建新 VM。

请确保你有最新版本的 AzureRM.Compute PowerShell 模块。运行以下命令来安装该模块。

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```

有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning)。

## 收集有关映像的信息

首先需要收集有关映像的基本信息并创建映像的变量。本示例使用**中国西北部**位置的 **myResourceGroup** 资源组中名为 **myImage** 的托管 VM 映像。

```
$rgName = "myResourceGroup"
$location = "West China North"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## 创建虚拟网络
创建[虚拟网络](../../virtual-network/virtual-networks-overview.md)的 vNet 和子网。

1. 创建子网。本示例创建名为 **mySubnet** 的子网，其地址前缀为 **10.0.0.0/24**。

    ```
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. 创建虚拟网络。本示例创建名为 **myVnet** 的虚拟网络，其地址前缀为 **10.0.0.0/16**。

    ```
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```

## 创建公共 IP 地址和网络接口

若要与虚拟网络中的虚拟机通信，需要一个[公共 IP 地址](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)和网络接口。

1. 创建公共 IP 地址。此示例创建名为 **myPip** 的公共 IP 地址。

    ```
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```

2. 创建 NIC。此示例创建名为 **myNic** 的 NIC。

    ```
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## 创建网络安全组和 RDP 规则

若要使用 RDP 登录到 VM，需要创建一个允许在端口 3389 上进行 RDP 访问的网络安全规则 (NSG)。

此示例创建名为 **myNsg** 的 NSG，其中包含一个允许通过端口 3389 传输 RDP 流量的、名为 **myRdpRule** 的规则。有关 NSG 的详细信息，请参阅 [Opening ports to a VM in Azure using PowerShell](../virtual-machines-windows-nsg-quickstart-powershell.md)（使用 PowerShell 在 Azure 中打开 VM 端口）。

```
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

## 为虚拟网络创建变量

为完成的虚拟网络创建变量。

```
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## 获取 VM 的凭据

以下 cmdlet 将打开一个窗口，需在其中输入远程访问 VM 所用的本地管理员帐户的新用户名和密码。

```
$cred = Get-Credential
```

## 设置 VM 名称和计算机名称的变量以及 VM 的大小

1. 创建 VM 名称与计算机名称的变量。本示例将 VM 名称设置为 **myVM**，将计算机名称设置为 **myComputer**。

    ```
    $vmName = "myVM"
    $computerName = "myComputer"
    ```

2. 设置虚拟机的大小。本示例将创建 **Standard\_DS1\_v2** 大小的 VM。有关详细信息，请参阅 [VM 大小](../virtual-machines-windows-sizes.md)文档。

    ```
    $vmSize = "Standard_DS1_v2"
    ```

3. 将 VM 的名称和大小添加到 VM 配置。

    $vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

## 将 VM 映像设置为新 VM 的源映像

使用托管 VM 映像的 ID 设置源映像。

```
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## 设置 OS 配置并添加 NIC。

输入 OS 磁盘的存储类型（PremiumLRS 或 StandardLRS）和大小。本示例将帐户类型设置为 **PremiumLRS**，将磁盘大小设置为 **128 GB**，将磁盘缓存设置为 **ReadWrite**。

```
$vm = Set-AzureRmVMOSDisk -VM $vm  -ManagedDiskStorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## 创建 VM

使用在 **$vm** 变量中生成和存储的配置创建新 VM。

```
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## 验证是否已创建 VM
完成后，应会在 [Azure 门户预览](https://portal.azure.cn)的“浏览”>“虚拟机”下看到新建的 VM，也可以使用以下 PowerShell 命令查看该 VM：

```
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## 后续步骤
若要使用 Azure PowerShell 管理新虚拟机，请参阅[使用 Azure Resource Manager 与 PowerShell 来管理虚拟机](../virtual-machines-windows-ps-manage.md)。

<!---HONumber=Mooncake_0313_2017-->