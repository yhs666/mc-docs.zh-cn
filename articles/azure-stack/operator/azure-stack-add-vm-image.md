---
title: 将自定义 VM 映像添加到 Azure Stack | Microsoft Docs
description: 了解如何在 Azure Stack 中添加或删除自定义 VM 映像。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
origin.date: 10/16/2019
ms.date: 11/18/2019
ms.author: v-jay
ms.reviewer: kivenkat
ms.lastreviewed: 06/08/2018
ms.openlocfilehash: cb11019fcace47e903702cdeeb9e5b70e95b5cb6
ms.sourcegitcommit: 7dfb76297ac195e57bd8d444df89c0877888fdb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020048"
---
# <a name="add-a-custom-vm-to-azure-stack"></a>将自定义 VM 添加到 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

在 Azure Stack 中，可将虚拟机 (VM) 自定义映像添加到市场供用户使用。 可以使用 Azure Stack 的 Azure 资源管理器模板添加 VM 映像。 还可以使用管理员门户或 Windows PowerShell 将 VM 映像作为市场项添加到 Azure 市场 UI。 可以使用来自 Azure 市场的映像，也可以使用你自己的自定义 VM 映像。

## <a name="generalize-the-vm-image"></a>通用化 VM 映像

### <a name="windows"></a>Windows

创建自定义的通用化 VHD。 如果 VHD 来自 Azure 外部，请遵循[上传通用化 VHD 并使用它在 Azure 中创建新 VM](/virtual-machines/windows/upload-generalized-managed) 中的步骤，正确地对 VHD 执行 **Sysprep** 并使其通用化。

