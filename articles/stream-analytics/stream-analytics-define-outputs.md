---
title: 了解 Azure 流分析的输出
description: 本文介绍 Azure 流分析提供的数据输出选项，包括用于分析结果的 Power BI。
services: stream-analytics
author: rockboyfor
ms.author: v-yeche
manager: digimobile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 05/14/2018
ms.date: 09/17/2018
ms.openlocfilehash: c69590c9202ae9cd472e1db5c173dee2b4166a80
ms.sourcegitcommit: 2700f127c3a8740a83fb70739c09bd266f0cc455
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2018
ms.locfileid: "45586622"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>了解 Azure 流分析的输出
本文将介绍适用于 Azure 流分析作业的不同类型的输出。 输出可帮助存储和保存流分析作业的结果。 使用输出数据，可进一步进行业务分析和数据的数据仓储。 

设计流分析查询时，使用 [INTO 子句](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics)引用输出的名称。 可针对每个作业使用单个输出，如果需要，也可通过在查询中提供多个 INTO 子句，针对每个流式处理作业使用多个输出。

要创建、编辑和测试流分析作业输出，可使用 [Azure 门户](stream-analytics-quick-create-portal.md#configure-output-to-the-job)、[Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job)、[.Net API](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet) 和 [REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output)。
<!-- Not Available on [Visual Studio](stream-analytics-quick-create-vs.md)-->

部分输出类型支持[分区](#partitioning)，并且[输出批大小](#output-batch-size)可变化以优化吞吐量。
<!-- Not Available ## Azure Data Lake Store-->
<!-- Not Available on Azure Data Lake Store output from Stream Analytics is currently not available in the Azure China (21Vianet) and Azure Germany (T-Systems International) regions.-->
<!-- Not Available ### Authorize an Azure Data Lake Store -->
<!-- Not Available ### Renew Data Lake Store Authorization -->

## <a name="sql-database"></a>SQL 数据库
可以将 [Azure SQL 数据库](https://www.azure.cn/home/features/sql-database/)用作本质上为关系型数据的输出，也可以将其用于所依赖的内容在关系数据库中托管的应用程序。 流分析作业将写入到 Azure SQL 数据库的现有表中。  请注意表架构必须与字段及其正从作业输出的类型完全匹配。 [Azure SQL 数据仓库](/sql-data-warehouse/)也可以通过 SQL 数据库输出选项指定为输出。 下表列出了属性名称和用于创建 SQL 数据库输出的属性说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |在查询中使用的友好名称，用于将查询输出定向到此数据库。 |
| 数据库 | 数据库的名称（正在向该数据库发送输出）。 |
| 服务器名称 | SQL 数据库服务器名称。 |
| 用户名 | 有权写入到数据库的用户名。 |
| 密码 | 用于连接到数据库的密码。 |
| 表 | 将写入输出的表名称。 表名称区分大小写，并且该表架构应与字段数量以及作业输出正在生成的字段类型完全匹配。 |

> [!NOTE]
> 目前，流分析中的作业输出支持 Azure SQL 数据库产品/服务。 但是，不支持带有附加数据库，运行 SQL Server 的 Azure 虚拟机。 这在将来的版本中可能会有所改变。
> 

## <a name="blob-storage"></a>Blob 存储
Blob 存储提供了一种经济高效且可扩展的解决方案，用于在云中存储大量非结构化数据。  有关 Azure Blob 存储及其用法的简介，请参阅[如何使用 Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) 处的文档。

下表列出了属性名称和用于创建 blob 输出的属性说明。

| 属性名称       | 说明                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| 输出别名        | 查询中使用的友好名称，用于将查询输出定向到此 blob 存储。 |
| 存储帐户     | 存储帐户的名称（正在向该存储帐户发送输出）。               |
| 存储帐户密钥 | 与存储帐户关联的密钥。                              |
| 存储容器   | 容器对存储在 Azure Blob 服务中的 blob 进行逻辑分组。 将 blob 上传到 Blob 服务时，必须为该 blob 指定一个容器。 |
| 路径模式 | 可选。 用于写入指定容器中的 blob 的文件路径模式。 <br /><br /> 在路径模式中，可以选择使用数据时间变量的一个或多个实例指定 blob 写入的频率： <br /> {date}、{time} <br /><br />如果已注册[预览版](https://aka.ms/ASAPreview)，则可以从事件数据中指定一个自定义 {field} 名称来对 blob 进行分区。 字段名称是字母数字，并且可以包含空格、连字符和下划线。 对自定义字段的限制包括以下内容： <ul><li>不区分大小写（不区分列“ID”和列“id”）</li><li>不允许嵌套字段（在作业查询中改用别名来“平展”字段）</li><li>不能使用表达式作为字段名称。</li></ul> <br /><br /> 在预览版中，还可以在路径中使用自定义日期/时间格式说明符配置。 一次只能指定一个自定义日期和时间格式，并用 {datetime:\<specifier>} 关键字括起来。 允许的输入 \<specifier> 为 yyyy、MM、M、dd、d、HH、H、mm、m、ss 或 s。 可能会在路径中多次使用 {datetime:\<specifier>} 关键字以形成自定义日期/时间配置。 <br /><br />示例： <ul><li>示例 1：cluster1/logs/{date}/{time}</li><li>示例 2：cluster1/logs/{date}</li><li>示例 3（预览版）：cluster1/{client_id}/{date}/{time}</li><li>示例 4（预览版）：cluster1/{datetime:ss}/{myField}，其中查询为：SELECT data.myField AS myField FROM Input;</li><li>示例 5（预览版）：cluster1/year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}</ul><br /><br />创建的文件夹结构的时间戳遵循 UTC 而不是本地时间。<br /><br/>文件命名将遵循以下约定： <br /><br />{路径前缀模式}/schemaHashcode_Guid_Number.extension<br /><br />示例输出文件：<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> |
| 日期格式 | 可选。 如果在前缀路径中使用日期令牌，可以选择组织文件所采用的日期格式。 示例：YYYY/MM/DD |
| 时间格式 | 可选。 如果在前缀路径中使用时间令牌，可以指定组织文件所采用的时间格式。 目前唯一支持的值是 HH。 |
| 事件序列化格式 | 输出数据的序列化格式。  支持 JSON、CSV 和 Avro。 |
| 编码    | 如果使用 CSV 或 JSON 格式，则必须指定一种编码格式。 目前只支持 UTF-8 这种编码格式。 |
| 分隔符   | 仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 格式      | 仅适用于 JSON 序列化。 分隔行指定通过新行分隔各个 JSON 对象，从而格式化输出。 数组指定输出会被格式化为 JSON 对象的数组。 仅当作业停止或流分析移动到下个时间段时，才关闭此数组。 一般而言，最好使用分隔行 JSON，因为在继续写入输出文件时，无需任何特殊处理。 |

当使用 blob 存储作为输出时，在以下情况下 blob 中创建一个新文件：

* 如果文件超出了允许的最大块数（目前为 50,000）。 可达到允许的最大块数，但不能达到允许的最大 blob 大小。 例如，如果输出率很高，则可以看到每个块的字节更多，并且文件大小会更大。 输出率较低时，每个块都有较少的数据，且文件大小较小。
* 如果输出中出现架构更改，输出格式也需要固定的架构（CSV 和 Avro）。  
* 如果作业重新启动，可选择在外部由用户停止或启动，或在内部进行系统维护或错误恢复。  
* 如果对查询进行完全分区，会为每个输出分区创建新文件。  
* 如果用户删除存储帐户的文件或容器。  
* 如果使用路径前缀模式对输出进行了时间分区，当查询移动到下一个小时后，会使用新的 blob。
* 如果按自定义字段对输出进行分区，则每个分区键都会创建新的 blob（如果不存在）。
* 如果按照自定义字段对输出进行分区（其中分区键基数超过 8000），则可能每个分区键创建一个新的 blob。

## <a name="event-hub"></a>事件中心
[Azure 事件中心](https://www.azure.cn/home/features/event-hubs/)服务是具有高扩展性的发布 - 订阅事件引入器。 事件中心每秒可收集数百万个事件。 当流分析作业的输出成为另一个流式处理作业的输入时，可以将事件中心用作输出。

将事件中心数据流配置为输出时，需要使用几个参数。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 | 查询中使用的友好名称，用于将查询输出定向到此事件中心。 |
| 事件中心命名空间 |事件中心命名空间是包含一组消息传递实体的容器。 创建新的事件中心后，还创建了事件中心命名空间。 |
| 事件中心名称 | 事件中心输出的名称。 |
| 事件中心策略名称 | 可以在事件中心的“配置”选项卡上创建的共享访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 事件中心策略密钥 | 用于对事件中心命名空间的访问权限进行身份验证的共享访问密钥。 |
| 分区键列 [可选] | 此列包含事件中心输出的分区键。 |
| 事件序列化格式 | 输出数据的序列化格式。  支持 JSON、CSV 和 Avro。 |
| 编码 | 对于 CSV 和 JSON，目前只支持 UTF-8 这种编码格式。 |
| 分隔符 | 仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 格式 | 仅适用于 JSON 序列化。 分隔行指定通过新行分隔各个 JSON 对象，从而格式化输出。 数组指定输出会被格式化为 JSON 对象的数组。 仅当作业停止或流分析移动到下个时间段时，才关闭此数组。 一般而言，最好使用分隔行 JSON，因为在继续写入输出文件时，无需任何特殊处理。 |


<!-- Not Available ## Power BI-->
<!-- Not Available on Power BI output from Stream Analytics is currently not available in the Azure China (21Vianet) and Azure Germany (T-Systems International) regions.-->
<!-- Not Available ### Authorize a Power BI account-->
<!-- Not Available ### Configure the Power BI output properties-->
<!-- Not Available ### Authorize a Power BI account-->
<!-- Not Available ### Renew Power BI Authorization-->

## <a name="table-storage"></a>表存储
[Azure 表存储](../storage/common/storage-introduction.md)提供了具有高可用性且可大规模缩放的存储，因此应用程序可以自动缩放以满足用户需求。 表存储是世纪互联推出的 NoSQL 键/属性存储，适用于对架构的约束较少的结构化数据。 Azure 表存储可用于持久地存储数据，方便进行高效的检索。

下表列出了属性名称和用于创建表输出的属性说明。

| 属性名称 | description |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此表存储。 |
| 存储帐户 |存储帐户的名称（正在向该存储帐户发送输出）。 |
| 存储帐户密钥 |与存储帐户关联的访问密钥。 |
| 表名称 |表的名称。 如果表不存在，则创建新表。 |
| 分区键 |包含分区键的输出列的名称。 分区键是给某个定表中分区的唯一标识，分区键构成了实体主键的第一部分。 分区键是一个最大为 1 KB 的字符串值。 |
| 行键 |包含行键的输出列的名称。 行键是某个给定分区中实体的唯一标识符。 行键构成了实体主键的第二部分。 行键是一个最大为 1 KB 的字符串值。 |
| 批大小 |批处理操作的记录数。 默认值 (100) 对大部分作业来说都已足够。 有关修改设置的详细信息，请参阅[表批处理操作规范](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx)。 |

## <a name="service-bus-queues"></a>服务总线队列
[服务总线队列](../service-bus-messaging/service-bus-queues-topics-subscriptions.md)为一个或多个竞争使用方提供先入先出 (FIFO) 消息传递方式。 通常情况下，接收方会按照消息添加到队列中的临时顺序来接收并处理消息，并且每条消息仅由一个消息使用方接收并处理。

下表列出了属性名称和用于创建队列输出的属性说明。

| 属性名称 | description |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此服务总线队列。 |
| 服务总线命名空间 |服务总线命名空间是包含一组消息传递实体的容器。 |
| 队列名称 |服务总线队列的名称。 |
| 队列策略名称 |在创建队列时，还可以在“队列配置”选项卡上创建共享的访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 队列策略密钥 |用于验证访问服务总线命名空间的共享访问密钥 |
| 事件序列化格式 |输出数据的序列化格式。  支持 JSON、CSV 和 Avro。 |
| 编码 |对于 CSV 和 JSON，目前只支持 UTF-8 这种编码格式 |
| 分隔符 |仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 格式 |仅适用于 JSON 类型。 分隔行指定通过新行分隔各个 JSON 对象，从而格式化输出。 数组指定输出会被格式化为 JSON 对象的数组。 |

分区数[基于服务总线 SKU 和大小](../service-bus-messaging/service-bus-partitioning.md)。 分区键是每个分区的唯一整数值。

## <a name="service-bus-topics"></a>服务总线主题
服务总线队列提供的是从发送方到接收方的一对一通信方法，而[服务总线主题](../service-bus-messaging/service-bus-queues-topics-subscriptions.md)提供的则是一对多形式的通信。

下表列出了属性名称和用于创建表输出的属性说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此服务总线主题。 |
| 服务总线命名空间 |服务总线命名空间是包含一组消息传递实体的容器。 创建新的事件中心后，还创建了服务总线命名空间 |
| 主题名称 |主题是消息传送实体，类似于事件中心和队列。 之所以设计队列，是为了从多个不同的设备和服务收集事件流。 在创建主题时，还会为其提供特定的名称。 发送到主题的消息在创建订阅后才会提供给用户，因此请确保主题下存在一个或多个订阅 |
| 主题策略名称 |创建主题时，还可以在“主题配置”选项卡上创建共享的访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 主题策略密钥 |用于验证访问服务总线命名空间的共享访问密钥 |
| 事件序列化格式 |输出数据的序列化格式。  支持 JSON、CSV 和 Avro。 |
| 编码 |如果使用 CSV 或 JSON 格式，则必须指定一种编码格式。 目前只支持 UTF-8 这种编码格式 |
| 分隔符 |仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |

分区数[基于服务总线 SKU 和大小](../service-bus-messaging/service-bus-partitioning.md)。 分区键是每个分区的唯一整数值。

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://www.azure.cn/home/features/documentdb/) 是一种多区域分布的多模型数据库服务，它提供中国范围内不设限的弹性缩放、丰富查询和自动索引（经由与架构无关的数据模型）、可靠的低延迟及行业领先的综合 SLA。 若要了解流分析的 Cosmos DB 集合选项，请参阅[将 Cosmos DB 用作输出的流分析](stream-analytics-documentdb-output.md)一文。

<!-- Not Available on Azure Cosmos DB output from Stream Analytics is currently not available in the Azure China (21Vianet) and Azure Germany (T-Systems International) regions.-->

> [!Note]
> 目前，Azure 流分析仅支持使用 SQL API 连接到 CosmosDB。
> 尚不支持使用其他 Azure Cosmos DB API。 如果使用其他 API 将 Azure 流分析指向 创建的 Azure Cosmos DB 帐户，则可能无法正确存储数据。 

下表描述了用于创建 Azure Cosmos DB 输出的属性。
| 属性名称 | description |
| --- | --- |
| 输出别名 | 用于在流分析查询中引用此输出的别名。 |
| 接收器 | Cosmos DB |
| 导入选项 | 选择“从订阅中选择 Cosmos DB”或“手动提供 Cosmos DB 设置”。
| 帐户 ID | Cosmos DB 帐户的名称或终结点 URI。 |
| 帐户密钥 | Cosmos DB 帐户的共享访问密钥。 |
| 数据库 | Cosmos DB 数据库名称。 |
| 集合名称模式 | 要使用的集合的集合名称或其模式。 <br/>可以使用可选的 {partition} 令牌（其中分区从 0 开始）构造集合名称格式。 两个示例：  <br/>1. _MyCollection_ - 必须存在一个名为“MyCollection”的集合。  <br/>2._MyCollection{partition}_ - 基于分区依据列。 <br/>分区依据列集合必须存在 -“MyCollection0”、“MyCollection1”、“MyCollection2”等。 |
| 分区键 | 可选。 仅当你在集合名称模式中使用 {partition} 令牌时，才需要此项。<br/> 分区键是输出事件中字段的名称，该字段用于指定跨集合分区输出的键。<br/> 对于单个集合输出，可使用任何任意输出列。 例如 PartitionId。 |
| 文档 ID |可选。 输出事件中的字段的名称，该字段用于指定插入或更新操作所基于的主键。  

<!-- Not Available on ## Azure Functions-->
<!-- Not Available on Azure Functions output from Stream Analytics is currently not available in the Azure China (21Vianet) and Azure Germany (T-Systems International) regions.-->
<!-- Notice From Global site on 07/16/2018-->
## <a name="partitioning"></a>分区

下表汇总了分区支持，以及每个输出类型的输出写入器数目：

| 输出类型 | 分区支持 | 分区键  | 输出写入器数目 | 
| --- | --- | --- | --- |
| Azure SQL 数据库 | 否 | 无 | 不适用。 | 
| Azure Blob 存储 | 是 | 在 Path 模式中使用事件字段中的 {date} 和 {time} 标记。 选择日期格式，例如 YYYY/MM/DD、DD/MM/YYYY、MM-DD-YYYY。 HH 用于时间格式。 作为[预览版](https://aka.ms/ASAPreview)的一部分，可以通过单个自定义事件属性 {fieldname} 或 {datetime:\<specifier>} 对 blob 输出进行分区。 | 按照[完全可并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 | 
| Azure 事件中心 | 是 | 是 | 按分区对齐方式变化。</br> 输出事件中心分区键与上游（上一个）查询步骤相同时，编写器的数量与输出事件中心分区的数量相同。 各编写器使用 EventHub 的 [EventHubSender class](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) 将事件发送到特定分区。 </br> 输出事件中心分区键与上游（上一个）查询步骤不相同时，编写器的数量与之前步骤中的分区数量不相同。 各编写器使用 EventHubClient [SendBatchAsync class](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) 将事件发送到所有输出分区。 |
| Azure 表存储 | 是 | 任何输出列。  | 按照[完全并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 | 
| Azure 服务总线主题 | 是 | 自动选择。 分区数基于[服务总线 SKU 和大小](../service-bus-messaging/service-bus-partitioning.md)。 分区键是每个分区的唯一整数值。| 与输出主题中的分区数量相同。  |
| Azure 服务总线队列 | 是 | 自动选择。 分区数基于[服务总线 SKU 和大小](../service-bus-messaging/service-bus-partitioning.md)。 分区键是每个分区的唯一整数值。| 与输出队列中的分区数量相同。 |
| Azure Cosmos DB | 是 | 在 Collection 命名模式中使用 {partition} 标记。 {partition} 值基于查询中的 PARTITION BY 子句。 | 按照[完全并行化的查询](stream-analytics-scale-jobs.md)的输入分区。 |

<!-- Not Available on | Azure Function -->
<!-- Not Available on | Azure Data Lake Store -->
<!-- Not Available on | Power BI -->

## <a name="output-batch-size"></a>输出批大小
Azure 流分析使用大小可变的批来处理事件和写入到输出。 通常流分析引擎不会一次写入一条消息，而是使用批来提高效率。 输入和输出事件速率都很高时，它会使用更大的批。 输出速率低时，使用较小的批来保证低延迟。 

下表介绍了输出批处理的一些注意事项：

| 输出类型 | 最大消息大小 | 批大小优化 |
| :--- | :--- | :--- | 
| Azure SQL 数据库 | 每个单次批量插入最大行数 10000</br>每个单次批量插入最小行数 100 </br>另请参阅 [Azure SQL 限制](../sql-database/sql-database-resource-limits.md) |  每个批最初都以最大批大小来批量插入，并可能会根据来自 SQL 的可重试错误将批拆分为二（直到达到最小批大小）。 |
| Azure Blob 存储 | 参阅 [Azure 存储限制](../azure-subscription-service-limits.md#storage-limits) | 最大 Blob 块大小为 4 MB</br>最大 blob 块计数为 50000 |
| Azure 事件中心   | 每个消息 256 KB </br>另请参阅[事件中心限制](../event-hubs/event-hubs-quotas.md) |    输入输出分区不相同时，每个事件会分别打包到 EventData 中，并按批发送（批的大小不得超出最大消息大小，高级 SKU 的最大消息大小为 1 MB）。 </br></br>  输入输出分区相同时，多个事件打包到单个 EventData，以最大消息大小发送。    |
| Azure 表存储 | 参阅 [Azure 存储限制](../azure-subscription-service-limits.md#storage-limits) | 默认为每个单一事务 100 个实体，并且可根据需要配置为更小的值。 |
| Azure 服务总线队列   | 每个消息 256 KB</br> 另请参阅[服务总线限制](../service-bus-messaging/service-bus-quotas.md) | 每个消息单一事件 |
| Azure 服务总线主题 | 每个消息 256 KB</br> 另请参阅[服务总线限制](../service-bus-messaging/service-bus-quotas.md) | 每个消息单一事件 |

<!-- Not Available on | Azure Cosmos DB -->
<!-- Not Available on | Azure Function -->
<!-- Not Available on | Azure Data Lake Store -->
<!-- Not Available on | Power BI -->

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [快速入门：使用 Azure 门户创建流分析作业](stream-analytics-quick-create-portal.md)

<!--Link references-->
<!-- URL is not Correct on [stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md --> [stream.analytics.scale.jobs]：stream-analytics-scale-jobs.md [stream.analytics.introduction]：stream-analytics-introduction.md [stream.analytics.get.started]：stream-analytics-real-time-fraud-detection.md [stream.analytics.query.language.reference]：http://go.microsoft.com/fwlink/?LinkID=513299 [stream.analytics.rest.api.reference]：http://go.microsoft.com/fwlink/?LinkId=517301

<!--Update_Description: update link, wroding update -->