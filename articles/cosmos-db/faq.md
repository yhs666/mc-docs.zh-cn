---
title: "Azure Cosmos DB 常见问题解答 | Azure"
description: "获取有关 Azure Cosmos DB（多区域分布式多模型数据库服务）的常见问题的解答。 了解容量、性能级别和缩放。"
keywords: "数据库问题, 常见问题, documentdb, azure, Microsoft azure"
services: cosmos-db
author: rockboyfor
manager: digimobile
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/02/2018
ms.date: 01/29/2018
ms.author: v-yeche
ms.openlocfilehash: 09ce758ac13ff702981b68cc74a90143335db088
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
<!-- meta.description: GLOBALLY to multiple-region -->
# <a name="azure-cosmos-db-faq"></a>Azure Cosmos DB 常见问题解答
## <a name="azure-cosmos-db-fundamentals"></a>Azure Cosmos DB 基础知识
### <a name="what-is-azure-cosmos-db"></a>什么是 Azure Cosmos DB？

Azure Cosmos DB 是多区域复制的多模型数据库服务，可针对无架构数据提供丰富的查询，帮助提供可靠的可配置性能，并支持快速开发。 这一切都通过一个托管平台来实现，而该平台有 Azure 强大的功能与影响力作为后盾。 
<!-- GLOBALLY to multiple-region -->

如果 Web、移动、游戏和 IoT 应用程序的关键要求是可预测的吞吐量、高可用性、低延迟和无架构数据模型，那么，Azure Cosmos DB 无疑是最合适的解决方案。 它提供了架构灵活性和丰富的索引，并且对集成的 JavaScript 提供了多文档事务性支持。 

