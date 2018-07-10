---
title: Azure CLI 脚本 - 缩放 Azure Database for MySQL 服务器
description: 此示例 CLI 脚本在查询指标后用于 MySQL 服务器的 Azure 数据库缩放为不同的性能级别。
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 06/16/2018
ms.openlocfilehash: acdc6924e68f031cf53e15ddd5e2cacc4c1e3753
ms.sourcegitcommit: 3d17c1b077d5091e223aea472e15fcb526858930
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2018
ms.locfileid: "37873275"
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>使用 Azure CLI 监视和缩放用于 MySQL 服务器的 Azure 数据库

> [!NOTE]
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql/)。

此示例 CLI 脚本在查询指标后用于 MySQL 服务器的单个 Azure 数据库缩放为不同的性能级别。 本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI 2.0]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。 

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。 将 `az monitor` 命令中使用的订阅 ID 替换为自己的订阅 ID。

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

# Monitor usage metrics - Compute
az monitor metrics list \
--resource-id "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresource/providers/Microsoft.DBforMySQL/servers/mysqlserver4demo" \
--metric-names compute_consumption_percent \
--time-grain PT1M

# Monitor usage metrics - Storage
az monitor metrics list \
--resource-id "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresource/providers/Microsoft.DBforMySQL/servers/mysqlserver4demo" \
--metric-names storage_used \
--time-grain PT1M

# Scale up the server to provision more Compute Units within the same Tier
az mysql server update \
--resource-group myresource \
--name mysqlserver4demo \
--compute-units 100
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
| [az mysql server create](/cli/mysql/server#az_mysql_server_create) | 创建用于托管数据库的 MySQL 服务器。 |
| [az monitor metrics list](/cli/monitor/metrics#az_monitor_metrics_list) | 列出资源的指标值。 |
| [az group delete](/cli/group#az_group_delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。
- 尝试其他脚本：[Azure Database for MySQL 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
- 有关缩放的详细信息，请参阅[服务层](../concepts-service-tiers.md)和[计算单元和存储单元](../concepts-compute-unit-and-storage.md)。
