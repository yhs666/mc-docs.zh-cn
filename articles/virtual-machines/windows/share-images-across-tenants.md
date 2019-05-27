---
title: 在 Azure 中跨租户共享库映像 | Azure
description: 了解如何使用共享映像库跨 Azure 租户共享 VM 映像。
services: virtual-machines-windows
author: rockboyfor
manager: digimobile
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
origin.date: 04/05/2019
ms.date: 05/20/2019
ms.author: v-yeche
ms.openlocfilehash: 36d203396413ceb427b2fe3e6aa658626a815f48
ms.sourcegitcommit: 878a2d65e042b466c083d3ede1ab0988916eaa3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835817"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>跨 Azure 租户共享库 VM 映像

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

## <a name="create-a-vm-using-powershell"></a>使用 PowerShell 创建 VM

使用应用程序 ID、机密和租户 ID 登录到两个租户中。 

```powershell
$applicationId = '<App ID>'
$secret = <Secret> | ConvertTo-SecureString -AsPlainText -Force
$tenant1 = "<Tenant 1 ID>"
$tenant2 = "<Tenant 2 ID>"
$cred = New-Object -TypeName PSCredential -ArgumentList $applicationId, $secret
Clear-AzContext
Connect-AzAccount -Environment AzureChinaCloud -ServicePrincipal -Credential $cred  -Tenant "<Tenant 1 ID>"
Connect-AzAccount -Environment AzureChinaCloud -ServicePrincipal -Credential $cred -Tenant "<Tenant 2 ID>"
```

在有应用注册权限的资源组中创建 VM。 请将此示例中的信息替换为你自己的。

```powershell
$resourceGroup = "myResourceGroup"
$image = "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>"
New-AzVm `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVMfromImage" `
   -Image $image `
   -Location "China East" `
   -VirtualNetworkName "myImageVnet" `
   -SubnetName "myImageSubnet" `
   -SecurityGroupName "myImageNSG" `
   -PublicIpAddressName "myImagePIP" `
   -OpenPorts 3389
```

<!--Not Available on ## Next steps-->

<!--Not Available on [Azure portal](shared-images-portal.md)-->

<!--Update_Description: new articles on share image accross tenants -->
<!--ms.date: 05/20/2019-->