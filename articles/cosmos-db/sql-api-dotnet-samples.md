---
title: Azure Cosmos DB：SQL API 的 .NET 示例 | Azure
description: 在 github 上查找使用 Azure Cosmos DB SQL API 的常见任务的 C# .NET 示例，包括 CRUD 操作。
keywords: NoSQL 示例
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: sample
origin.date: 02/22/2017
ms.date: 12/03/2018
ms.author: v-yeche
ms.openlocfilehash: b6dea2113e6e6471d57ad5d116cf22475a98c5a4
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675453"
---
# <a name="azure-cosmos-db-net-examples-for-the-sql-api"></a>Azure Cosmos DB：SQL API 的 .NET 示例
> [!div class="op_single_selector"]
> * [.NET 示例](sql-api-dotnet-samples.md)
> * [Java 示例](sql-api-java-samples.md)
> * [异步 Java 示例](sql-api-async-java-samples.md)
> * [Node.js 示例](sql-api-nodejs-samples.md)
> * [Python 示例](sql-api-python-samples.md)
> * [Azure 代码示例库](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

对 Azure Cosmos DB 资源执行 CRUD 操作和其他常见操作的最新示例解决方案包含在 [azure-documentdb-dotnet](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples) GitHub 存储库中。 本文提供：

* 指向每个示例 C# 项目文件中各项任务的链接。 
* 指向相关的 API 参考内容的链接。

**先决条件**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- 可以[激活 Visual Studio 订户权益](https://www.azure.cn/support/legal/offer-rate-plans/)：Visual Studio 订阅每月为用户提供可用于付费版 Azure 服务的信用额度。

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

也需要 [Microsoft.Azure.DocumentDB NuGet 程序包](http://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)。 

> [!NOTE]
> 每个示例都是独立的，自行对自身进行设置并在完成后自行进行清理。 同样地，这些示例对 CreateDocumentCollectionAsync() 发出多个调用。 每当执行此操作时，即会根据所创建的集合的性能层，对订阅收取使用 1 小时的费用。 
> 
> 

## <a name="database-examples"></a>数据库示例
DatabaseManagement 项目示例的 [RunDatabaseDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L72-L121) 方法演示如何执行以下任务。

| 任务 | API 参考 |
| --- | --- |
| [创建数据库](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L90) |[DocumentClient.CreateDatabaseAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createdatabaseasync) |
| [Query a database](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L81) |[DocumentQueryable.CreateDatabaseQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdatabasequery.aspx) |
| [按 ID 读取数据库](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L102) |[DocumentClient.ReadDatabaseAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readdatabaseasync) |
| [Read all the databases](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L108-L113) |[DocumentClient.ReadDatabaseFeedAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readdatabasefeedasync) |
| [删除数据库](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/DatabaseManagement/Program.cs#L118) |[DocumentClient.DeleteDatabaseAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.deletedatabaseasync) |

## <a name="collection-examples"></a>集合示例
CollectionManagement 项目示例的 [RunCollectionDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/530c8d9cf7c99df7300246da05206c57ce654233/samples/code-samples/CollectionManagement/Program.cs#L96-L185) 方法演示如何执行以下任务。

| 任务 | API 参考 |
| --- | --- |
| [创建集合](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L101) |[DocumentClient.CreateDocumentCollectionAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync) |
| [Get configured performance of a collection](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L198) |[DocumentQueryable.CreateOfferQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx) |
| [Change configured performance of a collection](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L207) |[DocumentClient.ReplaceOfferAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync) |
| [按 ID 获取集合](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L153) |[DocumentClient.ReadDocumentCollectionAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync) |
| [Read all the collections in a database](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L162) |[DocumentClient.ReadDocumentCollectionFeedAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentcollectionfeedasync) |
| [删除集合](https://github.com/Azure/azure-documentdb-dotnet/blob/89670bc8aefd9bdd932db7f9b6d2fcb9b6acf35e/samples/code-samples/CollectionManagement/Program.cs#L175) |[DocumentClient.DeleteDocumentCollectionAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentcollectionasync) |

## <a name="document-examples"></a>文档示例
DocumentManagement 项目示例的 [RunDocumentsDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L97-L102) 方法演示如何执行以下任务。

| 任务 | API 参考 |
| --- | --- |
| [创建文档](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L198) |[DocumentClient.CreateDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync) |
| [按 ID 读取文档](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L211) |[DocumentClient.ReadDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync) |
| [Read all the documents in a collection](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L227) |[DocumentClient.ReadDocumentFeedAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync) |
| [查询文档](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L248-L251) |[DocumentClient.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [替换文档](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L263) |[DocumentClient.ReplaceDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync) |
| [更新插入文档](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L300) |[DocumentClient.UpsertDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.upsertdocumentasync) |
| [删除文档](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L322) |[DocumentClient.DeleteDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.deletedocumentasync) |
| [使用 .NET 动态对象](https://github.com/Azure/azure-documentdb-dotnet/blob/f374cc601f4cf08d11c88f0c3fa7dcefaf7ecfe8/samples/code-samples/DocumentManagement/Program.cs#L331-L380) |[DocumentClient.CreateDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync)<br>[DocumentClient.ReadDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentasync)<br>[DocumentClient.ReplaceDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentasync) |
| [使用条件 ETag 检查替换文档](https://github.com/Azure/azure-documentdb-dotnet/blob/f2b11dec45a195ddeed333560ebba63055f5ed09/samples/code-samples/DocumentManagement/Program.cs#L398-L440) |[DocumentClient.AccessCondition](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.accesscondition.)<br>[Documents.Client.AccessConditionType](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.accessconditiontype) |
| [仅当文档已更改时读取文档](https://github.com/Azure/azure-documentdb-dotnet/blob/f2b11dec45a195ddeed333560ebba63055f5ed09/samples/code-samples/DocumentManagement/Program.cs#L442-L470) |[DocumentClient.AccessCondition](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.accesscondition)<br>[Documents.Client.AccessConditionType](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.accessconditiontype) |

## <a name="indexing-examples"></a>索引示例
IndexManagement 项目示例的 [RunIndexDemo](https://github.com/Azure/azure-documentdb-dotnet/blob/ea8c977b9c2f37ddc2894911ec239907ab60e40a/samples/code-samples/IndexManagement/Program.cs#L89-L117) 方法演示如何执行以下任务。

| 任务 | API 参考 |
| --- | --- |
| [从索引中排除文档](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L125-L163) |[IndexingDirective.Exclude](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.indexingdirective) |
| [使用手动（而非自动）索引](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L171-L209) |[IndexingPolicy.Automatic](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.indexingpolicy.automatic) |
| [使用延迟（而非一致）索引](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L221-L238) |[IndexingMode.Lazy](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.indexingpolicy.indexingmode#P:Microsoft.Azure.Documents.IndexingPolicy.IndexingMode) |
| [从索引中排除指定的文档路径](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L248-L297) |[IndexingPolicy.ExcludedPaths](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.indexingpolicy.excludedpaths) |
| [对哈希索引路径强制执行范围扫描操作](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L305-L340) |[FeedOptions.EnableScanInQuery](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.feedoptions.enablescaninquery) |
| [对字符串使用范围索引](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L342-L405) |[IndexingPolicy.IncludedPaths](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.indexingpolicy.includedpaths.aspx)<br>[RangeIndex](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.rangeindex) |
| [执行索引转换](https://github.com/Azure/azure-documentdb-dotnet/blob/2e9a48b6a446b47dd6182606c8608d439b88b683/samples/code-samples/IndexManagement/Program.cs#L407-L464) |[ReplaceDocumentCollectionAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentcollectionasync) |

有关索引的详细信息，请参阅 [Azure Cosmos DB 索引策略](indexing-policies.md)。

## <a name="geospatial-examples"></a>地理空间示例
地理空间示例文件 [azure-documentdb-dotnet/samples/code-samples/Geospatial/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs)演示如何执行以下任务。  

| 任务 | API 参考 |
| --- | --- |
| [对新集合启用地理空间索引](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L45-L63) |[IndexingPolicy](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.indexingpolicy) <br> [IndexKind.Spatial](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.indexkind) <br>[DataType.Point](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.datatype) |
| [使用 GeoJSON 点插入文档](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L116-L126) |[DocumentClient.CreateDocumentAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync?view=azure-dotnet) </br> [DataType.Point](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.datatype) |
| [查找指定距离内的点](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L152-L194) |[ST_DISTANCE](how-to-sql-query.md#BuiltinFunctions) </br> [GeometryOperationExtensions.Distance](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.distance?view=azure-dotnet) |
| [查找多边形内的点](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L196-L221) |[ST_WITHIN](how-to-sql-query.md#BuiltinFunctions) </br> [GeometryOperationExtensions.Within](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.distance?view=azure-dotnet) 和 </br>[多边形](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.spatial.polygon?view=azure-dotnet) |
| [对现有集合启用地理空间索引](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L312-L336) |[DocumentClient.ReplaceDocumentCollectionAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.replacedocumentcollectionasync.aspx)<br>[DocumentCollection.IndexingPolicy](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.documentcollection.indexingpolicy#P:Microsoft.Azure.Documents.DocumentCollection.IndexingPolicy) |
| [验证点和多边形数据](https://github.com/Azure/azure-documentdb-dotnet/blob/7b09c085817e850d683bc59bd864c2f6b552d275/samples/code-samples/Geospatial/Program.cs#L223-L265) |[ST_ISVALID](how-to-sql-query.md#BuiltinFunctions)<br>[ST_ISVALIDDETAILED](how-to-sql-query.md#BuiltinFunctions)<br>[GeometryOperationExtensions.IsValid](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.spatial.geometryoperationextensions.isvalid.aspx)<br>[GeometryOperationExtensions.IsValidDetailed](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.spatial.geometryoperationextensions.isvaliddetailed) |

有关使用地理空间数据的详细信息，请参阅[使用 Azure Cosmos DB 中的地理空间数据](geospatial.md)。  

## <a name="query-examples"></a>查询示例
查询文档文件 [azure-documentdb-dotnet/samples/code-samples/Queries/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs)演示如何通过 SQL 查询语法以及使用查询和 Lambda 的 LINQ 提供程序执行以下各项任务。

| 任务 | API 参考 |
| --- | --- |
| [查询所有文档](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L122-L138) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用 == 查询等式](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L251-L268) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用 != 和 NOT 查询不等式](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L270-L276) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用 >、<、>=、<= 等范围运算符进行查询](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L305-L325) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用范围运算符对字符串进行查询](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L337-L346) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用 Order By 进行查询](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L370-L392) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用聚合函数进行查询](https://github.com/arramac/azure-documentdb-dotnet/blob/198bed2865e54af6681fc96b3ca253d31b113b9a/samples/code-samples/Queries/Program.cs#L451-L455) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用子文档](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L394-L419) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用文档内联接进行查询](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L421-L435) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用字符串、数学和数组运算符进行查询](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L527-L552) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [通过使用 SqlQuerySpec 的参数化 SQL 进行查询](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L140-L174) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx.aspx)<br>[SqlQuerySpec](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.sqlqueryspec) |
| [使用显式分页进行查询](https://github.com/Azure/azure-documentdb-dotnet/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/Queries/Program.cs#L554-L576) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [并行查询已分区集合](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs#L664-L734) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |
| [使用 Order by 针对已分区集合进行查询](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs#L737-L810) |[DocumentQueryable.CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) |

有关编写查询的详细信息，请参阅 [Azure Cosmos DB 中的 SQL 查询](how-to-sql-query.md)。

## <a name="change-feed-examples"></a>更改源示例 
更改源示例，[azure-documentdb-dotnet/samples/code-samples/ChangeFeed/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ServerSideScripts/Program.cs) 演示如何执行以下任务。 

| 任务 | API 参考 |
| --- | --- |
| [读取更改源](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ChangeFeed/Program.cs#L132) |[DocumentClient.CreateDocumentChangeFeedQuery](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery) | 
| [读取分区键范围](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ChangeFeed/Program.cs#L118) |[DocumentClient.ReadPartitionKeyRangeFeedAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync) | 

更改源处理器示例：[更改源迁移工具](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedMigrationTool)演示如何使用更改源处理器库将数据复制到另一个 Cosmos DB 集合。   

## <a name="server-side-programming-examples"></a>服务器端编程示例
服务器端编程文件 [azure-documentdb-dotnet/samples/code-samples/ServerSideScripts/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/ServerSideScripts/Program.cs)演示如何执行以下任务。

| 任务 | API 参考 |
| --- | --- |
| [创建存储过程](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L112) |[DocumentClient.CreateStoredProcedureAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createstoredprocedureasync) |
| [执行存储过程](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L127) |[DocumentClient.ExecuteStoredProcedureAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.executestoredprocedureasync) |
| [读取存储过程的文档源](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L194) |[DocumentClient.ReadDocumentFeedAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync) |
| [使用 Order by 创建存储过程](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L219) |[DocumentClient.CreateStoredProcedureAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createstoredprocedureasync) |
| [创建预触发器](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L264) |[DocumentClient.CreateTriggerAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createtriggerasync) |
| [创建后触发器](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L329) |[DocumentClient.CreateTriggerAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createtriggerasync) |
| [创建用户定义函数 (UDF)](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/ServerSideScripts/Program.cs#L389) |[DocumentClient.CreateUserDefinedFunctionAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createuserdefinedfunctionasync) |

有关服务器端编程的详细信息，请参阅 [Azure Cosmos DB 服务器端编程：存储过程、数据库触发器和 UDF](programming.md)。

## <a name="user-management-examples"></a>用户管理示例
用户管理文件 [azure-documentdb-dotnet/samples/code-samples/UserManagement/Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/UserManagement/Program.cs)演示如何执行以下任务。

| 任务 | API 参考 |
| --- | --- |
| [创建用户](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L81) |[DocumentClient.CreateUserAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createuserasync) |
| [对集合或文档设置权限](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L85) |[DocumentClient.CreatePermissionAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.createpermissionasync) |
| [获取用户权限列表](https://github.com/Azure/azure-documentdb-net/blob/d17c0ca5be739a359d105cf4112443f65ca2cb72/samples/code-samples/UserManagement/Program.cs#L218) |[DocumentClient.ReadUserAsync](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.azure.documents.client.documentclient.readuserasync.aspx)<br>[DocumentClient.ReadPermissionFeedAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readpermissionfeedasync) |

<!-- Update_Description: update meta properties, update link -->