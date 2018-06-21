---
title: Azure CLI 脚本示例 - 根据前缀删除容器 | Microsoft Docs
description: 根据容器名称前缀删除 Azure 存储 blob 容器。
services: storage
documentationcenter: na
author: forester123
manager: digimobile
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
origin.date: 06/22/2017
ms.date: 10/23/2017
ms.author: v-johch
ms.openlocfilehash: 321c1e0f8cc013580a5215274c8342e7e4cbd864
ms.sourcegitcommit: ad7accbbd1bc7ce0aeb2b58ce9013b7cafa4668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
ms.locfileid: "29870911"
---
# <a name="delete-containers-based-on-container-name-prefix"></a>根据容器名称前缀删除容器

此脚本首先会在 Azure Blob 存储中创建几个示例容器，然后根据容器名称的前缀删除一些容器。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash
export AZURE_STORAGE_ACCOUNT=<storage-account-name>
export AZURE_STORAGE_ACCESS_KEY=<storage-account-key>

# Create a resource group
az group create --name myResourceGroup --location chinaeast

# Create some test containers
az storage container create --name test-container-001
az storage container create --name test-container-002
az storage container create --name production-container-001

# List only the containers with a specific prefix
az storage container list --prefix "test-" --query "[*].[name]" --output tsv

echo "Deleting test- containers..."

# Delete 
for container in `az storage container list --prefix "test-" --query "[*].[name]" --output tsv`; do
    az storage container delete --name $container
done

echo "Remaining containers:"
az storage container list --output table
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令，删除资源组、其余容器和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令根据容器名称前缀删除容器。 表中的每一项均链接到命令特定的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az storage account create](https://docs.azure.cn/cli/storage/account#az_storage_account_create) | 在指定资源组中创建 Azure 存储帐户。 |
| [az storage container create](https://docs.azure.cn/cli/storage/container#az_storage_container_create) | 在 Azure Blob 存储中创建容器。 |
| [az storage container list](https://docs.azure.cn/cli/storage/container#az_storage_container_list) | 列出 Azure 存储帐户中的容器。 |
| [az storage container delete](https://docs.azure.cn/cli/storage/container#az_storage_container_delete) | 删除 Azure 存储帐户中的容器。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/overview)。

可以在 [Azure 存储的 Azure CLI 示例](../blobs/storage-samples-blobs-cli.md)中找到其他存储 CLI 脚本示例。
