---
title: "在虚拟机规模集上部署应用"
description: "使用扩展在 Azure 虚拟机规模集上部署应用。"
services: virtual-machine-scale-sets
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/26/2017
ms.date: 07/24/2017
ms.author: v-dazen
ms.openlocfilehash: 4d57589d651d312ebcc066ec3297b5e1b0efc2cd
ms.sourcegitcommit: d5d647d33dba99fabd3a6232d9de0dacb0b57e8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>在虚拟机规模集上部署应用程序

本文介绍在预配规模集时安装软件的不同方式。

我们建议查看[规模集设计概述](virtual-machine-scale-sets-design-overview.md)一文，其中介绍了虚拟机规模集存在的一些限制。

## <a name="capture-and-reuse-an-image"></a>捕获和重用映像

可以使用 Azure 中的虚拟机为规模集准备基本映像。 此过程在存储帐户中创建一个托管磁盘，你可以将此磁盘引用为规模集的基本映像。 

执行以下步骤：

1. 创建 Azure 虚拟机
   * [Linux][linux-vm-create]
   * [Windows][windows-vm-create]

2. 远程连接到虚拟机，并根据自己的偏好自定义系统。

   如果需要，可以立即安装应用程序。 但请注意，如果现在就安装应用程序，则今后的应用程序升级就会变得更复杂，因为需要先删除应用程序。 可以使用此步骤安装应用程序可能需要的任何必备组件，例如特定的运行时或操作系统功能。

3. 请遵循适用于 [Linux][linux-vm-capture] 或 [Windows][windows-vm-capture] 的“捕获计算机”教程。

4. 使用在上一步骤中捕获的映像 URI 创建[虚拟机规模集][vmss-create]。

有关磁盘的详细信息，请参阅[托管磁盘概述](../storage/storage-managed-disks-overview.md)和[使用附加的数据磁盘](virtual-machine-scale-sets-attached-disks.md)。

## <a name="install-when-the-scale-set-is-provisioned"></a>预配规模集时安装

可将虚拟机扩展应用到虚拟机规模集。 使用虚拟机扩展可以在规模集中以整个组的形式自定义虚拟机。 有关扩展的详细信息，请参阅[虚拟机扩展](../virtual-machines/windows/extensions-features.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。

根据操作系统是基于 Linux 还是基于 Windows，可以使用三个主要扩展。

### <a name="windows"></a>Windows

对于基于 Windows 的操作系统，可以使用“自定义脚本 v1.8”扩展或“PowerShell DSC”扩展。

#### <a name="custom-script"></a>自定义脚本

“自定义脚本”扩展在规模集中的每个虚拟机实例上运行脚本。 配置文件或变量指示要将哪些文件下载到虚拟机，然后要运行哪个命令。 例如，可以使用此扩展来运行安装程序、脚本、批处理文件或任何可执行文件。

PowerShell 为设置使用哈希表。 此示例将配置自定义脚本扩展，以运行一个用于安装 IIS 的 PowerShell 脚本。

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>对于可能包含敏感信息的任何设置，请使用 `-ProtectedSetting` 开关。

---------

Azure CLI 为设置使用 json 文件。 此示例将配置自定义脚本扩展，以运行一个用于安装 IIS 的 PowerShell 脚本。 将以下 json 文件另存为 _settings.json_。

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

然后，运行以下 Azure CLI 命令。

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>对于可能包含敏感信息的任何设置，请使用 `--protected-settings` 开关。

### <a name="powershell-dsc"></a>PowerShell DSC

可以使用 PowerShell DSC 来自定义规模集 VM 实例。 **Microsoft.Powershell** 发布的 **DSC** 扩展在每个虚拟机实例上部署并运行提供的 DSC 配置。 配置文件或变量向扩展告知 *.zip* 包所在的位置，以及要运行的 _script-function_ 组合。

PowerShell 为设置使用哈希表。 此示例部署用于安装 IIS 的 DSC 包。

```powershell
# Setup extension configuration hashtable variable
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>对于可能包含敏感信息的任何设置，请使用 `-ProtectedSetting` 开关。

-----------

Azure CLI 为设置使用 json。 此示例部署用于安装 IIS 的 DSC 包。 将以下 json 文件另存为 _settings.json_。

```json
{
  "wmfVersion": "latest",
  "configuration": {
    "url": "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip",
    "script": "configure-http.ps1",
    "function": "WebsiteTest"
  }
}
```

然后，运行以下 Azure CLI 命令。

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>对于可能包含敏感信息的任何设置，请使用 `--protected-settings` 开关。

### <a name="linux"></a>Linux

在创建期间，Linux 可以使用“自定义脚本 v2.0”扩展或“cloud-init”。

自定义脚本是一个简单的扩展，可将文件下载到虚拟机实例并运行命令。

#### <a name="custom-script"></a>自定义脚本

将以下 json 文件另存为 _settings.json_。

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

使用 Azure CLI 将此扩展添加到现有的虚拟机规模集。 规模集中的每个虚拟机将自动运行该扩展。

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>对于可能包含敏感信息的任何设置，请使用 `--protected-settings` 开关。

#### <a name="cloud-init"></a>Cloud-Init

创建规模集时使用 Cloud-Init。 首先，创建名为 _cloud-init.txt_ 的本地文件并在其中添加配置。 有关示例，请参阅[此要点主题](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)

使用 Azure CLI 创建规模集。 `--custom-data` 字段接受 cloud-init 脚本的文件名。

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

## <a name="how-do-i-manage-application-updates"></a>如何管理应用程序更新？

如果通过扩展部署了应用程序，请以某种方式更改扩展定义。 这种更改会导致将该扩展重新部署到所有虚拟机实例。 对于扩展，**必须**在某些方面做出更改（例如，重命名引用的文件），否则 Azure 不知道该扩展已更改。

如果已将应用程序融入自己的操作系统映像后，请使用自动化的部署管道进行应用程序更新。 请合理设计体系结构，以帮助将分阶段的规模集快速切换到生产环境。 [Azure Spinnaker 驱动程序的工作原理](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure)很好地示范了这种方法 - [http://www.spinnaker.io/](http://www.spinnaker.io/)。

[Packer](https://www.packer.io/) 和 [Terraform](https://www.terraform.io/) 支持 Azure Resource Manager，因此也可以“代码方式”定义映像并在 Azure 中将其生成，然后在规模集中使用 VHD。 但是，这种做法会给应用商店映像造成问题，在这种情况下，扩展/自定义脚本变得更加重要，因为无法直接在应用商店中处理代码。

## <a name="what-happens-when-a-scale-set-scales-out"></a>扩展规模集时会发生什么情况？
将一个或多个虚拟机添加到规模集时，会自动安装应用程序。 例如，如果规模集包含定义的扩展，则每次创建新虚拟机时，这些扩展都会在该虚拟机上运行。 如果规模集基于自定义映像，所有新虚拟机都是自定义源映像的副本。 如果规模集虚拟机是容器主机，则你可以在自定义脚本扩展中使用启动代码来加载容器。

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>如何跨更新域推出 OS 更新？
假设要在更新 OS 映像的同时让虚拟机规模集持续运行。 PowerShell 和 Azure CLI 可以逐个更新虚拟机上的映像。 [升级虚拟机规模集](./virtual-machine-scale-sets-upgrade-scale-set.md)一文也进一步介绍了可用于跨虚拟机规模集执行操作系统升级的选项。

## <a name="next-steps"></a>后续步骤

* [使用 PowerShell 管理规模集](virtual-machine-scale-sets-windows-manage.md)。
* [创建规模集模板](virtual-machine-scale-sets-mvss-start.md)。

[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md
