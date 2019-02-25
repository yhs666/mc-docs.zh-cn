---
title: CLI 示例 - 创建 Azure SQL 数据库 | Microsoft Docs
description: 使用此 Azure CLI 示例脚本创建 SQL 数据库。
services: sql-database
ms.service: sql-database
ms.subservice: standalone-database
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
manager: digimobile
ms.author: v-jay
ms.reviewer: ''
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: 5172d5c6f938a54e057dea832435bc83c0f3233e
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663618"
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>使用 CLI 创建单一 Azure SQL 数据库并配置防火墙规则

此 Azure CLI 脚本示例创建 Azure SQL 数据库，并配置服务器级防火墙规则。 成功运行该脚本后，可以通过所有 Azure 服务和配置的 IP 地址访问 SQL 数据库。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本主题需要运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI]( /cli/install-azure-cli)。 

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

# Create a database in the server
az sql db create \
    --resource-group myResourceGroup \
    --server $servername \
    --name mySampleDatabase \
    --sample-name AdventureWorksLT \
    --service-objective S0

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
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](/cli/sql/server#az-sql-server-create) | 创建托管单一数据库或弹性池的 SQL 数据库服务器。 |
| [az sql server firewall create](/cli/sql/server/firewall-rule#az-sql-server-firewall-rule-create) | 创建一个防火墙规则，允许从输入的 IP 地址范围访问 SQL 数据库服务器上的所有单一数据库和弹性池。 |
| [az sql db create](/cli/sql/db#az-sql-db-create) | 创建单一数据库或弹性池。 |
| [az group delete](/cli/resource#az-resource-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。

<!--Update_Description: update sample scripts; update Global CLI 2.0 links to Mooncake CLI 2.0-->