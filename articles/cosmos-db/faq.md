---
title: 有关 Azure Cosmos DB 中不同 API 的常见问题
description: 获取有关 Azure Cosmos DB（多区域分布式多模型数据库服务）的常见问题的解答。 了解容量、性能级别和缩放。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 12/06/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.custom: seodec18
ms.openlocfilehash: 0a27d21c66260797001c58294f7b6ca38e2f44e3
ms.sourcegitcommit: 04392fdd74bcbc4f784bd9ad1e328e925ceb0e0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2019
ms.locfileid: "54333915"
---
<!-- meta.description: GLOBALLY to multiple-region -->
# <a name="frequently-asked-questions-about-different-apis-in-azure-cosmos-db"></a>有关 Azure Cosmos DB 中不同 API 的常见问题

### <a name="what-happened-to-the-documentdb-api"></a>DocumentDB API 有哪些变化？

Azure Cosmos DB DocumentDB API 或 SQL (DocumentDB) API 现在称为 Azure Cosmos DB SQL API。 无需进行任何更改即可继续运行使用 DocumentDB API 构建的应用。 功能保持相同。

如果你以前有一个 DocumentDB API 帐户，则现在有一个 SQL API 帐户，费用不会有任何变化。

### <a name="what-happened-to-azure-documentdb-as-a-service"></a>用作服务 Azure DocumentDB 发生了什么变化？

Azure DocumentDB 服务现在是 Azure Cosmos DB 服务的一部分，自身以 SQL API 形式呈现。 无需针对 Azure Cosmos DB SQL API 进行任何更改，就能运行针对 Azure DocumentDB 生成的应用程序。 Cosmos DB 还会直接在服务上实施 [MongoDB](mongodb-introduction.md) 线路协议。 这样你就可以将常用 NoSQL API 的客户端驱动程序（和工具）直接指向 Cosmos 数据库。
<!--Not Available on the Table API,  Graph API (Preview), and Cassandra API (Preview) -->

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Azure Cosmos DB 的典型用例有哪些？

对于侧重于以下要求的新 Web、移动、游戏和 IoT 应用程序而言，Azure Cosmos DB 是一个不错的选择：自动缩放、可预测的性能、毫秒响应时间的快速排序，以及查询无架构数据的能力。 Azure Cosmos DB 有助于快速开发，且支持应用程序数据模型的连续迭代。 用于管理用户生成的内容和数据的应用程序就是 [Azure Cosmos DB 的常见用例](use-cases.md)。

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Azure Cosmos DB 如何提供可预测的性能？

[请求单位](request-units.md) (RU) 是 Azure Cosmos DB 中吞吐量的衡量单位。 1 RU 吞吐量相当于通过 GET 操作获取 1 KB 文档的吞吐量。 在 Azure Cosmos DB 中进行的每个操作（包括读、写、SQL 查询和执行存储过程）都具有一个确定的 RU 值，该值基于完成该操作所需的吞吐量。 无需考虑 CPU、IO 和内存，以及它们会怎样影响应用程序吞吐量，只需从单个 RU 度量值的角度进行考虑即可。

