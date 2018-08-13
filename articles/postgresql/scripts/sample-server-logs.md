---
title: Azure CLI 脚本 - 下载 Azure Database for PostgreSQL 中的服务器日志
description: 此示例 Azure CLI 脚本演示如何启用和下载适用于 PostgreSQL 服务器的 Azure 数据库的服务器日志。
services: postgresql
author: WenJason
ms.author: v-jay
manager: digimobile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
origin.date: 02/28/2018
ms.date: 08/13/2018
ms.openlocfilehash: 65926869a530b8cfb408217c754bbede82be5f06
ms.sourcegitcommit: 15355a03ed66b36c9a1a84c3d9db009668dec0e3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722971"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>使用 Azure CLI 启用和下载适用于 PostgreSQL 服务器的 Azure 数据库的服务器慢查询日志
此示例 CLI 脚本可启用和下载单个适用于 PostgreSQL 服务器的 Azure 数据库的慢查询日志。 本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI 2.0]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。 将 `az monitor` 命令中的 <log_file_name> 替换自己的服务器日志文件名。

```cli
#!/bin/bash

# Add the Azure CLI extension 
az extension add --name rdbms

# Create a resource group
az group create \
--name myresourcegroup  \
--location chinaeast2

# Create a PostgreSQL server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure
# Substitute the <server_admin_password> with your own value
az postgres server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location chinaeast2 \
--admin-user myadmin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2 \

# List the configuration options for review
az postgres server configuration list \
--resource-group myresourcegroup  \
--server mydemoserver

# Turn on statement level log
az postgres server configuration set \
--name log_statement \
--resource-group myresourcegroup \
--server mydemoserver \
--value all

# Set log_min_duration_statement time to 10 sec
az postgres server configuration set \
--name log_min_duration_statement \
--resource-group myresourcegroup \
--server mydemoserver \
--value 10000

# List the available log files and direct to a text file
az postgres server-logs list \
--resource-group myresourcegroup \
--server mydemoserver > log_files_list.txt

# Download log file from Azure 
# Review log_files_list.txt to find the server log file name for the desired timeframe
# Substitute the <log_file_name> with your server log file name
# Creates the postgresql-<date>_000000.log file in the current command line path
az postgres server-logs download \
--name <log_file_name> \
--resource-group myresourcegroup \
--server mydemoserver
```

## <a name="clean-up-deployment"></a>清理部署
运行脚本示例后，请使用以下命令删除资源组以及与其关联的所有资源。 

```bash
#!/bin/bash
az group delete --name myresourcegroup
```

## <a name="script-explanation"></a>脚本说明
此脚本使用下表中列出的命令：

| **命令** | **说明** |
|---|---|
| [az group create](/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az postgres server create](/cli/postgres/server#az_msql_server_create) | 创建托管数据库的 PostgreSQL 服务器。 |
| [az postgres server configuration list](/cli/postgres/server/configuration#az_postgres_server_configuration_list) | 列出服务器的配置值。 |
| [az postgres server configuration set](/cli/postgres/server/configuration#az_postgres_server_configuration_set) | 更新服务器的配置。 |
| [az postgres server-logs list](/cli/postgres/server-logs#az_postgres_server_logs_list) | 列出服务器的日志文件。 |
| [az postgres server-logs download](/cli/postgres/server-logs#az_postgres_server_logs_download) | 下载日志文件。 |
| [az group delete](/cli/group#az_group_delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。
- 尝试其他脚本：[Azure Database for PostgreSQL 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
- [在 Azure 门户中配置和访问服务器日志](../howto-configure-server-logs-in-portal.md)
