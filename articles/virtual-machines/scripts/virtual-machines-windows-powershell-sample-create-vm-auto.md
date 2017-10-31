---
title: "在 Azure 中使用 New-AzVM 创建 Windows VM | Azure"
description: "Azure PowerShell 脚本示例 - 使用 New-AzVM cmdlet 创建 Windows VM。"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 09/29/2017
ms.date: 10/30/2017
ms.author: v-yeche
ms.openlocfilehash: 77464038dd13bce84efe98ebc6d8c1bc984320fc
ms.sourcegitcommit: da3265de286410af170183dd1804d1f08f33e01e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="create-a-fully-configured-virtual-machine-with-the-new-azvm-powershell-cmdlet-preview"></a>使用 New-AzVM PowerShell cmdlet（预览版）创建完全配置的虚拟机

此脚本使用 Cloud Shell 中的 New-AzVM cmdlet 创建运行 Windows Server 2016 的 Azure 虚拟机。 运行脚本后，可通过 RDP 访问虚拟机。 Cloud Shell 具有包含默认安装的 New-AzVM 的模块，便于用户更轻松地使用新功能。

此示例使用 New-AzVM cmdlet（预览版）。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<!--Not Available [!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]-->

## <a name="sample-script"></a>示例脚本

```powershell
New-AzVM -Name myVM `
    -VirtualNetworkName myVNet `
    -Location chinanorth `
    -SecurityGroupName myNSG `
    -PublicIpAddressName myPIP 
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](../windows/powershell-samples.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。
<!--Update_Description: new articles on creat VM auto-->
