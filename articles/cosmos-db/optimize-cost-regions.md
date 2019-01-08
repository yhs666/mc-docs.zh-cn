---
title: 在 Azure Cosmos DB 中优化多区域部署的成本
description: 本文说明如何在 Azure Cosmos DB 中管理多区域部署的成本。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 12/07/2018
ms.date: 12/31/2018
ms.author: v-yeche
ms.openlocfilehash: b3e880b14f174264e5a6309ff711e8e44e66bd54
ms.sourcegitcommit: 54ddd3dc2452d7af3a6fa66dae908ad0c4ef99dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2018
ms.locfileid: "53814803"
---
# <a name="optimize-the-cost-for-multi-region-deployments-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中优化多区域部署的成本

可随时对 Azure Cosmos 帐户添加或删除区域。 在与帐户关联的每个区域中会保留为各种 Azure Cosmos 数据库和容器配置的吞吐量。 如果每小时预配的吞吐量（即，跨所有数据库和容器为 Azure Cosmos 帐户配置的 RU/秒总和）为 `T`，并且与数据库帐户关联的 Azure 区域数是 `N`，则对于给定小时，Cosmos 帐户的总预配吞吐量等于：

1. ` T x N RU/s`，如果使用单个写入区域配置 Azure Cosmos 帐户。 

1. `T x (N+1) RU/s`，如果使用能够处理写入的所有区域配置 Azure Cosmos 帐户。 

<!-- Not Available on Provisioned throughput with single write region costs $0.008/hour per 100 RU/s and provisioned throughput with multiple writable regions costs $0.016/per hour per 100 RU/s.-->

若要了解详细信息，请参阅 Azure Cosmos DB [定价页](https://www.azure.cn/pricing/details/cosmos-db/)。

## <a name="costs-for-multiple-write-regions"></a>多个写入区域的成本

在多主数据库系统中，可用于写入操作的净 RU 会增加 `N` 倍，其中 `N` 是写入区域数。 与单区域写入不同，每个区域都可写，应支持冲突解决。 编写器的工作负载量会增加。 从成本规划角度来看，若要在中国范围内执行 ` M` RU/秒的写入，需要在容器或数据库级别预配 M `RUs`。 随后可以添加任意多个区域并将它们用于写入以在中国范围内执行 `M` RU 写入。 

<!-- Not Available on ### Example-->

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>按每个区域提高吞吐量利用率

如果使用率效率低下（例如，一个或多个区域利用率不足或过度利用），则可以执行以下步骤以提高吞吐量利用率：  

1. 请确保首先优化写入区域中的预配吞吐量 (RU)，然后通过使用来自读取区域等的更改源充分使用读取区域中的 RU。 

2. 多写入区域读取和写入可以跨与 Azure Cosmos 帐户关联的所有区域进行缩放。 

3. 监视区域中的活动，你可以按需添加和删除区域以缩放读取和写入吞吐量。

## <a name="next-steps"></a>后续步骤

接下来，可通过以下文章详细了解 Azure Cosmos DB 中的成本优化：

* 详细了解[开发和测试优化](optimize-dev-test.md)
<!--Not Available on * Learn more about [Understanding your Azure Cosmos DB bill](understand-your-bill.md)-->
* 详细了解如何[优化吞吐量成本](optimize-cost-throughput.md)
* 详细了解如何[优化存储成本](optimize-cost-storage.md)
* 详细了解如何[优化读取和写入成本](optimize-cost-reads-writes.md)
* 详细了解如何[优化查询成本](optimize-cost-queries.md)