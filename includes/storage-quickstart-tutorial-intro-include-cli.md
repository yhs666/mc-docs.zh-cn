---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/26/2018
ms.date: 01/14/2019
ms.author: v-jay
ms.openlocfilehash: b2d08761c8cf1cb3970ef6deb3ad4d11e7f2d6fb
ms.sourcegitcommit: 5eff40f2a66e71da3f8966289ab0161b059d0263
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193312"
---
## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](https://docs.azure.cn/cli/group#az_group_create) 命令创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。

```cli
az group create \
    --name myResourceGroup \
    --location "China East"
```

## <a name="create-a-storage-account"></a>创建存储帐户

使用 [az storage account create](https://docs.azure.cn/cli/storage/account#create) 命令创建常规用途存储帐户。 常规用途的存储帐户可用于以下四种服务：Blob、文件、表和队列。 

```cli
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location "China East" \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>指定存储帐户凭据

Azure CLI 需要存储帐户凭据才能执行本教程的大部分命令。 提供凭据的选项有多种，其中最简单方法之一是设置 `AZURE_STORAGE_ACCOUNT` 和 `AZURE_STORAGE_ACCESS_KEY` 环境变量。

首先，使用 [az storage account keys list](https://docs.azure.cn/cli/storage/account/keys#list) 命令显示存储帐户密钥：

```cli
az storage account keys list \
    --account-name mystorageaccount \
    --resource-group myResourceGroup \
    --output table
```

现在，设置 `AZURE_STORAGE_ACCOUNT` 和 `AZURE_STORAGE_ACCESS_KEY` 环境变量。 可以通过使用 `export` 命令在 Bash Shell 中完成此操作:

```bash
export AZURE_STORAGE_ACCOUNT="mystorageaccountname"
export AZURE_STORAGE_ACCESS_KEY="myStorageAccountKey"
```