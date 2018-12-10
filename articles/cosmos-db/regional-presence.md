---
title: Azure Cosmos DB 的区域覆盖范围
description: 本文介绍 Azure Cosmos DB 的区域覆盖范围以及可用的不同云环境。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 11/15/2018
ms.date: 12/03/2018
ms.author: v-yeche
ms.openlocfilehash: ac1ce57aa2f6af4585af4b56d3b66e991f12ab1e
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676813"
---
# <a name="regional-presence-of-azure-cosmos-db"></a>Azure Cosmos DB 的区域覆盖范围

目前，Azure 在中国的 [4 个区域](https://www.azure.cn/zh-cn/home/features/products-by-region)提供。 Azure Cosmos DB 是 Azure 中的一项基础服务，在所有提供 Azure 的区域均可使用。

<!--Not Available on ![Azure Cosmos DB regional availability](./media/regional-presence/regional-presence.png)--> Cosmos DB 在提供给客户的 Azure 中国云环境中均可使用：

* Microsoft 与中国最大的 Internet 提供商世纪互联展开独特合作，推出了 Azure 中国。


## <a name="regional-presence-with-multiple-region-distribution"></a>采用多区域分布的区域覆盖

所有 Azure 区域提供 Azure Cosmos DB（包括 SQL 和 MongoDB）公开的 API。 例如，Azure 中国云中的 Azure Cosmos DB 可以公开 MongoDB 和 SQL API。


<!-- Not Available on Cassandra, Gremlin, and Azure Table storage-->
<!-- Not Available on Germany, Government, and Department of Defense (DoD) regions--> Azure Cosmos DB 是[多区域分布式](distribute-data-globally.md)数据库。 可将任意数量的 Azure 区域与 Azure Cosmos 帐户相关联，并且数据会自动且透明地得到复制。 可随时向 Azure Cosmos 帐户添加或从中删除区域。 借助统包多区域分布功能和多主控复制协议，Azure Cosmos DB 能够在第 99 百分位提供不到 10 毫秒的读写延迟、提供 99.999 的读写可用性并能够在与 Azure Cosmos 帐户相关的所有区域中灵活扩展预配的读写吞吐量。 Azure Cosmos DB 还提供五种定义完善的一致性模型，可以选择对数据应用特定的一致性模型。 最后，Azure Cosmos DB 是业内唯一一种提供综合服务级别协议 (SLA) 的数据库服务，包括预配的吞吐量、第 99 百分位的延迟、高可用性和一致性。

## <a name="next-steps"></a>后续步骤

现可通过以下文章了解 Azure Cosmos DB 的不同概念：

* [全局数据分布](distribute-data-globally.md)
* [如何管理 Azure Cosmos DB 帐户](manage-account.md)
* [为 Azure Cosmos 容器和数据库预配吞吐量](set-throughput.md)
* [Cosmos DB SLA](https://www.azure.cn/support/sla/cosmos-db/)

<!-- Update_Description: new articles on cosmos db regionl presence -->
<!--ms.date: 12/03/2018-->