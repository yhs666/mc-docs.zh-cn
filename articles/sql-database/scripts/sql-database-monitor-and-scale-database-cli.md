---
title: "Azure CLI 脚本 - 监视和缩放单个 SQL 数据库 | Azure"
description: "Azure CLI 脚本示例 - 使用 Azure CLI 监视和缩放单个 SQL 数据库"
services: sql-database
documentationcenter: sql-database
author: Hayley244
manager: digimobile
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 04/24/2017
ms.date: 07/10/2017
ms.author: v-johch
ms.openlocfilehash: 52116cfcfdcaa54e952956b76206c87fcd84173b
ms.sourcegitcommit: f2f4389152bed7e17371546ddbe1e52c21c0686a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="monitor-and-scale-a-single-sql-database-using-the-azure-cli"></a>使用 Azure CLI 监视和缩放单个 SQL 数据库

此示例 CLI 脚本在查询数据库的大小信息后，将单个 Azure SQL 数据库缩放为不同的性能级别。 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]


## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set an admin login and password for your database
adminlogin=ServerAdmin
password=ChangeYourAdminPassword1
# the logical server name has to be unique in the system
servername=server-$RANDOM

# Create a resource group
az group create \
    --name myResourceGroup \
    -location "China East" 

# Create a server
az sql server create \
    --name $servername \
    --resource-group myResourceGroup \
    --location "China East" \
    --admin-user $adminlogin \
    --admin-password $password

# Create a database
az sql db create \
    --resource-group myResourceGroup \
    --server $servername \
    --name mySampleDatabase \
    --service-objective S0

# Monitor database size
az sql db list-usages \
    --name mySampleDatabase \
    --resource-group myResourceGroup \
    --name $servername

# Scale up database to S1 performance level (create command executes update if DB already exists)
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

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#create) | 创建用于托管数据库的逻辑服务器。 |
| [az sql db show-usage](https://docs.microsoft.com/cli/azure/sql/db#show-usage) | 显示数据库的大小使用情况信息。 |
| [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#update) | 更新数据库属性（如服务层或性能级别），或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
