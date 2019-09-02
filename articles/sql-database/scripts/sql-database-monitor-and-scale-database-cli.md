---
title: CLI 示例 - 监视缩放 - 单一 Azure SQL 数据库 | Microsoft Docs
description: 监视和缩放单一 Azure SQL 数据库的 Azure CLI 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 01/25/2019
ms.date: 04/29/2019
ms.openlocfilehash: 222b2158fe4da475212286ca38275b2daf8e33c9
ms.sourcegitcommit: 52ce0d62ea704b5dd968885523d54a36d5787f2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69544117"
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>使用 CLI 监视和缩放单一 SQL 数据库

此 Azure CLI 脚本示例在查询数据库的大小信息后，将单个 Azure SQL 数据库缩放为不同的计算大小。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI]( https://docs.azure.cn/cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# set execution context (if necessary)
az account set --subscription <replace with your subscription name or id>

# Set the resource group name and location for your server
resourceGroupName=myResourceGroup-$RANDOM
location=chinaeast

# Set an admin login and password for your database
adminlogin=ServerAdmin
password=`openssl rand -base64 16`
# password=<EnterYourComplexPasswordHere1>

# The logical server name has to be unique in the system
servername=server-$RANDOM

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

# Create a General Purpose Gen4 database with 1 vCore
az sql db create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name mySampleDatabase \
    --edition GeneralPurpose \
    --family Gen4 \
    --capacity 1 

# Monitor database size
az sql db list-usages \
    --name mySampleDatabase \
    --resource-group $resourceGroupName \
    --server $servername

# Scale up database to 2 vCores (create command executes update if DB already exists)
az sql db create \
    --resource-group $resourceGroupName \
    --server $servername \
    --name mySampleDatabase \
    --edition GeneralPurpose \
    --family Gen4 \
    --capacity 2

# Echo random password
echo $password
```

> [!TIP]
> 使用 [az sql db op list](/cli/sql/db/op?#az-sql-db-op-list) 获取对数据库执行的操作列表，并使用 [az sql db op cancel](/cli/sql/db/op#az-sql-db-op-cancel) 取消对数据库的更新操作。

## <a name="clean-up-deployment"></a>清理部署

使用以下命令删除资源组及其相关的所有资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](/cli/sql/server#az-sql-server-create) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [az sql db show-usage](/cli/sql#az-sql-show-usage) | 显示单一数据库或共用数据库的大小使用情况信息。 |
| [az sql db update](/cli/sql/db#az-sql-db-update) | 更新数据库属性（如服务层级或计算大小），或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [az group delete](/cli/vm/extension#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
