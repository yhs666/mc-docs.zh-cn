---
title: Azure CLI 脚本 - 更改服务器配置
description: 此示例 CLI 脚本列出了所有可用服务器配置，并更新了 innodb_lock_wait_timeout 的值。
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 06/15/2018
ms.openlocfilehash: 50d46fed0075fa646d5c3137211d43a97c9fe86a
ms.sourcegitcommit: 3d17c1b077d5091e223aea472e15fcb526858930
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2018
ms.locfileid: "37873665"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>使用 Azure CLI 列出和更新用于 MySQL 服务器的 Azure 数据库的配置

> [!NOTE]
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql/)。

此示例 CLI 脚本列出了 Azure 数据库所有适用于 MySQL 服务器的可用配置参数及其允许的值，并将 innodb_lock_wait_timeout 设置为默认值以外的值。 本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI 2.0]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。 

## <a name="sample-script"></a>示例脚本
在此示例脚本中进行编辑，将管理员用户名和密码更新为你自己的。

```cli
#!/bin/bash

# Create a resource group
az group create \
--name myresource \
--location chinaeast

# Create a MySQL server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az mysql server create \
--name mysqlserver4demo \
--resource-group myresource \
--location chinaeast \
--admin-user myadmin \
--admin-password <server_admin_password> \
--performance-tier Basic \
--compute-units 50

# Display all available configurations with valid values of an Azure Database for MySQL server
az mysql server configuration list \
--resource-group myresource \
--server-name mysqlserver4demo

# Set value of *innodb_lock_wait_timeout*
az mysql server configuration set \
--resource-group myresource \
--server-name mysqlserver4demo \
--name innodb_lock_wait_timeout \
--value 120

# Check the value of *innodb_lock_wait_timeout*
az mysql server configuration show \
--resource-group myresource \
--server-name mysqlserver4demo \
--name innodb_lock_wait_timeout
```

## <a name="clean-up-deployment"></a>清理部署
运行脚本示例后，请使用以下命令删除资源组以及与其关联的所有资源。 

```cli
#!/bin/bash
az group delete --name myresource
```

## <a name="script-explanation"></a>脚本说明
此脚本使用下表中列出的命令：

| **命令** | **说明** |
|---|---|
| [az group create](/cli/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az mysql server create](/cli/mysql/server#az_msql_server_create) | 创建用于托管数据库的 MySQL 服务器。 |
| [az mysql server configuration list](/cli/mysql/server/configuration#az_msql_server_configuration_list) | 列出用于 MySQL 服务器的 Azure 数据库的配置。 |
| [az mysql server configuration set](/cli/mysql/server/configuration#az_msql_server_configuration_set) | 更新用于 MySQL 服务器的 Azure 数据库的配置。 |
| [az mysql server configuration show](/cli/mysql/server/configuration#az_msql_server_configuration_show) | 显示用于 MySQL 服务器的 Azure 数据库的配置。 |
| [az group delete](/cli/group#az_group_delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。
- 尝试其他脚本：[Azure Database for MySQL 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
- 有关服务器参数的详细信息，请参阅[如何在 Azure 数据库中配置适用于 MySQL 的服务器参数](../howto-server-parameters.md)。
