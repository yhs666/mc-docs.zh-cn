---
title: CLI 示例 - 移动 Azure SQL 数据库 - SQL 弹性池 | Microsoft Docs
description: 在 SQL 弹性池中移动 SQL 数据库的 Azure CLI 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 06/25/2019
ms.date: 08/19/2019
ms.openlocfilehash: d86374ee56701c3366545b98748910d5d09a11fb
ms.sourcegitcommit: 52ce0d62ea704b5dd968885523d54a36d5787f2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69544114"
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a>使用 CLI 在 SQL 弹性池中移动 Azure SQL 数据库

此 Azure CLI 脚本示例创建两个弹性池，将 Azure SQL 数据库从一个 SQL 弹性池移到另一个 SQL 弹性池中，然后将数据库移出弹性池，并转为单一数据库计算大小。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本主题需要运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI]( https://docs.azure.cn/cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# set execution context (if necessary)
az account set --subscription <replace with your subscription name or id>

# Set the resource group name and location for your server
resourceGroupName=myResourceGroup$RANDOM
location=chinaeast

# Set an admin login and password for your database
adminlogin=ServerAdmin
password=`openssl rand -base64 16`
# password=<EnterYourComplexPasswordHere1>

# The logical server name has to be unique in the system
servername=server$RANDOM

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a logical server in the resource group
az sql server create \
    --name $servername \
    --resource-group $resourceGroupName \
    --location $location \
    --admin-user $adminlogin \
    --admin-password $password

# Create two pools in the logical server
az sql elastic-pool create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name myFirstPool \
    --edition GeneralPurpose \
    --family Gen4 \
    --capacity 1
az sql elastic-pool create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name mySecondPool \
    --edition GeneralPurpose \
    --family Gen4 \
    --capacity 1

# Create a database in the first pool
az sql db create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name mySampleDatabase \
    --elastic-pool myFirstPool

# Move the database to the second pool - create command updates the db if it exists
az sql db create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name mySampleDatabase \
    --elastic-pool mySecondPool

# Move the database to standalone S0 service tier
az sql db create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name mySampleDatabase \
    --service-objective S0

# Echo random password
echo $password
```

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```azurecli
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](/cli/sql/server#az-sql-server-create) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [az sql elastic-pools create](/cli/sql/elastic-pool#az-sql-elastic-pool-create) | 创建弹性池。 |
| [az sql db create](/cli/sql/db#az-sql-db-create) | 创建单一数据库或创建弹性池中的数据库。 |
| [az sql db update](/cli/sql/db#az-sql-db-update) | 更新数据库属性，或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [az group delete](/cli/vm/extension#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
