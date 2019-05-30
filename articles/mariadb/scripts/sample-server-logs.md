---
title: Azure CLI 脚本 - 下载 Azure Database for MariaDB 中的服务器日志
description: 此示例 Azure CLI 脚本演示如何启用和下载 Azure Database for MariaDB 服务器的服务器日志。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
origin.date: 11/28/2018
ms.date: 05/27/2019
ms.openlocfilehash: 2885cf40ee651d1e7a1943d546d210e65392ac58
ms.sourcegitcommit: 60169f39663ae62016f918bdfa223c411e249883
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66173306"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>使用 Azure CLI 启用和下载 Azure Database for MariaDB 服务器的服务器慢查询日志
此示例 CLI 脚本可启用和下载单个 Azure Database for MariaDB 服务器的慢查询日志。

本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。 

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。 将 `az monitor` 命令中的 &lt;log_file_name&gt; 替换自己的服务器日志文件名。
```Azure CLI
#<a name="binbash"></a>!/bin/bash

# <a name="create-a-resource-group"></a>创建资源组
az group create \
--name myresourcegroup  \
--location chinaeast2

# <a name="create-a-mariadb-server-in-the-resource-group"></a>在资源组中创建 MariaDB 服务器
# <a name="name-of-a-server-maps-to-dns-name-and-is-thus-required-to-be-globally-unique-in-azure"></a>服务器的名称会映射到 DNS 名称，因此该名称在 Azure 中需具有全局级别的唯一性
# <a name="substitute-the-serveradminpassword-with-your-own-value"></a>将 <server_admin_password> 替换为你自己的值
az mariadb server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location chinaeast2 \
--admin-user myadmin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2 \

# <a name="list-the-configuration-options-for-review"></a>列出供查看的配置选项
az mariadb server configuration list \
--resource-group myresourcegroup  \
--server mydemoserver

# <a name="turn-on-statement-level-log"></a>启用语句级别日志
az mariadb server configuration set \
--name log_statement \
--resource-group myresourcegroup \
--server mydemoserver \
--value all

# <a name="set-logmindurationstatement-time-to-10-sec"></a>将 log_min_duration_statement 时间设置为 10 秒
az mariadb server configuration set \
--name log_min_duration_statement \
--resource-group myresourcegroup \
--server mydemoserver \
--value 10000

# <a name="list-the-available-log-files-and-direct-to-a-text-file"></a>列出可用的日志文件并将其定向到某个文本文件
az mariadb server-logs list \
--resource-group myresourcegroup \
--server mydemoserver > log_files_list.txt

# <a name="download-log-file-from-azure"></a>从 Azure 下载日志文件 
# <a name="review-logfileslisttxt-to-find-the-server-log-file-name-for-the-desired-timeframe"></a>查看 log_files_list.txt，找出所需时间范围的服务器日志文件名称
# <a name="substitute-the-logfilename-with-your-server-log-file-name"></a>将 <log_file_name> 替换为你的服务器日志文件名称
# <a name="creates-the-postgresql-date000000log-file-in-the-current-command-line-path"></a>在当前的命令行路径中创建 postgresql-<date>_000000.log 文件
az mariadb server-logs download \
--name <log_file_name> \
--resource-group myresourcegroup \
--server mydemoserver
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
| [az mariadb server configuration list](/cli/mariadb/server/configuration#az-mariadb-server-configuration-list) | 列出服务器的配置值。 |
| [az mariadb server configuration set](/cli/mariadb/server/configuration#az-mariadb-server-configuration-set) | 更新服务器的配置。 |
| [az mariadb server-logs list](/cli/mariadb/server-logs#az-mariadb-server-logs-list) | 列出服务器的日志文件。 |
| [az mariadb server-logs download](/cli/mariadb/server-logs#az-mariadb-server-logs-download) | 下载日志文件。 |
| [az group delete](/cli/group#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 阅读有关 Azure CLI 的更多信息：[Azure CLI 文档](/cli/)。
- 请尝试其他脚本：[Azure Database for MariaDB 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
