---
title: include 文件
description: include 文件
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: include
origin.date: 04/23/2019
ms.date: 06/28/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: d77bc9e131e8d2ba450e40cb2e475b1771451026
ms.sourcegitcommit: ab87d30f4435c3b7c03f7edd33c9f374b7fe88c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2019
ms.locfileid: "67540090"
---
若要快速创建 VNet，可以在 Azure 本地 Shell 中打开 PowerShell 控制台。 请调整值，然后复制命令并将其粘贴到控制台窗口中。 

<!--Not Available on you can click "Try It" in this article to open a PowerShell console in Azure local Shell.-->

请务必确认创建的 VNet 的地址空间不与要连接到的其他 Vnet 的任何地址范围或者本地网络地址空间重叠。

### <a name="create-a-resource-group"></a>创建资源组

如果还没有想要使用的资源组，请创建一个新的资源组。 调整 PowerShell 命令以反映要使用的资源组名称，然后运行以下 cmdlet：

```powershell
New-AzResourceGroup -ResourceGroupName WANTestRG -Location ChinaNorth
```

### <a name="create-a-vnet"></a>创建 VNet

调整 PowerShell 命令以创建与环境兼容的 VNet。

```powershell
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location ChinaNorth `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```

<!--Update_Description: new articles on virtual wan tutorial vnet -->
<!--ms.date: 07/01/2019-->