---
title: 预配 Azure Cosmos DB 的吞吐量 | Azure
description: 了解如何为 Azure Cosmos DB 容器和集合设置预配吞吐量。
services: cosmos-db
author: rockboyfor
manager: digimobile
documentationcenter: ''
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/23/2018
ms.date: 04/23/2018
ms.author: v-yeche
ms.openlocfilehash: 6535d8af9d12d88c79889fab29ed3bc3241afe3a
ms.sourcegitcommit: c4437642dcdb90abe79a86ead4ce2010dc7a35b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2018
---
<!-- Notice: Meta Not Available on graphs, and tables -->

# <a name="set-throughput-for-azure-cosmos-db-containers"></a>设置 Azure Cosmos DB 容器的吞吐量

可在 Azure 门户中或通过使用客户端 SDK 设置 Azure Cosmos DB 容器的吞吐量。 

下表列出了适用于容器的吞吐量：

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>单分区容器</strong></p></td>
            <td valign="top"><p><strong>分区的容器</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>最小吞吐量</p></td>
            <td valign="top"><p>400 个请求单位/秒</p></td>
            <td valign="top"><p>1000 个请求单位/秒</p></td>
        </tr>
        <tr>
            <td valign="top"><p>最大吞吐量</p></td>
            <td valign="top"><p>10,000 个请求单位/秒</p></td>
            <td valign="top"><p>无限制</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>使用 Azure 门户设置吞吐量

1. 在新窗口中，打开 [Azure 门户](https://portal.azure.cn)。
2. 在左侧栏中单击“Azure Cosmos DB”，或者单击底部的“所有服务”，滚动到“数据库”，并单击“Azure Cosmos DB”。
3. 选择 Cosmos DB 帐户。
4. 在新窗口中，单击导航菜单中的“数据资源管理器”。
5. 在新窗口中展开数据库和容器，然后单击“设置和缩放”。
6. 在新窗口中的“吞吐量”框内键入新吞吐量值，然后单击“保存”。

<a name="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>使用用于 .NET 的 SQL API 设置吞吐量

下面的代码片段检索当前吞吐量并将其更改为 500 RU/s。 若要查看完整的代码示例，请参阅 GitHub 上的 [CollectionManagement](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L188-L216) 项目。

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 500 request units per second
offer = new OfferV2(offer, 500);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a name="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>使用 SQL API for Java 设置吞吐量

下面的代码片段检索当前吞吐量并将其更改为 500 RU/s。 若要查看完整的代码示例，请参阅 GitHub 上的 [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) 文件。 

```Java
// find offer associated with this collection
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 500;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="throughput-faq"></a>吞吐量常见问题

**是否可以将吞吐量设置为小于 400 RU/s？**

400 RU/秒是 Cosmos DB 单区容器提供的最小吞吐量（分区容器的最小值为 1000 RU/秒）。 请求单位按 100 RU/秒间隔进行设置，但吞吐量不能设置为 100 RU/秒或小于 400 RU/秒的任何值。 如果在寻找一种经济高效的方法来开发和测试 Cosmos DB，则可以使用免费的 [ 模拟器](local-emulator.md)（可以在本地免费部署）。 

**如何使用 MongoDB API 设置吞吐量？**

没有任何 MongoDB API 扩展可设置吞吐量。 建议使用 SQL API，如[使用用于 .NET 的 SQL API 设置吞吐量](#set-throughput-sdk)中所述。

## <a name="next-steps"></a>后续步骤

若要了解有关使用 Cosmos DB 进行预配和多区域扩展的详细信息，请参阅[使用 Cosmos DB 进行分区和缩放](partition-data.md)。
<!-- Notice: 全球 to 多个区域 -->
<!-- Update_Description: udpate meta properties, wording update -->