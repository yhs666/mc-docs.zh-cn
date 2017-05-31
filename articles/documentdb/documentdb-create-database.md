---
redirect_url: https://azure.cn/documentation/services/documentdb/
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 4f83783c8c7c3bfcf8ae371825b7b07b07adc808
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017



---
# <a name="how-to-create-a-database-for-azure-cosmos-db-using-the-azure-portal"></a>如何使用 Azure 门户创建 Azure Cosmos DB 数据库
若要使用 Azure Cosmos DB，必须拥有 [Azure Cosmos DB 帐户](documentdb-create-account.md)、数据库、集合和文档。 现在，在 Azure 门户中创建集合的同时，会创建 Azure Cosmos DB 数据库。 

若要在门户中创建 Azure Cosmos DB 数据库和集合，请参阅[如何使用 Azure 门户创建 Azure Cosmos DB 集合](documentdb-create-collection.md)。

## <a name="other-ways-to-create-an-azure-cosmos-db-database"></a>创建 Azure Cosmos DB 数据库的其他方式
不一定要使用门户来创建数据库，也可以使用 [DocumentDB SDK](documentdb-sdk-dotnet.md) 或 [REST API](https://msdn.microsoft.com/library/mt489072.aspx) 来创建数据库。 有关使用 .NET SDK 处理数据库的信息，请参阅 [.NET 数据库示例](documentdb-dotnet-samples.md#database-examples)。 有关使用 Node.js SDK 处理数据库的信息，请参阅 [Node.js 数据库示例](documentdb-nodejs-samples.md#database-examples)。 

## <a name="next-steps"></a>后续步骤
创建数据库和集合后，可以使用门户中的文档资源管理器来[添加 JSON 文档](documentdb-view-json-document-explorer.md)，使用 DocumentDB 数据迁移工具向集合[导入文档](documentdb-import-data.md)，或使用某个 [DocumentDB SDK](documentdb-sdk-dotnet.md) 来执行 CRUD 操作。 DocumentDB 有 .NET、Java、Python、Node.js 和 JavaScript API SDK。 有关说明如何创建、移除、更新和删除文档的 .NET 代码示例，请参阅 [.NET 文档示例](documentdb-dotnet-samples.md#document-examples)。 有关使用 Node.js SDK 处理文档的信息，请参阅 [Node.js 文档示例](documentdb-nodejs-samples.md#document-examples)。 

当集合中有文档后，可以使用门户中的[查询浏览器](documentdb-query-collections-query-explorer.md)、[REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) 或某个 [SDK](documentdb-sdk-dotnet.md) 针对文档使用 [DocumentDB SQL](documentdb-sql-query.md) [执行查询](documentdb-sql-query.md#ExecutingSqlQueries)。 



