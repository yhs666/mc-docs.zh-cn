---
title: 通过选择正确的分区键来执行多区域写入和读取操作 | Azure
description: 了解如何通过选择分区键使用 Azure Cosmos DB 设计应用程序体系结构，实现跨多个地理区域的本地读取和写入。
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: conceptual
origin.date: 06/06/2018
ms.date: 07/02/2018
ms.author: v-yeche
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e4c82f12390f17394987de624ebf9d04dd9986ed
ms.sourcegitcommit: 00c8a6a07e6b98a2b6f2f0e8ca4090853bb34b14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38939572"
---
# <a name="perform-multi-region-writes-and-reads-by-choosing-the-right-partitioning-key"></a>通过选择正确的分区键来执行多区域写入和读取操作

Azure Cosmos DB 支持统包的[多区域复制](distribute-data-globally.md)，允许在工作负荷中的任意位置通过低延迟的访问将数据分布到多个区域。 此模型常用于发布者/使用者工作负荷。在这些工作负荷中，单个地理区域包含一个作者，其他（读取）区域包含分布于多个区域的读者。 

还可以使用 Azure Cosmos DB 的多区域复制支持来构建作者和读者分布于多个区域的应用程序。 本文档概述一种使用 Azure Cosmos DB 为全球分布的作者实现本地写入和本地读取访问的模式。

<a name="ExampleScenario"></a>
## <a name="content-publishing---an-example-scenario"></a>内容发布 - 示例方案
让我们借助一个真实的方案，介绍如何在 Azure Cosmos DB 中使用多区域分布式多区域/多主读写模式。 假设已在 Azure Cosmos DB 上构建一个内容发布平台。 为了向发布者和使用者提供良好的用户体验，此平台必须满足一些要求。

* 作者和订户遍布中国 
* 作者必须将文章发布（写入）到本地（最近的）区域
* 作者的文章拥有遍布中国的读者/订户。 
* 新文章发布时，订户应会收到通知。
* 订户必须能从本地区域阅读文章。 订户还应能对这些文章添加评论。 
* 包括文章作者在内的任何人都应能在本地区域查看文章所附的所有评论。 

假设存在数百万的使用者和发布者以及数十亿篇文章，我们很快就必须面对扩展以及保证访问位置的问题。 与大多数可伸缩性问题一样，解决方案在于良好的分区策略。 接下来，让我们看看如何将文章、评论和通知作为文档建模、配置 Azure Cosmos DB 帐户以及实现数据访问层。 

若要了解有关分区和分区键的详细信息，请参阅 [Azure Cosmos DB 中的分区和缩放](partition-data.md)。

<a name="ModelingNotifications"></a>
## <a name="modeling-notifications"></a>为通知建模
通知是特定于用户的数据馈送。 因此，通知文档的访问模式始终发生在单个用户的上下文中。 例如，可以“向某个用户发布通知”或“为某个给定用户获取所有通知”。 因此，对于此类型，分区键的最佳选择是 `UserId`。

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // The user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // The partition Key for the resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of the notification. 
        public string ArticleId { get; set; } 
    }

<a name="ModelingSubscriptions"></a>
## <a name="modeling-subscriptions"></a>为订阅建模
订阅可以根据各种标准创建，如感兴趣的特定类别的文章或特定的发布者。 因此， `SubscriptionFilter` 是很好的分区键选择。

    class Subscriptions 
    { 
        // Unique ID for Subscription 
        public string Id { get; set; }

        // Subscription source. Could be Author | Category etc. 
        public string SubscriptionFilter { get; set; }

        // subscribing User. 
        public string UserId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.SubscriptionFilter; 
            } 
        } 
    }

<a name="ModelingArticles"></a>
## <a name="modeling-articles"></a>为文章建模
通过通知标识一篇文章后，后续查询通常基于 `Article.Id`。 选择 `Article.Id` 作为分区键可为在 Azure Cosmos DB 集合内存储文章提供最佳分布。 

    class Article 
    { 
        // Unique ID for Article 
        public string Id { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.Id; 
            } 
        }

        // Author of the article
        public string Author { get; set; }

        // Category/genre of the article
        public string Category { get; set; }

        // Tags associated with the article
        public string[] Tags { get; set; }

        // Title of the article
        public string Title { get; set; }

        //... 
    }

<a name="ModelingReviews"></a>
## <a name="modeling-reviews"></a>为评论建模
和文章一样，评论通常在文章上下文中写入和读取。 选择 `ArticleId` 作为分区键可为与文章相关的评论提供最佳分布和高效访问。 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of the review 
        public string ArticleId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.ArticleId; 
            } 
        }

        //Reviewer Id 
        public string UserId { get; set; }
        public string ReviewText { get; set; }

        public int Rating { get; set; } }
    }

<a name="DataAccessMethods"></a>
## <a name="data-access-layer-methods"></a>数据访问层方法
现在让我们看看需要实现的主要数据访问方法。 以下是 `ContentPublishDatabase` 需要的方法列表：

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);

        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);

        public async Task<Article> ReadArticleAsync(string articleId);

        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);

        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

<a name="Architecture"></a>
## <a name="azure-cosmos-db-account-configuration"></a>Azure Cosmos DB 帐户配置
若要保证本地读取和写入，数据分区不仅要基于分区键，还要基于不同区域的地理访问模式。 该模型依赖于每个区域具有异地复制的 Azure Cosmos DB 数据库帐户。 例如，对于两个区域，具有针对多区域写入的设置：