如果 VHD 来自 Azure，请先根据[使用 Sysprep 将源 VM 通用化](/virtual-machines/windows/upload-generalized-managed#generalize-the-source-vm-by-using-sysprep)中的说明操作，然后将 VHD 移植到 Azure Stack。

### <a name="linux"></a>Linux

如果 VHD 来自 Azure 外部，请根据相应的说明将 VHD 通用化：

- [基于 CentOS 的分发版](/virtual-machines/linux/create-upload-centos?toc=%2fvirtual-machines%2flinux%2ftoc.json)
- [Debian Linux](/virtual-machines/linux/debian-create-upload-vhd?toc=%2fvirtual-machines%2flinux%2ftoc.json)
- [Red Hat Enterprise Linux](/azure-stack/azure-stack-redhat-create-upload-vhd)
- [SLES 或 openSUSE](/virtual-machines/linux/suse-create-upload-vhd?toc=%2fvirtual-machines%2flinux%2ftoc.json)
- [Ubuntu Server](/virtual-machines/linux/create-upload-ubuntu?toc=%2fvirtual-machines%2flinux%2ftoc.json)

如果 VHD 来自 Azure，请根据以下说明将 VHD 通用化：

1. 停止 **waagent** 服务：

   ```bash
   # sudo waagent -force -deprovision
   # export HISTSIZE=0
   # logout
   ```

2. 关闭 VM 并下载 VHD。 如果要从 Azure 导入 VHD，可以使用磁盘导出来执行此操作，如[从 Azure 下载 Windows VHD](/virtual-machines/windows/download-vhd) 中所述。

请注意哪些 Azure Linux 代理版本适用于 Azure Stack，[此处有具体的说明](azure-stack-linux.md#azure-linux-agent)。 确保已执行 Sysprep 的映像包含与 Azure Stack 兼容的 Azure Linux 代理版本。

### <a name="common-steps-for-both-windows-and-linux"></a>同时适用于 Windows 和 Linux 的通用步骤

在上传映像之前，必须考虑以下因素：

- Azure Stack 仅支持采用固定磁盘 VHD 格式的第一代 VM。 固定格式在文件中将逻辑磁盘以线性方式布局，使磁盘偏移量 *X* 存储在 Blob 偏移量 *X* 的位置。在 Blob 末尾有一小段脚注，描述了 VHD 的属性。 若要确认磁盘是否为固定格式，请使用 **Get-VHD** PowerShell cmdlet。

- Azure Stack 不支持动态磁盘 VHD。 重设附加到 VM 的动态磁盘的大小会导致 VM 处于故障状态。 若要解决此问题，请删除 VM，但不删除 VM 的磁盘（存储帐户中的 VHD Blob）。 然后将 VHD 从动态磁盘转换为固定磁盘，接着重新创建 VM。

## <a name="add-a-vm-image-as-an-azure-stack-operator-using-the-portal"></a>以 Azure Stack 操作员的身份使用门户添加 VM 映像

1. 映像必须能够通过 Blob 存储 URI 进行引用。 以 VHD（不是 VHDX）格式准备 Windows 或 Linux 操作系统映像，然后将映像上传到 Azure 或 Azure Stack 中的存储帐户。

   - 如果 VHD 位于 Azure 中，且你在联网的 Azure Stack 上运行，可以使用 [Azcopy](/storage/common/storage-use-azcopy) 等工具直接在 Azure 与 Azure Stack 存储帐户之间传输 VHD。

   - 在离线的 Azure Stack 上，如果 VHD 位于 Azure 中，则需要将 VHD 下载到可以连接 Azure 和 Azure Stack 的计算机上。 然后，先从 Azure 将 VHD 复制到此计算机，再使用任何可跨 Azure 与 Azure Stack 使用的通用[存储数据传输工具](../user/azure-stack-storage-transfer.md)，将 VHD 传输到 Azure Stack。

2. 可以遵循[此示例](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd?view=azurermps-6.13.0)将 VHD 上传到 Azure Stack 管理员门户中的存储帐户。

   - 将映像上传到 Azure Stack Blob 存储比上传到 Azure Blob 存储更高效，因为将映像推送到 Azure Stack 映像存储库的时间更短。

   - 上传 [Windows VM 映像](/virtual-machines/windows/upload-generalized-managed)时，请确保将“登录到 Azure”步骤切换为  [配置 Azure Stack 操作员的 PowerShell 环境](azure-stack-powershell-configure-admin.md)步骤。  

3. 记下在其中上传映像的 Blob 存储 URI。 Blob 存储 URI 的格式如下： *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;* .vhd。

4. 若要使 Blob 可供匿名访问，请转到已在其中上传了 VM 映像 VHD 的存储帐户 Blob 容器。 选择“Blob”  ，然后选择“访问策略”  。 也可生成容器的共享访问签名，然后将其作为 Blob URI 的一部分包括进去。 此步骤确保 blob 可用。 如果 blob 不可匿名访问，则 VM 映像创建将处于失败状态。

   ![转到存储帐户 Blob](./media/azure-stack-add-vm-image/image1.png)

   ![将 Blob 访问权限设置为公共](./media/azure-stack-add-vm-image/image2.png)

5. 以操作员身份登录到 Azure Stack。 在菜单中的**计算** >   “添加”下，选择“所有服务”   >   “映像”。

6. 在“创建映像”下，  输入名称、订阅、资源组、位置、OS 磁盘、OS 类型、存储 Blob URI、帐户类型和主机缓存。 然后选择“创建”，开始创建 VM 映像。 

   ![开始创建映像](./media/azure-stack-add-vm-image/image4.png)

   成功创建映像后，VM 映像状态会更改为“已成功”。 

7. 添加映像时，它仅适用于基于 Azure 资源管理器的模板和 PowerShell 部署。 若要将映像作为市场项提供给用户，请使用[创建和发布市场项](azure-stack-create-and-publish-marketplace-item.md)一文中的步骤发布市场项 请务必记下“发布者”、“套餐”、“SKU”和“版本”的值。     在自定义 .azpkg 中编辑资源管理器模板和 Manifest.json 时，需要用到这些值。

## <a name="remove-the-vm-image-as-an-azure-stack-operator-using-the-portal"></a>以 Azure Stack 操作员的身份使用门户删除 VM 映像

1. 打开 Azure Stack [管理员门户](https://adminportal.local.azurestack.external)。

2. 如果 VM 映像有关联的市场项，请选择“市场管理”，然后选择要删除的 VM 市场项。 

3. 如果 VM 映像没有关联的市场项，请导航到“所有服务”>“计算”>“VM 映像”，然后选择 VM 映像旁边的省略号 ( **...** )。 

4. 选择“删除”  。

## <a name="add-a-vm-image-as-an-azure-stack-operator-using-powershell"></a>以 Azure Stack 操作员的身份使用 PowerShell 添加 VM 映像

1. [安装适用于 Azure Stack 的 PowerShell](azure-stack-powershell-install.md)。  

2. 以操作员身份登录到 Azure Stack。 有关说明，请参阅[以操作员身份登录到 Azure Stack](azure-stack-powershell-configure-admin.md)。

3. 使用权限提升的提示符打开 PowerShell，并运行：

   ```powershell
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<Offer>" `
      -sku "<SKU>" `
      -version "<#.#.#>" `
      -OSType "<OS type>" `
      -OSUri "<OS URI>"
   ```

   **Add-AzsPlatformimage** cmdlet 指定 Azure 资源管理器模板用来引用 VM 映像的值。 这些值包括：
   - **publisher**  
     例如： `Canonical`  
     VM 映像的**发布者**名称段，供用户在部署映像时使用。 不要在此字段中包含空格或其他特殊字符。  
   - **offer**  
     例如： `UbuntuServer`  
     VM 映像的**套餐**名称段，供用户在部署 VM 映像时使用。 不要在此字段中包含空格或其他特殊字符。  
   - **sku**  
     例如： `14.04.3-LTS`  
     VM 映像的 **SKU** 名称段，供用户在部署 VM 映像时使用。 不要在此字段中包含空格或其他特殊字符。  
   - **version**  
     例如： `1.0.0`  
     VM 映像的版本，供用户在部署 VM 映像时使用。 此版本采用 *\#.\#.\#* 格式。 不要在此字段中包含空格或其他特殊字符。  
   - **osType**  
     例如： `Linux`  
     映像的 **osType** 必须为 **Windows** 或 **Linux**。  
   - **OSUri**  
     例如： `https://storageaccount.blob.core.chinacloudapi.cn/vhds/Ubuntu1404.vhd`  
     可以指定 `osDisk` 的 Blob 存储 URI。  

     有关详细信息，请参阅 [Add-AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) 和 [New-DataDiskObject](https://docs.microsoft.com/powershell/module/Azs.Compute.Admin/New-DataDiskObject) cmdlet 的 PowerShell 参考。

## <a name="remove-a-vm-image-as-an-azure-stack-operator-using-powershell"></a>以 Azure Stack 操作员的身份使用 PowerShell 删除 VM 映像

不再需要上传的 VM 映像时，可使用以下 cmdlet 从市场中删除它：

1. [安装适用于 Azure Stack 的 PowerShell](azure-stack-powershell-install.md)。

2. 以操作员身份登录到 Azure Stack。

3. 使用权限提升的提示符打开 PowerShell，并运行：

   ```powershell  
   Remove-AzsPlatformImage `
    -publisher "<Publisher>" `
    -offer "<Offer>" `
    -sku "<SKU>" `
    -version "<Version>" `
   ```

   **Remove-AzsPlatformImage** cmdlet 指定 Azure 资源管理器模板用来引用 VM 映像的值。 这些值包括：
   - **publisher**  
     例如： `Canonical`  
     VM 映像的**发布者**名称段，供用户在部署映像时使用。 不要在此字段中包含空格或其他特殊字符。  
   - **offer**  
     例如： `UbuntuServer`  
     VM 映像的**套餐**名称段，供用户在部署 VM 映像时使用。 不要在此字段中包含空格或其他特殊字符。  
   - **sku**  
     例如： `14.04.3-LTS`  
     VM 映像的 **SKU** 名称段，供用户在部署 VM 映像时使用。 不要在此字段中包含空格或其他特殊字符。  
   - **version**  
     例如： `1.0.0`  
     VM 映像的版本，供用户在部署 VM 映像时使用。 此版本采用 *\#.\#.\#* 格式。 不要在此字段中包含空格或其他特殊字符。  

     有关 **Remove-AzsPlatformImage** cmdlet 的详细信息，请参阅 Azure PowerShell [Azure Stack 操作员模块文档](https://docs.microsoft.com/powershell/module/)。

## <a name="next-steps"></a>后续步骤

- [创建并发布自定义 Azure Stack 市场项](azure-stack-create-and-publish-marketplace-item.md)
