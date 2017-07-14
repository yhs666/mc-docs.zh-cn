---
title: "Azure Cosmos DB 常见问题解答 | Azure"
description: "获取有关 Azure Cosmos DB（全局分布式多模型数据库服务）的常见问题的解答。 了解容量、性能级别和缩放。"
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
origin.date: 05/29/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: bfb980672d779fe8d88631e7640b1b99cc04a60a
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# Azure Cosmos DB 常见问题解答
<a id="azure-cosmos-db-faq" class="xliff"></a>
## Azure Cosmos DB 基础知识
<a id="azure-cosmos-db-fundamentals" class="xliff"></a>
### 什么是 Azure Cosmos DB？
<a id="what-is-azure-cosmos-db" class="xliff"></a>
Azure Cosmos DB 是全局复制的多模型数据库服务，可针对无架构数据提供丰富的查询，帮助提供可靠的可配置性能，并支持快速开发。 这一切都通过一个托管平台来实现，而该平台有 Azure 强大的功能与影响力作为后盾。 

如果 Web、移动、游戏和 IoT 应用程序的关键要求是可预测的吞吐量、高可用性、低延迟和无架构数据模型，那么，Azure Cosmos DB 无疑是最合适的解决方案。 它提供了架构灵活性和丰富的索引，并且对集成的 JavaScript 提供了多文档事务性支持。 

有关部署和使用此服务的更多数据库问题、解答及说明，请参阅 [Azure Cosmos DB 文档页](/cosmos-db/)。

