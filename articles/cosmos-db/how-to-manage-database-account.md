---
title: 了解如何在 Azure Cosmos DB 中管理数据库帐户
description: 了解如何在 Azure Cosmos DB 中管理数据库帐户
services: cosmos-db
author: rockboyfor
ms.service: cosmos-db
ms.topic: sample
origin.date: 10/17/2018
ms.date: 12/03/2018
ms.author: v-yeche
ms.openlocfilehash: 927c76e09d72f628b98a1f3a7e87669bd5b1e5fa
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676565"
---
<!-- Verify Successfully-->
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中管理数据库帐户

本文介绍如何管理 Azure Cosmos DB 帐户，以便设置多宿主功能、添加/删除区域、配置多个写入区域，以及设置故障转移优先级。 

## <a name="create-a-database-account"></a>创建数据库帐户

<a name="create-database-account-via-portal"></a>
### <a name="azure-portal"></a>Azure 门户

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a name="create-database-account-via-cli"></a>
### <a name="azure-cli"></a>Azure CLI

```bash
# Create an account
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>配置多宿主客户端

<a name="configure-clients-multi-homing-dotnet"></a>
### <a name="net-sdk"></a>.NET SDK

```csharp
// Create a new Connection Policy
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to Preferred locations
// The name of the location will match what you see in the portal/etc.
policy.PreferredLocations.Add("China East");
policy.PreferredLocations.Add("China North");

// Pass the Connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

<a name="configure-clients-multi-homing-java-async"></a>
### <a name="java-async-sdk"></a>Java 异步 SDK

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setPreferredLocations(Collections.singleton("China North"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy).build();
```

<a name="configure-clients-multi-homing-java-sync"></a>
### <a name="java-sync-sdk"></a>Java 同步 SDK

```java
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
Collection<String> preferredLocations = new ArrayList<String>();
preferredLocations.add("China East");
connectionPolicy.setPreferredLocations(preferredLocations);
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy);
```

<a name="configure-clients-multi-homing-javascript"></a>
### <a name="nodejsjavascripttypescript-sdk"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Set up the connection policy with your preferred regions
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["China North", "China East"];

// Pass that connection policy to the client
const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy
});
```

<a name="configure-clients-multi-homing-python"></a>
### <a name="python-sdk"></a>Python SDK

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.PreferredLocations = ['China North', 'China East']
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy)

```

## <a name="addremove-regions-from-your-database-account"></a>在数据库帐户中添加/删除区域

<a name="add-remove-regions-via-portal"></a>
### <a name="azure-portal"></a>Azure 门户

1. 导航到 Azure Cosmos DB 帐户，打开“全局复制数据”菜单。

2. 若要添加区域，请单击与所需区域对应的包含“+”标签的空六边形，以便从地图中选择一个或多个区域。 也可先选择“+ 添加区域”选项，然后从下拉列表中选择一个区域，通过这种方式来添加区域。

3. 若要删除区域，请取消选中地图中的一个或多个区域，方法是：单击带复选标记的蓝色六边形，或者选择右侧区域旁边的“废纸篓”(🗑) 图标。

4. 单击“保存”，保存更改。

   ![“添加/删除区域”菜单](./media/how-to-manage-database-account/add-region.png)

在单区域写入模式下，不能删除写入区域。 在删除当前的写入区域之前，必须故障转移到另一区域。

在多区域写入模式下，可以添加/删除任意区域，前提是至少保留一个区域。

<a name="add-remove-regions-via-cli"></a>
### <a name="azure-cli"></a>Azure CLI

```bash
# Given an account created with 1 region like so
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'chinaeast=0'

# Add a new region by adding another region to the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'chinaeast=0 chinanorth=1'

# Remove a region by removing a region from the list
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'chinanorth=0'
```

## <a name="configure-multiple-write-regions"></a>配置多个写入区域

<a name="configure-multiple-write-regions-portal"></a>
### <a name="azure-portal"></a>Azure 门户

创建数据库帐户时，请确保启用“多区域写入”设置。

![Azure Cosmos 帐户创建屏幕截图](./media/how-to-manage-database-account/account-create.png)

<a name="configure-multiple-write-regions-cli"></a>
### <a name="azure-cli"></a>Azure CLI

```bash
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

