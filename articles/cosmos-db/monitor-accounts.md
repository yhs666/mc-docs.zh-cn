---
title: 监视 Azure Cosmos DB 请求和存储 | Azure
description: 了解如何监视 Azure Cosmos DB 帐户的性能指标（如请求和服务器错误）以及使用情况指标（如存储消耗）。
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
origin.date: 09/19/2017
ms.date: 12/03/2018
ms.author: v-yeche
ms.openlocfilehash: 8708b917875a453a2950e8905c34270674ea4c6a
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675046"
---
# <a name="monitor-azure-cosmos-db"></a>监视 Azure Cosmos DB
可以在 [Azure 门户](https://portal.azure.cn/)中监视 Azure Cosmos DB 帐户。 对于每个 Azure Cosmos DB 帐户，一整套指标可用于监视吞吐量、存储、可用性、延迟和一致性。

可在“帐户”页、新的“指标”页或 Azure Monitor.中查看指标。

## <a name="view-performance-metrics-on-the-metrics-page"></a>在“指标”页上查看性能指标
1. 在 [Azure 门户](https://portal.azure.cn/)中，单击“所有服务”，滚动到“数据库”，单击“Azure Cosmos DB”，然后单击要查看其性能指标的 Azure Cosmos DB 帐户的名称。
2. 新页加载时，在资源菜单的“监视”下，单击“指标”。
3. “指标”页打开时，从“集合”下拉列表中选择要查看的集合。

   Azure 门户显示了一套可用的集合指标。 请注意，吞吐量、存储、可用性、延迟和一致性指标在单独的选项卡上提供。 若要获取有关所提供指标的更多详细信息，请单击每个指标窗格右上方的双箭头。

   ![显示指标套件的“监视”可重用功能区的屏幕截图](./media/monitor-accounts/metrics-suite.png)

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>使用 Azure Monitor 查看性能指标
1. 在 [Azure 门户](https://portal.azure.cn/)中，单击左栏中的“监视”。
2. 在资源菜单中，单击“指标”。
3. 在“监视 - 指标”窗口的“资源组”下拉菜单中，选择与想要监视的 Azure Cosmos DB 帐户关联的资源组。 
4. 在“资源”  下拉菜单中，选择要监视的数据库帐户。
5. 在“可用指标” 列表中，选择要显示的指标。 使用 Ctrl 按钮进行多选。 

## <a name="view-performance-metrics-on-the-account-page"></a>在“帐户”页上查看性能指标
1. 在 [Azure 门户](https://portal.azure.cn/)中，单击“所有服务”，滚动到“数据库”，单击“Azure Cosmos DB”，然后单击要查看其性能指标的 Azure Cosmos DB 帐户的名称。
2. 默认情况下，“监视”可重用功能区  显示以下磁贴：

   * 当天的请求总数。
   * 使用的存储量。

   ![“监视”可重用功能区的屏幕截图，其中显示请求数和存储使用情况](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. 单击“请求”磁贴右上角的双箭头将打开详细的“指标”页。
4. “指标”页显示有关请求总数的详细信息。 

<!-- Not Available on ## Set up alerts in the portal-->

## <a name="monitor-azure-cosmos-db-programmatically"></a>以编程方式监视 Azure Cosmos DB
门户中可用的帐户级别指标（如帐户存储使用情况和请求总数）不可通过 SQL API 使用。 但是，可以使用 SQL API 在集合级别检索使用情况数据。 若要检索集合级别的数据，请执行以下操作：

* 若要使用 REST API，请[对集合执行 GET](https://msdn.microsoft.com/library/mt489073.aspx)。 集合的配额和使用情况信息返回到响应中的 x-ms-resource-quota 和 x-ms-resource-usage 标头中。
* 要使用 .NET SDK，请使用 [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) 方法，它返回 [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx)，其中包含大量使用情况属性，例如 **CollectionSizeUsage**、**DatabaseUsage**、**DocumentUsage** 等。

若要访问其他指标，请使用 [Azure Monitor SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights)。 可以通过调用以下命令检索可用的指标定义：

    https://management.chinacloudapi.cn/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

用于检索各个指标的查询使用以下格式：

    https://management.chinacloudapi.cn/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

有关详细信息，请参阅 [通过 Azure Monitor REST API 检索资源指标](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/)。 请注意，已将“Azure Insights”重命名为“Azure Monitor”。  本博客条目引用旧名称。

## <a name="next-steps"></a>后续步骤
若要深入了解 Azure Cosmos DB 容量规划，请参阅 [Azure Cosmos DB Capacity Planner 计算器](https://www.documentdb.com/capacityplanner)。

<!-- Update_Description: update meta properties  -->