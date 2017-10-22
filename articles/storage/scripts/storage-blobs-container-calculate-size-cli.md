---
title: "Azure CLI 脚本示例 - 计算 blob 容器大小 | Microsoft Docs"
description: "通过计算容器中 blob 的总大小来计算 Azure Blob 存储中容器的大小。"
services: storage
documentationcenter: na
author: forester123
manager: digimobile
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
origin.date: 06/28/2017
ms.date: 10/23/2017
ms.author: v-johch
ms.openlocfilehash: eb7708bea3d3b5cb4b7aa25492df9ea5b3f4d491
ms.sourcegitcommit: fea4940a09cecbae36256410227e701e5f0aab6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>计算 Blob 存储容器的大小

此脚本通过计算容器中 blob 的总大小来计算 Azure Blob 存储中容器的大小。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

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

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/account#create) | 将本地文件上传到 Azure Blob 存储容器。 |
| [az storage blob list](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | 列出 Azure Blob 存储容器中的 blob。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

有关其他存储 CLI 脚本示例，可参阅 [Azure Blob 存储的 Azure CLI 示例](../blobs/storage-samples-blobs-cli.md)。