可以按照每秒吞吐量的 RU 数来配置每个 Azure Cosmos DB 容器的预配吞吐量。 对于任何规模的应用程序，都可以通过将单个请求设为基准来测量其 RU 值，并通过预配容器来处理所有请求的请求单位总和。 也可以随着应用程序的发展需求，扩展或缩减容器的吞吐量。 如需请求单位的详细信息以及帮助确定容器需求，请尝试使用[吞吐量计算器](https://www.documentdb.com/capacityplanner)。

<!-- Not Available on Table API table, Graph -->
### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-columnar-and-document"></a>Azure Cosmos DB 如何支持各种数据模型（例如纵栏表和文档）？

纵栏表和文档数据模型都是原本就支持的，因为 Azure Cosmos DB 是基于 ARS（原子、记录和序列）设计构建的。 原子、记录和序列可以轻松映射和投影到各种数据模型。 现在提供了适用于一部分模型的 API（SQL 和 MongoDB API），将来会提供特定于其他数据模型的其他 API。
<!-- Not Available on Table and Graph -->

Azure Cosmos DB 有一个不受架构影响的索引编制引擎，能够自动为它引入的所有数据编制索引，不会要求开发者提供任何架构或辅助索引。 该引擎依赖于一组逻辑索引布局（倒置、纵栏表、树形），这些布局将存储布局从索引和查询处理子系统中分离出来。 Cosmos DB 还能够以可扩展方式支持一组连网协议和 API 并将它们高效地转换为核心数据模型 (1) 和逻辑索引布局 (2)，这使得它具有原本就支持多个数据模型的独特功能。

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Azure Cosmos DB 是否符合 HIPAA？

是的，Azure Cosmos DB 符合 HIPAA。 HIPAA 针对可识别个人身份的健康信息的使用、泄露与保护制定了要求。 有关详细信息，请转到 [Azure 信任中心](https://www.trustcenter.cn/zh-cn/Compliance)。

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Azure Cosmos DB 的存储限制是什么？

对于容器可以存储在 Azure Cosmos DB 中的数据总量并没有任何限制。

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Azure Cosmos DB 的吞吐量限制是什么？

对于 Azure Cosmos DB 中容器可以支持的吞吐量总量并没有任何限制。 关键是要让工作负荷大致均匀地分布到足够多的分区键中。

### <a name="are-direct-and-gateway-connectivity-modes-encrypted"></a>直接连接和网关连接模式是否加密？

是的，两种模式始终完全加密。

### <a name="how-much-does-azure-cosmos-db-cost"></a>Azure Cosmos DB 的费用如何？

有关详细信息，请参阅 [Azure Cosmos DB 定价详细信息](https://www.azure.cn/pricing/details/cosmos-db/)页。 Azure Cosmos DB 使用费取决于预配的容器数、容器的联机小时数，以及每个容器的预配吞吐量。
<!-- Not Avaialbe  Table API tables and Graph API graph -->

### <a name="is-a-trial-account-available"></a>有试用帐户吗？

<!-- Not Available [Try Azure Cosmos DB for free](https://www.azure.cn/try/cosmosdb/) -->

如果不熟悉 Azure，可以注册 [Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial/)，这样可以得到 30 天试用期和信用额度，以便试用所有 Azure 服务。 如果你有 Visual Studio 订阅，则还有资格[免费获取 Azure 信用额度](https://www.azure.cn/support/legal/offer-rate-plans/)，可用于任何 Azure 服务。

也可以使用 [Azure Cosmos DB 模拟器](local-emulator.md)在本地免费开发和测试应用程序，无需创建 Azure 订阅。 如果对应用程序在 Azure Cosmos DB 模拟器中的工作情况感到满意，则可以切换到在云中使用 Azure Cosmos DB 帐户。

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>如何获取 Azure Cosmos DB 的更多帮助？

若要询问技术问题，可以在下述两个问答论坛之一发帖：

* [MSDN 论坛](https://www.azure.cn/support/contact/)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-cosmosdb)。 Stack Overflow 适合编程问题。 请确保提问[切中主题](https://stackoverflow.com/help/on-topic)并[尽可能提供较多的详细信息，使问题清楚明了，便于回答](https://stackoverflow.com/help/how-to-ask)。

若要请求新功能，请在 [Uservoice](https://support.azure.cn/zh-cn/support/support-azure/) 上提交新的请求。

若要修复帐户问题，请在 Azure 门户中提交[支持请求](https://support.azure.cn/zh-cn/support/support-azure/)。

其他问题可以通过 [Azure 支持](https://www.azure.cn/support/contact/)提交给支持团队；不过，该论坛不是技术支持论坛。

<a name="try-cosmos-db"></a>

<!-- Not Avaialble ## Try Azure Cosmos DB subscriptions-->

## <a name="set-up-azure-cosmos-db"></a>设置 Azure Cosmos DB

### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>如何注册 Azure Cosmos DB？

可以在 Azure 门户中注册 Azure Cosmos DB。 首先，注册 Azure 订阅。 注册后，可将 Azure Cosmos DB 帐户添加到 Azure 订阅。

<!-- Not Available Table API , Graph API (Preview) and Cassandra API -->

### <a name="what-is-a-master-key"></a>什么是主密钥？

主密钥是用于访问帐户的所有资源的安全令牌。 拥有此密钥的人对数据库帐户中的所有资源具有读取和写入访问权。 分发主密钥时需谨慎。 [Azure 门户][azure-portal]的“密钥”边栏选项卡中提供主要主密钥和辅助主密钥。 有关密钥的详细信息，请参阅[查看、复制和重新生成访问密钥](manage-with-cli.md#list-account-keys)。

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>可以将 PreferredLocations 设置为哪些区域？

可以将 PreferredLocations 值设置为提供 Cosmos DB 的任何 Azure 区域。 有关可用区域的列表，请参阅 [Azure 区域](https://www.azure.cn/support/service-dashboard/)。

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-china-via-the-azure-datacenters"></a>通过 Azure 数据中心在中国分配数据时需要注意什么？

Azure Cosmos DB 存在于所有 Azure 中国区域，详见 [Azure 区域](https://www.azure.cn/support/service-dashboard/)页。 由于它属于核心服务，因此每个新的数据中心都有 Azure Cosmos DB。

<!--Notice: accross all Azure China regions-->

设置区域时，请记住，Azure Cosmos DB 遵从主权和政府云的要求。 也就是说，如果帐户是在某个主权区域中创建的，则不能在该主权区域外进行复制。 同样，也无法通过外部帐户启用到其他主权位置的复制。 

<!-- Not Available on [sovereign region](https://www.azure.cn/global-infrastructure/)-->

### <a name="is-it-possible-to-switch-from-container-level-throughput-provisioning-to-database-level-throughput-provisioning-or-vice-versa"></a>是否可以从容器级吞吐量预配切换到数据库级吞吐量预配？ 或者反之亦然？

容器级和数据库级吞吐量预配是不同的产品，在这两者之间切换需要将数据从源迁移到目标。 这意味着你需要创建新数据库或新集合，然后使用[批量执行程序库](bulk-executor-overview.md)迁移数据。

<!--Not Available on  [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)-->

### <a name="does-azure-cosmosdb-support-time-series-analysis"></a>Azure CosmosDB 是否支持时序分析？

是的，Azure CosmosDB 支持时序分析，下面是[时序模式](https://github.com/Azure/azure-cosmosdb-dotnet/tree/master/samples/Patterns)的示例。 此示例显示如何使用更改源来构建时序数据的聚合视图。 可以使用 Spark 流式处理或其他流数据处理器来扩展此方法。

## <a name="sql-api"></a>SQL API

### <a name="how-do-i-start-developing-against-the-sql-api"></a>如何开始针对 SQL API 进行开发？

首先必须注册 Azure 订阅。 注册 Azure 订阅后，即可将 SQL API 容器添加到 Azure 订阅。 有关添加 Azure Cosmos DB 帐户的说明，请参阅[创建 Azure Cosmos DB 数据库帐户](create-sql-api-dotnet.md#create-account)。

[SDK](sql-api-sdk-dotnet.md) 适用于 .NET、Python、Node.js、JavaScript 和 Java。 开发人员也可以利用 [RESTful HTTP API](https://docs.microsoft.com/rest/api/cosmos-db/)，从各种平台使用各种语言与 Azure Cosmos DB 资源进行交互。

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>是否可以访问一些现成的示例来帮助自己入门？

GitHub 上提供了 SQL API [.NET](sql-api-dotnet-samples.md)、[Java](https://github.com/Azure/azure-documentdb-java)、[Node.js](sql-api-nodejs-samples.md) 和 [Python](sql-api-python-samples.md) SDK 的示例。

### <a name="does-the-sql-api-database-support-schema-free-data"></a>SQL API 数据库是否支持无架构数据？

是的，SQL API 允许应用程序存储任意 JSON 文档，不需要架构定义或提示。 通过 Azure Cosmos DB SQL 查询接口可立即查询数据。

### <a name="does-the-sql-api-support-acid-transactions"></a>SQL API 是否支持 ACID 事务？

是的，SQL API 支持以 JavaScript 存储过程和触发器形式表示的跨文档事务。 事务以每个容器内的单个分区为范围，且以 ACID 语义执行，也就是“全有或全无”，与其他并发执行的代码和用户请求隔离。 如果在服务器端执行 JavaScript 应用程序代码的过程中引发了异常，则会回滚整个事务。 

### <a name="what-is-a-container"></a>什么是容器？

容器是文档及其关联的 JavaScript 应用程序逻辑的组。 容器是一个计费实体，其[成本](performance-levels.md)由吞吐量和已用存储确定。 容器可以跨一个或多个分区或服务器，并且能缩放以处理几乎无限制增长的存储或吞吐量。

* 对于 SQL API 帐户以及 Cosmos DB 的用于 MongoDB 的 API 帐户，容器映射到集合。

<!-- Not Available on * For Cassandra and Table API accounts, a container maps to a Table. -->
<!-- Not Available on * For Gremlin API accounts, a container maps to a Graph. -->

容器也是 Azure Cosmos DB 的计费实体。 每个容器根据预配的吞吐量和已用的存储空间按小时计费。 有关详细信息，请参阅 [Azure Cosmos DB 定价](https://www.azure.cn/pricing/details/cosmos-db/)。

### <a name="how-do-i-create-a-database"></a>我如何创建数据库？

可以使用 [Azure 门户](https://portal.azure.cn)（详见[添加集合](create-sql-api-dotnet.md#create-collection)中所述）、某个 [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) 或 [REST API](https://docs.microsoft.com/rest/api/cosmos-db/) 来创建数据库。

### <a name="how-do-i-set-up-users-and-permissions"></a>我如何设置用户和权限？

可以使用某个 [Cosmos DB API SDK](sql-api-sdk-dotnet.md) 或 [REST API](https://docs.microsoft.com/rest/api/cosmos-db/) 来创建用户和权限。

### <a name="does-the-sql-api-support-sql"></a>SQL API 是否支持 SQL？

SQL API 支持的 SQL 查询语言是 SQL Server 支持的查询功能增强子集。 Azure Cosmos DB SQL 查询语言通过基于 JavaScript 的用户定义函数 (UDF)，提供丰富的分层和关系运算符以及可扩展性。 JSON 语法允许将 JSON 文档模型化为带标签节点的树状，由 Azure Cosmos DB 的自动索引技术及 Azure Cosmos DB 的 SQL 查询方言使用。 若要了解如何使用 SQL 语法，请参阅 [SQL 查询][query]一文。

### <a name="does-the-sql-api-support-sql-aggregation-functions"></a>SQL API 是否支持 SQL 聚合函数？

SQL API 支持通过聚合函数 `COUNT`、`MIN`、`MAX`、`AVG` 和 `SUM` 通过 SQL 语法实现的任何规模的低延迟聚合。 有关详细信息，请参阅[聚合函数](how-to-sql-query.md#Aggregates)。

### <a name="how-does-the-sql-api-provide-concurrency"></a>SQL API 如何提供并发性？

SQL API 通过 HTTP 实体标记或 ETag 支持乐观并发控制 (OCC)。 每个 SQL API 资源都有一个 ETag。每次更新文档时，都会在服务器上设置此 ETag。 ETag 标头和当前值包含在所有响应消息中。 ETag 可与 If-Match 标头配合使用，让服务器决定是否应更新资源。 If-Match 值是用作检查依据的 ETag 值。 如果 ETag 值与服务器的 ETag 值匹配，就会更新资源。 如果 ETag 不再是最新状态，则服务器会拒绝该操作，并提供“HTTP 412 不满足前提条件”响应代码。 客户端接着必须重新提取资源，以获取该资源当前的 ETag 值。 此外，ETag 可以与 If-None-Match 标头配合使用，以确定是否需要重新提取资源。

若要在 .NET 中使用乐观并发，可以使用 [AccessCondition](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.accesscondition) 类。 如需 .NET 示例，请参阅 GitHub 上 DocumentManagement 示例中的 [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs)。

### <a name="how-do-i-perform-transactions-in-the-sql-api"></a>如何在 SQL API 中执行事务？

SQL API 通过 JavaScript 存储过程和触发器支持语言集成式事务。 脚本中的所有数据库操作都是在进行快照隔离的情况下执行的。 如果是单分区集合，则在集合范围内执行。 如果集合已分区，则执行范围为该集合中具有相同分区键值的文档。 文档版本 (ETag) 的快照是在事务开始时获取的，且只有当脚本成功运行时才会提交。 如果 JavaScript 引发错误，则会回滚事务。 有关详细信息，请参阅 [Azure Cosmos DB 的服务器端 JavaScript 编程](stored-procedures-triggers-udfs.md)。

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>如何将文档批量插入到 Document DB 中？

可以通过以下方法之一，将文档批量插入到 Azure Cosmos DB 中：

* 批量执行程序工具，如[使用批量执行程序 .NET 库](bulk-executor-dot-net.md)和[使用批量执行程序 Java 库](bulk-executor-java.md)中所述
* 数据集成工具，如 [Azure Cosmos DB 的数据迁移工具](import-data.md)中所述。
* 存储过程，如 [Azure Cosmos DB 的服务器端 JavaScript 编程](stored-procedures-triggers-udfs.md)中所述。

### <a name="ive-set-up-my-container-to-use-lazy-indexing-i-see-that-my-queries-dont-return-expected-results"></a>我将自己的容器设置为使用延迟索引编制，结果发现查询没有返回预期的结果。

如索引编制部分所述，延迟索引编制可能导致此行为。 应该始终对所有应用程序使用一致的索引编制。

### <a name="does-the-sql-api-support-resource-link-caching"></a>SQL API 是否支持资源链接缓存？

是的，因为 Azure Cosmos DB 是 RESTful 服务，而资源链接固定不变，所以可以缓存。 SQL API 客户端可以通过指定“If-None-Match”标头来读取任何资源（例如文档或集合），然后在服务器版本更改后更新本地副本。

### <a name="is-a-local-instance-of-sql-api-available"></a>SQL API 的本地实例是否可用？

是的。 [Azure Cosmos DB 模拟器](local-emulator.md)提供对 Cosmos DB 服务的高保真模拟。 它支持和 Azure Cosmos DB 相同的功能，包括支持创建和查询 JSON 文档、预配集合和调整集合的规模，以及执行存储过程和触发器。 可以使用 Azure Cosmos DB 模拟器开发和测试应用程序，并通过对 Azure Cosmos DB 的连接终结点进行单一配置更改将其部署到多区域范围的 Azure。

<!-- Notice: 全球范围 to 多个区域范围 -->

### <a name="why-are-long-floating-point-values-in-a-document-rounded-when-viewed-from-data-explorer-in-the-portal"></a>当从门户中的数据资源管理器查看时，为何会对文档中的长浮点值进行舍入？

这是 JavaScript 的限制。 JavaScript 根据 IEEE 754 中的规定使用双精度浮点格式的数字，并且只能安全地保存 -(253 - 1) 和 253-1（即 9007199254740991）之间的数字。

### <a name="where-are-permissions-allowed-in-the-object-hierarchy"></a>在对象层次结构中的何处启用权限？

可以在容器级别以及其下的级别（例如文档级别、附件级别）使用 ResourceTokens 来创建权限。 这意味着，目前不允许在数据库或帐户级别创建权限。

## <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB 的用于 MongoDB 的 API

### <a name="what-is-the-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB 的用于 MongoDB 的 API 是什么？

Azure Cosmos DB 的用于 MongoDB 的 API 是一个线路协议兼容层，允许应用程序使用现有的、社区支持的 SDK 和用于 MongoDB 的驱动程序轻松、透明地与本机 Azure Cosmos DB 数据库引擎通信。开发人员现在可以使用现有的 MongoDB 工具链和技能生成可以充分利用 Azure Cosmos DB 的应用程序。 开发人员可以使用 Azure Cosmos DB 的独特功能，其中包括带多主数据库复制功能的多区域分发、自动索引、备份维护、获得财务支持的服务级别协议 (SLA) 等。

### <a name="how-do-i-connect-to-my-database"></a>如何连接到数据库？

若要通过 Azure Cosmos DB 的用于 MongoDB 的 API 连接到 Cosmos 数据库，最快捷的方法是使用 [Azure 门户](https://portal.azure.cn)。 转到帐户，然后在左侧导航菜单上单击“快速启动”。 “快速启动”是获取连接到数据库的代码片段的最佳方式。

Azure Cosmos DB 实施严格的安全要求和标准。 Azure Cosmos DB 帐户需要通过 SSL 进行身份验证和安全通信，因此务必使用 TLSv1.2。

有关详细信息，请参阅[通过 Azure Cosmos DB 的用于 MongoDB 的 API 连接到 Cosmos 数据库](connect-mongodb-account.md)。

使用 Azure Cosmos DB 的用于 MongoDB 的 API 时，是否有其他需要处理的错误代码？

除了常见的 MongoDB 错误代码外，Azure Cosmos DB 的用于 MongoDB 的 API 还有自己的特定错误代码：

| 错误               | 代码  | 说明  | 解决方案  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | 使用的请求单位总数超过了集合的预配请求单位率，已达到限制。 | 考虑从 Azure 门户中对分配给一个容器或一组容器的吞吐量进行缩放，或者重试。 |
| ExceededMemoryLimit | 16501 | 作为一种多租户服务，操作已超出客户端的内存配额。 | 通过限制性更强的查询条件缩小操作的作用域，或者通过 [Azure 门户](https://support.azure.cn/zh-cn/support/support-azure/)联系技术支持。 <br><br>示例：*&nbsp;&nbsp;&nbsp;&nbsp;db.getCollection('users').aggregate([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {name:"Andy"}}, <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$sort: {age: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

### <a name="is-the-simba-driver-for-mongodb-supported-for-use-with-azure-cosmos-dbs-api-for-mongodb"></a>是否支持将 MongoDB 的 Simba 驱动程序与 Azure CosmosDB 的用于 MongoDB 的 API 一起使用？

是的，可以将 Simba 的 Mongo ODBC 驱动程序与 Azure Cosmos DB 的用于 MongoDB 的 API 一起使用

<!-- Not Available ## Table API -->

<!-- Not Available ## Gremlin API -->



<!-- Not Available on ## Cassandra API -->

[azure-portal]: https://portal.azure.cn
[query]: sql-api-sql-query.md

<!--Update_Description: update meta properties, update link, wording update -->