---
title: CLI 示例 - 将单一数据库添加到故障转移组 - Azure SQL 数据库
description: Azure CLI 示例脚本，用于创建 Azure SQL 数据库单一数据库，将其添加到故障转移组，然后测试故障转移。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
origin.date: 07/16/2019
ms.date: 09/09/2019
ms.openlocfilehash: 03cc495e4daed49da5ead944784b1258528a59d4
ms.sourcegitcommit: 2610641d9fccebfa3ebfffa913027ac3afa7742b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2019
ms.locfileid: "70373068"
---
# <a name="use-cli-to-add-an-azure-sql-database-single-database-into-a-failover-group"></a>使用 CLI 将 Azure SQL 数据库单一数据库添加到故障转移组

此 PowerShell 脚本示例创建一个单一数据库，创建一个故障转移组，向其中添加该数据库，然后测试故障转移。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本主题需要运行 Azure CLI 版本 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli)。

## <a name="sample-script"></a>示例脚本

```cli
#!/bin/bash
# Set variables
$subscriptionID=<SubscriptionID>
$resourceGroupName=myResourceGroup-$RANDOM
$location=ChinaEast2
$adminLogin=azureuser
$password="PWD27!"+`openssl rand -base64 18`
$serverName=mysqlserver-$RANDOM
$databaseName=mySampleDatabase
$drLocation=ChinaNorth2
$drServerName=mysqlsecondary-$RANDOM
$failoverGroupName=failovergrouptutorial-$RANDOM

# The ip address range that you want to allow access to your DB. 
# Leaving at 0.0.0.0 will prevent outside-of-azure connections
$startip=0.0.0.0
$endip=0.0.0.0

# Print out randomized variables
echo Resource group name is $resourceGroupName
echo Passowrd is $password
echo Servername is $serverName
echo DR Server name $drServerName
echo Failover group name $failoverGroupName

# Set the subscription context for the Azure account
az account set -s $subscriptionID

# Create a resource group
echo "Creating resource group..."
az group create \
   --name $resourceGroupName \
   --location $location \
   --tags Owner[=SQLDB-Samples]

# Create a logical server in the resource group
echo "Creating primary logical server..."
az sql server create \
   --name $serverName \
   --resource-group $resourceGroupName \
   --location $location  \
   --admin-user $adminLogin \
   --admin-password $password

# Configure a firewall rule for the server
echo "Configuring firewall..."
az sql server firewall-rule create \
   --resource-group $resourceGroupName \
   --server $serverName \
   -n AllowYourIp \
   --start-ip-address $startip \
   --end-ip-address $endip

# Create a gen5 2vCore database in the server 
echo "Creating a gen5 2 vCore database..."
az sql db create \
   --resource-group $resourceGroupName \
   --server $serverName \
   --name $databaseName \
   --sample-name AdventureWorksLT \
   --edition GeneralPurpose \
   --family Gen5 \
   --capacity 2

# Create a secondary server in the failover region
echo "Creating a secondary logical server in the DR region..."
az sql server create \
   --name $drServerName \
   --resource-group $resourceGroupName \
   --location $drLocation  \
   --admin-user $adminLogin\
   --admin-password $password

# Create a failover group between the servers and add the database
echo "Creating a failover group between the two servers..."
az sql failover-group create \
   --name $failoverGroupName  \
   --partner-server $drServerName \
   --resource-group $resourceGroupName \
   --server $serverName \
   --add-db $databaseName
   --failover-policy Automatic

# Verify which server is secondary
echo "Verifying which server is in the secondary role..."
az sql failover-group list \
   --server $serverName \
   --resource-group $resourceGroupName

# Failover to the secondary server
echo "Failing over group to the secondary server..."
az sql failover-group set-primary \
   --name $failoverGroupName \
   --resource-group $resourceGroupName \
   --server $drServerName
echo "Successfully failed failover group over to" $drServerName

# Revert failover group back to the primary server
echo "Failing over group back to the primary server..."
az sql failover-group set-primary \
   --name $failoverGroupName \
   --resource-group $resourceGroupName \
   --server $serverName
echo "Successfully failed failover group back to" $serverName

# Print out randomized variables
echo Resource group name is $resourceGroupName
echo Password is $password
echo Servername is $serverName
echo DR Server name $drServerName
echo Failover group name $failoverGroupName

# Clean up resources by removing the resource group
# echo "Cleaning up resources by removing the resource group..."
# az group delete \
#   --name $resourceGroupName 
# echo "Successfully removed resource group" $resourceGroupName
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
| [az account set](/cli/account?view=azure-cli-latest#az-account-set) | 将订阅设置为当前的活动订阅。 | 
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](/cli/sql/server#az-sql-server-create) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [az sql server firewall-rule create](/cli/sql/server/firewall-rule) | 创建服务器的防火墙规则。 | 
| [az sql failover-group create](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | 创建故障转移组。 | 
| [az sql failover-group list](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | 列出某个服务器中的故障转移组。 |
| [az sql failover-group set-primary](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | 通过对当前主服务器上的所有数据库进行故障转移来设置故障转移组的主服务器。 | 
| [az group delete](/cli/vm/extension#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

其他 SQL 数据库 CLI 脚本示例可以在 [Azure SQL 数据库文档](../sql-database-cli-samples.md)中找到。