<a name="configure-multiple-write-regions-arm"></a>
### <a name="resource-manager-template"></a>Resource Manager 模板

以下 JSON 代码是一个示例性的资源管理器模板。 可以使用它来部署 Azure Cosmos 帐户，并将一致性策略设置为“有限过期”，将最大过期时间间隔设置为 5 秒钟，将可以忍受的最大过期请求数设置为 100。 如需了解资源管理器模板的格式和语法，请参阅[资源管理器](../azure-resource-manager/resource-group-authoring-templates.md)文档。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "BoundedStaleness",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```

<a name="manual-failover"></a>
## <a name="enable-manual-failover-for-your-azure-cosmos-account"></a>为 Azure Cosmos 帐户启用手动故障转移

<a name="enable-manual-failover-via-portal"></a>
### <a name="azure-portal"></a>Azure 门户

1. 导航到 Azure Cosmos 帐户，打开“全局复制数据”菜单。

2. 在菜单顶部单击“手动故障转移”按钮。

   ![“复制多区域数据”菜单](./media/how-to-manage-database-account/replicate-data-globally.png)

3. 在“手动故障转移”菜单上，选择新的写入区域，然后选中标记你了解此选项会更改写入区域的框。

4. 单击“确定”，触发故障转移。

   ![手动故障转移门户菜单](./media/how-to-manage-database-account/manual-failover.png)

<a name="enable-manual-failover-via-cli"></a>
### <a name="azure-cli"></a>Azure CLI

```bash
# Given your account currently has regions with priority like so: 'chinaeast=0 chinanorth=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --locations 'chinaeast=1 chinanorth=0'
```

<a name="automatic-failover"></a>
## <a name="enable-automatic-failover-for-your-azure-cosmos-account"></a>为 Azure Cosmos 帐户启用自动故障转移

<a name="enable-automatic-failover-via-portal"></a>
### <a name="azure-portal"></a>Azure 门户

1. 在 Azure Cosmos 帐户中，打开“全局复制数据”窗格。 

2. 在窗格顶部单击“自动故障转移”按钮。

   ![“复制多区域数据”菜单](./media/how-to-manage-database-account/replicate-data-globally.png)

3. 在“自动故障转移”窗格中，确保将“启用自动故障转移”设置为“开”。 

4. 单击菜单底部的“保存”。

   ![自动故障转移门户菜单](./media/how-to-manage-database-account/automatic-failover.png)

也可在此菜单上设置自动故障转移优先级。

<a name="enable-automatic-failover-via-cli"></a>
### <a name="azure-cli"></a>Azure CLI

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Azure Cosmos account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>为 Azure Cosmos 帐户设置故障转移优先级

<a name="set-failover-priorities-via-portal"></a>
### <a name="azure-portal"></a>Azure 门户

1. 在 Azure Cosmos 帐户中，打开“全局复制数据”窗格。 

2. 在窗格顶部单击“自动故障转移”按钮。

   ![“复制多区域数据”菜单](./media/how-to-manage-database-account/replicate-data-globally.png)

3. 在“自动故障转移”窗格中，确保将“启用自动故障转移”设置为“开”。 

4. 可以修改故障转移优先级，方法是通过行左侧的三个点（当鼠标悬停在其上方时显示）单击并拖动读取区域。 

5. 单击菜单底部的“保存”。

   ![自动故障转移门户菜单](./media/how-to-manage-database-account/automatic-failover.png)

不能在此菜单上修改写入区域。 若要手动更改写入区域，必须执行手动故障转移。

<a name="set-failover-priorities-via-cli"></a>
### <a name="azure-cli"></a>Azure CLI

```bash
az cosmosdb failover-priority-change --name <Azure Cosmos account name> --resource-group <Resource Group name> --failover-policies 'chinaeast=0 chinanorth=2 chinaeast2=1'
```
<!--Notice:  chinaeast2=1-->
## <a name="next-steps"></a>后续步骤

可以通过以下文档了解如何在 Azure Cosmos DB 中管理一致性级别和数据冲突：

* [如何管理一致性](how-to-manage-consistency.md)
* [如何管理区域之间的冲突](how-to-manage-conflicts.md)

<!-- Update_Description: new articles on cosmos db how to manage database account-->
<!--ms.date: 12/03/2018-->