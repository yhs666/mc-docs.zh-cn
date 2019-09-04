---
title: 创建存储帐户 - Azure 存储
description: 本操作指南文章介绍如何使用 Azure 门户、Azure PowerShell 或 Azure CLI 创建存储帐户。 Azure 存储帐户在 Azure 中提供唯一的命名空间，用于存储和访问在 Azure 存储中创建的数据对象。
services: storage
author: WenJason
ms.custom: mvc
ms.service: storage
ms.topic: article
origin.date: 06/28/2019
ms.date: 09/09/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: 4bad6237eb58f9910b699fb04b78e23e382f0ac2
ms.sourcegitcommit: 66a77af2fab8a5f5b34723dc99e4d7ce0c380e78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2019
ms.locfileid: "70209374"
---
# <a name="create-a-storage-account"></a>创建存储帐户

Azure 存储帐户包含所有的 Azure 存储数据对象：Blob、文件、队列、表和磁盘。 存储帐户为你的 Azure 存储数据提供了一个唯一的命名空间，可以从世界上的任何位置通过 HTTP 或 HTTPS 访问该命名空间。 Azure 存储帐户中的数据是持久的，高度可用、安全且可大规模缩放。

本操作指南文章介绍如何使用 [Azure 门户](https://portal.azure.cn/)、[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) 或 [Azure CLI](/cli/?view=azure-cli-latest) 创建存储帐户。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

无。

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

本操作指南文章需要 Azure PowerShell 模块 Az 0.7 或更高版本。 运行 `Get-Module -ListAvailable Az` 即可查找当前版本。 如果需要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-Az-ps)。

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

可以安装 CLI 并在本地运行 CLI 命令。

### <a name="install-the-cli-locally"></a>在本地安装 CLI

也可在本地安装和使用 Azure CLI。 本操作指南文章要求运行 Azure CLI 2.0.4 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。 

# <a name="templatetabtemplate"></a>[模板](#tab/template)

无。

---

## <a name="sign-in-to-azure"></a>登录 Azure

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

