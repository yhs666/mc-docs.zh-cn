---
title: Azure CLI 脚本 - 更改服务器配置
description: 此示例 CLI 脚本列出了所有可用服务器配置，并更新了 innodb_lock_wait_timeout 的值。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
origin.date: 11/28/2018
ms.date: 05/27/2019
ms.openlocfilehash: f859e5be73f2c6e2ea03482fba029c094e5f1d9f
ms.sourcegitcommit: 60169f39663ae62016f918bdfa223c411e249883
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66173252"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>使用 Azure CLI 列出和更新 Azure Database for MariaDB 服务器的配置
此示例 CLI 脚本列出了 Azure Database for MariaDB 服务器的可用配置参数及其允许的值，并将 innodb_lock_wait_timeout  设置为默认值以外的值。

本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。 

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。
```Azure CLI
#<a name="binbash"></a>!/bin/bash

# <a name="create-a-resource-group"></a>创建资源组
az group create \
--name myresourcegroup \
--location chinaeast2

# <a name="create-a-mariadb-server-in-the-resource-group"></a>在资源组中创建 MariaDB 服务器
# <a name="name-of-a-server-maps-to-dns-name-and-is-thus-required-to-be-globally-unique-in-azure"></a>服务器的名称会映射到 DNS 名称，因此该名称在 Azure 中需具有全局级别的唯一性。
# <a name="substitute-the-serveradminpassword-with-your-own-value"></a>将 <server_admin_password> 替换为你自己的值。
az mariadb server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location chinaeast2 \
--admin-user myadmin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2 \

# <a name="display-all-available-configurations-with-valid-values-of-an-azure-database-for-mariadb-server"></a>显示具有 Azure Database for MariaDB 服务器的有效值的所有可用配置
az mariadb server configuration list \
--resource-group myresourcegroup \
--server-name mydemoserver

# <a name="set-value-of-innodblockwaittimeout"></a>设置 *innodb_lock_wait_timeout* 的值
az mariadb server configuration set \
--resource-group myresourcegroup \
--server-name mydemoserver \
--name innodb_lock_wait_timeout \
--value 120

# <a name="check-the-value-of-innodblockwaittimeout"></a>检查 *innodb_lock_wait_timeout* 的值
az mariadb server configuration show \
--resource-group myresourcegroup \
--server-name mydemoserver \
--name innodb_lock_wait_timeout
```

## Clean up deployment
Use the following command to remove the resource group and all resources associated with it after the script has been run.
```Azure CLI
#!/bin/bash
az group delete --name myresourcegroup
```

## <a name="script-explanation"></a>脚本说明
此脚本使用下表中列出的命令：

| **命令** | **说明** |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az mariadb server create](/cli/mariadb/server#az-mariadb-server-create) | 创建用于托管数据库的 MariaDB 服务器。 |
| [az mariadb server configuration list](/cli/mariadb/server/configuration#az-mariadb-server-configuration-list) | 列出 Azure Database for MariaDB 服务器的配置。 |
| [az mariadb server configuration set](/cli/mariadb/server/configuration#az-mariadb-server-configuration-set) | 更新 Azure Database for MariaDB 服务器的配置。 |
| [az mariadb server configuration show](/cli/mariadb/server/configuration#az-mariadb-server-configuration-show) | 显示 Azure Database for MariaDB 服务器的配置。 |
| [az group delete](/cli/group#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 阅读有关 Azure CLI 的更多信息：[Azure CLI 文档](/cli/)。
- 请尝试其他脚本：[Azure Database for MariaDB 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
- 有关服务器参数的详细信息，请参阅[如何在 Azure Database for MariaDB 中配置服务器参数](../howto-server-parameters.md)。