| 帐户名 | 写入区域 | 读取区域 |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.cn` | `China North` |`China North` |
| `contentpubdatabase-europe.documents.azure.cn` | `China North` |`China North` |

下图显示了如何在使用此设置的典型应用程序中执行读取和写入：

![Azure Cosmos DB 多主体系结构](./media/multi-master-workaround/multi-master.png)

以下代码片段演示如何在 `China North` 区域中运行的 DAL 中初始化客户端。

    ConnectionPolicy writeClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    writeClientPolicy.PreferredLocations.Add(LocationNames.ChinaNorth);
    writeClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

    DocumentClient writeClient = new DocumentClient(
        new Uri("https://contentpubdatabase-usa.documents.azure.cn"), 
        writeRegionAuthKey,
        writeClientPolicy);

    ConnectionPolicy readClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    readClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
    readClientPolicy.PreferredLocations.Add(LocationNames.ChinaNorth);

    DocumentClient readClient = new DocumentClient(
        new Uri("https://contentpubdatabase-europe.documents.azure.cn"),
        readRegionAuthKey,
        readClientPolicy);

通过上述设置，数据访问层可以根据其部署位置将所有写入转发到本地帐户。 通过从两个帐户读取来执行读取以获得数据的全局视图。 这种方法可以扩展到所需的任意多个区域。 例如，以下是三个地理区域的设置：

| 帐户名 | 写入区域 | 读取区域 1 | 读取区域 2 |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.cn` | `China North` |`China North` |`China East` |
| `contentpubdatabase-europe.documents.azure.cn` | `China North` |`China North` |`China East` |
| `contentpubdatabase-asia.documents.azure.cn` | `China East` |`China North` |`China North` |

<a name="DataAccessImplementation"></a>
## <a name="data-access-layer-implementation"></a>数据访问层实现
现在让我们看一下如何实现具有两个可写区域的应用程序的数据访问层 (DAL)。 DAL 必须执行以下步骤：

* 为每个帐户创建多个 `DocumentClient` 实例。 在两个区域的情况下，每个 DAL 实例具有 1 个 `writeClient` 和 1 个 `readClient`。 
* 根据应用程序的部署区域来配置 `writeclient` 和 `readClient` 的终结点。 例如，部署在 `China North` 中的 DAL 使用 `contentpubdatabase-usa.documents.azure.cn` 进行写入。 部署在 `NorthEurope` 中的 DAL 使用 `contentpubdatabase-europ.documents.azure.cn` 进行写入。

通过上述设置，可以实现数据访问方法。 写入操作将写入转发到相应的 `writeClient`。

    public async Task CreateSubscriptionAsync(string userId, string category)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Subscriptions
        {
            UserId = userId,
            SubscriptionFilter = category
        });
    }

    public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Review
        {
            UserId = userId,
            ArticleId = articleId,
            ReviewText = reviewText,
            Rating = rating
        });
    }

若要读取通知和评论，必须从两个区域读取并合并结果，如以下代码片段所示：

    public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId)
    {
        IDocumentQuery<Notification> writeAccountNotification = (
            from notification in this.writeClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();

        IDocumentQuery<Notification> readAccountNotification = (
            from notification in this.readClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();

        List<Notification> notifications = new List<Notification>();

        while (writeAccountNotification.HasMoreResults || readAccountNotification.HasMoreResults)
        {
            IList<Task<FeedResponse<Notification>>> results = new List<Task<FeedResponse<Notification>>>();

            if (writeAccountNotification.HasMoreResults)
            {
                results.Add(writeAccountNotification.ExecuteNextAsync<Notification>());
            }

            if (readAccountNotification.HasMoreResults)
            {
                results.Add(readAccountNotification.ExecuteNextAsync<Notification>());
            }

            IList<FeedResponse<Notification>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Notification> feed in notificationFeedResult)
            {
                notifications.AddRange(feed);
            }
        }
        return notifications;
    }

    public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId)
    {
        IDocumentQuery<Review> writeAccountReviews = (
            from review in this.writeClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();

        IDocumentQuery<Review> readAccountReviews = (
            from review in this.readClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();

        List<Review> reviews = new List<Review>();

        while (writeAccountReviews.HasMoreResults || readAccountReviews.HasMoreResults)
        {
            IList<Task<FeedResponse<Review>>> results = new List<Task<FeedResponse<Review>>>();

            if (writeAccountReviews.HasMoreResults)
            {
                results.Add(writeAccountReviews.ExecuteNextAsync<Review>());
            }

            if (readAccountReviews.HasMoreResults)
            {
                results.Add(readAccountReviews.ExecuteNextAsync<Review>());
            }

            IList<FeedResponse<Review>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Review> feed in notificationFeedResult)
            {
                reviews.AddRange(feed);
            }
        }

        return reviews;
    }

因此，通过选择合适的分区键和静态的基于帐户的分区，可以使用 Azure Cosmos DB 实现多区域本地写入和读取。

<a name="NextSteps"></a>
## <a name="next-steps"></a>后续步骤
本文以内容发布作为示例方案，介绍如何通过 Azure Cosmos DB 使用多区域分布式多区域读写模式。

* 了解 Azure Cosmos DB 如何支持[多区域分发](distribute-data-globally.md)
* 了解 [Azure Cosmos DB 中的自动和手动故障转移](regional-failover.md)
* 了解 [Azure Cosmos DB 的全局一致性](consistency-levels.md)
* 使用 [Azure Cosmos DB - SQL API](tutorial-global-distribution-sql-api.md) 在使用多个区域的情况下进行开发
* 使用 [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md) 通过多个区域进行开发
* 使用 [Azure Cosmos DB - 表 API](tutorial-global-distribution-table.md)
<!-- Update_Description: new article on cosmos db multi master workaround -->
<!--ms.date: 07/02/2018--> 通过多个区域进行开发