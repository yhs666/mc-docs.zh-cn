---
title: 上传通用化 VHD 并使用它在 Azure 中创建新 VM | Azure
description: 将通用化 VHD 上传到 Azure，然后在 Resource Manager 部署模型中使用它来创建新的 VM。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
origin.date: 09/25/2018
ms.date: 11/11/2019
ms.author: v-yeche
ms.openlocfilehash: cc9c532cafca1f116c262925446843e4c22bf09f
ms.sourcegitcommit: 1fd822d99b2b487877278a83a9e5b84d9b4a8ce7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2019
ms.locfileid: "74116874"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>上传通用化 VHD 并使用它在 Azure 中创建新 VM

本文逐步讲解如何使用 PowerShell 将通用化 VM 的 VHD 上传到 Azure、从该 VHD 创建映像，然后从该映像创建新 VM。 可以上传从本地虚拟化工具或其他云导出的 VHD。 对新的 VM 使用[托管磁盘](managed-disks-overview.md)可以简化 VM 管理，在将 VM 置于可用性集中时提供更好的可用性。 

有关示例脚本，请参阅[将 VHD 上传到 Azure 并创建新的 VM 的示例脚本](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)。

## <a name="before-you-begin"></a>准备阶段

- 将任何 VHD 上传到 Azure 之前，应该遵循[准备要上传到 Azure 的 Windows VHD 或 VHDX](prepare-for-upload-vhd-image.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中的步骤操作。
- 开始迁移到[托管磁盘](managed-disks-overview.md)之前，请先查看[规划迁移到托管磁盘](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks)。

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="generalize-the-source-vm-by-using-sysprep"></a>使用 Sysprep 通用化源 VM

Sysprep 将删除所有个人帐户信息及其他某些数据，并准备好要用作映像的计算机。 有关 Sysprep 的详细信息，请参阅 [Sysprep 概述](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)。

确保 Sysprep 支持计算机上运行的服务器角色。 有关详细信息，请参阅 [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)（Sysprep 对服务器角色的支持）。

> [!IMPORTANT]
> 如果计划在首次将 VHD 上传到 Azure 之前运行 Sysprep，请确保先[准备好 VM](prepare-for-upload-vhd-image.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。 
> 
> 

1. 登录到 Windows 虚拟机。
2. 以管理员身份打开“命令提示符”窗口。 将目录切换到 %windir%\system32\sysprep，然后运行 `sysprep.exe`。
3. 在“系统准备工具”对话框中，选择“进入系统全新体验(OOBE)”，确保已选中“通用化”复选框。   
4. 在“关机选项”  中选择“关机”  。
5. 选择“确定”  。

    ![启动 Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. 在 Sysprep 完成时，它会关闭虚拟机。 不要重新启动 VM。

## <a name="upload-the-vhd-to-your-storage-account"></a>将 VHD 上传到存储帐户

现在可以直接将 VHD 上传到托管磁盘中。 有关说明，请参阅[使用 Azure PowerShell 将 VHD 上传到 Azure](disks-upload-vhd-to-managed-disk-powershell.md)。

## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>通过上传的 VHD 创建托管映像 

从通用 OS 托管磁盘创建托管映像。 将以下值替换为自己的信息。

首先设置一些参数：

```powershell
$location = "China East" 
$imageName = "myImage"
```

使用通用 OS VHD 创建映像。

```powershell
$imageConfig = New-AzImageConfig `
   -Location $location
$imageConfig = Set-AzImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```

## <a name="create-the-vm"></a>创建 VM

现在，你已有了一个映像，可以从该映像创建一个或多个新 VM。 本示例从 *myResourceGroup* 中的 *myImage* 创建名为 *myVM* 的 VM。

```powershell
New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -ImageName $imageName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```

## <a name="next-steps"></a>后续步骤

登录新虚拟机。 有关详细信息，请参阅 [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)（如何连接并登录到运行 Windows 的 Azure 虚拟机）。

<!-- Update_Description: update meta properties, wording update, update link -->