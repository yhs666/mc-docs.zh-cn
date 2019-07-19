---
title: 使用 Python 创建 Azure 数据资源管理器群集和数据库
description: 了解如何使用 Python 创建 Azure 数据资源管理器群集和数据库。
author: oflipman
ms.author: v-biyu
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
origin.date: 03/25/2019
ms.date: 07/22/2019
ms.openlocfilehash: ae0f5eff89f86179e3ec7c92def53536f536648c
ms.sourcegitcommit: ea5dc30371bc63836b3cfa665cc64206884d2b4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717338"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>使用 Python 创建 Azure 数据资源管理器群集和数据库

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Azure 数据资源管理器是一项快速、完全托管的数据分析服务，用于实时分析从应用程序、网站和 IoT 设备等资源流式传输的海量数据。 若要使用 Azure 数据资源管理器，请先创建群集，再在该群集中创建一个或多个数据库。 然后将数据引入（加载）到数据库，以便对其运行查询。 在本文中，将使用 Python 创建群集和数据库。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="install-python-package"></a>安装 Python 包

要为 Azure 数据资源管理器 (Kusto) 安装 Python 包，请打开其路径中包含 Python 的命令提示符。 运行以下命令：

```
pip install azure-mgmt-kusto
```

## <a name="create-the-azure-data-explorer-cluster"></a>创建 Azure 数据资源管理器群集

1. 请使用以下命令创建群集：

    ```Python
    from azure.mgmt.kusto.kusto_management_client import KustoManagementClient
    from azure.mgmt.kusto.models import Cluster, AzureSku

    credentials = xxxxxxxxxxxxxxx
    
    subscription_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx'
    location = 'China East2'
    sku = 'D11_v2'
    capacity = 5
    resource_group_name = 'testrg'
    cluster_name = 'mykustocluster'
    cluster = Cluster(location=location, sku=AzureSku(name=sku, capacity=capacity))
    
    kustoManagementClient = KustoManagementClient(credentials, subscription_id)
    
    cluster_operations = kustoManagementClient.clusters
    
    cluster_operations.create_or_update(resource_group_name, cluster_name, cluster)
    ```

   |**设置** | **建议的值** | **字段说明**|
   |---|---|---|
   | cluster_name | mykustocluster  | 所需的群集名称。|
   | sku | *D11_v2* | 将用于群集的 SKU。 |
   | resource_group_name | *testrg* | 将在其中创建群集的资源组名称。 |

    可以使用其他可选参数，例如群集的容量。
    
1. 设置[凭据  ](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python)

1. 运行以下命令，检查群集是否已成功创建：

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

如果结果包含带 `Succeeded` 值的 `provisioningState`，则表示已成功创建群集。

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>在 Azure 数据资源管理器群集中创建数据库

1. 请使用以下命令创建数据库：

    ```Python
    from azure.mgmt.kusto.models import Database
    from datetime import timedelta
    
    softDeletePeriod = timedelta(days=3650)
    hotCachePeriod = timedelta(days=3650)
    databaseName="mykustodatabase"
    
    database_operations = kusto_management_client.databases 
    _database = Database(location=location,
                        soft_delete_period=softDeletePeriod,
                        hot_cache_period=hotCachePeriod)
    
    database_operations.create_or_update(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName, parameters = _database)
    ```

   |**设置** | **建议的值** | **字段说明**|
   |---|---|---|
   | cluster_name | mykustocluster  | 将在其中创建数据库的群集的名称。|
   | database_name | mykustodatabase  | 数据库名称。|
   | resource_group_name | *testrg* | 将在其中创建群集的资源组名称。 |
   | soft_delete_period | 3650 天，0:00:00  | 供查询使用的数据的保留时间。 |
   | hot_cache_period | 3650 天，0:00:00  | 数据将在缓存中保留的时间。 |

1. 若要查看已创建的数据库，请运行以下命令：

    ```Python
    database_operations.get(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName)
    ```

现在，你有了一个群集和一个数据库。

## <a name="clean-up-resources"></a>清理资源

* 如果计划学习我们的其他文章，请保留已创建的资源。
* 若要清理资源，请删除群集。 删除群集时，也会删除其中的所有数据库。 使用以下命令删除群集：

    ```Python
    cluster_operations.delete(resource_group_name = resource_group_name, cluster_name = clusterName)
    ```

## <a name="next-steps"></a>后续步骤

* [使用 Azure 数据资源管理器 Python 库引入数据](python-ingest-data.md)
