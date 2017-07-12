---
title: "从通用化本地 VHD 创建托管 Azure VM | Azure"
description: "将通用化 VHD 上传到 Azure，然后在 Resource Manager 部署模型中使用它来创建新的 VM。"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 05/19/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: e982113d9f63f76987811e64c153f63801177fae
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<a id="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure" class="xliff"></a>

# 上传通用化 VHD 并使用它在 Azure 中创建新 VM

本主题逐步讲解如何使用 PowerShell 将通用化 VM 的 VHD 上传到 Azure、从该 VHD 创建映像，然后从该映像创建新 VM。 可以上传从本地虚拟化工具或其他云导出的 VHD。 对新的 VM 使用[托管磁盘](../../storage/storage-managed-disks-overview.md)可以简化 VM 管理，在将 VM 置于可用性集中时提供更好的可用性。 

若要使用示例脚本，请参阅[将 VHD 上传到 Azure 并创建新的 VM 的示例脚本](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

<a id="before-you-begin" class="xliff"></a>

## 开始之前

- 将任何 VHD 上传到 Azure 之前，应按照[准备要上传到 Azure 的 Windows VHD 或 VHDX](prepare-for-upload-vhd-image.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json) 进行操作
- 开始迁移到[托管磁盘](../../storage/storage-managed-disks-overview.md)之前，请先查看[规划迁移到托管磁盘](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks)。
- 请确保你有最新版本的 AzureRM.Compute PowerShell 模块。 运行以下命令来安装该模块。

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azure/overview)。

<a id="generalize-the-windows-vm-using-sysprep" class="xliff"></a>

## 使用 Sysprep 通用化 Windows VM

