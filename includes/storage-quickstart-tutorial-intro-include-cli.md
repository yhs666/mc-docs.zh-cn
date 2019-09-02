---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/26/2018
ms.date: 05/27/2019
ms.author: v-jay
ms.openlocfilehash: 4bf825ade5498eb6805b6b0452351355067179dd
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66039215"
---
## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/group) 命令创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。

```cli
az group create \
    --name myResourceGroup \
    --location "China East"
```

## <a name="create-a-storage-account"></a>创建存储帐户

使用 [az storage account create](https://docs.azure.cn/cli/storage/account) 命令创建常规用途存储帐户。 常规用途的存储帐户可用于以下四种服务：Blob、文件、表和队列。 

```cli
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location "China East" \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>指定存储帐户凭据

Azure CLI 需要存储帐户凭据才能执行本教程的大部分命令。 提供凭据的选项有多种，其中最简单方法之一是设置 `AZURE_STORAGE_ACCOUNT` 和 `AZURE_STORAGE_KEY` 环境变量。

首先，使用 [az storage account keys list](https://docs.azure.cn/cli/storage/account/keys) 命令显示存储帐户密钥：

```cli
az storage account keys list \
    --account-name mystorageaccount \
    --resource-group myResourceGroup \
    --output table
```

现在，设置 `AZURE_STORAGE_ACCOUNT` 和 `AZURE_STORAGE_KEY` 环境变量。 可以通过使用 `export` 命令在 Bash Shell 中完成此操作:

```bash
export AZURE_STORAGE_ACCOUNT="mystorageaccountname"
export AZURE_STORAGE_KEY="myStorageAccountKey"
```