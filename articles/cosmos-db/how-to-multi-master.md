---
title: 如何在 Azure Cosmos DB 中配置多主数据库
description: 了解如何在 Azure Cosmos DB 中配置应用程序中的多主数据库
author: rockboyfor
ms.service: cosmos-db
ms.topic: sample
origin.date: 04/15/2019
ms.date: 05/13/2019
ms.author: v-yeche
ms.openlocfilehash: e0791263bc5afbade55c79cab451c6e8fbc74239
ms.sourcegitcommit: 71172ca8af82d93d3da548222fbc82ed596d6256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65668944"
---
# <a name="how-to-configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>如何在使用 Azure Cosmos DB 的应用程序配置多主数据库

要在应用程序中使用多主数据库功能，需要启用多区域写入并在 Azure Cosmos DB 中配置多宿主功能。 可通过设置部署应用程序的区域来配置多宿主功能。

<a name="netv2"></a>
## <a name="net-sdk-v2"></a>.NET SDK v2

若要在应用程序中启用多主数据库，请将 `UseMultipleWriteLocations` 设置为 true，并将 `SetCurrentLocation` 配置为在其中部署应用程序并复制 Azure Cosmos DB 的区域。

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("China North 2");
```

<a name="netv3"></a>
## <a name="net-sdk-v3-preview"></a>.NET SDK v3（预览版）

若要在应用程序中启用多主数据库，请将 `UseCurrentRegion` 配置为在其中部署应用程序并复制 Cosmos DB 的区域。

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("China North");
CosmosClient client = new CosmosClient(config);
```

<a name="java"></a>
## <a name="java-async-sdk"></a>Java 异步 SDK

若要在应用程序中启用多主数据库，请设置 `policy.setUsingMultipleWriteLocations(true)`，并将 `policy.setPreferredLocations` 配置为在其中部署应用程序并复制 Cosmos DB 的区域。

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

<a name="javascript"></a>
## <a name="nodejs-javascript-typescript-sdk"></a>Node.js、JavaScript、TypeScript SDK

若要在应用程序中启用多主数据库，请将 `connectionPolicy.UseMultipleWriteLocations` 设置为 true，并将 `connectionPolicy.PreferredLocations` 配置为在其中部署应用程序并复制 Cosmos DB 的区域。

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

<a name="python"></a>
## <a name="python-sdk"></a>Python SDK

若要在应用程序中启用多主数据库，请将 `connection_policy.UseMultipleWriteLocations` 设置为 true，并将 `connection_policy.PreferredLocations` 配置为在其中部署应用程序并复制 Cosmos DB 的区域。

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>后续步骤

接下来可以阅读以下文章：

* [利用可以在 Azure Cosmos DB 中管理一致性的会话令牌](how-to-manage-consistency.md#utilize-session-tokens)
* [Azure Cosmos DB 中的冲突类型和解决策略](conflict-resolution-policies.md)
* [Azure Cosmos DB 中的高可用性](high-availability.md)
* [Azure Cosmos DB 中的一致性级别](consistency-levels.md)
* [在 Azure Cosmos DB 中选择适当的一致性级别](consistency-levels-choosing.md)
* [Azure Cosmos DB 中的一致性、可用性和性能权衡](consistency-levels-tradeoffs.md)
* [各种一致性级别的可用性和性能权衡](consistency-levels-tradeoffs.md)
* [全局缩放预配的吞吐量](scaling-throughput.md)
* [多区域分布 - 揭秘](global-dist-under-the-hood.md)

<!--Update_Description: wording update, update link-->