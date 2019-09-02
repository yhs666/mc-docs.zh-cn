---
title: Azure Cosmos DB 多主数据库优势
description: 了解 Azure Cosmos DB 中的多主数据库的优势。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 07/08/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: 82484f237aa0e6abecce3ef33fe783f4b2f6244f
ms.sourcegitcommit: b418463868dac6b3c82b292f70d4a17bc5e01e95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2019
ms.locfileid: "69578614"
---
# <a name="understand-multi-master-benefits-in-azure-cosmos-db"></a>了解 Azure Cosmos DB 中的多主数据库优势

Cosmos DB 帐户操作员应该选择适当的多区域分发配置，确保满足其应用程序的延迟、可用性和 RTO 要求。 配置了多个写入位置的 Azure Cosmos 帐户相对于只有单个写入位置的帐户有很大的优势，其中包括：99.999% 的写入可用性 SLA、99% 情况下 <10 毫秒的写入延迟 SLA，以及在发生区域灾难时 RTO = 0。

## <a name="comparison-of-features"></a>功能比较

|应用程序要求|多个写入位置|单个写入位置|注意|
|---|---|---|---|
|99% 情况下 <10 毫秒的写入延迟 SLA|**是**|否|使用单个写入位置的帐户在每次写入时会引发额外的跨区域网络延迟。|
|99% 情况下 <10 毫秒的读取延迟 SLA|**是**|是| |
|99.999% 的写入 SLA|**是**|否|使用单个写入位置的帐户可确保 99.99% 的 SLA|
|RTO = 0|**是**|否|在发生区域灾难时进行写入的停机时间为零。 使用单个写入位置的帐户的 RTO 为 15 分钟。|

## <a name="next-steps"></a>后续步骤

如果仍要在 Azure Cosmos 帐户中禁用 EnableMultipleWriteLocations，请[开具支持票证](https://support.azure.cn/support/support-azure/)。