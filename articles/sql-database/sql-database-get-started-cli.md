---
title: "Azure CLI：创建 SQL 数据库 | Azure"
description: "了解如何使用 Azure CLI 创建 SQL 数据库逻辑服务器、服务器级防火墙规则和数据库。"
keywords: "SQL 数据库教程：创建 SQL 数据库"
services: sql-database
documentationcenter: 
author: Hayley244
manager: digimobile
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
origin.date: 04/17/2017
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 3a388378fb0d0adcfb430d5f1c53137024300327
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 使用 Azure CLI 创建单一 Azure SQL 数据库
<a id="create-a-single-azure-sql-database-using-the-azure-cli" class="xliff"></a>

Azure CLI 用于从命令行或脚本创建和管理 Azure 资源。 本指南详述了如何使用 Azure CLI 在 [Azure 资源组](../azure-resource-manager/resource-group-overview.md)的 [Azure SQL 数据库逻辑服务器](sql-database-features.md)中部署 Azure SQL 数据库。

若要完成本快速入门教程，请确保已安装最新的 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)。 

如果没有 Azure 订阅，可在开始前创建一个 [1 元人民币试用](https://www.azure.cn/pricing/1rmb-trial/)帐户。

## 登录 Azure
<a id="log-in-to-azure" class="xliff"></a>

使用 [az login](https://docs.microsoft.com/cli/azure/#login) 命令登录到 Azure 订阅，并按照屏幕上的说明进行操作。

```azurecli
az cloud set -n AzureChinaCloud
az login 
```

## 定义变量
<a id="define-variables" class="xliff"></a>

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
export endip = "0.0.0.1"
# The database name
export databasename = mySampleDatabase
```

## 创建资源组
<a id="create-a-resource-group" class="xliff"></a>

使用 [az group create](https://docs.microsoft.com/cli/azure/group#create) 命令创建 [Azure 资源组](../azure-resource-manager/resource-group-overview.md)。 资源组是在其中以组的形式部署和管理 Azure 资源的逻辑容器。 以下示例在 `China East` 位置创建名为 `myResourceGroup` 的资源组。

```azurecli
az group create --name $resourcegroupname --location $location
```
## 创建逻辑服务器
<a id="create-a-logical-server" class="xliff"></a>

使用 [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#create) 命令创建 [Azure SQL 数据库逻辑服务器](sql-database-features.md)。 逻辑服务器包含一组作为组管理的数据库。 以下示例使用管理员用户名 `ServerAdmin` 和密码 `ChangeYourAdminPassword1` 在资源组中创建随机命名的服务器。 根据需要替换这些预定义的值。

```azurecli
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## 配置服务器防火墙规则
<a id="configure-a-server-firewall-rule" class="xliff"></a>

使用 [az sql server firewall create](https://docs.microsoft.com/cli/azure/sql/server/firewall-rule#create) 命令创建 [Azure SQL 数据库服务器级防火墙规则](sql-database-firewall-configure.md)。 服务器级防火墙规则允许外部服务器（例如 SQL Server Management Studio 或 SQLCMD 实用程序）通过 SQL 数据库服务防火墙连接到 SQL 数据库。 在以下示例中，防火墙仅对其他 Azure 资源开放。 若要启用外部连接，请将 IP 地址更改为适合环境的地址。 若要开放所有 IP 地址，请使用 0.0.0.0 作为起始 IP 地址，使用 255.255.255.255 作为结束地址。  

```azurecli
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> 通过端口 1433 进行 SQL 数据库通信。 如果尝试从企业网络内部进行连接，则该网络的防火墙可能不允许经端口 1433 的出站流量。 如果是这样，则无法连接到 Azure SQL 数据库服务器，除非 IT 部门打开了端口 1433。
>

## 使用示例数据在服务器中创建数据库
<a id="create-a-database-in-the-server-with-sample-data" class="xliff"></a>

使用 [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#create) 命令在服务器中创建 [S0 性能级别](sql-database-service-tiers.md)的数据库。 以下示例创建名为 `mySampleDatabase` 的数据库，并将 AdventureWorksLT 示例数据加载到该数据库中。 根据需要替换这些预定义的值（此集合中的其他快速入门基于此快速入门中的值）。

```azurecli
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## 清理资源
<a id="clean-up-resources" class="xliff"></a>

本教程系列中的其他快速入门教程是在本文的基础上制作的。 

> [!TIP]
> 如果计划继续使用后续的快速入门，请勿清除在本快速入门中创建的资源。 如果不打算继续，请在 Azure 门户中执行以下步骤，删除通过此快速入门创建的所有资源。
>

```azurecli
az group delete --name $resourcegroupname
```

## 后续步骤
<a id="next-steps" class="xliff"></a>

有了数据库以后，即可使用偏好的工具进行连接和查询。 若要了解详细信息，请选择下面的工具：

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

