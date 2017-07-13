---
title: "在 Azure 中创建托管映像 | Azure"
description: "在 Azure 中创建通用 VM 或 VHD 的托管映像。 映像可用于创建多个使用托管磁盘的 VM。"
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
origin.date: 02/27/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: fc5abae9b42ce4e9449745e494454cdbaed75d1d
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 在 Azure 中创建通用 VM 的托管映像
<a id="create-a-managed-image-of-a-generalized-vm-in-azure" class="xliff"></a>

可以从在存储帐户中存储为托管磁盘或非托管磁盘的通用 VM 中创建托管映像资源。 然后可以使用该映像创建多个 VM。 

## 使用 Sysprep 通用化 Windows VM
<a id="generalize-the-windows-vm-using-sysprep" class="xliff"></a>

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

## 使用 Powershell 创建 VM 的托管映像
<a id="create-a-managed-image-of-a-vm-using-powershell" class="xliff"></a>

直接从 VM 创建映像可确保映像中包含与 VM 关联的所有磁盘，包括 OS 磁盘和任何数据磁盘。

在开始之前，请确保你有最新版本的 AzureRM.Compute PowerShell 模块。 运行以下命令来安装该模块。

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
有关详细信息，请参阅 [Azure PowerShell 版本控制](https://docs.microsoft.com/powershell/azure/overview)。

1. 创建一些变量。 
    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "ChinaEast"
    $imageName = "myImage"
    ```
2. 确保 VM 已解除分配。

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```

3. 将虚拟机的状态设置为“通用化”。 

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```

4. 获取虚拟机。 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. 创建映像配置。

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. 创建映像。

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 

## 在 PowerShell 中创建 VHD 的托管映像
<a id="create-a-managed-image-of-a-vhd-in-powershell" class="xliff"></a>

使用通用 OS VHD 创建托管映像。

1.  首先，设置公共参数：

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "China North" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.chinacloudapi.cn/vhdcontainer/osdisk.vhd"
    ```
2. 停止\解除分配 VM。

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```

3. 将 VM 标记为通用。

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  使用通用 OS VHD 创建映像。

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## 使用 Powershell 从快照创建托管映像
<a id="create-a-managed-image-from-a-snapshot-using-powershell" class="xliff"></a>

还可以从通用 VM 的 VHD 快照创建托管映像。

1. 创建一些变量。 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "ChinaEast"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. 获取快照。

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```

3. 创建映像配置。

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. 创建映像。

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 

## 后续步骤
<a id="next-steps" class="xliff"></a>
- 现在，你可以[从通用托管映像创建 VM](create-vm-generalized-managed.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。