### DocumentDB 有哪些变化？
<a id="what-happened-to-documentdb" class="xliff"></a>
DocumentDB API 是适用于 Azure Cosmos DB 的受支持 API 和数据模型之一。 此外，Azure Cosmos DB 还支持图形 API（预览）、表 API（预览）和 MongoDB API。 有关详细信息，请参阅[来自 DocumentDB 客户的问题](#moving-to-cosmos-db)。

### 如何在 Azure 门户中访问 DocumentDB 帐户？
<a id="how-do-i-get-to-my-documentdb-account-in-the-azure-portal" class="xliff"></a>
在 Azure 门户的左侧窗格中，单击 Azure Cosmos DB 图标。 如果你以前已创建了一个 DocumentDB 帐户，则现在也有了一个 Azure Cosmos DB 帐户，费用不会有任何变化。

### Azure Cosmos DB 的典型用例有哪些？
<a id="what-are-the-typical-use-cases-for-azure-cosmos-db" class="xliff"></a>
对于侧重于以下要求的新 Web、移动、游戏和 IoT 应用程序而言，Azure Cosmos DB 是一个不错的选择：自动缩放、可预测的性能、毫秒响应时间的快速排序，以及查询无架构数据的能力。 Azure Cosmos DB 有助于快速开发，且支持应用程序数据模型的连续迭代。 用于管理用户生成的内容和数据的应用程序就是 [Azure Cosmos DB 的常见用例](use-cases.md)。 

### Azure Cosmos DB 如何提供可预测的性能？
<a id="how-does-azure-cosmos-db-offer-predictable-performance" class="xliff"></a>
[请求单位](request-units.md) (RU) 是 Azure Cosmos DB 中吞吐量的衡量单位。 1 RU 吞吐量相当于通过 GET 操作获取 1 KB 文档的吞吐量。 在 Azure Cosmos DB 中进行的每个操作（包括读、写、SQL 查询和执行存储过程）都具有一个确定的 RU 值，该值基于完成该操作所需的吞吐量。 无需考虑 CPU、IO 和内存，以及它们会怎样影响应用程序吞吐量，只需从单个 RU 度量值的角度进行考虑即可。

可以按照每秒吞吐量的 RU 数来保留每个 Azure Cosmos DB 容器的预配吞吐量。 对于任何规模的应用程序，都可以通过将单个请求设为基准来测量其 RU 值，并通过预配容器来处理所有请求的请求单位总和。 也可以随着应用程序的发展需求，扩展或缩减容器的吞吐量。 如需请求单位的详细信息以及确定容器需求方面的帮助，请参阅[估计吞吐量需求](request-units.md#estimating-throughput-needs)并尝试使用[吞吐量计算器](https://www.documentdb.com/capacityplanner)。 在这里，术语“容器”是指 DocumentDB API 集合、图形 API 图、MongoDB API 集合以及表 API 表。 

### Azure Cosmos DB 是否符合 HIPAA？
<a id="is-azure-cosmos-db-hipaa-compliant" class="xliff"></a>
是的，Azure Cosmos DB 符合 HIPAA。 HIPAA 针对可识别个人身份的健康信息的使用、泄露与保护制定了要求。 有关详细信息，请参阅 [Microsoft 信任中心](https://www.microsoft.com/TrustCenter/Compliance/HIPAA)。

### Azure Cosmos DB 的存储限制是什么？
<a id="what-are-the-storage-limits-of-azure-cosmos-db" class="xliff"></a>
对于容器可以存储在 Azure Cosmos DB 中的数据总量并没有任何限制。

### Azure Cosmos DB 的吞吐量限制是什么？
<a id="what-are-the-throughput-limits-of-azure-cosmos-db" class="xliff"></a>
在 Azure Cosmos DB 中，对于容器能够支持的总吞吐量并没有任何限制。 关键是要让工作负荷大致均匀地分布到足够多的分区键中。

### Azure Cosmos DB 的费用如何？
<a id="how-much-does-azure-cosmos-db-cost" class="xliff"></a>
有关详细信息，请参阅 [Azure Cosmos DB 定价详细信息](https://www.azure.cn/pricing/details/cosmos-db/)页。 Azure Cosmos DB 使用费取决于预配的容器数、容器的联机小时数，以及每个容器的预配吞吐量。 在这里，术语“容器”是指 DocumentDB API 集合、图形 API 图、MongoDB API 集合以及表 API 表。 

### 有免费帐户吗？
<a id="is-a-free-account-available" class="xliff"></a>
如果不熟悉 Azure，可以注册 [Azure 免费帐户](https://www.azure.cn/pricing/1rmb-trial/)，这样可以得到 30 天试用期和 200 美元信用额度，可试用所有 Azure 服务。 或者，如果有 Visual Studio 订阅，则有资格[免费获取每月 150 美元的 Azure 信用额度](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)，可用于任何 Azure 服务。 

也可以使用 [Azure Cosmos DB 模拟器](local-emulator.md)在本地免费开发和测试应用程序，无需创建 Azure 订阅。 如果对应用程序在 Azure Cosmos DB 模拟器中的工作情况感到满意，则可以切换到在云中使用 Azure Cosmos DB 帐户。

### 如何获取 Azure Cosmos DB 的更多帮助？
<a id="how-can-i-get-additional-help-with-azure-cosmos-db" class="xliff"></a>
如需任何帮助，请在 [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb) 或 [MSDN 论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDocumentDB)上联系我们，或者通过向 [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) 发送邮件安排与 Azure Cosmos DB 工程团队进行一对一交谈。 

## 设置 Azure Cosmos DB
<a id="set-up-azure-cosmos-db" class="xliff"></a>
### 如何注册 Azure Cosmos DB？
<a id="how-do-i-sign-up-for-azure-cosmos-db" class="xliff"></a>
可以在 Azure 门户中注册 Azure Cosmos DB。 首先，注册 Azure 订阅。 注册后即可将 DocumentDB API、图形 API（预览）、表 API（预览）或 MongoDB API 帐户添加到 Azure 订阅。

### 什么是主密钥？
<a id="what-is-a-master-key" class="xliff"></a>
主密钥是用于访问帐户的所有资源的安全令牌。 拥有此密钥的人对数据库帐户中的所有资源具有读取和写入访问权。 分发主密钥时需谨慎。 [Azure 门户][azure-portal]的“密钥”边栏选项卡中提供主要主密钥和辅助主密钥。 有关密钥的详细信息，请参阅[查看、复制和重新生成访问密钥](manage-account.md#keys)。

### 可以将 PreferredLocations 设置为哪些区域？
<a id="what-are-the-regions-that-preferredlocations-can-be-set-to" class="xliff"></a> 
可以将 PreferredLocations 值设置为提供 Cosmos DB 的任何 Azure 区域。 有关可用区域的列表，请参阅 [Azure 区域](https://azure.microsoft.com/regions/)。

### 通过 Azure 数据中心在全球分配数据时需要注意什么？
<a id="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters" class="xliff"></a> 
Azure Cosmos DB 存在于所有 Azure 区域，详见 [Azure 区域](https://azure.microsoft.com/regions/)页。 由于 Azure Cosmos DB 是核心服务，每个新数据中心都部署了它。 

设置区域时，请记住，Azure Cosmos DB 遵从主权和政府云的要求。 也就是说，如果你在某个主权区域创建了一个帐户，则不能将数据从该主权区域复制到外部区域。 同样，你不能将数据从外部帐户复制到其他主权位置。 

## 针对 DocumentDB API 进行开发
<a id="develop-against-the-documentdb-api" class="xliff"></a>

### 如何开始针对 DocumentDB API 进行开发？
<a id="how-do-i-start-developing-against-the-documentdb-api" class="xliff"></a>
[Azure 门户][azure-portal]提供 Microsoft DocumentDB API。 首先必须注册 Azure 订阅。 注册 Azure 订阅后，即可将 DocumentDB API 容器添加到 Azure 订阅。 有关添加 Azure Cosmos DB 帐户的说明，请参阅[创建 Azure Cosmos DB 数据库帐户](create-documentdb-dotnet.md#create-account)。 如果以前已创建了一个 DocumentDB 帐户，则现在也有了一个 Azure Cosmos DB 帐户。 

[SDK](documentdb-sdk-dotnet.md) 适用于 .NET、Python、Node.js、JavaScript 和 Java。 开发人员也可以利用 [RESTful HTTP API](https://msdn.microsoft.com/zh-cn/library/azure/dn781481.aspx)，从各种平台使用各种语言与 Azure Cosmos DB 资源进行交互。

### 是否可以访问一些现成的示例来帮助自己入门？
<a id="can-i-access-some-ready-made-samples-to-get-a-head-start" class="xliff"></a>
GitHub 上提供 DocumentDB API [.NET](documentdb-dotnet-samples.md)、[Java](https://github.com/Azure/azure-documentdb-java)、[Node.js](documentdb-nodejs-samples.md) 和 [Python](documentdb-python-samples.md) SDK 的示例。

### DocumentDB API 数据库是否支持无架构数据？
<a id="does-the-documentdb-api-database-support-schema-free-data" class="xliff"></a>
是的，DocumentDB API 允许应用程序存储任意 JSON 文档，不需要架构定义或提示。 通过 Azure Cosmos DB SQL 查询接口可立即查询数据。  

### DocumentDB API 是否支持 ACID 事务？
<a id="does-the-documentdb-api-support-acid-transactions" class="xliff"></a>
是的，DocumentDB API 支持以 JavaScript 存储过程和触发器形式表示的跨文档事务。 事务以每个集合内的单个分区为范围，且以 ACID 语义执行，也就是“全或无”，与其他并发执行代码和用户请求隔离。 如果在服务器端执行 JavaScript 应用程序代码的过程中引发了异常，则会回滚整个事务。 有关事务的详细信息，请参阅[数据库程序事务](programming.md#database-program-transactions)。

### 什么是集合？
<a id="what-is-a-collection" class="xliff"></a>
集合是一组文档及其关联的 JavaScript 应用程序逻辑。 集合是一个计费实体，其[成本](performance-levels.md)取决于吞吐量和所用存储。 集合可以跨一个或多个分区或服务器，并且可以通过伸缩处理几乎无限制增长的存储或吞吐量。

集合也是 Azure Cosmos DB 的计费实体。 每个集合根据预配的吞吐量和使用的存储空间按小时计费。 有关详细信息，请参阅 [DocumentDB API 定价](https://www.azure.cn/pricing/details/cosmos-db/)。 

### 我如何创建数据库？
<a id="how-do-i-create-a-database" class="xliff"></a>
可以使用 [Azure 门户](https://portal.azure.cn)（详见[添加集合](create-documentdb-dotnet.md#create-collection)中所述）、某个 [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) 或 [REST API](https://msdn.microsoft.com/zh-cn/library/azure/dn781481.aspx) 来创建数据库。 

### 我如何设置用户和权限？
<a id="how-do-i-set-up-users-and-permissions" class="xliff"></a>
可使用某个 [DocumentDB API SDK](documentdb-sdk-dotnet.md) 或 [REST API](https://msdn.microsoft.com/zh-cn/library/azure/dn781481.aspx) 来创建用户和权限。  

### DocumentDB API 是否支持 SQL？
<a id="does-the-documentdb-api-support-sql" class="xliff"></a>
SQL 查询语言是 SQL 支持的查询功能增强子集。 Azure Cosmos DB SQL 查询语言通过基于 JavaScript 的用户定义函数 (UDF)，提供丰富的分层和关系运算符以及可扩展性。 JSON 语法允许将 JSON 文档模型化为带标签节点的树状，由 Azure Cosmos DB 的自动索引技术及 Azure Cosmos DB 的 SQL 查询方言使用。 若要了解如何使用 SQL 语法，请参阅 [QueryDocumentDB][query] 一文。

### DocumentDB API 是否支持 SQL 聚合函数？
<a id="does-the-documentdb-api-support-sql-aggregation-functions" class="xliff"></a>
DocumentDB API 支持使用 SQL 语法通过聚合函数 `COUNT`、`MIN`、`MAX`、`AVG` 和 `SUM` 实现的任何规模的低延迟聚合。 有关详细信息，请参阅[聚合函数](documentdb-sql-query.md#Aggregates)。

### DocumentDB API 如何提供并发性？
<a id="how-does-the-documentdb-api-provide-concurrency" class="xliff"></a>
DocumentDB API 支持通过 HTTP 实体标记（也称 ETag）来实现乐观并发控制 (OCC)。 每个 DocumentDB API 资源都有一个 ETag。每次更新文档时，都会在服务器上设置此 ETag。 ETag 标头和当前值包含在所有响应消息中。 ETag 可与 If-Match 标头配合使用，让服务器决定是否应更新资源。 If-Match 值是用作检查依据的 ETag 值。 如果 ETag 值与服务器的 ETag 值匹配，就会更新资源。 如果 ETag 不再是最新状态，则服务器会拒绝该操作，并提供“HTTP 412 不满足前提条件”响应代码。 客户端接着会重新提取资源，以获取该资源当前的 ETag 值。 此外，ETag 可以与 If-None-Match 标头配合使用，以确定是否需重新提取资源。

若要在 .NET 中使用乐观并发，可以使用 [AccessCondition](https://msdn.microsoft.com/zh-cn/library/azure/microsoft.azure.documents.client.accesscondition.aspx) 类。 如需 .NET 示例，请参阅 GitHub 上 DocumentManagement 示例中的 [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs)。

### 如何在 DocumentDB API 中执行事务？
<a id="how-do-i-perform-transactions-in-the-documentdb-api" class="xliff"></a>
DocumentDB API 支持通过 JavaScript 存储过程和触发器进行的语言集成式事务。 脚本中的所有数据库操作都是在进行快照隔离的情况下执行的。 如果是单分区集合，则执行范围为集合。 如果集合已分区，则执行范围为该集合中具有相同分区键值的文档。 文档版本 (ETag) 的快照是在事务开始时获取的，且只有当脚本成功运行时才会提交。 如果 JavaScript 引发错误，则会回滚事务。 有关详细信息，请参阅 [DocumentDB API 服务器端编程](programming.md)。

### 如何将文档批量插入到 DocumentDB API 中？
<a id="how-can-i-bulk-insert-documents-into-the-documentdb-api" class="xliff"></a>
可以通过下述两种方式之一将文档批量插入到 Azure Cosmos DB 中：

* 数据迁移工具，如[将数据导入 DocumentDB API](import-data.md) 中所述。
* 存储过程，如 [DocumentDB API 服务器端编程](programming.md)中所述。

### DocumentDB API 是否支持资源链接缓存？
<a id="does-the-documentdb-api-support-resource-link-caching" class="xliff"></a>
是的，因为 Azure Cosmos DB 是 RESTful 服务，而资源链接固定不变，所以可以缓存。 DocumentDB API 客户端可以通过指定“If-None-Match”标头来读取任何资源（例如文档或集合），然后在服务器版本更改后更新本地副本。

### DocumentDB API 的本地实例是否可用？
<a id="is-a-local-instance-of-documentdb-api-available" class="xliff"></a>
是的。 [Azure Cosmos DB 模拟器](local-emulator.md)提供对 DocumentDB API 服务的高保真模拟。 它支持和 Azure Cosmos DB 相同的功能，包括支持创建和查询 JSON 文档、预配集合和调整集合的规模，以及执行存储过程和触发器。 可以使用 Azure Cosmos DB 模拟器开发和测试应用程序，并通过对 Azure Cosmos DB 的连接终结点进行单一配置更改将其部署到全局范围的 Azure。

## 针对 API for MongoDB 进行开发
<a id="develop-against-the-api-for-mongodb" class="xliff"></a>
### Azure Cosmos DB API for MongoDB 是什么？
<a id="what-is-the-azure-cosmos-db-api-for-mongodb" class="xliff"></a>
Azure Cosmos DB API for MongoDB 是一个兼容层，允许应用程序使用现有的、社区支持的 Apache MongoDB API 和驱动程序轻松、透明地与本机 Azure Cosmos DB 数据库引擎通信。 开发人员现在可以使用现有的 MongoDB 工具链和技术，生成能够充分利用 Azure Cosmos DB 的应用程序。 开发人员可以使用 Azure Cosmos DB 的独特功能，其中包括自动索引、备份维护、获得财务支持的服务级别协议 (SLA) 等。

### 如何连接到 API for MongoDB 数据库？
<a id="how-do-i-connect-to-my-api-for-mongodb-database" class="xliff"></a>
若要连接到 Azure Cosmos DB API for MongoDB，最快捷的方式是使用 [Azure 门户](https://portal.azure.cn)。 转到帐户，然后在左侧导航菜单上单击“快速启动”。 “快速启动”是获取连接到数据库的代码片段的最佳方式。 

Azure Cosmos DB 实施严格的安全要求和标准。 Azure Cosmos DB 帐户需要通过 SSL 进行身份验证和安全通信，因此务必使用 TLSv1.2。

有关详细信息，请参阅[连接到 API for MongoDB 数据库](connect-mongodb-account.md)。

### MongoDB 的 API 数据库是否有其他错误代码？
<a id="are-there-additional-error-codes-for-an-api-for-mongodb-database" class="xliff"></a>
除了常见的 MongoDB 错误代码，MongoDB API 还有其独特的错误代码：

| 错误               | 代码  | 说明  | 解决方案  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | 使用的请求单位总数超过了集合的预配请求单位比率，已达到限制。 | 请考虑从 Azure 门户缩放集合的吞吐量，或者重试。 |
| ExceededMemoryLimit | 16501 | 作为一种多租户服务，操作已超出客户端的内存配额。 | 通过限制性更强的查询条件缩小操作的作用域，或者通过 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)联系技术支持。 <br><br>示例：*&nbsp;&nbsp;&nbsp;&nbsp;db.getCollection('users').aggregate([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {name: "Andy"}}, <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$sort: {age: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## 使用表 API（预览）进行开发
<a id="develop-with-the-table-api-preview" class="xliff"></a>

### 术语
<a id="terms" class="xliff"></a> 
Azure Cosmos DB：表 API（预览）是 Azure Cosmos DB 的一款高级产品，旨在提供 Build 2017 中通告的表支持。 

标准表 SDK 是现有的 Azure 存储表 SDK。 

### 如何使用新的表 API（预览）产品？
<a id="how-can-i-use-the-new-table-api-preview-offering" class="xliff"></a> 
[Azure 门户][azure-portal]提供 Azure Cosmos DB 表 API。 首先必须注册 Azure 订阅。 注册以后，即可向 Azure 订阅添加 Azure Cosmos DB 表 API 帐户，然后向帐户添加表。 

在预览期间推出适用于 .NET 的 [SDK](../cosmos-db/table-sdk-dotnet.md) 后，即可完成[表 API](../cosmos-db/create-table-dotnet.md) 快速入门文章中的入门操作。

### 是否需要有新的 SDK 才能使用表 API（预览）？
<a id="do-i-need-a-new-sdk-to-use-the-table-api-preview" class="xliff"></a> 
是的，可以在 NuGet 上获取 [Windows Azure 存储高级表（预览）SDK](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable)。 如需更多信息，请参阅 [Azure Cosmos DB Table .NET API: Download and release notes](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md)（Azure Cosmos DB 表 .NET API：下载和发行说明）页。 

### 如何提供有关 SDK 或 Bug 的反馈？
<a id="how-do-i-provide-feedback-about-the-sdk-or-bugs" class="xliff"></a>
可通过以下途径提供反馈：

* [Uservoice](https://feedback.azure.com/forums/263030-documentdb)
* [MSDN 论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDocumentDB)
* [堆栈溢出](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### 连接到表 API（预览）需要使用哪个连接字符串？
<a id="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api-preview" class="xliff"></a>
连接字符串为 `DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.cn`。 可从 Azure 门户中的“密钥”页获取该连接字符串。 

### 如何在新的表 API（预览）中替代请求选项的配置设置？
<a id="how-do-i-override-the-config-settings-for-the-request-options-in-the-new-table-api-preview" class="xliff"></a>
有关配置设置的信息，请参阅 [Azure Cosmos DB 功能](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities)。 若要更改设置，可以在客户端应用程序中将其添加到 app.config 的 appSettings 节。

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>

### 对于使用现有的标准表 SDK 的客户，是否存在任何更改？
<a id="are-there-any-changes-for-customers-who-are-using-the-existing-standard-table-sdk" class="xliff"></a>
无。 对于使用现有的标准表 SDK 的现有客户或新客户来说，没有任何更改。 

### 如何查看 Azure Cosmos DB 中存储的、在表 API（预览）中使用的表数据？
<a id="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api-review" class="xliff"></a> 
可以使用 Azure 门户浏览数据。 也可以使用下一解答中所述的表 API（预览）代码或工具。 

### 哪些工具适用于表 API（预览）？
<a id="which-tools-work-with-the-table-api-preview" class="xliff"></a> 
可以使用旧版 Azure 资源管理器 (0.8.9)。

只要工具能够灵活使用前述格式的连接字符串，该工具就会支持新的表 API（预览）。 [Azure 存储客户端工具](../storage/storage-explorers.md)页中提供了表工具列表。 

### PowerShell 或 Azure CLI 是否适用于新的表 API（预览）？
<a id="do-powershell-or-azure-cli-work-with-the-new-table-api-preview" class="xliff"></a>
我们计划针对表 API（预览）添加对 PowerShell 和 Azure CLI 的支持。 

### 操作并发性是否受控制？
<a id="is-the-concurrency-on-operations-controlled" class="xliff"></a>
是的，通过使用 ETag 机制提供乐观并发。 

### 是否支持针对实体的 OData 查询模型？
<a id="is-the-odata-query-model-supported-for-entities" class="xliff"></a> 
是的，表 API（预览）支持 OData 查询和 LINQ 查询。 

### 是否可以同时在同一个应用程序中连接到标准 Azure 表和新的高级表 API（预览）？
<a id="can-i-connect-to-the-standard-azure-table-and-the-new-premium-table-api-preview-side-by-side-in-the-same-application" class="xliff"></a> 
是的，可以通过创建两个不同的 CloudTableClient 实例进行连接，每个实例都通过连接字符串指向各自的 URI。

### 如何将现有的 Azure 表存储应用程序迁移到此新产品？
<a id="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-new-offering" class="xliff"></a>
如果想要针对现有的表存储数据使用新的表 API 产品，请联系 [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com)。 

### 此服务的路线图是什么？何时提供其他标准表 API 功能？
<a id="what-is-the-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality" class="xliff"></a>
我们已计划在推出正式版的过程中，不断添加对 SAS 令牌、ServiceContext、统计、客户端加密、分析和其他功能的支持。 可以在 [Uservoice](https://feedback.azure.com/forums/263030-documentdb) 上提供反馈。 

### 如何在特定情况下为此服务扩展存储大小？比如，最初我有 *n* GB 的数据，但一段时间后我的数据会增长到 1 TB。
<a id="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time" class="xliff"></a> 
根据设计，可以通过横向缩放让 Azure Cosmos DB 提供无限的存储。 可以通过服务来监视并有效地增大存储。 

### 如何监视表 API（预览）产品？
<a id="how-do-i-monitor-the-table-api-preview-offering" class="xliff"></a>
可以使用表 API（预览）的“指标”窗格来监视请求和存储用量。 

### 如何计算所需的吞吐量？
<a id="how-do-i-calculate-the-throughput-i-require" class="xliff"></a>
可以使用容量估算器来计算操作所需的 TableThroughput。 有关详细信息，请参阅 [Estimate Request Units and Data Storage](https://www.documentdb.com/capacityplanner)（估算请求单位和数据存储）。 通常，可以将实体表示为 JSON，并为操作提供所需数量。 

### 是否可在本地将新的表 API（预览）SDK 与模拟器配合使用？
<a id="can-i-use-the-new-table-api-preview-sdk-locally-with-the-emulator" class="xliff"></a>
是的，使用新 SDK 时，可以在本地模拟器中使用表 API（预览）。 若要下载新的模拟器，请参阅[将 Azure Cosmos DB 模拟器用于本地开发和测试](local-emulator.md)。 app.config 中的 StorageConnectionString 值必须是 `DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`。 

### 现有的应用程序能否使用表 API（预览）？
<a id="can-my-existing-application-work-with-the-table-api-preview" class="xliff"></a> 
在 .NET API 中执行创建、删除、更新、查询操作时，新表 API（预览）的外围应用与现有的 Azure 标准表 SDK 兼容。 确保有一个行键，因为表 API（预览）需要分区键和行键。 我们还计划在正式推出此服务产品时，添加更多的 SDK 支持。

### 如果不想要使用表 API（预览）功能，是否需要将现有的基于 Azure 表的应用程序迁移到新的 SDK？
<a id="do-i-need-to-migrate-my-existing-azure-table-based-applications-to-the-new-sdk-if-i-do-not-want-to-use-the-table-api-preview-features" class="xliff"></a>
否。你可以创建和使用现有的标准表资产，不会遇到任何类型的中断。 但是，如果不使用新的表 API（预览），则无法使用自动索引、其他一致性选项或全局分发。 

### 如何为高级表 API（预览）中跨多个 Azure 区域的数据添加复制功能？
<a id="how-do-i-add-replication-of-the-data-in-the-premium-table-api-preview-across-multiple-regions-of-azure" class="xliff"></a>
可以使用 Azure Cosmos DB 门户的[全局复制设置](tutorial-global-distribution-documentdb.md#portal)来添加适合应用程序的区域。 若要开发全局分布式应用程序，还应添加其 PreferredLocation 信息已设置为本地区域的应用程序，以减轻读取延迟。 

### 如何更改高级表 API（预览）中帐户的主要写入区域？
<a id="how-do-i-change-the-primary-write-region-for-the-account-in-the-premium-table-api-preview" class="xliff"></a>
可以使用 Azure Cosmos DB 的全局复制门户窗格来添加区域，然后故障转移到所需的区域。 有关说明，请参阅[使用多区域 Azure Cosmos DB 帐户进行开发](regional-failover.md)。 

### 如何配置首选的读取区域以降低分配数据时的延迟？
<a id="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data" class="xliff"></a> 
请使用 app.config 文件中的 PreferredLocation 键，方便从本地位置读取。 对于现有应用程序，表 API（预览）会在已设置 LocationMode 的情况下引发错误。 请删除该代码，因为高级表 API（预览）可从 app.config 文件选取该信息。 有关详细信息，请参阅 [Azure Cosmos DB 功能](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities)。

### 对于表 API（预览）中的一致性级别，应如何考虑？
<a id="how-should-i-think-about-consistency-levels-in-the-table-api-preview" class="xliff"></a> 
Azure Cosmos DB 在一致性、可用性和延迟之间提供合理的平衡。 Azure Cosmos DB 为表 API（预览）开发人员提供五种一致性级别，因此，你可以在表级别选择适当的一致性模型，并在查询数据时发出单个请求。 客户端可以在连接时指定一致性级别。 可以根据 TableConsistencyLevel 键的值，通过 app.config 设置更改该级别。 

表 API（预览）默认采用“会话”一致性，允许“读取你自己的写入内容”，因此在读取时延迟不严重。 有关详细信息，请参阅[一致性级别](consistency-levels.md)。 

Azure 表存储默认在区域中提供“强”一致性，在辅助位置提供“最终”一致性。 

### Azure Cosmos DB 是否提供比标准表更多的一致性级别？
<a id="does-azure-cosmos-db-offer-more-consistency-levels-than-standard-tables" class="xliff"></a>
是的。若要了解如何利用 Azure Cosmos DB 的分布式特性，请参阅[一致性级别](consistency-levels.md)。 由于一致性级别有充分的保证，因此可以放心使用。 有关详细信息，请参阅 [Azure Cosmos DB 功能](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities)。

### 启用全局分发后，需要花费多长时间复制数据？
<a id="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data" class="xliff"></a>
我们将数据持久存储于本地区域，在将其推送到其他区域时则速度很快，只需数毫秒的时间。 这种形式的复制只取决于数据中心的往返时间 (RTT)。 若要详细了解 Azure Cosmos DB 的全局分发功能，请参阅 [Azure Cosmos DB：Azure 上的全局分布式数据库服务](distribute-data-globally.md)。

### 是否可以更改读取请求一致性级别？
<a id="can-the-read-request-consistency-level-be-changed" class="xliff"></a>
使用 Azure Cosmos DB 时，可以在容器级别（在表上）设置一致性级别。 使用 SDK 时，若要更改一致性级别，可以在 app.config 文件中提供 TableConsistencyLevel 键值。 可能的值：Strong、Bounded Staleness、Session、ConsistentPrefix、Eventual。 有关详细信息，请参阅 [Azure Cosmos DB 中可以优化的数据一致性级别](consistency-levels.md)。 关键是，在设置请求一致性级别时，不能超出表的设置。 例如，不能将表的一致性级别设置为“Eventual”，将请求一致性级别设置为“Strong”。 

### 区域发生故障时，高级表 API（预览）帐户如何处理故障转移？
<a id="how-does-the-premium-table-api-preview-account-handle-failover-if-a-region-goes-down" class="xliff"></a> 
高级表 API（预览）利用 Azure Cosmos DB 的全局分布式平台。 若要确保应用程序能够容许数据中心停机，请在 Azure Cosmos DB 门户中为帐户额外启用至少一个区域。详见[使用多区域 Azure Cosmos DB 帐户进行开发](regional-failover.md)。 可以使用门户设置区域的优先级。请参阅[使用多区域 Azure Cosmos DB 帐户进行开发](regional-failover.md)。 

可以视需要为帐户添加任意数目的区域，并通过提供故障转移优先级来控制可将该帐户故障转移到哪个位置。 当然，若要使用数据库，还需在其中提供应用程序。 这样操作时，客户不会遇到停机的情况。 客户端 SDK 可自动寻址。 换句话说，它能够检测到有故障的区域，并自动故障转移到新区域。

### 高级表 API（预览）是否支持备份？
<a id="is-the-premium-table-api-preview-enabled-for-backups" class="xliff"></a>
是的，高级表 API（预览）可利用 Azure Cosmos DB 的平台进行备份。 可自动进行备份。 有关详细信息，请参阅[使用 Azure Cosmos DB 进行联机备份和还原](online-backup-and-restore.md)。

### 默认情况下，表 API（预览）是否为实体的所有属性编制索引？
<a id="does-the-table-api-preview-index-all-attributes-of-an-entity-by-default" class="xliff"></a>
是的，默认情况下会为实体的所有属性编制索引。 有关详细信息，请参阅 [Azure Cosmos DB：索引策略](indexing-policies.md)。 

### 这是否意味着，无需创建多个索引来满足查询要求？
<a id="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries" class="xliff"></a> 
是的，Azure Cosmos DB 针对所有属性提供自动索引，无需任何架构定义。 这种自动化操作可以让开发人员将工作重点放在应用程序上，不必考虑索引的创建和管理。 有关详细信息，请参阅 [Azure Cosmos DB：索引策略](indexing-policies.md)。

### 是否可以更改索引策略？
<a id="can-i-change-the-indexing-policy" class="xliff"></a>
是的，可以通过提供索引定义来更改索引策略。 有关详细信息，请参阅 [Azure Cosmos DB 功能](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities)。 需要在 app.config 文件中使用字符串 json 格式 

适当地将这些设置编码并转义：
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

### 平台形式的 Azure Cosmos DB 似乎有许多的功能，例如排序、聚合、分层等。 是否要向表 API 添加这些功能？
<a id="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api" class="xliff"></a> 
在预览版中，表 API 提供与 Azure 表存储相同的查询功能。 Azure Cosmos DB 还支持排序、聚合、地理空间查询、层次结构和各种内置函数。 我们会通过将来的服务更新在表 API 中提供更多功能。 有关详细信息，请参阅 [Azure Cosmos DB 查询](../documentdb/documentdb-sql-query.md)。

### 何时应更改表 API（预览）的 TableThroughput？
<a id="when-should-i-change-tablethroughput-for-the-table-api-preview" class="xliff"></a>
应在符合以下某个条件时，更改 TableThroughput：
* 要对数据执行提取、转换和加载 (ETL) 操作，或者需在短时间内上传大量数据。 
* 需要来自后端容器的更多吞吐量。 例如，你发现所使用的吞吐量超出预配的吞吐量，导致操作受限。 有关详细信息，请参阅[设置吞吐量](set-throughput.md)。

### 是否可以扩展或缩减表 API（预览）表的吞吐量？
<a id="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-preview-table" class="xliff"></a> 
是的，可以使用 Azure Cosmos DB 门户的缩放窗格来缩放吞吐量。 有关详细信息，请参阅[设置吞吐量](set-throughput.md)。

### 高级表 API（预览）是否可以使用“每分钟 RU”服务？
<a id="can-the-premium-table-api-preview-take-advantage-of-the-ru-per-minute-offering" class="xliff"></a> 
是的，高级表 API（预览）可以利用 Azure Cosmos DB 的功能，提供有关性能、延迟、可用性和一致性的 SLA。 此功能确保表可以使用“每分钟 RU”服务。 有关详细信息，请参阅 [Azure Cosmos DB 中的请求单位](request-units.md)。 借助此功能，客户无需预配峰值时的吞吐量，而且能使工作负荷的尖峰变得平缓。

### 新预配的表是否设置了默认的 TableThroughput？
<a id="is-a-default-tablethroughput-set-for-newly-provisioned-tables" class="xliff"></a>
是的，如果未通过 app.config 替代 TableThroughput，并且未使用 Azure Cosmos DB 中预先创建的容器，则该服务将创建吞吐量为 400 的表。

### 适用于现有标准表 API 客户的价格是否有变化？
<a id="is-there-any-change-of-pricing-for-existing-customers-of-the-standard-table-api" class="xliff"></a>
无。 适用于现有标准表 API 客户的价格没有变化。 

### 表 API（预览）的价格是怎样计算的？
<a id="how-is-the-price-calculated-for-the-table-api-preview" class="xliff"></a> 
价格取决于分配的 TableThroughput。 

### 如何在表 API（预览）产品中处理对表设置的任何限制？
<a id="how-do-i-handle-any-throttling-on-the-tables-in-table-api-preview-offering" class="xliff"></a> 
如果请求率超过了底层容器的预配吞吐量容量，则会出现错误，SDK 会应用重试策略来重试调用。

### 为何需要选择吞吐量而不是 PartitionKey 和 RowKey 来利用 Azure Cosmos DB 的高级表 API（预览）产品？
<a id="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-premium-table-api-preview-offering-of-azure-cosmos-db" class="xliff"></a>
如果未在 app.config 文件中提供吞吐量，Azure Cosmos DB 将为容器设置默认的吞吐量。 

Azure Cosmos DB 针对操作设置上限，在性能和延迟方面提供保证。 如果引擎可以针对租户的操作实施调控，则可以提供这样的保证。 设置 TableThroughput 可确保吞吐量和延迟值得到保证，因为平台会保留相应的容量，保证操作成功。 

考虑到应用程序在使用需求方面存在“季节性”，可以通过吞吐量规范来弹性更改吞吐量，在满足吞吐量需求的同时节省成本。

### Microsoft Azure 存储 SDK 对我来说很经济适用，因为我只需支付数据存储费用，我很少进行查询。 但是，即使我未执行任何事务或者存储任何数据，新的 Azure Cosmos DB 产品似乎也要收费。 能否请你解释一下？
<a id="microsoft-azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-new-azure-cosmos-db-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain" class="xliff"></a>

根据设计，Azure Cosmos DB 是一个全局分布式的、基于 SLA 的系统，在可用性、延迟和吞吐量方面提供保证。 如果在 Azure Cosmos DB 中保留吞吐量，吞吐量就会获得保证，这与其他系统的吞吐量有所不同。 Azure Cosmos DB 会根据客户请求提供额外功能，例如辅助索引和全局分发。 我们在预览期提供吞吐量优化模型，并计划最终提供存储优化模型来满足客户的需求。 

### 将数据引入表存储时，我从来不会收到“配额已用完”通知（表明分区已满）。 但使用表 API（预览）时，我会收到该消息。 是此产品有限制，迫使我更改现有的应用程序吗？
<a id="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-table-storage-with-the-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application" class="xliff"></a>

Azure Cosmos DB 是基于 SLA 的系统，可提供无限缩放，并在延迟、吞吐量、可用性和一致性方面提供保证。 请确保数据大小和索引可管理且可缩放，这样才能获得有保障的高级性能。 针对每个分区键的实体或项数实施 10 GB 限制是为了确保提供优异的查找和查询性能。 为了确保应用程序即使对于 Azure 存储也能正常缩放，建议不要创建热分区（将所有信息存储在一个分区中并查询该分区）。 

### 那么，新的表 API（预览）是否仍然需要 PartitionKey 和 RowKey？
<a id="so-partitionkey-and-rowkey-are-still-required-with-the-new-table-api-preview" class="xliff"></a> 
是的。 由于表 API（预览）的外围应用与表存储 SDK 的类似，使用分区键可以有效地分配数据。 行键在该分区中是唯一的。 行键必须存在，且不能为 null（在标准 SDK 中可为 null）。 RowKey 的长度为 255 字节，PartitionKey 的长度为 100 字节（即将提高到 1 KB）。 

### 表 API（预览）显示哪些错误消息？
<a id="what-are-the-error-messages-for-the-table-api-preview" class="xliff"></a>
由于此预览版与标准表兼容，大多数错误与标准表中的错误是一致的。 

### 在表 API（预览）中尝试一个接一个创建大量的表时，为何会受到限制？
<a id="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api-preview" class="xliff"></a>
Azure Cosmos DB 是基于 SLA 的系统，提供延迟、吞吐量、可用性和一致性方面的保证。 由于它是预配的系统，因此会保留资源来保证满足这些要求。 以极快的频率创建表会被系统检测到，因此会受到限制。 建议你查看表的创建频率，将其降到每分钟 5 个表以下。 请记住，表 API（预览）是一个预配的系统。 计费从预配那一刻开始。 

## 针对图形 API（预览）进行开发
<a id="develop-against-the-graph-api-preview" class="xliff"></a>
### 如何对 Azure Cosmos DB 应用图形 API（预览）的功能？
<a id="how-can-i-apply-the-functionality-of-graph-api-preview-to-azure-cosmos-db" class="xliff"></a>
可以通过扩展库来应用图形 API（预览）的功能。 该库称为 Azure Graph，在 NuGet 上提供。 

### 看起来你们支持 Gremlin 图形遍历语言。 是否计划添加更多形式的查询？
<a id="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query" class="xliff"></a>
是的，我们计划将来添加其他查询机制。 

### 如何使用新的图形 API（预览）产品？
<a id="how-can-i-use-the-new-graph-api-preview-offering" class="xliff"></a> 
完成[图形 API](../cosmos-db/create-graph-dotnet.md) 快速入门一文即可开始。

<a id="moving-to-cosmos-db"></a>
## 来自 DocumentDB 客户的问题
<a id="questions-from-documentdb-customers" class="xliff"></a>
### 为何要改用 Azure Cosmos DB？
<a id="why-are-you-moving-to-azure-cosmos-db" class="xliff"></a> 

Azure Cosmos DB 是全局分布式大规模云数据库的重大飞跃。 现在，DocumentDB 客户可以访问 Azure Cosmos DB 提供的突破性系统和功能。

Azure Cosmos DB 在 2010 年以“Project Florence”的名义开始解决 Microsoft 内部负责构建大型应用程序的开发人员所面临的难题。 构建全局分布式应用并不是只有 Microsoft 才会遇到的难题，因此我们在 2015 年以 Azure DocumentDB 的形式向 Azure 开发人员提供了与此相关的第一代技术。 

自此之后，我们添加了新的特性，并引入了重要的新功能。 成果就是 Azure Cosmos DB。 此版本的一项功能是允许 DocumentDB 客户自动且无缝地成为 Azure Cosmos DB 客户，不会丢失其数据。 这些功能涉及的领域包括核心数据库引擎以及全局分发、弹性可伸缩性，并享有行业领先的全面 SLA 保障。 具体而言，我们已改进了 Azure Cosmos DB 数据库引擎，使其能够有效地将所有热门数据模型、类型系统和 API 映射到 Azure Cosmos DB 的基础数据模型。 

对于开发人员而言，此项工作的最新成果包括对 [Gremlin](../cosmos-db/graph-introduction.md) 和[表存储 API](../cosmos-db/table-introduction.md) 的全新支持。 而这只是开端。 我们计划不断添加其他常用 API 和更新的数据模型，为全球规模的性能和存储技术带来更多进步。 

必须指出的是，DocumentDB 的 [SQL 语言](../documentdb/documentdb-sql-query.md)始终是基础 Azure Cosmos DB 能够支持的多种 API 之一。 开发人员使用 Azure Cosmos DB 等完全托管的服务时，该服务的唯一接口就是该服务公开的 API。 现有 DocumentDB 客户在操作时与以往其实并无不同。 Azure Cosmos DB 提供的 SQL API 与 DocumentDB 提供的完全相同。 我们会在现在（以及未来）向你提供以前接触不到的其他功能。 

我们持续努力的另一个成果是扩建了吞吐量和存储的全局分发与弹性缩放的基础。 就可伸缩性来说，其中的一个前沿成果是 [RU/分钟](../cosmos-db/request-units-per-minute.md)，但我们计划推出更多功能，为客户减少各种工作负荷的成本。 我们对全局分发子系统做了几项基础性的增强。 在许多可供开发人员使用的功能中，其中之一就是前缀一致性模型（总共提供五个定义完善的一致性模型）。 我们还有许多其他的有用功能，一旦其成熟就会发布。 

### 如何确保 DocumentDB 资源可继续在 Azure Cosmos DB 上运行？
<a id="what-do-i-need-to-do-to-ensure-that-my-documentdb-resources-continue-to-run-on-azure-cosmos-db" class="xliff"></a>

不需要进行任何更改。 DocumentDB 资源现在称为 Azure Cosmos DB 资源，过渡后，服务不会发生任何中断。

### 要进行哪些更改才能让应用配合 Azure Cosmos DB 工作？
<a id="what-changes-do-i-need-to-make-for-my-app-to-work-with-azure-cosmos-db" class="xliff"></a>

不需要进行任何更改。 类、命名空间和 NuGet 包名称都没有变化。 与往常一样，我们建议你使用最新的 SDK，以利用最新的功能和改进项目。 

### Azure 门户有哪些变化？
<a id="whats-changed-in-the-azure-portal" class="xliff"></a>

DocumentDB 不再以 Azure 服务的形式显示在门户中。 代替它的是新的 Azure Cosmos DB 图标，如下图所示。 可以像以前一样使用所有集合，并且仍可缩放吞吐量、更改一致性级别和监视 SLA。 数据资源管理器（预览）的功能已得到增强。 现在，可以在一个页面中查看和编辑文档、创建和运行查询，以及使用存储过程、触发器和 UDF，如下图所示： 

![“Azure Cosmos DB 集合”边栏选项卡](./media/faq/cosmos-db-data-explorer.png)

### 价格是否有变化？
<a id="are-there-changes-to-pricing" class="xliff"></a>

没有，在 Azure Cosmos DB 上运行应用的费用与以前相同。 但是，你会受益于新推出的“每分钟请求单位数”功能。 有关详细信息，请参阅[缩放每分钟吞吐量](../cosmos-db/request-units-per-minute.md)一文。

### SLA 是否有变化？
<a id="are-there-changes-to-the-slas" class="xliff"></a>

没有，有关可用性、一致性、延迟和吞吐量的 SLA 都保持不变，并且仍会显示在门户中。 有关详细信息，请参阅 [SLA for Azure Cosmos DB](https://www.azure.cn/support/sla/cosmos-db/)（Azure Cosmos DB 的 SLA）。

![包含示例数据的待办事项应用](./media/faq/azure-cosmosdb-portal-metrics-slas.png)

[azure-portal]: https://portal.azure.cn
[query]: documentdb-sql-query.md