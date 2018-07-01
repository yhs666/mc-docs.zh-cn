---
title: CLI 示例 - 创建 Azure SQL 数据库 | Azure
description: 使用此 Azure CLI 示例脚本创建 SQL 数据库。
services: sql-database
documentationcenter: sql-database
author: forester123
manager: digimobile
editor: carlrab
ms.service: sql-database
ms.custom: DBs & servers, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 05/17/2018
ms.date: 07/02/2018
ms.author: v-johch
ms.openlocfilehash: 1476f0b147f6c7983f26c1340879bc627cec18df
ms.sourcegitcommit: 8b36b1e2464628fb8631b619a29a15288b710383
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2018
ms.locfileid: "36948031"
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>使用 CLI 创建单一 Azure SQL 数据库并配置防火墙规则

此 Azure CLI 脚本示例创建 Azure SQL 数据库，并配置服务器级防火墙规则。 成功运行该脚本后，可以通过所有 Azure 服务和配置的 IP 地址访问 SQL 数据库。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

如果选择在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0]( /cli/install-azure-cli)。 

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Set an admin login and password for your database
export adminlogin=ServerAdmin
export password=ChangeYourAdminPassword1
# The logical server name has to be unique in the system
export servername=server-$RANDOM
# The ip address range that you want to allow to access your DB
export startip=0.0.0.0
export endip=0.0.0.0

# Create a resource group
az group create \
    --name myResourceGroup \
    --location chinaeast

# Create a logical server in the resource group
az sql server create \
    --name $servername \
    --resource-group myResourceGroup \
    --location chinaeast  \
    --admin-user $adminlogin \
    --admin-password $password

# Configure a firewall rule for the server
az sql server firewall-rule create \
    --resource-group myResourceGroup \
    --server $servername \
    -n AllowYourIp \
    --start-ip-address $startip \
    --end-ip-address $endip

# Create a database in the server with zone redundancy as true
az sql db create \
    --resource-group myResourceGroup \
    --server $servername \
    --name mySampleDatabase \
    --sample-name AdventureWorksLT \
    --service-objective S0 \
    --zone-redundant

# Update database and set zone redundancy as false
az sql db update \
    --resource-group myResourceGroup \
    --server $servername \
    --name mySampleDatabase \
    --zone-redundant false

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
| [az group create](/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](/cli/sql/server#az_sql_server_create) | 创建用于托管 SQL 数据库的逻辑服务器。 |
| [az sql server firewall create](/cli/sql/server/firewall-rule#az_sql_server_firewall_rule_create) | 创建一个防火墙规则，以允许从输入的 IP 地址范围访问服务器上的所有 SQL 数据库。 |
| [az sql db create](/cli/sql/db#az_sql_db_create) | 在逻辑服务器中创建 SQL 数据库。 |
| [az group delete](/cli/resource#az_resource_delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/overview)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。

<!--Update_Description: update sample scripts; update Global CLI 2.0 links to Mooncake CLI 2.0-->