---
title: "从通用化 Azure VM 捕获 VM 映像 | Azure"
description: "了解如何从 Resource Manager 部署模型中创建的通用化 Azure VM 捕获 VM 映像"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: afdae4a1-6dfb-47b4-902a-f327f9bfe5b4
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 02/15/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: 234c73733c7ecad9200056dae20cc86f161234f4
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 如何从通用化 Azure VM 捕获 VM 映像
<a id="how-to-capture-a-vm-image-from-a-generalized-azure-vm" class="xliff"></a>
本文说明如何使用 Azure PowerShell 创建通用化 Azure VM 的映像。 然后可以使用该映像来创建另一个 VM。 该映像包含 OS 磁盘和附加到虚拟机的数据磁盘。 该映像不包含虚拟网络资源，因此，创建新 VM 时需要设置这些资源。 

## 先决条件
<a id="prerequisites" class="xliff"></a>
需要安装 Azure PowerShell 1.0.x 版或更新版本。 如果尚未安装 PowerShell，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) 了解安装步骤。

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

还可以使用 `sudo waagent -deprovision+user` 通用化 Linux VM，然后使用 PowerShell 捕获该 VM。 有关使用 CLI 捕获 VM 的信息，请参阅[如何使用 Azure CLI 对 Linux 虚拟机进行通用化和捕获](../linux/capture-image.md)

## 登录到 Azure PowerShell
<a id="log-in-to-azure-powershell" class="xliff"></a>
1. 打开 Azure PowerShell 并登录到 Azure 帐户。

    ```powershell
    Login-AzureRmAccount -EnvironmentName AzureChinaCloud
    ```

    将打开一个弹出窗口，可以输入 Azure 帐户凭据。
2. 获取可用订阅的订阅 ID。

    ```powershell
    Get-AzureRmSubscription
    ```
3. 使用订阅 ID 设置正确的订阅。

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## 解除分配 VM 并将状态设置为通用化
<a id="deallocate-the-vm-and-set-the-state-to-generalized" class="xliff"></a>
1. 解除分配 VM 资源。

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    Azure 门户中该 VM 的“状态”将从“已停止”更改为“已停止(已解除分配)”。
2. 将虚拟机的状态设置为“通用化”。 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. 检查 VM 的状态。 VM 的“OSState/通用化”部分中的“DisplayStatus”应设置为“VM 已通用化”。  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## 创建映像
<a id="create-the-image" class="xliff"></a>
1. 使用以下命令将虚拟机映像复制到目标存储容器。 该映像在创建时所在的存储帐户与原始虚拟机的相同。 `-Path` 参数在本地保存 JSON 模板的副本。 `-DestinationContainerName` 参数是要在其中保存映像的容器的名称。 如果该容器不存在，系统将自动创建。

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    可以从 JSON 文件模板获取映像的 URL。 转到“资源” > “storageProfile” > “osDisk” > “映像” > “URI”部分即可查找映像的完整路径。 `https://<storageAccountName>.blob.core.chinacloudapi.cn/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`。

    也可以在门户中验证 URI。 映像将复制到存储帐户中名为 **system** 的容器。 

## 后续步骤
<a id="next-steps" class="xliff"></a>
* 现在即可[从映像创建 VM](create-vm-generalized.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。
