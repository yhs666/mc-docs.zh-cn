---
title: Azure 流分析作业的输出类型
description: 了解有关设定流分析数据输出选项（包括 Power BI）目标，用于分析结果的信息。
services: stream-analytics
author: rockboyfor
ms.author: v-yeche
manager: digimobile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 04/16/2018
ms.date: 05/07/2018
ms.openlocfilehash: 5a1bb488cba83ecdfda8bac924aefc845e4a721f
ms.sourcegitcommit: c3084384ec9b4d313f4cf378632a27d1668d6a6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
---
# <a name="stream-analytics-outputs-options-for-storage-and-analysis"></a>流分析输出：存储和分析选项
创作流分析作业时，需考虑如何使用生成的数据。 如何查看流分析作业的结果？流分析作业的结果存储在何处？

为了启用多种应用程序模式，Azure 流分析提供了不同的选项来存储输出和查看分析结果。 这样可以轻松地查看作业输出，并可灵活地使用和存储作业输出，以便进行数据仓库操作和其他操作。 必须先存在作业中配置的输出，才能启动作业并开始事件的流动。 例如，如果使用 Blob 存储作为输出，该作业将不会自动创建存储帐户。 启动流分析作业之前，请创建一个存储帐户。
<!-- Not Available ## Azure Data Lake Store-->
<!-- Not Available ### Authorize an Azure Data Lake Store -->
<!-- Not Available ### Renew Data Lake Store Authorization -->

