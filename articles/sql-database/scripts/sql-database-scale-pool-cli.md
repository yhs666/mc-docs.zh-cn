---
title: "CLI 示例 - 缩放 SQL 弹性池 - Azure SQL 数据库 | Azure"
description: "在 Azure SQL 数据库中缩放 SQL 弹性池的 Azure CLI 示例脚本"
services: sql-database
documentationcenter: sql-database
author: forester123
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
origin.date: 06/23/2017
ms.date: 10/02/2017
ms.author: v-johch
ms.openlocfilehash: 6c41fcb24b4384df15cf920cab1762de9041ad32
ms.sourcegitcommit: 82bb249562dea81871d7306143fee73be72273e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2017
---
# <a name="use-cli-to-scale-a-sql-elastic-pool-in-azure-sql-database"></a>使用 CLI 在 Azure SQL 数据库中缩放 SQL 弹性池

此 Azure CLI 脚本示例创建 SQL 弹性池，移动入池数据库，并更改弹性池性能级别。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)] 本主题需要运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0]( https://docs.microsoft.com/cli/azure/install-azure-cli)。 

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
    --location "China East" 

# Create a server
az sql server create \
    --name $servername \
    --resource-group myResourceGroup \
    --location "China East" \
    --admin-user $adminlogin \
    --admin-password $password

# Create a pool
az sql elastic-pools create \
    --resource-group myResourceGroup \
    --location "China East"  \
    --server $servername \
    --name samplepool \
    --dtu 50 \
    --database-dtu-max 20

# Create two database in the pool
az sql db create \
    --resource-group myResourceGroup \
    --server $servername \
    --name myFirstSampleDatabase \
    --elastic-pool-name samplepool
az sql db create \
    --resource-group myResourceGroup \
    --server $servername \
    --name mySecondSampleDatabase \
    --elastic-pool-name samplepool

# Scale up to the pool to 100 eDTU
az sql elastic-pools update \
    --resource-group myResourceGroup \
    --server $servername \
    --name samplepool \
    --set dtu=100
```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、逻辑服务器、SQL 数据库和防火墙规则。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | 创建用于托管 SQL 数据库的逻辑服务器。 |
| [az sql elastic-pools create](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create) | 在逻辑服务器中创建弹性数据库池。 |
| [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_create) | 在逻辑服务器中创建 SQL 数据库。 |
| [az sql elastic-pools update](https://docs.microsoft.com/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update) | 更新弹性数据库池，在此示例中更改分配的 eDTU。 |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。

<!--Update_Description: update bookmarks-->