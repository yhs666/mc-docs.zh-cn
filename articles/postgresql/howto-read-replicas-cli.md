---
title: 通过 Azure CLI、REST API 管理 Azure Database for PostgreSQL（单一服务器）的只读副本
description: 了解如何通过 Azure CLI 和 REST API 管理 Azure Database for PostgreSQL（单一服务器）中的只读副本
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 09/12/2019
ms.date: 09/30/2019
ms.openlocfilehash: 8e867f2ca350892ece42b488c1b8e56e508d006c
ms.sourcegitcommit: 849418188e5c18491ed1a3925829064935d2015c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2019
ms.locfileid: "71307866"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli-rest-api"></a>通过 Azure CLI、REST API 创建和管理只读副本

本文介绍如何使用 Azure CLI 和 REST API 在 Azure Database for PostgreSQL 中创建和管理只读副本。 若要详细了解只读副本，请参阅[概述](concepts-read-replicas.md)。

## <a name="azure-cli"></a>Azure CLI
可以使用 Azure CLI 创建和管理只读副本。

### <a name="prerequisites"></a>先决条件

- [安装 Azure CLI 2.0](/cli/install-azure-cli?view=azure-cli-latest)
- 用作主服务器的 [Azure Database for PostgreSQL 服务器](quickstart-create-server-up-azure-cli.md)。


### <a name="prepare-the-master-server"></a>准备主服务器
必须使用以下步骤在“常规用途”和“内存优化”层中准备主服务器。

主服务器上的 `azure.replication_support` 参数必须设置为 **REPLICA**。 更改此静态参数后，需要重启服务器才能使更改生效。

1. 将 `azure.replication_support` 设置为 REPLICA。

   ```azurecli
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

2. 重启服务器以应用更改。

   ```azurecli
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

### <a name="create-a-read-replica"></a>创建只读副本

[az postgres server replica create](/cli/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) 命令需要以下参数：

| 设置 | 示例值 | 说明  |
| --- | --- | --- |
| resource-group | myresourcegroup |  要在其中创建副本服务器的资源组。  |
| name | mydemoserver-replica | 所创建的新副本服务器的名称。 |
| source-server | mydemoserver | 要从中进行复制的现有主服务器的名称或资源 ID。 |

在下面的 CLI 示例中，副本在主服务器所在的区域中创建。

```azurecli
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

若要创建跨区域只读副本，请使用 `--location` 参数。 下面的 CLI 示例在中国北部创建副本。

```azurecli
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup --location chinanorth
```

> [!NOTE]
> 若要详细了解可以在哪些区域中创建副本，请访问[只读副本概念文章](concepts-read-replicas.md)。 

如果尚未在“常规用途”或“内存优化”主服务器上将 `azure.replication_support` 参数设置为 **REPLICA** 并重启服务器，将会收到错误。 请在创建副本之前完成这两个步骤。

使用与主服务器相同的计算和存储设置创建副本。 创建副本后，可以独立于主服务器更改多项设置：计算代系、vCore 数、存储和备份保留期。 定价层也可以独立更改，但“基本”层除外。

> [!IMPORTANT]
> 将主服务器设置更新为新值之前，请将副本设置更新为一个相等或更大的值。 此操作可帮助副本与主服务器发生的任何更改保持同步。

### <a name="list-replicas"></a>列出副本
可以使用 [az postgres server replica list](/cli/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) 命令查看主服务器的副本的列表。

```azurecli
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

### <a name="stop-replication-to-a-replica-server"></a>停止复制到副本服务器
可以使用 [az postgres server replica stop](/cli/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) 命令停止在主服务器和只读副本之间的复制。

停止复制到主服务器和只读副本后，无法撤消该操作。 只读副本将成为支持读取和写入的独立服务器。 独立服务器不能再次成为副本。

```azurecli
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

### <a name="delete-a-master-or-replica-server"></a>删除主服务器或副本服务器
若要删除主服务器或副本服务器，请使用 [az postgres server delete](/cli/postgres/server?view=azure-cli-latest#az-postgres-server-delete) 命令。

删除主服务器后，将停止复制到所有只读副本。 只读副本将成为支持读取和写入的独立服务器。

```azurecli
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="rest-api"></a>REST API
可以使用 [Azure REST API](/rest/api/azure/) 创建和管理只读副本。

### <a name="prepare-the-master-server"></a>准备主服务器
必须使用以下步骤在“常规用途”和“内存优化”层中准备主服务器。

主服务器上的 `azure.replication_support` 参数必须设置为 **REPLICA**。 更改此静态参数后，需要重启服务器才能使更改生效。

1. 将 `azure.replication_support` 设置为 REPLICA。

   ```http
   PUT https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/configurations/azure.replication_support?api-version=2017-12-01
   ```

   ```JSON
   {
    "properties": {
    "value": "replica"
    }
   }
   ```

2. [重启服务器](https://docs.microsoft.com/rest/api/postgresql/servers/restart)以应用更改。

   ```http
   POST https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/restart?api-version=2017-12-01
   ```

### <a name="create-a-read-replica"></a>创建只读副本
可以使用[创建 API](https://docs.microsoft.com/rest/api/postgresql/servers/create) 创建只读副本：

```http
PUT https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "chinaeast",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}"
  }
}
```

> [!NOTE]
> 若要详细了解可以在哪些区域中创建副本，请访问[只读副本概念文章](concepts-read-replicas.md)。 

如果尚未在“常规用途”或“内存优化”主服务器上将 `azure.replication_support` 参数设置为 **REPLICA** 并重启服务器，将会收到错误。 请在创建副本之前完成这两个步骤。

使用与主服务器相同的计算和存储设置创建副本。 创建副本后，可以独立于主服务器更改多项设置：计算代系、vCore 数、存储和备份保留期。 定价层也可以独立更改，但“基本”层除外。


> [!IMPORTANT]
> 将主服务器设置更新为新值之前，请将副本设置更新为一个相等或更大的值。 此操作可帮助副本与主服务器发生的任何更改保持同步。

### <a name="list-replicas"></a>列出副本
可以使用[副本列表 API](https://docs.microsoft.com/rest/api/postgresql/replicas/listbyserver) 查看主服务器的副本列表：

```http
GET https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>停止复制到副本服务器
可以使用[更新 API](https://docs.microsoft.com/rest/api/postgresql/servers/update) 停止主服务器与只读副本之间的复制。

停止复制到主服务器和只读副本后，无法撤消该操作。 只读副本将成为支持读取和写入的独立服务器。 独立服务器不能再次成为副本。

```http
PATCH https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-master-or-replica-server"></a>删除主服务器或副本服务器
若要删除主服务器或副本服务器，请使用[删除 API](https://docs.microsoft.com/rest/api/postgresql/servers/delete)：

删除主服务器后，将停止复制到所有只读副本。 只读副本将成为支持读取和写入的独立服务器。

```http
DELETE https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforPostgreSQL/servers/{serverName}?api-version=2017-12-01
```

## <a name="next-steps"></a>后续步骤
* 详细了解 [Azure Database for PostgreSQL 中的只读副本](concepts-read-replicas.md)。
* 了解如何[在 Azure 门户中创建和管理只读副本](howto-read-replicas-portal.md)。