---
title: Azure CLI：创建 SQL 数据库 | Azure
description: 了解如何使用 Azure CLI 创建 SQL 数据库逻辑服务器、服务器级防火墙规则和数据库。
keywords: SQL 数据库教程：创建 SQL 数据库
services: sql-database
author: forester123
manager: digimobile
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.devlang: azurecli
ms.topic: quickstart
origin.date: 10/11/2017
ms.date: 11/06/2017
ms.author: v-johch
ms.openlocfilehash: 2b500bbed608656496e308bc9f61750c581cba17
ms.sourcegitcommit: 2793c9971ee7a0624bd0777d9c32221561b36621
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2018
---
# <a name="create-a-single-azure-sql-database-using-the-azure-cli"></a>使用 Azure CLI 创建单一 Azure SQL 数据库

Azure CLI 用于从命令行或脚本创建和管理 Azure 资源。 本指南详述了如何使用 Azure CLI 在 [Azure 资源组](../azure-resource-manager/resource-group-overview.md)的 [Azure SQL 数据库逻辑服务器](sql-database-features.md)中部署 Azure SQL 数据库。

如果没有 Azure 订阅，可在开始前创建一个 [1 元人民币试用](https://www.azure.cn/pricing/1rmb-trial/)帐户。


本主题需要运行 Azure CLI 2.0.4 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.azure.cn/cli/install-azure-cli)。 

## <a name="define-variables"></a>定义变量

定义在本快速入门的脚本中使用的变量。

```azurecli
# The data center and resource name for your resources
export resourcegroupname = myResourceGroup
export location = "China East"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
export servername = server-$RANDOM
# Set an admin login and password for your database
export adminlogin = ServerAdmin
export password = ChangeYourAdminPassword1
# The ip address range that you want to allow to access your DB
export startip = "0.0.0.0"
export endip = "0.0.0.0"
# The database name
export databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/group#create) 命令创建 [Azure 资源组](../azure-resource-manager/resource-group-overview.md)。 资源组是在其中以组的形式部署和管理 Azure 资源的逻辑容器。 以下示例在 `China East` 位置创建名为 `myResourceGroup` 的资源组。

```azurecli
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a>创建逻辑服务器

使用 [az sql server create](/cli/sql/server#create) 命令创建 [Azure SQL 数据库逻辑服务器](sql-database-features.md)。 逻辑服务器包含一组作为组管理的数据库。 以下示例使用管理员用户名 `ServerAdmin` 和密码 `ChangeYourAdminPassword1` 在资源组中创建随机命名的服务器。 根据需要替换这些预定义的值。

```azurecli
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a>配置服务器防火墙规则

使用 [az sql server firewall create](/cli/sql/server/firewall-rule#create) 命令创建 [Azure SQL 数据库服务器级防火墙规则](sql-database-firewall-configure.md)。 服务器级防火墙规则允许外部服务器（例如 SQL Server Management Studio 或 SQLCMD 实用程序）通过 SQL 数据库服务防火墙连接到 SQL 数据库。 在以下示例中，防火墙仅对其他 Azure 资源开放。 要启用外部连接，请将 IP 地址更改为适合环境的地址。 若要开放所有 IP 地址，请使用 0.0.0.0 作为起始 IP 地址，使用 255.255.255.255 作为结束地址。  

```azurecli
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> 通过端口 1433 进行 SQL 数据库通信。 如果尝试从企业网络内部进行连接，则该网络的防火墙可能不允许经端口 1433 的出站流量。 如果是这样，则无法连接到 Azure SQL 数据库服务器，除非 IT 部门打开了端口 1433。
>

## <a name="create-a-database-in-the-server-with-sample-data"></a>使用示例数据在服务器中创建数据库

使用 [az sql db create](/cli/sql/db#create) 命令在服务器中创建 [S0 性能级别](sql-database-service-tiers.md)的数据库。 以下示例创建名为 `mySampleDatabase` 的数据库，并将 AdventureWorksLT 示例数据加载到该数据库中。 根据需要替换这些预定义的值（此集合中的其他快速入门基于此快速入门中的值）。

```azurecli
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a>清理资源

本教程系列中的其他快速入门教程是在本文的基础上制作的。 

> [!TIP]
> 如果计划继续使用后续的快速入门，请勿清除在本快速入门中创建的资源。 如果不打算继续，请在 Azure 门户中执行以下步骤，删除通过此快速入门创建的所有资源。
>

```azurecli
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a>后续步骤

有了数据库以后，即可使用偏好的工具进行连接和查询。 若要了解详细信息，请选择下面的工具：

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

<!--Update_Description:update Global CLI 2.0 links to Mooncake CLI 2.0 links-->
