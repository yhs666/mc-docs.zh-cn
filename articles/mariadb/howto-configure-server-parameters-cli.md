---
title: 在 Azure Database for MariaDB 中配置服务参数
description: 本文介绍了如何使用 Azure CLI 命令行实用工具在 Azure Database for MariaDB 中配置服务参数。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
origin.date: 11/09/2018
ms.date: 07/22/2019
ms.openlocfilehash: b072ab48688acfe083d029c695d76a9e504cce29
ms.sourcegitcommit: 1dac7ad3194357472b9c0d554bf1362c391d1544
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308883"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>使用 Azure CLI 自定义服务器配置参数
可以使用 Azure CLI、Azure 命令行实用工具来列出、显示和更新 Azure Database for MariaDB 服务器的配置参数。 在服务器级别会公开引擎配置的一个子集，并可以进行修改。

## <a name="prerequisites"></a>先决条件
若要逐步执行本操作方法指南，需要：
- [Azure Database for MariaDB 服务器](quickstart-create-mariadb-server-database-using-azure-cli.md)
- [Azure CLI](/cli/install-azure-cli) 命令行实用工具。

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>列出 Azure Database for MariaDB 服务器的服务器配置参数
若要列出服务器中的所有可修改参数及其值，请运行 [az mariadb server configuration list](/cli/mariadb/server/configuration#az-mariadb-server-configuration-list) 命令。

可以列出资源组 **myresourcegroup** 下服务器 **mydemoserver.mariadb.database.azure.com** 的服务器配置参数。
```azurecli
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

有关每个列出参数的定义，请参阅[服务器系统变量](https://mariadb.com/kb/en/library/server-system-variables/)上的 MariaDB 参考部分。

## <a name="show-server-configuration-parameter-details"></a>显示服务器配置参数详细信息
若要显示服务器的某个特定配置参数的详细信息，请运行 [az mariadb server configuration show](/cli/mariadb/server/configuration#az-mariadb-server-configuration-show) 命令。

本示例显示了资源组 **myresourcegroup** 下服务器 **mydemoserver.mariadb.database.chinacloudapi.cn** 的服务器配置参数 **slow\_query\_log** 的详细信息。
```azurecli
az mariadb server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>修改服务器配置参数值
此外，你还可以修改某个服务器配置参数的值，这会更新 MariaDB 服务器引擎的基础配置值。 若要更新配置，请使用 [az mariadb server configuration set](/cli/mariadb/server/configuration#az-mariadb-server-configuration-set) 命令。 

更新资源组 **myresourcegroup** 下服务器 **mydemoserver.mariadb.database.chinacloudapi.cn** 的服务器配置参数 **slow\_query\_log**。
```azurecli
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```

若要重置配置参数的值，省去可选的 `--value` 参数，服务将应用默认值。 对于上述示例，它将如下所示：
```azurecli
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

此代码会将 slow\_query\_log  配置重置为默认值 OFF  。 

## <a name="next-steps"></a>后续步骤

- 如何配置 [Azure 门户中的服务器参数](howto-server-parameters.md)
