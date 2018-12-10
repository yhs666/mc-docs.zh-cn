---
title: 快速入门：创建存储帐户 - Azure | Azure 存储
description: 本快速入门介绍如何使用 Azure 门户、Azure PowerShell 或 Azure CLI 创建存储帐户。 Azure 存储帐户在 Azure 中提供唯一的命名空间，用于存储和访问在 Azure 存储中创建的数据对象。
services: storage
author: WenJason
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
origin.date: 09/18/2018
ms.date: 11/05/2018
ms.author: v-jay
ms.component: common
ms.openlocfilehash: 1fa366cf4df5a0ea28e83a16a0fde75227d7352c
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52645639"
---
# <a name="create-a-storage-account"></a>创建存储帐户

本快速入门介绍如何使用 [Azure 门户](https://portal.azure.cn/)、[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) 或 [Azure CLI](/cli/?view=azure-cli-latest) 创建存储帐户。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

# <a name="portaltabportal"></a>[Portal](#tab/portal)

无。

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

本快速入门需要 Azure PowerShell 模块 3.6 版或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找当前版本。 如果需要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

可以登录到 Azure，然后采用以下两种方式之一运行 Azure CLI 命令：

可以安装 CLI 并在本地运行 CLI 命令  

### <a name="install-the-cli-locally"></a>在本地安装 CLI

也可在本地安装和使用 Azure CLI。 本快速入门需要运行 Azure CLI 2.0.4 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。 

---

## <a name="log-in-to-azure"></a>登录 Azure

# <a name="portaltabportal"></a>[Portal](#tab/portal)

登录到 [Azure 门户](https://portal.azure.cn)。

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

使用 `Connect-AzureRmAccount` 命令登录到 Azure 订阅，然后按照屏幕上的说明进行身份验证。

```powershell
Connect-AzureRmAccount -Environment AzureChinaCloud
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

若要登录到本地安装的 CLI，请运行登录命令：

```cli
az cloud set -n AzureChinaCloud
az login
```

---

## <a name="create-a-storage-account"></a>创建存储帐户

现在可以创建存储帐户。

每个存储帐户都必须属于 Azure 资源组。 资源组是对 Azure 资源进行分组的逻辑容器。 在创建存储帐户时，可以选择创建新的资源组，也可以使用现有资源组。 本快速入门介绍了如何创建新资源组。 

可以使用常规用途 v2 存储帐户访问所有 Azure 存储服务：Blob、文件、队列、表和磁盘。 本快速入门创建常规用途 v2 存储帐户，但创建任何类型的存储帐户的步骤都相似。   

# <a name="portaltabportal"></a>[Portal](#tab/portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

首先，使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 命令，通过 PowerShell 创建新的资源组： 

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

如果不确定为 `-Location` 参数指定哪个区域，可使用 [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) 命令检索订阅支持的区域的列表：

```powershell
Get-AzureRmLocation | select Location 
$location = "chinaeast"
```

然后，创建具有本地冗余存储 (LRS) 的常规用途 v2 存储帐户。 使用 [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount) 命令： 

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 
```

若要使用异地冗余存储 (GRS) 或读取访问异地冗余存储 (RA-GRS) 创建常规用途 v2 存储帐户，请将 **SkuName** 参数替换为下表中的所需值。 

|复制选项  |SkuName 参数  |
|---------|---------|
|本地冗余存储 (LRS)     |Standard_LRS         |
|异地冗余存储 (GRS)     |Standard_GRS         |
|读取访问异地冗余存储 (GRS)     |Standard_RAGRS         |

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

首先，使用 [az group create](/cli/group#az_group_create) 命令，通过 Azure CLI 创建新的资源组。 

```azurecli
az group create \
    --name storage-quickstart-resource-group \
    --location chinaeast
```

如果不确定为 `--location` 参数指定哪个区域，可使用 [az account list-locations](/cli/account#az_account_list) 命令检索订阅支持的区域的列表。

```azurecli
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

然后，创建具有本地冗余存储的常规用途 v2 存储帐户。 使用 [az storage account create](/cli/storage/account#az_storage_account_create) 命令：

```azurecli
az storage account create \
    --name storagequickstart \
    --resource-group storage-quickstart-resource-group \
    --location chinaeast \
    --sku Standard_LRS \
    --kind StorageV2
```

若要使用异地冗余存储 (GRS) 或读取访问异地冗余存储 (RA-GRS) 创建常规用途 v2 存储帐户，请将 **sku** 参数替换为下表中的所需值。 

|复制选项  |sku 参数  |
|---------|---------|
|本地冗余存储 (LRS)     |Standard_LRS         |
|异地冗余存储 (GRS)     |Standard_GRS         |
|读取访问异地冗余存储 (GRS)     |Standard_RAGRS         |

---

有关可用的复制选项的详细信息，请参阅[存储复制选项](storage-redundancy.md)。

## <a name="clean-up-resources"></a>清理资源

若要清理本快速入门创建的资源，可以直接删除资源组。 删除资源组也会删除相关联的存储帐户，以及与资源组相关联的任何其他资源。

# <a name="portaltabportal"></a>[Portal](#tab/portal)

若要使用 Azure 门户删除资源组，请执行以下操作：

1. 在 Azure 门户中展开左侧的菜单，打开服务菜单，然后选择“资源组”以显示资源组的列表。
2. 找到要删除的资源组，右键单击列表右侧的“更多”按钮 (**...**)。
3. 选择“删除资源组”并进行确认。

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令： 

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [az group delete](/cli/group#az_group_delete) 命令。

```azurecli
az group delete --name myResourceGroup
```

---

## <a name="next-steps"></a>后续步骤

在本快速入门中，已创建一个通用的标准存储帐户。 若要了解如何通过存储帐户上传和下载 Blob，请继续阅读 Blob 存储快速入门。

# <a name="portaltabportal"></a>[Portal](#tab/portal)

> [!div class="nextstepaction"]
> [通过 Azure 门户使用 Blob](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

> [!div class="nextstepaction"]
> [通过 PowerShell 使用 Blob](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [通过 Azure CLI 使用 Blob 存储](../blobs/storage-quickstart-blobs-cli.md)

---