有关部署和使用此服务的更多数据库问题、解答及说明，请参阅 [Azure Cosmos DB 文档页面]((https://docs.azure.cn/cosmos-db/)。

### <a name="what-happened-to-the-documentdb-api"></a>DocumentDB API 有哪些变化？

Azure Cosmos DB DocumentDB API 或 SQL (DocumentDB) API 现在称为 Azure Cosmos DB SQL API。 无需进行任何更改即可继续运行使用 DocumentDB API 构建的应用。 功能保持相同。

如果你以前有一个 DocumentDB API 帐户，则现在有一个 SQL API 帐户，费用不会有任何变化。 

### <a name="what-happened-to-azure-documentdb-as-a-service"></a>用作服务 Azure DocumentDB 发生了什么变化？

Azure DocumentDB 服务现在是 Azure Cosmos DB 服务的一部分，自身以 SQL API 形式呈现。 无需针对 Azure Cosmos DB SQL API 进行任何更改，就能运行针对 Azure DocumentDB 生成的应用程序。 此外，Azure Cosmos DB 还支持表 API 和 MongoDB API。
<!--Not Available on  Graph API (Preview), and Cassandra API (Preview) -->

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Azure Cosmos DB 的典型用例有哪些？
对于侧重于以下要求的新 Web、移动、游戏和 IoT 应用程序而言，Azure Cosmos DB 是一个不错的选择：自动缩放、可预测的性能、毫秒响应时间的快速排序，以及查询无架构数据的能力。 Azure Cosmos DB 有助于快速开发，且支持应用程序数据模型的连续迭代。 用于管理用户生成的内容和数据的应用程序就是 [Azure Cosmos DB 的常见用例](use-cases.md)。 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Azure Cosmos DB 如何提供可预测的性能？
[请求单位](request-units.md) (RU) 是 Azure Cosmos DB 中吞吐量的衡量单位。 1 RU 吞吐量相当于通过 GET 操作获取 1 KB 文档的吞吐量。 在 Azure Cosmos DB 中进行的每个操作（包括读、写、SQL 查询和执行存储过程）都具有一个确定的 RU 值，该值基于完成该操作所需的吞吐量。 无需考虑 CPU、IO 和内存，以及它们会怎样影响应用程序吞吐量，只需从单个 RU 度量值的角度进行考虑即可。

可以按照每秒吞吐量的 RU 数来保留每个 Azure Cosmos DB 容器的预配吞吐量。 对于任何规模的应用程序，都可以通过将单个请求设为基准来测量其 RU 值，并通过预配容器来处理所有请求的请求单位总和。 也可以根据应用程序的发展需求，相应地增加或减少容器的吞吐量。 如需请求单位的详细信息以及确定容器需求方面的帮助，请参阅[估计吞吐量需求](request-units.md#estimating-throughput-needs)并尝试使用[吞吐量计算器](https://www.documentdb.com/capacityplanner)。 在这里，术语“容器”是指 SQL API 集合、MongoDB API 集合以及表 API 表。 
<!-- Not Available on  Graph -->

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-and-document"></a>Azure Cosmos DB 如何支持各种数据模型（例如键/值、纵栏表和文档）？
键/值（表）、纵栏表和文档数据模型都是原本就支持的，因为 Azure Cosmos DB 是基于 ARS（原子、记录和序列）设计构建的。 原子、记录和序列可以轻松映射和投影到各种数据模型。 现在提供了适用于一部分模型（SQL、MongoDB 和表 API）的 API，将来会提供特定于其他数据模型的其他 API。
<!-- Not Available on  Graph -->

Azure Cosmos DB 有一个不受架构影响的索引编制引擎，能够自动为它引入的所有数据编制索引，不会要求开发者提供任何架构或辅助索引。 该引擎依赖于一组逻辑索引布局（倒置、纵栏表、树形），这些布局将存储布局从索引和查询处理子系统中分离出来。 Cosmos DB 还能够以可扩展方式支持一组连网协议和 API 并将它们高效地转换为核心数据模型 (1) 和逻辑索引布局 (2)，这使得它具有原本就支持多个数据模型的独特功能。

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Azure Cosmos DB 是否符合 HIPAA？
是的，Azure Cosmos DB 符合 HIPAA。 HIPAA 针对可识别个人身份的健康信息的使用、泄露与保护制定了要求。 有关详细信息，请参阅 [Microsoft 信任中心](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA)。

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Azure Cosmos DB 的存储限制是什么？
对于容器可以存储在 Azure Cosmos DB 中的数据总量并没有任何限制。

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Azure Cosmos DB 的吞吐量限制是什么？
在 Azure Cosmos DB 中，对于容器能够支持的总吞吐量并没有任何限制。 关键是要让工作负荷大致均匀地分布到足够多的分区键中。

### <a name="how-much-does-azure-cosmos-db-cost"></a>Azure Cosmos DB 的费用是多少？
有关详细信息，请参阅 [Azure Cosmos DB 定价详细信息](https://www.azure.cn/pricing/details/cosmos-db/)页。 Azure Cosmos DB 使用费取决于预配的容器数、容器的联机小时数，以及每个容器的预配吞吐量。 在这里，术语“容器”是指 SQL API 集合、MongoDB API 集合以及表 API 表。 
<!-- Not Avaialbe  Graph API graph -->

### <a name="is-a-trial-account-available"></a>有试用帐户吗？
<!-- Not Available [Try Azure Cosmos DB for free](https://www.azure.cn/try/cosmosdb/) -->
如果不熟悉 Azure，可以注册 [Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial/)，这样可以得到 30 天试用期和信用额度，以便试用所有 Azure 服务。 如果你有 Visual Studio 订阅，则还有资格[免费获取 Azure 信用额度](https://www.azure.cn/support/legal/offer-rate-plans/)，可用于任何 Azure 服务。 

也可以使用 [Azure Cosmos DB 模拟器](local-emulator.md)在本地免费开发和测试应用程序，无需创建 Azure 订阅。 如果对应用程序在 Azure Cosmos DB 模拟器中的工作情况感到满意，则可以切换到在云中使用 Azure Cosmos DB 帐户。

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>如何获取 Azure Cosmos DB 的更多帮助？
如需任何帮助，请在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb) 或 [MSDN Azure 和 CSDN Azure](https://www.azure.cn/support/forums/) 上联系我们，或者通过向 [Azure 支持部门](https://www.azure.cn/support/contact/)发送邮件安排与 Azure Cosmos DB 工程团队进行一对一交谈。 

<!-- Not Avaialble ## Try Azure Cosmos DB subscriptions-->
## <a name="set-up-azure-cosmos-db"></a>设置 Azure Cosmos DB
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>如何注册 Azure Cosmos DB？
可以在 Azure 门户中注册 Azure Cosmos DB。 首先，注册 Azure 订阅。 注册后即可将 SQL API、表 API 或 MongoDB API 帐户添加到 Azure 订阅。
<!-- Not Available Graph API (Preview) and Cassandra API -->

### <a name="what-is-a-master-key"></a>什么是主密钥？
主密钥是用于访问帐户的所有资源的安全令牌。 拥有此密钥的人对数据库帐户中的所有资源具有读取和写入访问权。 分发主密钥时需谨慎。 [Azure 门户][azure-portal]的“密钥”边栏选项卡中提供主要主密钥和辅助主密钥。 有关密钥的详细信息，请参阅[查看、复制和重新生成访问密钥](manage-account.md#keys)。

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>可以将 PreferredLocations 设置为哪些区域？ 
可以将 PreferredLocations 值设置为提供 Cosmos DB 的任何 Azure 区域。 有关可用区域的列表，请参阅 [Azure 区域](https://www.azure.cn/support/service-dashboard/)。

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>通过 Azure 数据中心在全球分配数据时需要注意什么？ 
Azure Cosmos DB 存在于所有 Azure 区域，详见 [Azure 区域](https://www.azure.cn/support/service-dashboard/)页。 由于 Azure Cosmos DB 是核心服务，每个新数据中心都部署了它。 

设置区域时，请记住，Azure Cosmos DB 遵从主权和政府云的要求。 也就是说，如果你在某个主权区域创建了一个帐户，则不能将数据从该主权区域复制到外部区域。 同样，你不能将数据从外部帐户复制到其他主权位置。 

## <a name="develop-against-the-sql-api"></a>针对 SQL API 进行开发

### <a name="how-do-i-start-developing-against-the-sql-api"></a>如何开始针对 SQL API 进行开发？
首先必须注册 Azure 订阅。 注册 Azure 订阅后，即可将 SQL API 容器添加到 Azure 订阅。 有关添加 Azure Cosmos DB 帐户的说明，请参阅[创建 Azure Cosmos DB 数据库帐户](create-sql-api-dotnet.md#create-account)。 

[SDK](sql-api-sdk-dotnet.md) 适用于 .NET、Python、Node.js、JavaScript 和 Java。 开发人员也可以利用 [RESTful HTTP API](https://docs.microsoft.com/rest/api/documentdb/)，从各种平台使用各种语言与 Azure Cosmos DB 资源进行交互。

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>是否可以访问一些现成的示例来帮助自己入门？
GitHub 上提供了 SQL API [.NET](sql-api-dotnet-samples.md)、[Java](https://github.com/Azure/azure-documentdb-java)、[Node.js](sql-api-nodejs-samples.md) 和 [Python](sql-api-python-samples.md) SDK 的示例。

### <a name="does-the-sql-api-database-support-schema-free-data"></a>SQL API 数据库是否支持无架构数据？
是的，SQL API 允许应用程序存储任意 JSON 文档，不需要架构定义或提示。 通过 Azure Cosmos DB SQL 查询接口可立即查询数据。  

### <a name="does-the-sql-api-support-acid-transactions"></a>SQL API 是否支持 ACID 事务？
是的，SQL API 支持以 JavaScript 存储过程和触发器形式表示的跨文档事务。 事务以每个集合内的单个分区为范围，且以 ACID 语义执行，也就是“全或无”，与其他并发执行代码和用户请求隔离。 如果在服务器端执行 JavaScript 应用程序代码的过程中引发了异常，则会回滚整个事务。 有关事务的详细信息，请参阅[数据库程序事务](programming.md#database-program-transactions)。

### <a name="what-is-a-collection"></a>什么是集合？
集合是文档和其关联的 JavaScript 应用程序逻辑的组。 集合是一个计费实体，其[成本](performance-levels.md)取决于吞吐量和所用存储。 集合可以跨一个或多个分区或服务器，并且可以通过伸缩处理几乎无限制增长的存储或吞吐量。

集合也是 Azure Cosmos DB 的计费实体。 每个集合根据预配的吞吐量和已用的存储空间按小时计费。 有关详细信息，请参阅 [Azure Cosmos DB 定价](https://www.azure.cn/pricing/details/cosmos-db/)。 

### <a name="how-do-i-create-a-database"></a>我如何创建数据库？
可以使用 [Azure 门户](https://portal.azure.cn)（详见[添加集合](create-sql-api-dotnet.md#create-collection)中所述）、某个 [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) 或 [REST API](https://docs.microsoft.com/rest/api/documentdb/) 来创建数据库。 

### <a name="how-do-i-set-up-users-and-permissions"></a>我如何设置用户和权限？
可以使用某个 [Cosmos DB API SDK](sql-api-sdk-dotnet.md) 或 [REST API](https://docs.microsoft.com/rest/api/documentdb/) 来创建用户和权限。  

### <a name="does-the-sql-api-support-sql"></a>SQL API 是否支持 SQL？
SQL API 支持的 SQL 查询语言是 SQL Server 支持的查询功能增强子集。 Azure Cosmos DB SQL 查询语言通过基于 JavaScript 的用户定义函数 (UDF)，提供丰富的分层和关系运算符以及可扩展性。 JSON 语法允许将 JSON 文档模型化为带标签节点的树状，由 Azure Cosmos DB 的自动索引技术及 Azure Cosmos DB 的 SQL 查询方言使用。 若要了解如何使用 SQL 语法，请参阅 [SQL 查询][query]一文。

### <a name="does-the-sql-api-support-sql-aggregation-functions"></a>SQL API 是否支持 SQL 聚合函数？
SQL API 支持通过聚合函数 `COUNT`、`MIN`、`MAX`、`AVG` 和 `SUM` 通过 SQL 语法实现的任何规模的低延迟聚合。 有关详细信息，请参阅[聚合函数](sql-api-sql-query.md#Aggregates)。

### <a name="how-does-the-sql-api-provide-concurrency"></a>SQL API 如何提供并发性？
SQL API 通过 HTTP 实体标记或 ETag 支持乐观并发控制 (OCC)。 每个 SQL API 资源都有一个 ETag。每次更新文档时，都会在服务器上设置此 ETag。 ETag 标头和当前值包含在所有响应消息中。 ETag 可与 If-Match 标头配合使用，让服务器决定是否应更新资源。 If-Match 值是用作检查依据的 ETag 值。 如果 ETag 值与服务器的 ETag 值匹配，就会更新资源。 如果 ETag 不再是最新状态，则服务器会拒绝该操作，并提供“HTTP 412 不满足前提条件”响应代码。 客户端接着会重新提取资源，以获取该资源当前的 ETag 值。 此外，ETag 可以与 If-None-Match 标头配合使用，以确定是否需重新提取资源。

若要在 .NET 中使用乐观并发，可以使用 [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) 类。 如需 .NET 示例，请参阅 GitHub 上 DocumentManagement 示例中的 [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs)。

### <a name="how-do-i-perform-transactions-in-the-sql-api"></a>如何在 SQL API 中执行事务？
SQL API 通过 JavaScript 存储过程和触发器支持语言集成式事务。 脚本中的所有数据库操作都是在进行快照隔离的情况下执行的。 如果是单分区集合，则执行范围为集合。 如果集合已分区，则执行范围为该集合中具有相同分区键值的文档。 文档版本 (ETag) 的快照是在事务开始时获取的，且只有当脚本成功运行时才会提交。 如果 JavaScript 引发错误，则会回滚事务。 有关详细信息，请参阅 [Azure Cosmos DB 的服务器端 JavaScript 编程](programming.md)。

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>如何将文档批量插入到 Document DB 中？
可以通过下述两种方式之一将文档批量插入到 Azure Cosmos DB 中：

* 数据集成工具，如 [Azure Cosmos DB 的数据迁移工具](import-data.md)中所述。
* 存储过程，如 [Azure Cosmos DB 的服务器端 JavaScript 编程](programming.md)中所述。

### <a name="does-the-sql-api-support-resource-link-caching"></a>SQL API 是否支持资源链接缓存？
是的，因为 Azure Cosmos DB 是 RESTful 服务，而资源链接固定不变，所以可以缓存。 SQL API 客户端可以通过指定“If-None-Match”标头来读取任何资源（例如文档或集合），然后在服务器版本更改后更新本地副本。

### <a name="is-a-local-instance-of-sql-api-available"></a>SQL API 的本地实例是否可用？
是的。 [Azure Cosmos DB 模拟器](local-emulator.md)提供对 Cosmos DB 服务的高保真模拟。 它支持和 Azure Cosmos DB 相同的功能，包括支持创建和查询 JSON 文档、预配集合和调整集合的规模，以及执行存储过程和触发器。 可以使用 Azure Cosmos DB 模拟器开发和测试应用程序，并通过对 Azure Cosmos DB 的连接终结点进行单一配置更改将其部署到多区域范围的 Azure。
<!-- Notice: 全球范围 to 多个区域范围 -->

## <a name="develop-against-the-api-for-mongodb"></a>针对 API for MongoDB 进行开发
### <a name="what-is-the-azure-cosmos-db-api-for-mongodb"></a>Azure Cosmos DB API for MongoDB 是什么？
Azure Cosmos DB API for MongoDB 是一个兼容层，允许应用程序使用现有的、社区支持的 Apache MongoDB API 和驱动程序轻松、透明地与本机 Azure Cosmos DB 数据库引擎通信。 开发人员现在可以使用现有的 MongoDB 工具链和技术，生成能够充分利用 Azure Cosmos DB 的应用程序。 开发人员可以使用 Azure Cosmos DB 的独特功能，其中包括自动索引、备份维护、获得财务支持的服务级别协议 (SLA) 等。

### <a name="how-do-i-connect-to-my-api-for-mongodb-database"></a>如何连接到 API for MongoDB 数据库？
若要连接到 Azure Cosmos DB API for MongoDB，最快捷的方式是使用 [Azure 门户](https://portal.azure.cn)。 转到帐户，然后在左侧导航菜单上单击“快速启动”。 “快速启动”是获取连接到数据库的代码片段的最佳方式。 

Azure Cosmos DB 实施严格的安全要求和标准。 Azure Cosmos DB 帐户需要通过 SSL 进行身份验证和安全通信，因此务必使用 TLSv1.2。

有关详细信息，请参阅[连接到 API for MongoDB 数据库](connect-mongodb-account.md)。

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>MongoDB 的 API 数据库是否有其他错误代码？
除了常见的 MongoDB 错误代码，MongoDB API 还有其独特的错误代码：

| 错误               | 代码  | 说明  | 解决方案  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | 使用的请求单位总数超过了集合的预配请求单位比率，已达到限制。 | 请考虑从 Azure 门户缩放集合的吞吐量，或者重试。 |
| ExceededMemoryLimit | 16501 | 作为一种多租户服务，操作已超出客户端的内存配额。 | 通过限制性更强的查询条件缩小操作的作用域，或者通过 [Azure 门户](https://www.azure.cn/support/support-azure/)联系技术支持。 <br><br>示例：*&nbsp;&nbsp;&nbsp;&nbsp;db.getCollection('users').aggregate([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {name: "Andy"}}, <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$sort: {age: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-the-table-api"></a>使用表 API 进行开发

### <a name="how-can-i-use-the-table-api-offering"></a>如何使用表 API 产品/服务？ 
[Azure 门户][azure-portal]提供 Azure Cosmos DB 表 API。 首先必须注册 Azure 订阅。 注册以后，即可向 Azure 订阅添加 Azure Cosmos DB 表 API 帐户，然后向帐户添加表。 

可以在 [Azure Cosmos DB 表 API 简介](table-introduction.md)中找到支持的语言和相关的快速入门。

### <a name="do-i-need-a-new-sdk-to-use-the-table-api"></a>是否需要新的 SDK 才能使用表 API？ 
不是，现有的存储 SDK 仍然适用。 但是，我们建议始终使用最新的 SDK，以获得最佳支持，并在许多场合下获得优异的性能。 请参阅 [Azure Cosmos DB 表 API 简介](table-introduction.md)中的可用语言列表。

### <a name="where-is-table-api-not-identical-with-azure-table-storage-behavior"></a>表 API 与 Azure 表存储的行为有哪些不同之处？
想要使用 Azure Cosmos DB 表 API 创建表的 Azure 表存储用户应注意以下这些行为差异：

* Azure Cosmos DB 表 API 使用保留容量模型来保障性能，但这意味着，一旦创建了表，就必须立即支付容量费用，即使容量未被使用。 使用 Azure 表存储时，只需为实际使用的容量付费。 这也说明了，表 API 在 99% 的时间里为何能够提供 10 毫秒的读取延迟和 15 毫秒的写入延迟 SLA，而 Azure 表存储提供 10 秒延迟 SLA。 因此，使用表 API 表（即使是不带任何请求的空表）时，要达到 Azure Cosmos DB 所提供的 SLA，必须支付费用来确保提供所需的容量来处理对这些表发出的所有请求。
* 表 API 返回的查询结果未按分区键/行键顺序排序，因为它们在 Azure 表存储中。
* 行键最多只能包含 255 个字节
* 批最多只能包含 2 MB
* 目前不支持 CORS
* Azure 表存储中的表名不区分大小写，但出现在 Azure Cosmos DB 表 API 中
* Azure Cosmos DB 的某些编码信息内部格式，例如二进制字段，目前不如想像的那么有效。 因此，这会导致数据大小受到意外限制。 例如，目前无法使用整整有 1 MB 的表实体来存储二进制数据，因为编码会增大数据大小。

对于 REST API，有大量的终结点/查询选项不受 Azure Cosmos DB 表 API 的支持：
| REST 方法 | REST 终结点/查询选项 | 文档 URL | 说明 |
| ------------| ------------- | ---------- | ----------- |
| GET、PUT | /?restype=service@comp=properties| [设置表服务属性](https://docs.microsoft.com/rest/api/storageservices/set-table-service-properties)和[获取表服务属性](https://docs.microsoft.com/rest/api/storageservices/get-table-service-properties) | 此终结点用于设置 CORS 规则、存储分析配置和日志记录设置。 CORS 目前不受支持，Azure Cosmos DB 与 Azure 存储表中以不同的方式处理分析和日志记录 |
| OPTIONS | /<table-resource-name> | [预检 CORS 表请求](https://docs.microsoft.com/rest/api/storageservices/preflight-table-request) | 这是 Azure Cosmos DB 目前不支持的 CORS 部分。 |
| GET | /?restype=service@comp=stats | [获取表服务统计信息](https://docs.microsoft.com/rest/api/storageservices/get-table-service-stats) | 提供有关主节点与辅助节点之间的数据复制速度的信息。 由于复制是写入的一部分，因此在 Cosmos DB 中不需要此选项。 |
| GET、PUT | /mytable?comp=acl | [获取表 ACL](https://docs.microsoft.com/rest/api/storageservices/get-table-acl) 和[设置表 ACL](https://docs.microsoft.com/rest/api/storageservices/set-table-acl) | 获取和设置用于管理共享访问签名 (SAS) 的存储访问策略。 尽管支持 SAS，但其设置和管理方式不同。 |

此外，Azure Cosmos DB 表 API 仅支持 JSON 格式，而不支持 ATOM。

尽管 Azure Cosmos DB 支持共享访问签名 (SAS)，但它不支持某些策略，具体而言，是与管理操作相关的策略，例如创建新表的权限。

对于特定的 .NET SDK，Azure Cosmos DB 目前不支持某些类和方法。

| 类 | 不支持的方法 |
|-------|-------- |
| CloudTableClient | \*ServiceProperties* |
|                  | \*ServiceStats* |
| CloudTable | SetPermissions* |
|            | GetPermissions* |
| TableServiceContext | *（实际上此类已弃用） |
| TableServiceEntity | " " |
| TableServiceExtensions | " " |
| TableServiceQuery | " " |

如果其中的任何差异会给项目造成问题，请联系 [Azure 支持部门](https://www.azure.cn/support/contact/)并告诉我们。

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>如何提供有关 SDK 或 Bug 的反馈？
可通过以下途径提供反馈：

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN 论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDocumentDB)
* [堆栈溢出](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api"></a>连接到表 API 需要使用哪个连接字符串？
连接字符串为：
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB>;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountName>.documents.azure.cn
```
可以通过 Azure 门户中的“连接字符串”页获取连接字符串。 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-net-sdk-for-the-table-api"></a>如何在表 API 的 .NET SDK 中替代请求选项的配置设置？
有关配置设置的信息，请参阅 [Azure Cosmos DB 功能](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities)。 有些设置是通过 CreateCloudTableClient 方法处理的，还有一些设置是通过客户端应用程序中 appSettings 节的 app.config 处理的。

### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-azure-table-storage-sdks"></a>使用现有 Azure 表存储 SDK 的客户是否需要进行任何更改？
无。 使用现有 Azure 表存储 SDK 的现有客户或新客户无需进行任何更改。 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api"></a>如何查看 Azure Cosmos DB 中存储的用于表 API 的表数据？ 
可以使用 Azure 门户浏览数据。 也可以使用表 API 代码或下一个问题答案中提到的工具。 

### <a name="which-tools-work-with-the-table-api"></a>哪些工具适用于表 API？ 
可以使用 [Azure 存储资源管理器](/vs-azure-tools-storage-manage-with-storage-explorer)。

灵活地采用之前指定格式的连接字符串的工具可以支持新的表 API。 [Azure 存储客户端工具](../storage/common/storage-explorers.md)页中提供了表工具列表。 

### <a name="do-powershell-or-azure-cli-work-with-the-table-api"></a>PowerShell 或 Azure CLI 是否适用于表 API？
支持 [PowerShell](table-powershell.md)。 目前不支持 Azure CLI。

### <a name="is-the-concurrency-on-operations-controlled"></a>操作并发性是否受控制？
是的，通过使用 ETag 机制提供乐观并发。 

### <a name="is-the-odata-query-model-supported-for-entities"></a>是否支持针对实体的 OData 查询模型？ 
是，表 API 支持 OData 查询和 LINQ 查询。 

### <a name="can-i-connect-to-azure-table-storage-and-azure-cosmos-db-table-api-side-by-side-in-the-same-application"></a>是否可以在同一应用程序中同时连接到 Azure 表存储和 Azure Cosmos DB 表 API？ 
是的，可以通过创建两个不同的 CloudTableClient 实例进行连接，每个实例都通过连接字符串指向各自的 URI。

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-offering"></a>如何将现有 Azure 表存储应用程序迁移到此服务？
支持使用 [AzCopy](/storage/common/storage-use-azcopy) 和 [Azure Cosmos DB 数据迁移工具](import-data.md)。

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>如何在特定情况下为此服务扩展存储大小？比如，最初我有 *n* GB 的数据，但一段时间后我的数据会增长到 1 TB。 
根据设计，可以通过横向缩放让 Azure Cosmos DB 提供无限的存储。 可以通过服务来监视并有效地增大存储。 

### <a name="how-do-i-monitor-the-table-api-offering"></a>如何监视表 API 服务？
可以使用表 API 的“指标”窗格来监视请求和存储使用情况。 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>如何计算所需的吞吐量？
可以使用容量估算器来计算操作所需的 TableThroughput。 有关详细信息，请参阅 [Estimate Request Units and Data Storage](https://www.documentdb.com/capacityplanner)（估算请求单位和数据存储）。 通常，可以将实体表示为 JSON，并为操作提供所需数量。 

### <a name="can-i-use-the-table-api-sdk-locally-with-the-emulator"></a>是否可以在本地将表 API SDK 用于模拟器？
目前没有。

### <a name="can-my-existing-application-work-with-the-table-api"></a>现有的应用程序是否适用于表 API？ 
是的，支持相同的 API。

### <a name="do-i-need-to-migrate-my-existing-azure-table-storage-applications-to-the-sdk-if-i-do-not-want-to-use-the-table-api-features"></a>如果我不想使用表 API 功能，是否需要将现有 Azure 表存储应用程序迁移到该 SDK？
否，可以在没有任何干扰的情况下创建和使用现有 Azure 表存储资产。 但是，如果不使用表 API，则无法从自动索引、其他一致性选项或多区域分发中受益。 

### <a name="how-do-i-add-replication-of-the-data-in-the-table-api-across-multiple-regions-of-azure"></a>如何在跨多个 Azure 区域的表 API 中添加数据复制？
可以使用 Azure Cosmos DB 门户的[多区域复制设置](tutorial-global-distribution-sql-api.md#portal)来添加适合应用程序的区域。 若要开发多区域分布式应用程序，还应添加其 PreferredLocation 信息已设置为本地区域的应用程序，以减轻读取延迟。 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-table-api"></a>如何在表 API 中更改帐户的主要写入区域？
可以使用 Azure Cosmos DB 的多区域复制门户窗格来添加区域，然后故障转移到所需的区域。 有关说明，请参阅[使用多区域 Azure Cosmos DB 帐户进行开发](regional-failover.md)。 
<!-- Notice: 全球范围 to 多个区域范围 -->

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>如何配置首选的读取区域以降低分配数据时的延迟？ 
请使用 app.config 文件中的 PreferredLocation 键，方便从本地位置读取。 对于现有应用程序，如果设置 LocationMode，表 API 会引发错误。 请删除该代码，因为表 API 会从 app.config 文件中选取此信息。 有关详细信息，请参阅 [Azure Cosmos DB 功能](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities)。

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api"></a>如何理解表 API 中的一致性级别？ 
Azure Cosmos DB 在一致性、可用性和延迟之间提供合理的平衡。 Azure Cosmos DB 为表 API 开发人员提供五个一致性级别，因此可以在表级别选择合适的一致性模型，并在查询数据时发出相应的请求。 客户端可以在连接时指定一致性级别。 可以通过 CreateCloudTableClient 的 consistencyLevel 参数更改级别。 

如果将“有限过时”一致性设置为默认值，表 API 可通过“读取自己写入的数据”提供低延迟的读取。 有关详细信息，请参阅[一致性级别](consistency-levels.md)。 

Azure 表存储默认在区域中提供“强”一致性，在辅助位置提供“最终”一致性。 

### <a name="does-azure-cosmos-db-table-api-offer-more-consistency-levels-than-azure-table-storage"></a>Azure Cosmos DB 表 API 是否比 Azure 表存储提供更多的一致性级别？
是的。若要了解如何利用 Azure Cosmos DB 的分布式特性，请参阅[一致性级别](consistency-levels.md)。 由于一致性级别有充分的保证，因此可以放心使用。 有关详细信息，请参阅 [Azure Cosmos DB 功能](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities)。

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>启用全局分发后，需要花费多长时间复制数据？
Azure Cosmos DB 会在本地区域持续提交数据，然后在几毫秒内将数据立即推送到其他区域。 这种形式的复制只取决于数据中心的往返时间 (RTT)。 若要详细了解 Azure Cosmos DB 的全局分发功能，请参阅 [Azure Cosmos DB：Azure 上的多区域分布式数据库服务](distribute-data-globally.md)。

### <a name="can-the-read-request-consistency-level-be-changed"></a>是否可以更改读取请求一致性级别？
使用 Azure Cosmos DB 时，可以在容器级别（在表上）设置一致性级别。 使用 .NET SDK，可以通过在 app.config 文件中提供 TableConsistencyLevel 键值来更改级别。 可能的值：Strong、Bounded Staleness、Session、ConsistentPrefix、Eventual。 有关详细信息，请参阅 [Azure Cosmos DB 中可以优化的数据一致性级别](consistency-levels.md)。 关键是，在设置请求一致性级别时，不能超出表的设置。 例如，不能将表的一致性级别设置为“Eventual”，将请求一致性级别设置为“Strong”。 

### <a name="how-does-the-table-api-handle-failover-if-a-region-goes-down"></a>在某个区域出现故障时，表 API 如何处理故障转移？ 
表 API 利用 Azure Cosmos DB 的多区域分布式平台。 若要确保应用程序能够容许数据中心停机，请在 Azure Cosmos DB 门户中为帐户额外启用至少一个区域。详见[使用多区域 Azure Cosmos DB 帐户进行开发](regional-failover.md)。 可以通过使用门户设置区域的优先级（[使用多区域 Azure Cosmos DB 帐户进行开发](regional-failover.md)）。 
<!-- Notice: 全球 to 多个区域 -->

可以视需要为帐户添加任意数目的区域，并通过提供故障转移优先级来控制可将该帐户故障转移到哪个位置。 当然，若要使用数据库，还需在其中提供应用程序。 这样操作时，客户不会遇到停机的情况。 [最新的 .NET 客户端 SDK](table-sdk-dotnet.md)可自动寻址，但其他 SDK 则不可以。 换句话说，它能够检测到有故障的区域，并自动故障转移到新区域。

### <a name="is-the-table-api-enabled-for-backups"></a>是否能够为表 API 启用备份？
可以，表 API 利用 Azure Cosmos DB 的平台进行备份。 可自动进行备份。 有关详细信息，请参阅[使用 Azure Cosmos DB 进行联机备份和还原](online-backup-and-restore.md)。

### <a name="does-the-table-api-index-all-attributes-of-an-entity-by-default"></a>表 API 是否默认对实体的所有属性编制索引？
是的，默认情况下会为实体的所有属性编制索引。 有关详细信息，请参阅 [Azure Cosmos DB：索引策略](indexing-policies.md)。 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>这是否意味着，无需创建多个索引来满足查询要求？ 
是，Azure Cosmos DB 表 API 针对所有属性提供自动索引，无需任何架构定义。 这种自动化操作可以让开发人员将工作重点放在应用程序上，不必考虑索引的创建和管理。 有关详细信息，请参阅 [Azure Cosmos DB：索引策略](indexing-policies.md)。

### <a name="can-i-change-the-indexing-policy"></a>是否可以更改索引策略？
是的，可以通过提供索引定义来更改索引策略。 有关详细信息，请参阅 [Azure Cosmos DB 功能](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities)。 需要在 app.config 文件中使用字符串 json 格式 

对于非 .NET SDK，只能在门户中设置索引策略，方法是：打开“数据资源管理器”，导航到想要更改的特定表，转到“缩放和设置”->“索引策略”，进行所需的更改，并单击“保存”。

对于 .NET SDK，可以在 app.config 文件中提交更改：
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>平台形式的 Azure Cosmos DB 似乎有许多的功能，例如排序、聚合、分层等。 是否要向表 API 添加这些功能？ 
表 API 提供与 Azure 表存储相同的查询功能。 Azure Cosmos DB 还支持排序、聚合、地理空间查询、层次结构和各种内置函数。 我们会通过将来的服务更新在表 API 中提供更多功能。 有关详细信息，请参阅 [SQL 查询](sql-api-sql-query.md)。

### <a name="when-should-i-change-tablethroughput-for-the-table-api"></a>何时应更改表 API 的 TableThroughput？
应在符合以下某个条件时，更改 TableThroughput：
* 要对数据执行提取、转换和加载 (ETL) 操作，或者需在短时间内上传大量数据。 
* 需要来自后端容器的更多吞吐量。 例如，你发现所使用的吞吐量超出预配的吞吐量，导致操作受限。 有关详细信息，请参阅[为 Azure Cosmos DB 容器设置吞吐量](set-throughput.md)。

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-table"></a>是否可以提高或降低表 API 表的吞吐量？ 
是的，可以使用 Azure Cosmos DB 门户的缩放窗格来缩放吞吐量。 有关详细信息，请参阅[设置吞吐量](set-throughput.md)。

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>新预配的表是否设置了默认的 TableThroughput？
是的，如果未通过 app.config 替代 TableThroughput，并且未使用 Azure Cosmos DB 中预先创建的容器，则该服务将创建吞吐量为 400 的表。

### <a name="is-there-any-change-of-pricing-for-existing-customers-of-the-azure-table-storage-service"></a>对于 Azure 表存储服务的现有客户，定价是否有任何变化？
无。 对于现有的 Azure 表存储客户，价格上没有任何更改。 

### <a name="how-is-the-price-calculated-for-the-table-api"></a>表 API 的价格是如何计算的？ 
价格取决于分配的 TableThroughput。 

### <a name="how-do-i-handle-any-throttling-on-the-tables-in-table-api-offering"></a>如何在表 API 服务中处理对表设置的任何限制？ 
如果请求速率超出了为基础容器预配的吞吐量容量，则会出现错误，SDK 会使用重试策略重试调用。

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-table-api-offering-of-azure-cosmos-db"></a>为何需要选择吞吐量而不是 PartitionKey 和 RowKey 来利用 Azure Cosmos DB 的表 API 服务？
如果未在 app.config 文件中或通过门户提供吞吐量，Azure Cosmos DB 将为容器设置默认的吞吐量。 

Azure Cosmos DB 针对操作设置上限，在性能和延迟方面提供保证。 如果引擎可以针对租户的操作实施调控，则可以提供这样的保证。 设置 TableThroughput 可确保吞吐量和延迟值得到保证，因为平台会保留相应的容量，保证操作成功。 

考虑到应用程序在使用需求方面存在“季节性”，可以通过吞吐量规范来弹性更改吞吐量，在满足吞吐量需求的同时节省成本。

### <a name="azure-table-storage-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-azure-cosmos-db-table-api-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Azure 表存储对我而言非常便宜，因为我只需支付数据的存储费用，并且我很少进行查询。 但是，即使我未执行任何事务或存储任何数据，Azure Cosmos DB 表 API 服务似乎也要收费。 能否请你解释一下？

根据设计，Azure Cosmos DB 是一个多区域分布式的、基于 SLA 的系统，在可用性、延迟和吞吐量方面提供保证。 如果在 Azure Cosmos DB 中保留吞吐量，吞吐量就会获得保证，这与其他系统的吞吐量有所不同。 Azure Cosmos DB 会根据客户请求提供额外功能，例如辅助索引和全局分发。  
<!-- Notice: 全球范围 to 多个区域范围 -->

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-azure-table-storage-with-the-table-api-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>在向 Azure 表存储引入数据时，我从未收到过“配额已满”通知（指示分区已满）。 但使用表 API 时会收到此消息。 是此产品有限制，迫使我更改现有的应用程序吗？

Azure Cosmos DB 是基于 SLA 的系统，可提供无限缩放，并在延迟、吞吐量、可用性和一致性方面提供保证。 请确保数据大小和索引可管理且可缩放，这样才能获得有保障的高级性能。 针对每个分区键的实体或项数实施 10 GB 限制是为了确保提供优异的查找和查询性能。 若要确保即使针对 Azure 存储，应用程序也能很好地进行缩放，建议*不要*创建热分区，即，将所有信息存储在一个分区内并查询它。 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-table-api"></a>表 API 是否仍然需要 PartitionKey 和 RowKey？ 
是的。 由于表 API 的外围应用类似于 Azure 表存储 SDK，因此使用分区键可以高效地分发数据。 行键在该分区中是唯一的。 行键必须存在，且不能为 null（在标准 SDK 中可为 null）。 RowKey 的长度为 255 个字节，PartitionKey 的长度为 1 KB。 

### <a name="what-are-the-error-messages-for-the-table-api"></a>表 API 的错误消息有哪些？
Azure 表存储和 Azure Cosmos DB 表 API 使用相同的 SDK，因此，大多数错误是相同的。

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api"></a>在表 API 中尝试一个接一个地创建许多表时，为何会受到限制？
Azure Cosmos DB 是基于 SLA 的系统，在可用性、延迟和吞吐量方面提供保障。 由于它是预配的系统，因此会保留资源来保证满足这些要求。 以极快的频率创建表会被系统检测到，因此会受到限制。 建议你查看表的创建频率，将其降到每分钟 5 个表以下。 请记住，表 API 是预配的系统。 计费从预配那一刻开始。 

<!-- Not Available ## Develop against the Graph API (Preview) -->



<!-- Not Available on ## Develop with the Apache Cassandra API (preview) -->

[azure-portal]: https://portal.azure.cn
[query]: sql-api-sql-query.md

<!--Update_Description: update link, wording update -->