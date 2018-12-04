---
title: 创建 Azure Data Lake Storage Gen2 预览版存储帐户 | Microsoft Docs
description: 快速学习使用 Azure 门户、Azure PowerShell 或 Azure CLI 创建能够访问 Data Lake Storage Gen2 预览版的一个新存储帐户。
services: storage
author: WenJason
ms.component: data-lake-storage-gen2
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
origin.date: 06/27/2018
ms.date: 11/05/2018
ms.author: v-jay
ms.openlocfilehash: 4ea25da821f238869516b35757bf58336ff29c07
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52643738"
---
# <a name="quickstart-create-an-azure-data-lake-storage-gen2-preview-storage-account"></a>快速入门：创建 Azure Data Lake Storage Gen2 预览版存储帐户

Azure Data Lake Storage Gen2 预览版帐户[支持分层命名空间服务](introduction.md)，该服务提供了一个适合与 Hadoop 分布式文件系统 (HDFS) 配合使用的基于原生目录的文件系统。 可以通过 [ABFS 驱动程序](abfs-driver.md)从 HDFS 访问 Data Lake Storage Gen2 数据。

若要在存储帐户上启用 Data Lake Storage Gen2 功能，请[填写预览版调查来请求访问](https://aka.ms/adlsgen2signup)。 得到批准后，你将能够创建新的 Data Lake Storage Gen2 帐户。 本快速入门展示了如何使用 [Azure 门户](https://portal.azure.cn/)、[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) 或通过 [Azure CLI](/cli/?view=azure-cli-latest) 创建帐户。

> [!NOTE]
> 当你获批创建 Data Lake Storage Gen2 帐户后，Azure 门户中的创建帐户 UI 将会更新。 同样，与 Data Lake Storage Gen2 相关的 PowerShell 和 CLI 参数只有在你获批使用此预览版后才起作用。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。

|           | 先决条件 |
|-----------|--------------|
|门户     | 无         |
|PowerShell | 本快速入门需要 Azure PowerShell 模块 **5.0.4-preview** 版或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找当前版本。 如果需要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。 |
|CLI        | 可以登录到 Azure，然后在 PowerShell 中运行 Azure CLI 命令 |

使用命令行时，可以选择在本地安装 CLI。

### <a name="install-the-cli-locally"></a>在本地安装 CLI

可在本地安装和使用 Azure CLI。 本快速入门需要运行 Azure CLI 2.0.38 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。

## <a name="overview-of-creating-an-azure-data-lake-storage-gen2-account"></a>创建 Azure Data Lake Storage Gen2 帐户概述

在创建帐户前，首先创建一个资源组，使其充当你创建的存储帐户或任何其他 Azure 资源的逻辑容器。 若要清理本快速入门创建的资源，可以直接删除资源组。 删除资源组也会删除相关联的存储帐户，以及与资源组相关联的任何其他资源。 有关资源组的详细信息，请参阅 [Azure 资源管理器概述](../../azure-resource-manager/resource-group-overview.md)。

> [!NOTE]
> 必须将新的存储帐户创建为 **StorageV2(常规用途 v2 )** 类型才能利用 Data Lake Storage Gen2 功能。  

有关存储帐户的详细信息，请参阅 [Azure 存储帐户概述](../common/storage-account-overview.md)。

为存储帐户命名时，请记住以下规则：

- 存储帐户名称必须为 3 到 24 个字符，并且只能包含数字和小写字母。
- 存储帐户名称在 Azure 中必须是唯一的。 没有两个存储帐户可以有相同的名称。

## <a name="create-an-account-using-the-azure-portal"></a>使用 Azure 门户创建帐户

登录到 [Azure 门户](https://portal.azure.cn)。

### <a name="create-a-resource-group"></a>创建资源组

若要在 Azure 门户中创建资源组，请执行以下步骤：

1. 在 Azure 门户中展开左侧的菜单，打开服务菜单，然后选择“资源组”。
2. 单击“添加”按钮添加新的资源组。
3. 输入新资源组的名称。
4. 选择要在其中创建新资源组的订阅。
5. 选择资源组的位置。
6. 单击“创建”  按钮。  

![显示 Azure 门户中资源组创建情况的屏幕截图](./media/quickstart-create-account/create-resource-group.png)

### <a name="create-a-general-purpose-v2-storage-account"></a>创建常规用途 v2 存储帐户

若要在 Azure 门户中创建常规用途 v2 存储帐户，请执行以下步骤：

> [!NOTE]
> 只有美国西部 2 和美国中西部启用了分层命名空间。 在创建存储帐户时，请确保指定这两个位置之一。

1. 在 Azure 门户中展开左侧的菜单，打开服务菜单，然后选择“所有服务”。 然后向下滚动到“存储”，接着选择“存储帐户”。 在显示的“存储帐户”窗口中，选择“添加”。
2. 输入存储帐户的名称。
3. 将“部署模型”保留设置为默认值。
4. 将“帐户类型”字段设置为“StorageV2 (常规用途 v2)”。
5. 将“位置”设置为“美国西部 2”。
6. 将“复制”字段保持设置为“本地冗余存储(LRS)”。
7. 将以下字段保留设置为其默认值：“复制”、 “性能”、“访问层”。
8. 选择要在其中创建存储帐户的订阅。
9. 在“资源组”部分选择“使用现有资源组”，然后选择在上一部分创建的资源组。
10. 保留“虚拟网络”的默认值。
11. 在“Data Lake Storage Gen2(预览版)”部分中，将“分层命名空间”设置为“已启用”。
12. 单击“创建”以创建存储帐户。

![显示 Azure 门户中存储帐户创建情况的屏幕截图](./media/quickstart-create-account/azure-data-lake-storage-account-create.png)

现在已通过门户创建了存储帐户。

### <a name="clean-up-resources"></a>清理资源

若要使用 Azure 门户删除资源组，请执行以下操作：

1. 在 Azure 门户中展开左侧的菜单，打开服务菜单，然后选择“资源组”以显示资源组的列表。
2. 找到要删除的资源组，右键单击列表右侧的“更多”按钮 (**...**)。
3. 选择“删除资源组”并进行确认。

## <a name="create-an-account-using-powershell"></a>使用 PowerShell 创建帐户

使用 `Login-AzureRmAccount` 命令登录到 Azure 订阅，然后按照屏幕上的说明进行身份验证。

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

### <a name="upgrade-your-powershell-module"></a>升级 powershell 模块

若要通过 PowerShell 来与 Data Lake Storage Gen2 交互，必须将模块升级到预览版。

为此，请打开提升的 PowerShell 并输入以下命令：`Install-Module AzureRM.Storage –Repository PSGallery -RequiredVersion 5.0.4-preview –AllowPrerelease –AllowClobber –Force `

然后重启 shell。

### <a name="create-a-resource-group"></a>创建资源组

若要通过 PowerShell 创建新的资源组，请使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 命令： 

> [!NOTE]
> 只有中国东部 2 和中国北部启用了分层命名空间。 在创建存储帐户时，请确保指定这两个位置之一。

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
$location = "chinaeast2"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-general-purpose-v2-storage-account"></a>创建常规用途 v2 存储帐户

若要使用本地冗余存储 (LRS) 从 PowerShell 创建常规用途 v2 存储帐户，请使用 [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount) 命令：

```powershell
Get-AzureRmLocation | select Location 
$location = "chinaeast2"

New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 
  -EnableHierarchicalNamespace $True
```

### <a name="clean-up-resources"></a>清理资源

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令： 

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="create-an-account-using-azure-cli"></a>使用 Azure CLI 创建帐户

登录到 [Azure 门户](https://portal.azure.cn)。

若要登录到本地安装的 CLI，请运行登录命令：

```cli
az login
```

### <a name="upgrade-your-cli-module"></a>升级 CLI 模块

若要通过 CLI 来与 Data Lake Storage Gen2 交互，必须将扩展添加到 shell。

为此，使用 Powershell 输入以下命令：`az extension add --name storage-preview`

### <a name="create-a-resource-group"></a>创建资源组

若要通过 Azure CLI 创建新的资源组，请使用 [az group create](/cli/group#az_group_create) 命令。

```azurecli-interactive
az group create \
    --name storage-quickstart-resource-group \
    --location chinaeast2
```

> [!NOTE]
> 只有中国东部 2 和中国北部启用了分层命名空间。 在创建存储帐户时，请确保指定这两个位置之一。

### <a name="create-a-general-purpose-v2-storage-account"></a>创建常规用途 v2 存储帐户

若要使用本地冗余存储从 Azure CLI 创建常规用途 v2 存储帐户，请使用 [az storage account create](/cli/storage/account#az_storage_account_create) 命令。

```azurecli-interactive
az storage account create \
    --name storagequickstart \
    --resource-group storage-quickstart-resource-group \
    --location chinaeast2 \
    --sku Standard_LRS \
    --kind StorageV2 \
    --hierarchical-namespace true
```

### <a name="clean-up-resources"></a>清理资源

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [az group delete](/cli/group#az_group_delete) 命令。

```azurecli-interactive
az group delete --name myResourceGroup
```

