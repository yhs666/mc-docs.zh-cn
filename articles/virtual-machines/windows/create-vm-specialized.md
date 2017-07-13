---
title: "从 Azure 中的专用 VHD 创建 VM | Azure"
description: "在 Resource Manager 部署模型中，通过将专用托管磁盘附加为 OS 磁盘来创建新 VM。"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 05/17/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: 5ac05866c0f3a09aaf5a4a69f05aa2d583799bea
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 从专用磁盘创建 VM
<a id="create-a-vm-from-a-specialized-disk" class="xliff"></a>

通过使用 Powershell 将专用托管磁盘附加为 OS 磁盘来创建新 VM。 专用磁盘是保留原始 VM 中用户帐户、应用程序和其他状态数据的现有 VM 中 VHD 的副本。 

可以使用两个选项：
* [上传 VHD](#option-1-upload-a-specialized-vhd)
* [复制现有 Azure VM 的 VHD](#option-2-copy-the-vhd-from-an-existing-azure-vm)

本主题介绍如何使用托管磁盘，如果存在需要使用存储帐户的旧部署，请参阅[从存储帐户中的专用 VHD 创建 VM](sa-create-vm-specialized.md)

## 开始之前
<a id="before-you-begin" class="xliff"></a>
如果使用 PowerShell，请确保使用的是最新版本的 AzureRM.Compute PowerShell 模块。 运行以下命令来安装该模块。

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azure/overview)。

## 选项 1：上传专用 VHD
<a id="option-1-upload-a-specialized-vhd" class="xliff"></a>

可从使用本地虚拟化工具（如 Hyper-V）创建的专用 VM 或从另一个云导出的 VM 上传 VHD。

### 准备 VM
<a id="prepare-the-vm" class="xliff"></a>
如果想要使用当前 VHD 创建新 VM，请确保完成以下步骤。 

  * [准备好要上传到 Azure 的 Windows VHD](prepare-for-upload-vhd-image.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。 **不要**使用 Sysprep 通用化 VM。
  * 删除 VM 上安装的所有来宾虚拟化工具和代理（例如 VMware 工具）。
  * 确保 VM 配置为通过 DHCP 来提取其 IP 地址和 DNS 设置。 这确保服务器在启动时在 VNet 中获取 IP 地址。 

### 获取存储帐户
<a id="get-the-storage-account" class="xliff"></a>
需要 Azure 中的存储帐户来存储上传的 VHD。 可以使用现有存储帐户，也可以创建新存储帐户。 

显示可用的存储帐户，请键入：

```powershell
Get-AzureRmStorageAccount
```

如果要使用现有存储帐户，请转到[上传 VHD](#upload-the-vhd-to-your-storage-account) 部分。

若要创建存储帐户，请执行以下步骤：

1. 需要应在其中创建存储帐户的资源组的名称。 若要查找订阅中的所有资源组，请键入：

    ```powershell
    Get-AzureRmResourceGroup
    ```

    若要在**中国北部**区域中创建名为 **myResourceGroup** 的资源组，请键入：

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "China North"
    ```

2. 使用 [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet 在此资源组中创建名为 **mystorageaccount** 的存储帐户：

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "China North" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### 将 VHD 上传到存储帐户
<a id="upload-the-vhd-to-your-storage-account" class="xliff"></a> 
使用 [Add-AzureRmVhd](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd) cmdlet 将 VHD 上传到存储帐户中的容器。 本示例将文件 **myVHD.vhd** 从 `"C:\Users\Public\Documents\Virtual hard disks\"` 上传到 **myResourceGroup** 资源组中名为 **mystorageaccount** 的存储帐户。 该文件将放入名为 **mycontainer** 的容器，新文件名为 **myUploadedVHD.vhd**。

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.chinacloudapi.cn/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedVhd `
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

## 选项 2：复制现有 Azure VM 的 VHD
<a id="option-2-copy-the-vhd-from-an-existing-azure-vm" class="xliff"></a>

可以在另一个存储帐户中为 VM 创建 VHD 副本。

### 开始之前
<a id="before-you-begin" class="xliff"></a>

请确保：

* 获取有关**源和目标存储帐户**的信息。 对于源 VM，需要具有存储帐户和容器名称。 通常，容器名称为 **vhds**。 还需要获取目标存储帐户。 如果尚未拥有存储帐户，可以使用门户（“更多服务”>“存储帐户”>“添加”）或使用 [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet 创建一个存储帐户。 
* 已下载并安装 [AzCopy 工具](../../storage/storage-use-azcopy.md)。 

### 解除分配 VM
<a id="deallocate-the-vm" class="xliff"></a>
解除分配 VM，释放要复制的 VHD。 

* **门户**：单击“虚拟机” > “myVM”>“停止”
* Powershell：使用 [Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) 停止（解除分配）资源组 myResourceGroup 中名为 myVM 的 VM。

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Azure 门户中该 VM 的“状态”将从“已停止”更改为“已停止(已解除分配)”。

### 获取存储帐户 URL
<a id="get-the-storage-account-urls" class="xliff"></a>
需要源和目标存储帐户的 URL。 URL 类似于： `https://<storageaccount>.blob.core.chinacloudapi.cn/<containerName>/`。 如果已经知道了存储帐户和容器名称，则只需替换括号之间的信息即可创建 URL。 

可以使用 Azure 门户或 Azure PowerShell 获取 URL：

* 门户：单击>“更多服务” > “存储帐户” > “存储帐户” > “Blob”，源 VHD 文件可能位于 vhd 容器中。 单击容器的“属性”并复制标记为 **URL** 的文本。 你将需要用到源和目标容器的 URL。 
* Powershell：使用 [Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) 获取资源组 myResourceGroup 中名为 myVM 的 VM 的信息。 在结果中，查看 **Vhd Uri** 的 **Storage profile** 部分。 URI 的第一部分是容器的 URL，最后一部分是 VM 的 OS VHD 名称。

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## 获取存储访问密钥
<a id="get-the-storage-access-keys" class="xliff"></a>
查找源和目标存储帐户的访问密钥。 有关访问密钥的详细信息，请参阅[关于 Azure 存储帐户](../../storage/storage-create-storage-account.md)。

* 门户：单击“更多服务” > “存储帐户” > “存储帐户” > “访问密钥”。 复制标记为 **key1** 的密钥。
* Powershell：使用 [Get-AzureRmStorageAccountKey](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccountkey) 获取资源组 myResourceGroup 中存储帐户 mystorageaccount 的存储密钥。 复制标记为 key1 的密钥。

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### 复制 VHD
<a id="copy-the-vhd" class="xliff"></a>
可以使用 AzCopy 在存储帐户之间复制文件。 对于目标容器，如果指定的容器不存在，系统会自动创建该容器。 

若要使用 AzCopy，请在本地计算机上打开命令提示符，然后导航到安装 AzCopy 的文件夹。 路径类似于 *C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy*。 

若要复制容器中的所有文件，请使用 **/S** 开关。 此开关可用于复制 OS VHD 和所有数据磁盘（如果它们在同一个容器中）。 本示例演示如何将存储帐户 **mysourcestorageaccount** 中容器 **mysourcecontainer** 内的所有文件复制到存储帐户 **mydestinationstorageaccount** 中的容器 **mydestinationcontainer**。 将存储帐户和容器的名称替换为自己的名称。 将 `<sourceStorageAccountKey1>` 和 `<destinationStorageAccountKey1>` 替换为自己的密钥。

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.chinacloudapi.cn/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.chinacloudapi.cn/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

如果只想要复制包含多个文件的容器中的特定 VHD，则还可以使用 /Pattern 开关指定文件名。 在本示例中，将复制名为 **myFileName.vhd** 的文件。

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.chinacloudapi.cn/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.chinacloudapi.cn/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```

完成后，将出现如下所示的消息：

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### 故障排除
<a id="troubleshooting" class="xliff"></a>

使用 AZCopy 时，如果看到错误“服务器无法对请求进行身份验证”，请确保授权标头的值构成正确（包括签名）。 如果使用的是密钥 2 或辅助存储密钥，则请尝试使用主密钥或第一个存储密钥。

## 创建新 VM
<a id="create-the-new-vm" class="xliff"></a> 

需创建新 VM 使用的网络和其他 VM 资源。

### 创建子网和 vNet
<a id="create-the-subnet-and-vnet" class="xliff"></a>

创建[虚拟网络](../../virtual-network/virtual-networks-overview.md)的 vNet 和子网。

创建子网。 本示例在资源组 **myResourceGroup** 中创建名为 **mySubnet** 的子网，并将子网地址前缀设置为 **10.0.0.0/24**。

```powershell
$rgName = "myResourceGroup"
$subnetName = "mySubNet"
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

创建 vNet。 本示例将虚拟网络名称设置为 **myVnetName**，将位置设置为“中国北部”，将虚拟网络的地址前缀设置为 **10.0.0.0/16**。 

```powershell
$location = "China North"
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    

### 创建公共 IP 地址和 NIC
<a id="create-a-public-ip-address-and-nic" class="xliff"></a>
若要与虚拟网络中的虚拟机通信，需要一个 [公共 IP 地址](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) 和网络接口。

创建公共 IP。 在此示例中，公共 IP 地址名称设置为 **myIP**。

```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
   -AllocationMethod Dynamic
```       

创建 NIC。 在此示例中，NIC 名称设置为 **myNicName**。

```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
```

### 创建网络安全组和 RDP 规则
<a id="create-the-network-security-group-and-an-rdp-rule" class="xliff"></a>
若要使用 RDP 登录到 VM，需要创建一个允许在端口 3389 上进行 RDP 访问的安全规则。 由于新 VM 的 VHD 是从现有专用 VM 创建的，因此在创建 VM 后，可以使用源虚拟机中有权通过 RDP 登录的现有帐户。
本示例将 NSG 名称设置为 **myNsg**，将 RDP 规则名称设置为 **myRdpRule**。

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule

```

有关终结点和 NSG 规则的详细信息，请参阅 [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)（使用 PowerShell 在 Azure 中打开 VM 端口）。

### 设置 VM 名称和大小
<a id="set-the-vm-name-and-size" class="xliff"></a>

此示例将 VM 名称设置为“myVM”，将 VM 大小设置为“Standard_A2”。

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### 添加 NIC
<a id="add-the-nic" class="xliff"></a>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

### 从 VHD 创建托管磁盘
<a id="create-a-managed-disk-from-the-vhd" class="xliff"></a>

使用 [New-AzureRMDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk) 通过存储帐户中的专用 VHD 创建托管磁盘。 此示例使用 myOSDisk1 作为磁盘名称，将磁盘置于 StandardLRS 存储中并使用 https://storageaccount.blob.core.chinacloudapi.cn/vhdcontainer/osdisk.vhd 作为上传或复制到存储帐户的源 VHD 的 URI。

```powershell
$sourceUri = https://storageaccount.blob.core.chinacloudapi.cn/vhdcontainer/osdisk.vhd)
$osDisk = New-AzureRmDisk -DiskName "myOSDisk1" -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $rgName
```

使用 [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk) 将 OS 磁盘添加到配置。 此示例将磁盘大小设置为 **128 GB** 并附加托管磁盘作为 **Windows** OS 磁盘。

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

可选：附加其他托管磁盘作为数据磁盘。 此选项假定通过[创建托管数据磁盘](create-managed-disk-ps.md)创建了托管数据磁盘。 

```powershell
$vm = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
```

### 完成该 VM
<a id="complete-the-vm" class="xliff"></a> 

使用刚刚创建的 [New-AzureRMVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm) 配置创建 VM。

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

如果此命令成功，你将看到类似于下面的输出：

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### 验证是否已创建 VM
<a id="verify-that-the-vm-was-created" class="xliff"></a>
应会在 [Azure 门户](https://portal.azure.cn)的“浏览” > “虚拟机”下看到新建的 VM，也可以使用以下 PowerShell 命令查看该 VM：

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## 后续步骤
<a id="next-steps" class="xliff"></a>
若要登录到新虚拟机，请在[门户](https://portal.azure.cn)中浏览到该 VM，单击“连接”，然后打开远程桌面 RDP 文件。 使用原始虚拟机的帐户凭据登录到新虚拟机。 有关详细信息，请参阅 [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md)（如何连接并登录到运行 Windows 的 Azure 虚拟机）。
