---
title: 使用 Python 为 Azure 数据资源管理器创建事件中心数据连接
description: 本文介绍如何使用 Python 为 Azure 数据资源管理器创建事件中心数据连接。
author: lucygoldbergmicrosoft
ms.author: v-tawe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
origin.date: 12/16/2019
ms.openlocfilehash: 7df408ee3674c0a2d930c575adff71bddb7f079c
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335938"
---
# <a name="create-an-event-hub-data-connection-for-azure-data-explorer-by-using-python"></a>使用 Python 为 Azure 数据资源管理器创建事件中心数据连接

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-hub.md)
> * [C#](data-connection-event-hub-csharp.md)
> * [Python](data-connection-event-hub-python.md)

Azure 数据资源管理器是一项快速且高度可缩放的数据探索服务，适用于日志和遥测数据。 Azure 数据资源管理器提供了从事件中心、IoT 中心和写入 blob 容器的 blob 引入数据（数据加载）的功能。 在本文中，你将使用 Python 为 Azure 数据资源管理器创建事件中心数据连接。

## <a name="prerequisites"></a>先决条件

* 如果没有 Azure 订阅，请在开始前创建一个[试用 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)。

* 创建[群集和数据库](create-cluster-database-csharp.md)

* 创建[表和列映射](net-standard-ingest-data.md#create-a-table-on-your-test-cluster)

* 设置[数据库和表策略](database-table-policies-csharp.md)（可选）

* 创建[包含要引入的数据的事件中心](ingest-data-event-hub.md#create-an-event-hub)。 

[!INCLUDE [data-explorer-data-connection-install-package-python](../../includes/data-explorer-data-connection-install-package-python.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-event-hub-data-connection"></a>添加事件中心数据连接

以下示例演示如何以编程方式添加事件中心数据连接。 请参阅[连接到事件中心](ingest-data-event-hub.md#connect-to-the-event-hub)，了解如何使用 Azure 门户添加事件中心数据连接。

```Python
from azure.mgmt.kusto import KustoManagementClient
from azure.mgmt.kusto.models import EventHubDataConnection
from azure.common.credentials import ServicePrincipalCredentials

#Directory (tenant) ID
tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Application ID
client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Client Secret
client_secret = "xxxxxxxxxxxxxx"
subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )
kusto_management_client = KustoManagementClient(credentials, subscription_id)

resource_group_name = "testrg";
#The cluster and database that are created as part of the Prerequisites
cluster_name = "mykustocluster";
database_name = "mykustodatabase";
data_connection_name = "myeventhubconnect";
#The event hub that is created as part of the Prerequisites
event_hub_resource_id = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.EventHub/namespaces/xxxxxx/eventhubs/xxxxxx";
consumer_group = "$Default";
location = "China East 2";
#The table and column mapping that are created as part of the Prerequisites
table_name = "StormEvents";
mapping_rule_name = "StormEvents_CSV_Mapping";
data_format = "csv";
#Returns an instance of LROPoller, check https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
poller = kusto_management_client.data_connections.create_or_update(resource_group_name=resource_group_name, cluster_name=cluster_name, database_name=database_name, data_connection_name=data_connection_name,
                                        parameters=EventHubDataConnection(event_hub_resource_id=event_hub_resource_id, consumer_group=consumer_group, location=location,
                                                                            table_name=table_name, mapping_rule_name=mapping_rule_name, data_format=data_format))
```

|**设置** | **建议的值** | **字段说明**|
|---|---|---|
| tenant_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | 租户 ID。 也称为目录 ID。|
| subscriptionId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | 用于创建资源的订阅 ID。|
| client_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | 可以访问租户中资源的应用程序的客户端 ID。|
| client_secret | *xxxxxxxxxxxxxx* | 可以访问租户中资源的应用程序的客户端密码。 |
| resource_group_name | *testrg* | 包含群集的资源组的名称。|
| cluster_name | mykustocluster  | 群集的名称。|
| database_name | mykustodatabase  | 群集中目标数据库的名称。|
| data_connection_name | *myeventhubconnect* | 所需的数据连接名称。|
| table_name | *StormEvents* | 目标数据库中目标表的名称。|
| mapping_rule_name | *StormEvents_CSV_Mapping* | 与目标表相关的列映射的名称。|
| data_format | *csv* | 消息的数据格式。|
| event_hub_resource_id | *资源 ID* | 包含要引入的数据的事件中心的资源 ID。 |
| consumer_group | *$Default* | 事件中心的使用者组。|
| location | *中国东部 2* | 数据连接资源的位置。|

[!INCLUDE [data-explorer-data-connection-clean-resources-python](../../includes/data-explorer-data-connection-clean-resources-python.md)]