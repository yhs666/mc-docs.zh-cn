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
ms.date: 08/12/2019
ms.author: v-yeche
ms.openlocfilehash: dc9fe83db254bd350bdf11a6e425d59c367563a6
ms.sourcegitcommit: d624f006b024131ced8569c62a94494931d66af7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69538971"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>跨 Azure 租户共享库 VM 映像

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

> [!IMPORTANT]
> 不能使用门户从另一个 Azure 租户中的映像部署 VM。 若要从租户之间共享的映像创建 VM，必须使用 [Azure CLI](../linux/share-images-across-tenants.md) 或 Powershell。

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

<!--Update_Description: wording update -->
