---
title: Azure CLI 脚本 - 创建 Azure Database for MariaDB
description: 此示例 CLI 脚本创建 Azure Database for MariaDB 服务器，并配置服务器级防火墙规则。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
origin.date: 11/28/2018
ms.date: 05/27/2019
ms.openlocfilehash: 02c8cfb408f93da92c2d8fa8a13fc36b5897222e
ms.sourcegitcommit: 60169f39663ae62016f918bdfa223c411e249883
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66173245"
---
# <a name="create-a-mariadb-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>使用 Azure CLI 创建 MariaDB 服务器并配置防火墙规则
此示例 CLI 脚本创建 Azure Database for MariaDB 服务器，并配置服务器级防火墙规则。 成功运行此脚本后，可以通过所有 Azure 服务和配置的 IP 地址访问 MariaDB 服务器。

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

# <a name="configure-a-firewall-rule-for-the-server"></a>配置服务器的防火墙规则
# <a name="the-ip-address-range-that-you-want-to-allow-to-access-your-server"></a>你希望其能够访问服务器的 IP 地址范围
az mariadb server firewall-rule create \
--resource-group myresourcegroup \
--server mydemoserver \
--name AllowIps \
--start-ip-address 0.0.0.0 \
--end-ip-address 255.255.255.255
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
| [az mariadb server firewall create](/cli/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create) | 创建一个防火墙规则，以允许从输入的 IP 地址范围访问服务器及其下的所有数据库。 |
| [az group delete](/cli/group#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 阅读有关 Azure CLI 的更多信息：[Azure CLI 文档](/cli)。
- 请尝试其他脚本：[Azure Database for MariaDB 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
