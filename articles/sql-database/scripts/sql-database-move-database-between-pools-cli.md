---
title: CLI 示例 - 移动 Azure SQL 数据库 - SQL 弹性池 | Microsoft Docs
description: 在 SQL 弹性池中移动 SQL 数据库的 Azure CLI 示例脚本
services: sql-database
documentationcenter: sql-database
author: WenJason
manager: digimobile
editor: carlrab
tags: azure-service-management
ms.assetid: ''
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 09/14/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.openlocfilehash: 5b65af8121ff86717b1b73e040f3de7cf544d0f4
ms.sourcegitcommit: d8b4e1fbda8720bb92cc28631c314fa56fa374ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913795"
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a>使用 CLI 在 SQL 弹性池中移动 Azure SQL 数据库

此 Azure CLI 脚本示例创建两个弹性池，将 Azure SQL 数据库从一个 SQL 弹性池移到另一个 SQL 弹性池中，然后将数据库移出弹性池，并转为单一 Azure 数据库计算大小。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本主题需要运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0]( https://docs.azure.cn/cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set an admin login and password for your database
export adminlogin=ServerAdmin
export password=ChangeYourAdminPassword1
# The logical server name has to be unique in the system
export servername=server-$RANDOM

# Create a resource group
az group create \
    --name myResourceGroup \
    --location "China East" 

# Create a logical server in the resource group
az sql server create \
    --name $servername \
    --resource-group myResourceGroup \
    --location "China East" \
    --admin-user $adminlogin \
    --admin-password $password

# Create two pools in the logical server
az sql elastic-pools create \
    --resource-group myResourceGroup \
    --location "China East"  \
    --server $servername \
    --name myFirstPool \
    --dtu 50 \
    --database-dtu-max 20
az sql elastic-pools create \
    --resource-group myResourceGroup \
    --location "China East"  \
    --server $servername \
    --name MySecondPool \
    --dtu 50 \
    --database-dtu-max 50

# Create a database in the first pool
az sql db create \
    --resource-group myResourceGroup \
    --server $servername \
    --name mySampleDatabase \
    --elastic-pool-name myFirstPool

# Move the database to the second pool - create command updates the db if it exists
az sql db create \
    --resource-group myResourceGroup \
    --server-name $servername \
    --name mySampleDatabase \
    --elastic-pool-name mySecondPool

# Move the database to standalone S1 performance level
az sql db create \
    --resource-group myResourceGroup \
    --server $servername \
    --name mySampleDatabase \
    --service-objective S1
```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](https://docs.azure.cn/cli/sql/server#az_sql_server_create) | 创建用于托管数据库或弹性池的逻辑服务器。 |
| [az sql elastic-pools create](https://docs.azure.cn/cli/sql/elastic-pool#az_sql_elastic_pool_create) | 在逻辑服务器中创建弹性池。 |
| [az sql db create](https://docs.azure.cn/cli/sql/db#az_sql_db_create) | 在逻辑服务器中创建数据库作为单一数据库或入池数据库。 |
| [az sql db update](https://docs.azure.cn/cli/sql/db#az_sql_db_update) | 更新数据库属性，或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [az group delete](https://docs.azure.cn/cli/vm/extension#az_vm_extension_set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。

<!--Update_Description: update Global CLI 2.O links to Mooncake CLI 2.O links-->