## <a name="sql-database"></a>SQL 数据库
可以将 [Azure SQL 数据库](https://www.azure.cn/home/features/sql-database/)用作本质上为关系型数据的输出，也可以将其用于所依赖的内容在关系数据库中托管的应用程序。 流分析作业将写入到 Azure SQL 数据库的现有表中。  请注意表架构必须与字段及其正从作业输出的类型完全匹配。 [Azure SQL 数据仓库](/sql-data-warehouse/)也可以通过 SQL 数据库输出选项指定为输出。 下表列出了属性名称和用于创建 SQL 数据库输出的属性说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此数据库。 |
| 数据库 |数据库的名称（正在向该数据库发送输出） |
| 服务器名称 |SQL 数据库服务器名称 |
| 用户名 |有权写入到数据库的用户名 |
| 密码 |用于连接到数据库的密码 |
| 表 |将写入输出的表名称。 表名称区分大小写，并且该表架构应与字段数量以及作业输出正在生成的字段类型完全匹配。 |

> [!NOTE]
> 目前，流分析中的作业输出支持 Azure SQL 数据库产品/服务。 但是，不支持附加了数据库，运行 SQL Server 的 Azure 虚拟机。 这在将来的版本中可能会有所改变。
> 
> 

## <a name="blob-storage"></a>Blob 存储
Blob 存储提供了一种经济高效且可缩放的解决方案，用于在云中存储大量非结构化数据。  有关 Azure Blob 存储及其用法的简介，请参阅[如何使用 Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) 处的文档。

下表列出了用于创建 blob 输出的属性名称及其说明。

<table>
<tbody>
<tr>
<td>属性名称</td>
<td>说明</td>
</tr>
<tr>
<td>输出别名</td>
<td>该名称是在查询中使用的友好名称，用于将查询输出定向到此 blob 存储。</td>
</tr>
<tr>
<td>存储帐户</td>
<td>存储帐户的名称（正在向该存储帐户发送输出）。</td>
</tr>
<tr>
<td>存储帐户密钥</td>
<td>与存储帐户关联的密钥。</td>
</tr>
<tr>
<td>存储容器</td>
<td>容器对存储在 Azure Blob 服务中的 blob 进行逻辑分组。 将 blob 上传到 Blob 服务时，必须为该 blob 指定一个容器。</td>
</tr>
<tr>
<td>路径前缀模式 [可选]</td>
<td>用于编写指定容器中的 blob 的文件路径模式。 <BR> 在路径模式中，可以选择使用以下 2 个变量的一个或多个实例指定 blob 写入的频率： <BR> {date}、{time} <BR> 示例 1：cluster1/logs/{date}/{time} <BR> 示例 2：cluster1/logs/{date} <BR> <BR> 文件命名将遵循以下约定： <BR> {路径前缀模式}/schemaHashcode_Guid_Number.extension <BR> <BR> 示例输出文件： <BR> Myoutput/20170901/00/45434_gguid_1.csv <BR> Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR> 另外还存在已创建新文件的情况： <BR> 1. 当前文件超出了允许的最大块数（目前为 50,000） <BR> 2. 在输出架构中进行更改 <BR> 3. 在外部或内部重启作业  </td>
</tr>
<tr>
<td>日期格式 [可选]</td>
<td>如果在前缀路径中使用日期令牌，可以选择组织文件所采用的日期格式。 示例：YYYY/MM/DD</td>
</tr>
<tr>
<td>时间格式 [可选]</td>
<td>如果在前缀路径中使用时间令牌，可指定组织文件所采用的时间格式。 目前唯一支持的值是 HH。</td>
</tr>
<tr>
<td>事件序列化格式</td>
<td>输出数据的序列化格式。  支持 JSON、CSV 和 Avro。</td>
</tr>
<tr>
<td>编码</td>
<td>如果是 CSV 或 JSON 格式，则必须指定一种编码格式。 目前只支持 UTF-8 这种编码格式。</td>
</tr>
<tr>
<td>分隔符</td>
<td>仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。</td>
</tr>
<tr>
<td>格式</td>
<td>仅适用于 JSON 序列化。 分隔行指定了通过新行分隔各个 JSON 对象，从而格式化输出。 数组指定输出会被格式化为 JSON 对象的数组。 仅当作业停止或流分析移动到下个时间段时，才关闭此数组。 一般而言，最好使用分隔行 JSON，因为在继续写入输出文件时，无需任何特殊处理。</td>
</tr>
</tbody>
</table>

当使用 blob 存储作为输出时，在以下情况下 blob 中创建一个新文件：

* 如果此文件超过了允许块 （请注意，可能但未到达最大允许的 blob 大小达到最大允许块数量最大数量。 例如，如果输出率很高，你可以看到每个块，更多字节和文件大小大于。 当输出率不足时，每个块都有较少的数据，且文件大小较小。  
* 如果在输出中，架构更改，并且输出格式需要固定的架构（CSV 和 Avro）。  
* 如果重启作业是外部或内部重启的作业。  
* 如果完全分区查询，为每个输出分区创建新文件。  
* 如果用户删除文件或存储帐户的容器。  
* 如果了输出，则使用路径前缀模式分区的时间，当查询移动到下一个小时，则使用新 blob。

## <a name="event-hub"></a>事件中心
[事件中心](https://www.azure.cn/home/features/event-hubs/)是具有高扩展性的发布-订阅事件引入器。 事件中心每秒可收集数百万个事件。 当流分析作业的输出将要成为另一个流式处理作业的输入时，可以将事件中心用作输出。

将事件中心数据流配置成输出时，需要使用几个参数。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此事件中心。 |
| 服务总线命名空间 |服务总线命名空间是包含一组消息传递实体的容器。 创建新的事件中心后，还创建了服务总线命名空间 |
| 事件中心 |事件中心输出的名称 |
| 事件中心策略名称 |可以在事件中心的“配置”选项卡上创建的共享访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 事件中心策略密钥 |用于验证对服务总线命名空间的访问权限的共享访问密钥 |
| 分区键列 [可选] |此列包含事件中心输出的分区键。 |
| 事件序列化格式 |输出数据的序列化格式。  支持 JSON、CSV 和 Avro。 |
| 编码 |对于 CSV 和 JSON，目前只支持 UTF-8 这种编码格式 |
| 分隔符 |仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 格式 |仅适用于 JSON 序列化。 分隔行指定了通过新行分隔各个 JSON 对象，从而格式化输出。 数组指定输出会被格式化为 JSON 对象的数组。 仅当作业停止或流分析移动到下个时间段时，才关闭此数组。 一般而言，最好使用分隔行 JSON，因为在继续写入输出文件时，无需任何特殊处理。 |


<!-- Not Available ## Power BI-->
<!-- Not Available ### Authorize a Power BI account-->
<!-- Not Available ### Configure the Power BI output properties-->
<!-- Not Available ### Authorize a Power BI account-->
<!-- Not Available ### Authorize a Power BI account-->

## <a name="table-storage"></a>表存储
[Azure 表存储](../storage/common/storage-introduction.md)提供了具有高可用性且可大规模缩放的存储，因此应用程序可以自动缩放以满足用户需求。 表存储是世纪互联推出的 NoSQL 键/属性存储，适用于对架构的约束较少的结构化数据。 Azure 表存储可用于持久地存储数据，方便进行高效的检索。

下表列出了属性名称和用于创建表输出的属性说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此表存储。 |
| 存储帐户 |存储帐户的名称（正在向该存储帐户发送输出）。 |
| 存储帐户密钥 |与存储帐户关联的访问密钥。 |
| 表名称 |表的名称。 如果表不存在，则创建新表。 |
| 分区键 |包含分区键的输出列的名称。 分区键是某个给定表中分区的唯一标识符，分区键构成了实体主键的第一部分。 分区键是一个最大为 1 KB 的字符串值。 |
| 行键 |包含行键的输出列的名称。 行键是某个给定分区中实体的唯一标识符。 行键构成了实体主键的第二部分。 行键是一个最大为 1 KB 的字符串值。 |
| 批大小 |批处理操作的记录数。 通常情况下，默认值对于大多数作业来说已经足够；若要修改此设置，请参阅[表批处理操作规范](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx)以获取详细信息。 |

## <a name="service-bus-queues"></a>服务总线队列
[服务总线队列](../service-bus-messaging/service-bus-queues-topics-subscriptions.md)为一个或多个竞争使用方提供先入先出 (FIFO) 消息传递方式。 通常情况下，接收方会按照消息添加到队列中的临时顺序来接收并处理消息，并且每条消息仅由一个消息使用方接收并处理。

下表列出了用于创建队列输出的属性名称及其说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此服务总线队列。 |
| 服务总线命名空间 |服务总线命名空间是包含一组消息传递实体的容器。 |
| 队列名称 |服务总线队列的名称。 |
| 队列策略名称 |在创建队列时，还可以在“队列配置”选项卡上创建共享的访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 队列策略密钥 |用于验证访问服务总线命名空间的共享访问密钥 |
| 事件序列化格式 |输出数据的序列化格式。  支持 JSON、CSV 和 Avro。 |
| 编码 |对于 CSV 和 JSON，目前只支持 UTF-8 这种编码格式 |
| 分隔符 |仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |
| 格式 |仅适用于 JSON 类型。 分隔行指定了通过新行分隔各个 JSON 对象，从而格式化输出。 数组指定输出会被格式化为 JSON 对象的数组。 |

## <a name="service-bus-topics"></a>服务总线主题
服务总线队列提供的是从发送方到接收方的一对一通信方法，而[服务总线主题](../service-bus-messaging/service-bus-queues-topics-subscriptions.md)提供的则是一对多形式的通信。

下表列出了用于创建表输出的属性名称及其说明。

| 属性名称 | 说明 |
| --- | --- |
| 输出别名 |该名称是在查询中使用的友好名称，用于将查询输出定向到此服务总线主题。 |
| 服务总线命名空间 |服务总线命名空间是包含一组消息传递实体的容器。 创建新的事件中心后，还创建了服务总线命名空间 |
| 主题名称 |主题是消息传递实体，类似于事件中心和队列。 设计用于从多个不同的设备和服务收集事件流。 在创建主题时，还会为其提供特定的名称。 发送到主题的消息在创建订阅后才会提供给用户，因此请确保主题下存在一个或多个订阅 |
| 主题策略名称 |创建主题时，还可以在“主题配置”选项卡上创建共享的访问策略。每个共享访问策略具有名称、所设权限以及访问密钥。 |
| 主题策略密钥 |用于验证对服务总线命名空间的访问权限的共享访问密钥 |
| 事件序列化格式 |输出数据的序列化格式。  支持 JSON、CSV 和 Avro。 |
 | 编码 |如果是 CSV 或 JSON 格式，则必须指定一种编码格式。 目前只支持 UTF-8 编码格式 |
| 分隔符 |仅适用于 CSV 序列化。 流分析支持大量的常见分隔符以对 CSV 格式的数据进行序列化。 支持的值为逗号、分号、空格、制表符和竖线。 |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://www.azure.cn/home/features/documentdb/) 是一种多区域分布的多模型数据库服务，它提供中国范围内不设限的弹性缩放、丰富查询和自动索引（经由与架构无关的数据模型）、可靠的低延迟及行业领先的综合 SLA。 若要了解流分析的 Cosmos DB 集合选项，请参阅[将 Cosmos DB 用作输出的流分析](stream-analytics-documentdb-output.md)一文。

> [!Note]
> 目前，Azure 流分析仅支持使用 SQL API 连接到 CosmosDB。
> 尚不支持使用其他 Azure Cosmos DB API。 如果使用其他 API 将 Azure 流分析指向 创建的 Azure Cosmos DB 帐户，则可能无法正确存储数据。 

下表描述了用于创建 Azure Cosmos DB 输出的属性。
| 属性名称 | 说明 |
| --- | --- |
| 输出别名 | 用于在流分析查询中引用此输出的别名。 |
| 接收器 | Cosmos DB |
| 导入选项 | 选择“从订阅中选择 Cosmos DB”或“手动提供 Cosmos DB 设置”。
| 帐户 ID | Cosmos DB 帐户的名称或终结点 URI。 |
| 帐户密钥 | Cosmos DB 帐户的共享访问密钥。 |
| 数据库 | Cosmos DB 数据库名称。 |
| 集合名称模式 | 要使用的集合的集合名称或其模式。 <br/>可以使用可选的 {partition} 令牌（其中分区从 0 开始）构造集合名称格式。 两个示例：  <br/>1. _MyCollection_ - 必须存在一个名为“MyCollection”的集合。  <br/>2._MyCollection{partition}_ - 基于分区依据列。 <br/>分区依据列集合必须存在 -“MyCollection0”、“MyCollection1”、“MyCollection2”等。 |
| 分区键 | 可选。 仅当你在集合名称模式中使用 {partition} 令牌时，才需要此项。<br/> 分区键是输出事件中字段的名称，该字段用于指定跨集合分区输出的键。<br/> 对于单个集合输出，可使用任何任意输出列（例如 PartitionId）。 |
| 文档 ID |可选。 输出事件中的字段的名称，该字段用于指定插入或更新操作所基于的主键。  
<!-- Not Available on 3rd May 2018 ## Azure Functions -->
## <a name="get-help"></a>获取帮助
如需进一步的帮助，请尝试我们的 [Azure 流分析论坛](https://www.azure.cn/support/contact/)

## <a name="next-steps"></a>后续步骤
我们已经向你介绍了流分析，这是一种托管服务，适用于对物联网的数据进行流式分析。 若要了解有关此服务的详细信息，请参阅：

* [Azure 流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
<!-- URL is not Correct on [stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md -->
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

<!--Update_Description: update link, wroding update, add cosmos db output content -->