---
title: Azure CLI 脚本 - 缩放 Azure Database for PostgreSQL
description: Azure CLI 脚本示例 - 在查询指标后用于 PostgreSQL 服务器的 Azure 数据库缩放为不同的性能级别。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.devlang: azurecli
ms.custom: mvc
ms.topic: sample
origin.date: 04/05/2018
ms.date: 12/31/2018
ms.openlocfilehash: 1c21a1beb2a9cced250edc0a2abdd18765aeb715
ms.sourcegitcommit: e96e0c91b8c3c5737243f986519104041424ddd5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806178"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>使用 Azure CLI 监视和缩放单个 PostgreSQL 服务器
此示例 CLI 脚本在查询指标后用于 PostgreSQL 服务器的单个 Azure 数据库缩放为不同的性能级别。 

本文需要 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI]( /cli/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。 将 `az monitor` 命令中使用的 SubscriptionID 替换为自己的订阅 ID。

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

# Monitor usage metrics - CPU
az monitor metrics list \
--resource "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver" \
--metric cpu_percent \
--interval PT1M

# Monitor usage metrics - Storage
az monitor metrics list \
--resource "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver" \
--metric storage_used \
--interval PT1M

# Scale up the server to provision more vCores within the same Tier
az postgres server update \
--resource-group myresourcegroup \
--name mydemoserver \
--sku-name GP_Gen5_4

# Scale up the server to provision a storage size of 7GB
az postgres server update \
--resource-group myresourcegroup \
--name mydemoserver \
--storage-size 7168
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
| [az monitor metrics list](/cli/monitor/metrics#az_monitor_metrics_list) | 列出资源的指标值。 |
| [az group delete](/cli/group#az_group_delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 阅读有关 Azure CLI 的更多信息：[Azure CLI 文档](/cli)
- 请尝试其他脚本：[用于 PostgreSQL 的 Azure 数据库的 Azure CLI 示例](../sample-scripts-azure-cli.md)
- 阅读有关缩放的详细信息：[服务层](../concepts-service-tiers.md)以及[计算单位和存储单元](../concepts-compute-unit-and-storage.md)
