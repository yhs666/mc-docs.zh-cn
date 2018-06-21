---
title: Azure CLI 脚本示例 - 计算 blob 容器大小 | Microsoft Docs
description: 通过计算容器中 blob 的总大小来计算 Azure Blob 存储中容器的大小。
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
origin.date: 06/28/2017
ms.date: 10/23/2017
ms.author: v-johch
ms.openlocfilehash: 8d49f03b4498cd096f3416c6328620e8248b341b
ms.sourcegitcommit: ad7accbbd1bc7ce0aeb2b58ce9013b7cafa4668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
ms.locfileid: "29870893"
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>计算 Blob 存储容器的大小

此脚本通过计算容器中 blob 的总大小来计算 Azure Blob 存储中容器的大小。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> 此 CLI 脚本提供容器的估计大小，不应用于计费计算。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash
export AZURE_STORAGE_ACCOUNT=<storage-account-name>
export AZURE_STORAGE_ACCESS_KEY=<storage-account-key>

# Create a resource group
az group create --name myResourceGroup --location chinaeast

# Create a container
az storage container create --name mycontainer

# Create sample files to upload as blobs
for i in `seq 1 3`; do
    echo $RANDOM > container_size_sample_file_$i.txt
done

# Upload sample files to container
az storage blob upload-batch \
    --pattern "container_size_sample_file_*.txt" \
    --source . \
    --destination mycontainer

# Calculate total size of container. Use the --query parameter to display only
# blob contentLength and output it in TSV format so only the values are
# returned. Then pipe the results to the paste and bc utilities to total the
# size of the blobs in the container.
bytes=`az storage blob list \
    --container-name mycontainer \
    --query "[*].[properties.contentLength]" \
    --output tsv |
    paste --serial --delimiters=+ | bc`

# Display total bytes
echo "Total bytes in container: $bytes"

# Delete the sample files created by this script
rm container_size_sample_file_*.txt
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、容器和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令来计算 Blob 存储容器的大小。 表中的每一项均链接到命令特定的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az storage blob upload](https://docs.azure.cn/cli/storage/account#az_storage_account_create) | 将本地文件上传到 Azure Blob 存储容器。 |
| [az storage blob list](https://docs.azure.cn/cli/storage/account/keys#az_storage_account_keys_list) | 列出 Azure Blob 存储容器中的 blob。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/overview)。

有关其他存储 CLI 脚本示例，可参阅 [Azure Blob 存储的 Azure CLI 示例](../blobs/storage-samples-blobs-cli.md)。