Sysprep 将删除所有个人帐户信息及其他某些数据，并准备好要用作映像的计算机。 有关 Sysprep 的详细信息，请参阅[如何使用 Sysprep：简介](http://technet.microsoft.com/library/bb457073.aspx)。

确保 Sysprep 支持计算机上运行的服务器角色。 有关详细信息，请参阅 [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> 如果在首次将 VHD 上传到 Azure 之前运行 Sysprep，请确保先[准备好 VM](prepare-for-upload-vhd-image.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)，然后再运行 Sysprep。 
> 
> 

1. 登录到 Windows 虚拟机。
2. 以管理员身份打开“命令提示符”窗口。 将目录切换到 **%windir%\system32\sysprep**，然后运行 `sysprep.exe`。
3. 在“系统准备工具”对话框中，选择“进入系统全新体验(OOBE)”，确保已选中“通用化”复选框。
4. 在“关机选项”中选择“关机”。
5. 单击 **“确定”**。

    ![启动 Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. 在 Sysprep 完成时，它会关闭虚拟机。 不要重新启动 VM。

<a id="log-in-to-azure" class="xliff"></a>

## 登录到 Azure
如果尚未安装 Azure PowerShell 1.4 版或更高版本，请阅读 [如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。

1. 打开 Azure PowerShell 并登录到 Azure 帐户。 将打开一个弹出窗口，可以输入 Azure 帐户凭据。

    ```powershell
    Login-AzureRmAccount -EnvironmentName AzureChinaCloud
    ```
2. 获取可用订阅的订阅 ID。

    ```powershell
    Get-AzureRmSubscription
    ```
3. 使用订阅 ID 设置正确的订阅。 将 *<subscriptionID>* 替换为正确订阅的 ID。

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

<a id="get-the-storage-account" class="xliff"></a>

## 获取存储帐户
需要在 Azure 中创建存储帐户来存储上传的 VM 映像。 可以使用现有存储帐户，也可以创建新存储帐户。 

如果将使用 VHD 为 VM 创建托管磁盘，存储帐户位置必须与要创建 VM 的位置相同。

显示可用的存储帐户，请键入：

```powershell
Get-AzureRmStorageAccount
```

如果要使用现有存储帐户，请转到 [上传 VM 映像](#upload-the-vhd-to-your-storage-account) 部分。

若要创建存储帐户，请执行以下步骤：

1. 需要应在其中创建存储帐户的资源组的名称。 若要查找订阅中的所有资源组，请键入：

    ```powershell
    Get-AzureRmResourceGroup
    ```

    若要在**中国东部**区域中创建名为 **myResourceGroup** 的资源组，请键入：

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "China East"
    ```

2. 使用 [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet 在此资源组中创建名为 **mystorageaccount** 的存储帐户：

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "China East"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

    -SkuName 的有效值为：

   * **Standard_LRS** - 本地冗余存储。 
   * **Standard_ZRS** - 区域冗余存储。
   * **Standard_GRS** - 异地冗余存储。 
   * **Standard_RAGRS** - 读取访问权限异地冗余存储。 
   * **Premium_LRS** - 高级本地冗余存储。 

<a id="upload-the-vhd-to-your-storage-account" class="xliff"></a>

## 将 VHD 上传到存储帐户

使用 [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet 将 VHD 上传到存储帐户中的容器。 本示例将文件 *myVHD.vhd* 从 *"C:\Users\Public\Documents\Virtual hard disks\"* 上传到 *myResourceGroup* 资源组中名为 *mystorageaccount* 的存储帐户。 该文件将放入名为 *mycontainer* 的容器，新文件名为 *myUploadedVHD.vhd*。

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.chinacloudapi.cn/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```

如果成功，将显示类似于下面的响应：

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.chinacloudapi.cn/mycontainer/myUploadedVHD.vhd
```

完成执行此命令可能需要一段时间，具体取决于网络连接速度和 VHD 文件的大小

如果要使用上传的 VHD 创建托管磁盘或新 VM，请保存 **目标 URI** 路径供稍后使用。

<a id="other-options-for-uploading-a-vhd" class="xliff"></a>

### 用于上传 VHD 的其他选项

也可以使用以下方法之一将 VHD 上传到存储帐户：

- [AzCopy](http://aka.ms/downloadazcopy)
- [Azure 存储复制 Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure 存储资源管理器上传 Blob](https://azurestorageexplorer.codeplex.com/)
- [Storage Import/Export Service REST API Reference](https://msdn.microsoft.com/library/dn529096.aspx)（存储导入/导出服务 REST API 参考）
-   如果预估上传时间大于 7 天，建议使用导入/导出服务。 可根据数据大小和传输单位，利用 [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) 预估时间。 
    导入/导出可用于复制到标准存储帐户。 需要使用 AzCopy 等工具从标准存储复制到高级存储帐户。

<a id="create-a-managed-image-from-the-uploaded-vhd" class="xliff"></a>

## 通过上传的 VHD 创建托管映像 

使用通用 OS VHD 创建托管映像。 将值替换为自己的信息。

1.  首先，设置公共参数：

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "China East" 
    $imageName = "yourImageName"
    ```

4.  使用通用 OS VHD 创建映像。

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

<a id="create-a-virtual-network" class="xliff"></a>

## 创建虚拟网络
创建[虚拟网络](../../virtual-network/virtual-networks-overview.md)的 vNet 和子网。

1. 创建子网。 此示例创建名为 *mySubnet* 的子网，其地址前缀为 *10.0.0.0/24*。  

    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. 创建虚拟网络。 此示例创建名为 *myVnet* 的虚拟网络，其地址前缀为 *10.0.0.0/16*。  

    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

<a id="create-a-public-ip-address-and-network-interface" class="xliff"></a>

## 创建公共 IP 地址和网络接口

若要与虚拟网络中的虚拟机通信，需要一个 [公共 IP 地址](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) 和网络接口。

1. 创建公共 IP 地址。 此示例创建名为 *myPip*的公共 IP 地址。 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. 创建 NIC。 此示例创建名为 **myNic**的 NIC。 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

<a id="create-the-network-security-group-and-an-rdp-rule" class="xliff"></a>

## 创建网络安全组和 RDP 规则

若要使用 RDP 登录到 VM，需要创建一个允许在端口 3389 上进行 RDP 访问的网络安全规则 (NSG)。 

此示例创建名为 *myNsg* 的 NSG，其中包含一个允许通过端口 3389 传输 RDP 流量的、名为 *myRdpRule* 的规则。 有关 NSG 的详细信息，请参阅 [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)（使用 PowerShell 在 Azure 中打开 VM 端口）。

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

<a id="create-a-variable-for-the-virtual-network" class="xliff"></a>

## 为虚拟网络创建变量

为完成的虚拟网络创建变量。 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

<a id="get-the-credentials-for-the-vm" class="xliff"></a>

## 获取 VM 的凭据

以下 cmdlet 将打开一个窗口，需在其中输入远程访问 VM 所用的本地管理员帐户的新用户名和密码。 

```powershell
$cred = Get-Credential
```

<a id="add-the-vm-name-and-size-to-the-vm-configuration" class="xliff"></a>

## 将 VM 的名称和大小添加到 VM 配置。

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

<a id="set-the-vm-image-as-source-image-for-the-new-vm" class="xliff"></a>

## 将 VM 映像设置为新 VM 的源映像

使用托管 VM 映像的 ID 设置源映像。

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

<a id="set-the-os-configuration-and-add-the-nic" class="xliff"></a>

## 设置 OS 配置并添加 NIC。

输入 OS 磁盘的存储类型（PremiumLRS 或 StandardLRS）和大小。 此示例将帐户类型设置为 *PremiumLRS*，将磁盘大小设置为 *128 GB*，将磁盘缓存设置为 *ReadWrite*。

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<a id="create-the-vm" class="xliff"></a>

## 创建 VM

使用存储在 **$vm** 变量中的配置创建新 VM。

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

<a id="verify-that-the-vm-was-created" class="xliff"></a>

## 验证是否已创建 VM
完成后，应会在 [Azure 门户](https://portal.azure.cn)的“浏览” > “虚拟机”下看到新建的 VM，也可以使用以下 PowerShell 命令查看该 VM：

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

<a id="next-steps" class="xliff"></a>

## 后续步骤

若要登录到新虚拟机，请在[门户](https://portal.azure.cn)中浏览到该 VM，单击“连接”，然后打开远程桌面 RDP 文件。 使用原始虚拟机的帐户凭据登录到新虚拟机。 有关详细信息，请参阅 [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)（如何连接并登录到运行 Windows 的 Azure 虚拟机）。
