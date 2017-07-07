---
title: "Azure CLI 脚本 - 缩放弹性池 | Azure"
description: "Azure CLI 脚本示例 - 缩放弹性数据库池"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: azurecli
ms.topic: samples
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 04/24/2017
ms.author: v-johch
ms.openlocfilehash: 6ca85691e9d731f530a71b8bbc7ef74b3d3222b0
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: Human Translation
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="scale-an-elastic-pool-in-azure-sql-database-using-the-azure-cli"></a>使用 Azure CLI 缩放 Azure SQL 数据库中的弹性池

此示例 CLI 脚本创建弹性池，移动入池数据库，并更改性能级别。 

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
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#create) | 创建用于托管 SQL 数据库的逻辑服务器。 |
| [az sql elastic-pools create](https://docs.microsoft.com/cli/azure/sql/elastic-pools#create) | 在逻辑服务器中创建弹性数据库池。 |
| [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#create) | 在逻辑服务器中创建 SQL 数据库。 |
| [az sql elastic-pools update](https://docs.microsoft.com/cli/azure/sql/elastic-pools#update) | 更新弹性数据库池，在此示例中更改分配的 eDTU。 |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
