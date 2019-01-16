---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/26/2018
ms.date: 01/14/2019
ms.author: v-jay
ms.openlocfilehash: 4f2aa2c4da60ba72948c6fe2eacf3d38f47b9deb
ms.sourcegitcommit: 5eff40f2a66e71da3f8966289ab0161b059d0263
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54192938"
---
## <a name="sign-in-to-azure"></a>登录 Azure

运行 `Connect-AzureRmAccount -EnvironmentName AzureChinaCloud` 命令以登录 Azure 订阅，并按照屏幕上的说明操作。

```powershell
Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
```

如果你不知道要使用哪个位置，可以列出可用的位置。 使用以下代码示例显示位置列表，并找到要使用的位置。 此示例使用“chinaeast”。 将位置存储在变量中，并使用该变量，这样就可以在一个位置更改它。

```powershell
Get-AzureRmLocation | select Location
$location = "China East"
```

## <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>创建存储帐户

使用 [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount) 创建具有 LRS 复制功能的标准常规用途存储帐户。 接下来，获取用于定义要使用的存储帐户的存储帐户上下文。 对存储帐户执行操作时，引用上下文而不是重复传入凭据。 使用以下示例创建一个名为 mystorageaccount 的存储帐户，该帐户默认启用本地冗余存储 (LRS) 和 Blob 加密。

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -SkuName Standard_LRS `
  -Location $location `

$ctx = $storageAccount.Context
```
