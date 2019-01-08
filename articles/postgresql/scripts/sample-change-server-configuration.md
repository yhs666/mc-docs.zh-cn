---
title: Azure CLI 脚本 - 更改服务器配置
description: 此示例 CLI 脚本列出所有可用的服务器配置选项，并更新某个选项的值。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
origin.date: 02/28/2018
ms.date: 12/31/2018
ms.openlocfilehash: b9b60c1cffa4ef9a9797ab3b8b168c047bed0ae0
ms.sourcegitcommit: e96e0c91b8c3c5737243f986519104041424ddd5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806193"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>使用 Azure CLI 列出和更新 Azure Database for PostgreSQL 服务器的配置
此示例 CLI 脚本列出所有 Azure Database for PostgreSQL 服务器的可用配置参数及其允许的值，并将 log_retention_days 设置为默认值以外的值。

本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。 

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。

```cli
#!/bin/bash

# Create a resource group
az group create \
--name myresourcegroup \
--location chinaeast2

# Create a PostgreSQL server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az postgres server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location chinaeast2 \
--admin-user myadmin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2 \

# Display all available configurations with valid values of an Azure Database for PostgreSQL server
az postgres server configuration list \
--resource-group myresourcegroup \
--server-name mydemoserver

# Set value of **log_retention_days**
az postgres server configuration set \
--resource-group myresourcegroup \
--server-name mydemoserver \
--name log_retention_days \
--value 7

# Check the value of **log_retention_days**
az postgres server configuration show \
--resource-group myresourcegroup \
--server-name mydemoserver \
--name log_retention_days 
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
| [az postgres server create](/cli/postgres/server#az_postgres_server_create) | 创建托管数据库的 PostgreSQL 服务器。 |
| [az postgres server configuration list](/cli/postgres/server/configuration#az_postgres_server_configuration_list) | 列出 Azure Database for PostgreSQL 服务器的配置。 |
| [az postgres server configuration set](/cli/postgres/server/configuration#az_postgres_server_configuration_set) | 更新 Azure Database for PostgreSQL 服务器的配置。 |
| [az postgres server configuration show](/cli/postgres/server/configuration#az_postgres_server_configuration_show) | 显示 Azure Database for PostgreSQL 服务器的配置。 |
| [az group delete](/cli/group#az_group_delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 阅读有关 Azure CLI 的更多信息：[Azure CLI 文档](/cli)。
- 请尝试其他脚本：[用于 PostgreSQL 的 Azure 数据库的 Azure CLI 示例](../sample-scripts-azure-cli.md)
- 有关服务器参数的详细信息，请参阅[如何在 Azure 门户中配置服务器参数](../howto-configure-server-parameters-using-portal.md)。
