<!-- not suitable for Mooncake -->

---
title: 将 VM 从非托管磁盘转换为托管磁盘 - Azure | Azure
description: 在 Resource Manager 部署模型中使用 PowerShell 将 VM 从非托管磁盘转换为托管磁盘
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
ms.date: 02/08/2017
wacn.date: 03/20/2017
ms.author: cynthn
---

# 将 VM 从非托管磁盘转换为托管磁盘

如果现有的 Azure VM 在存储帐户中使用非托管磁盘，但你希望能够利用[托管磁盘](../../storage/storage-managed-disks-overview.md)，可以转换这些 VM。这个过程会同时将 OS 磁盘和任何附加的数据磁盘，从在存储帐户中使用非托管磁盘转换为使用托管磁盘。VM 将关闭并解除分配，然后，你可以使用 PowerShell 将 VM 转换为使用托管磁盘。完成转换后，请重新启动 VM，然后它将开始使用托管磁盘。

在开始之前，请确保已查看[规划迁移到托管磁盘](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks)。在生产中执行迁移之前，请务必先通过迁移测试虚拟机来测试迁移过程，因为迁移过程是不可逆的。

> [!IMPORTANT] 
在转换期间，会解除分配 VM。解除分配 VM 意味着它在完成转换并重新启动后将使用新的 IP 地址。如果依赖于固定 IP，应使用保留 IP。

## 托管磁盘和 Azure 存储服务加密 (SSE)

如果任何附加的非托管磁盘位于使用或曾使用 [Azure 存储服务加密 (SSE)](../../storage/storage-service-encryption.md) 加密的存储帐户中，则无法将在 Resource Manager 部署模型中创建的非托管 VM 转换为托管磁盘。以下步骤详细说明了如何转换位于（或曾位于）已加密存储帐户的非托管 VM：

**数据磁盘**：
1. 从 VM 中分离数据磁盘。
2. 将 VHD 复制到从未启用 SSE 的存储帐户。若要将磁盘复制到另一存储帐户，请使用 [AzCopy](../../storage/storage-use-azcopy.md)：`https://sourceaccount.blob.core.chinacloudapi.cn/myvhd.vhd  https://destaccount.blob.core.chinacloudapi.cn/myvhd_no_encrypt.vhd /sourcekey:key1 /destkey:key1`
3. 将复制的磁盘附加到 VM，并转换 VM。

**OS 磁盘**：
1. 停止（解除分配）VM。保存 VM 配置（如果需要）。
2. 将 OS VHD 复制到从未启用 SSE 的存储帐户。若要将磁盘复制到另一存储帐户，请使用 [AzCopy](../../storage/storage-use-azcopy.md)：`https://sourceaccount.blob.core.chinacloudapi.cn/myvhd.vhd  https://destaccount.blob.core.chinacloudapi.cn/myvhd_no_encrypt.vhd /sourcekey:key1 /destkey:key1`
3. 创建使用托管磁盘的 VM，并在创建过程中将 VHD 文件作为 OS 磁盘附加。

## 开始之前
如果使用 PowerShell，请确保使用的是最新版本的 AzureRM.Compute PowerShell 模块。运行以下命令安装该模块。

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```

有关详细信息，请参阅 [Azure PowerShell Versioning](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning)（Azure PowerShell 版本控制）。

## <a name="convert-vms-in-an-availability-set-to-managed-disks-in-a-managed-availability-set"></a> 将可用性集中的 VM 转换为托管可用性集中的托管磁盘

如果要转换为托管磁盘的 VM 位于可用性集中，则需要先将可用性集转换为托管可用性集。

以下脚本将可用性集更新为托管可用性集，然后解除分配，转换磁盘，并在可用性集中重新启动每个 VM。

```
$rgName = 'myResourceGroup'
$avSetName = 'myAvailabilitySet'

$avSet =  Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Managed

foreach($vmInfo in $avSet.VirtualMachinesReferences)
    {
   $vm =  Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}

   Stop-AzureRmVM -ResourceGroupName $rgName -Name  $vm.Name -Force

   ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name

    }
```

## 将现有 Azure VM 转换为同一存储类型的托管磁盘

> [!IMPORTANT]
执行以下过程后，默认的 /vhds 容器中将保留一个单独的块 Blob。文件的名称是“VMName.xxxxxxx.status”。请不要删除此剩余状态对象。将来应能解决此问题。

本部分介绍如何在使用相同存储类型时，将现有 Azure VM 从存储帐户中的非托管磁盘转换为托管磁盘。可以通过此过程，将高级 (SSD) 非托管磁盘转换为高级托管磁盘，或将标准 (HDD) 非托管磁盘转换为标准托管磁盘。

1. 创建变量并解除分配 VM。此示例将资源组名称设置为 **myResourceGroup**，将 VM 名称设置为 **myVM**。

    ```
    $rgName = "myResourceGroup"
    $vmName = "myVM"
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```

    Azure 门户预览中该 VM 的“状态”将从“已停止”更改为“已停止(已解除分配)”。

2. 转换与 VM 关联的所有磁盘，包括 OS 磁盘和所有数据磁盘。

    ```
    ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
    ```

## 将使用标准非托管磁盘的现有 Azure VM 迁移到高级托管磁盘

本部分说明如何将标准非托管磁盘上的现有 Azure VM 转换为高级托管磁盘。若要使用高级托管磁盘，VM 必须使用支持高级存储的 [VM 大小](../virtual-machines-windows-sizes.md)。

1.  首先设置通用参数。确保所选的 [VM 大小](../virtual-machines-windows-sizes.md)支持高级存储。

    ```
    $resourceGroupName = 'YourResourceGroupName'
    $vmName = 'YourVMName'
    $size = 'Standard_DS2_v2'
    ```

1.  获取使用非托管磁盘的 VM

    ```
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
    ```

1.  停止（解除分配）VM。

    ```
    Stop-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName -Force
    ```

1.  将 VM 的大小更新为该 VM 所在区域中支持高级存储的可用大小。

    ```
    $vm.HardwareProfile.VmSize = $size
    Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroupName
    ```

1.  将使用非托管磁盘的虚拟机转换为托管磁盘。

    如果遇到内部服务器错误，请先重试 2-3 次，然后与我们的支持团队联系。

    ```
    ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $resourceGroupName -VMName $vmName
    ```

1. 停止（解除分配）VM。

    ```
    Stop-AzureRmVM -ResourceGroupName $resourceGroupName -VMName $vmName -Force
    ```

2.  将所有磁盘升级为高级存储。

    ```
    $vmDisks = Get-AzureRmDisk -ResourceGroupName $resourceGroupName 
    foreach ($disk in $vmDisks) 
        {
        if($disk.OwnerId -eq $vm.Id)
            {
             $diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType PremiumLRS
             Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $resourceGroupName `
             -DiskName $disk.Name
            }
        }
    ```

1. 启动 VM。

    ```
    Start-AzureRmVM -ResourceGroupName $resourceGroupName -VMName $vmName
    ```

也可以对磁盘混合使用标准和高级存储。

## 后续步骤

使用[快照](../virtual-machines-windows-snapshot-copy-managed-disk.md)获取 VM 的只读副本。

<!---HONumber=Mooncake_0313_2017-->