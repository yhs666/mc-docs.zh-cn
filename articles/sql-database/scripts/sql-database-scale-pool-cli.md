---
title: CLI 示例 - 缩放 SQL 弹性池 - Azure SQL 数据库 | Microsoft Docs
description: 在 Azure SQL 数据库中缩放弹性池的 Azure CLI 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: ''
origin.date: 06/25/2019
ms.date: 09/09/2019
ms.openlocfilehash: 11b33252256881e7b8705ec4165fb9f4a7f9748d
ms.sourcegitcommit: 2610641d9fccebfa3ebfffa913027ac3afa7742b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2019
ms.locfileid: "70372977"
---
# <a name="use-cli-to-scale-an-elastic-pool-in-azure-sql-database"></a>使用 CLI 在 Azure SQL 数据库中缩放弹性池

此 Azure CLI 脚本示例创建弹性池、移动共用数据库，并更改弹性池计算大小。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本主题需要运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI]( https://docs.azure.cn/cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# set execution context (if necessary)
az account set --subscription <replace with your subscription name or id>

# Set the resource group name and location for your server
$resourceGroupName=myResourceGroup-$RANDOM
$location=chinaeast

# Set an admin login and password for your database
$adminlogin=ServerAdmin
$password=`openssl rand -base64 16`
# password=<EnterYourComplexPasswordHere1>

# The logical server name has to be unique in the system
$servername=server-$RANDOM

# Create a resource group
az group create \
--name $resourceGroupName \
--location $location

# Create a server
az sql server create \
    --name $servername \
    --resource-group $resourceGroupName \
    --location $location \
    --admin-user $adminlogin \
    --admin-password $password

# Create a pool with 5 vCores and a max storage of 756 GB
az sql elastic-pool create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name samplepool \
    --edition GeneralPurpose \
    --family Gen4 \
    --capacity 5 \
    --db-max-capacity 4 \
    --db-min-capacity 1 \
    --max-size 756GB

# Create two database in the pool
az sql db create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name myFirstSampleDatabase \
    --elastic-pool samplepool

az sql db create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name mySecondSampleDatabase \
    --elastic-pool samplepool

# Scale up to the pool to 10 vCores
az sql elastic-pool update \
    --resource-group $resourceGroupName \
    --server $servername \
    --name samplepool \
    --capacity 10 \
    --max-size 1536GB

# Echo random password
echo $password
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```azurecli
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、SQL 数据库服务器、单一数据库和 SQL 数据库防火墙规则。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](/cli/sql/server#az-sql-server-create) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [az sql elastic-pools create](/cli/sql/elastic-pool#az-sql-elastic-pool-create) | 创建弹性池。 |
| [az sql db create](/cli/sql/db#az-sql-db-create) | 创建单一数据库或共用数据库。 |
| [az sql elastic-pools update](/cli/sql/elastic-pool#az-sql-elastic-pool-update) | 更新弹性池，在此示例中更改分配的 eDTU。 |
| [az group delete](/cli/vm/extension#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