登录到 [Azure 门户](https://portal.azure.cn)。

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

使用 `Connect-AzAccount` 命令登录到 Azure 订阅，然后按照屏幕上的说明进行身份验证。

```powershell
Connect-AzAccount -Environment AzureChinaCloud
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

若要登录到本地安装的 CLI，请运行 [az login](/cli/reference-index#az-login) 命令：

```cli
az cloud set -n AzureChinaCloud
az login
```

# <a name="templatetabtemplate"></a>[模板](#tab/template)

不适用

---

## <a name="create-a-storage-account"></a>创建存储帐户

现在可以创建存储帐户。

每个存储帐户都必须属于 Azure 资源组。 资源组是对 Azure 资源进行分组的逻辑容器。 在创建存储帐户时，可以选择创建新的资源组，也可以使用现有资源组。 本文介绍如何创建新资源组。

可以使用常规用途 v2 存储帐户访问所有 Azure 存储服务：Blob、文件、队列、表和磁盘  。 本文所述的步骤将创建常规用途 v2 存储帐户，但创建任何类型的存储帐户的步骤都相似。

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

首先，使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 命令，通过 PowerShell 创建新的资源组： 

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hard-coding it repeatedly
$resourceGroup = "storage-resource-group"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

如果不确定为 `-Location` 参数指定哪个区域，可使用 [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) 命令检索订阅支持的区域的列表：

```powershell
Get-AzLocation | select Location
$location = "chinaeast"
```

接下来，通过 [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/New-azStorageAccount) 命令创建使用读取访问异地冗余存储 (RA-GRS) 的常规用途 v2 存储帐户。 请记住，存储帐户的名称必须在 Azure 中唯一，因此请将括号中的占位符值替换为你自己的唯一值：

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name <account-name> `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2 
```

若要使用不同的复制选项创建常规用途 v2 存储帐户，请将 **SkuName** 参数替换为下表中的所需值。

|复制选项  |SkuName 参数  |
|---------|---------|
|本地冗余存储 (LRS)     |Standard_LRS         |
|异地冗余存储 (GRS)     |Standard_GRS         |
|读取访问异地冗余存储 (GRS)     |Standard_RAGRS         |

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

首先，使用 [az group create](/cli/group#az_group_create) 命令，通过 Azure CLI 创建新的资源组。 

```azurecli
az group create \
    --name storage-resource-group \
    --location chinaeast
```

如果不确定为 `--location` 参数指定哪个区域，可使用 [az account list-locations](/cli/account#az_account_list) 命令检索订阅支持的区域的列表。

```azurecli
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

接下来，通过 [az storage account create](/cli/storage/account#az_storage_account_create) 命令创建使用读取访问异地冗余存储的常规用途 v2 存储帐户。 请记住，存储帐户的名称必须在 Azure 中唯一，因此请将括号中的占位符值替换为你自己的唯一值：

```azurecli
az storage account create \
    --name <account-name> \
    --resource-group storage-resource-group \
    --location chinaeast \
    --sku Standard_RAGRS \
    --kind StorageV2
```

若要使用不同的复制选项创建常规用途 v2 存储帐户，请将 **sku** 参数替换为下表中的所需值。

|复制选项  |sku 参数  |
|---------|---------|
|本地冗余存储 (LRS)     |Standard_LRS         |
|异地冗余存储 (GRS)     |Standard_GRS         |
|读取访问异地冗余存储 (GRS)     |Standard_RAGRS         |
# <a name="templatetabtemplate"></a>[模板](#tab/template)

可以使用 Azure Powershell 或 Azure CLI 来部署资源管理器模板以创建存储帐户。 本操作指南文章中使用的模板来自 [Azure 资源管理器快速入门模板](https://azure.microsoft.com/resources/templates/101-storage-account-create/)。 若要运行脚本，请选择“试用”  打开 Azure Cloud shell。 若要粘贴脚本，请右键单击 shell，然后选择“粘贴”  。

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. chinaeast)"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. chinaeast):" &&
read location &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-file "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

若要了解如何创建模板，请参阅：

- [Azure 资源管理器文档](/azure-resource-manager/)。
- [存储帐户模板参考](https://docs.microsoft.com/azure/templates/microsoft.storage/allversions)。

---

有关可用的复制选项的详细信息，请参阅[存储复制选项](storage-redundancy.md)。

## <a name="clean-up-resources"></a>清理资源

若要清理本操作指南文章创建的资源，可以删除资源组。 删除资源组也会删除相关联的存储帐户，以及与资源组相关联的任何其他资源。

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

若要使用 Azure 门户删除资源组，请执行以下操作：

1. 在 Azure 门户中展开左侧的菜单，打开服务菜单，然后选择“资源组”以显示资源组的列表。 
2. 找到要删除的资源组，右键单击列表右侧的“更多”按钮 ( **...** )。 
3. 选择“删除资源组”并进行确认。 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 命令： 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [az group delete](/cli/group#az_group_delete) 命令。

```azurecli
az group delete --name storage-resource-group
```

# <a name="templatetabtemplate"></a>[模板](#tab/template)

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 Azure PowerShell 或 Azure CLI。

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

---

## <a name="next-steps"></a>后续步骤

在本操作指南文章中，你已创建一个常规用途 v2 标准存储帐户。 若要了解如何通过存储帐户上传和下载 Blob，请继续阅读 Blob 存储快速入门之一。

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

> [!div class="nextstepaction"]
> [通过 Azure 门户使用 Blob](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [通过 PowerShell 使用 Blob](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [通过 Azure CLI 使用 Blob](../blobs/storage-quickstart-blobs-cli.md)

# <a name="templatetabtemplate"></a>[模板](#tab/template)

> [!div class="nextstepaction"]
> [通过 Azure 门户使用 Blob](../blobs/storage-quickstart-blobs-portal.md)

---
