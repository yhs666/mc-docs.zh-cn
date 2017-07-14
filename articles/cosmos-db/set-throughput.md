---
title: "预配 Azure Cosmos DB 的吞吐量 | Azure"
description: "了解如何为 Azure Cosmos DB 容器、集合、图形和表设置预配吞吐量。"
services: cosmos-db
author: rockboyfor
manager: digimobile
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/12/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 92ae3f7cde8de6c9960bf326447957c51c6f4af1
ms.sourcegitcommit: b15d77b0f003bef2dfb9206da97d2fe0af60365a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# 设置 Azure Cosmos DB 容器的吞吐量
<a id="set-throughput-for-azure-cosmos-db-containers" class="xliff"></a>

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
            <td valign="top"><p>2,500 个请求单位/秒</p></td>
        </tr>
        <tr>
            <td valign="top"><p>最大吞吐量</p></td>
            <td valign="top"><p>10,000 个请求单位/秒</p></td>
            <td valign="top"><p>不受限制</p></td>
        </tr>
    </tbody>
</table>

## 使用 Azure 门户设置吞吐量
<a id="to-set-the-throughput-by-using-the-azure-portal" class="xliff"></a>

1. 在新窗口中，打开 [Azure 门户](https://portal.azure.cn)。
2. 在左侧栏中单击“Azure Cosmos DB”，或者单击底部的“更多服务”，滚动到“数据库”，然后单击“Azure Cosmos DB”。
3. 选择 Cosmos DB 帐户。
4. 在新窗口中的导航菜单内单击“数据资源管理器(预览)”。
5. 在新窗口中展开数据库和容器，然后单击“设置和缩放”。
6. 在新窗口中的“吞吐量”框内键入新吞吐量值，然后单击“保存”。

<a id="set-throughput-sdk"></a>

## 使用用于 .NET 的 DocumentDB API 设置吞吐量
<a id="to-set-the-throughput-by-using-the-documentdb-api-for-net" class="xliff"></a>

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## 吞吐量常见问题
<a id="throughput-faq" class="xliff"></a>

**可否将吞吐量设置为 400 RU/s 以下？**

400 RU/秒是 Cosmos DB 单区集合提供的最小吞吐量（分区集合的最小值为 2500 RU/秒）。 请求单位按 100 RU/秒间隔进行设置，但吞吐量不能设置为 100 RU/秒或小于 400 RU/秒的任何值。 如果正在寻找一种经济高效的方法来开发和测试 Cosmos DB，则可以使用免费的 [Azure Cosmos DB 模拟器](local-emulator.md)来免费进行本地部署。 

**如何使用 MongoDB API 设置吞吐量？**

没有任何 MongoDB API 扩展可设置吞吐量。 建议根据[使用用于 .NET 的 DocumentDB API 设置吞吐量](#set-throughput-sdk)中所述使用 DocumentDB API。

## 后续步骤
<a id="next-steps" class="xliff"></a>

若要了解有关使用 Cosmos DB 进行预配和全球扩展的详细信息，请参阅[使用 Cosmos DB 进行分区和缩放](partition-data.md)。