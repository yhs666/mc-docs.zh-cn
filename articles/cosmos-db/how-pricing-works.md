---
title: Azure Cosmos DB 的定价模型
description: 本文介绍 Azure Cosmos DB 的定价模型，以及该模型如何简化成本管理和成本计划。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 11/28/2018
ms.date: 12/31/2018
ms.author: v-yeche
ms.openlocfilehash: 8c8632fab0fe7ee86e00067fe13d8317ce29ac59
ms.sourcegitcommit: 54ddd3dc2452d7af3a6fa66dae908ad0c4ef99dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2018
ms.locfileid: "53814800"
---
# <a name="pricing-model-of-azure-cosmos-db"></a>Azure Cosmos DB 的定价模型 

Azure Cosmos DB 的定价模型可简化成本管理和计划。 使用 Azure Cosmos DB 时，需要为预配的吞吐量和消耗的存储支付费用。

* **预配的吞吐量**：预配的吞吐量（也称为预留吞吐量）可保证提供任何规模的高性能。 指定所需的吞吐量 (RU/s)，Azure Cosmos DB 提供所需资源来保证配置的吞吐量。 按指定时间内最大的预配吞吐量以小时来收费。

   > [!NOTE]
   > 由于预配的吞吐量模型会将资源提供给容器或数据库，因此即使不运行任何工作负载，也要为预配的吞吐量付费。

* **使用的存储**：按指定时间内用于数据和索引的总存储量 (GBs) 来收取统一费用。

预配的吞吐量按每秒的[请求单位数](request-units.md)（即 RU/s）指定，允许从容器中读取数据或者将数据写入容器或数据库。 可以[在数据库或容器上预配吞吐量](set-throughput.md)。 根据工作负荷需求，可以随时调整吞吐量的大小。 Azure Cosmos DB 的定价是弹性的，且与数据库或容器上配置的吞吐量成正比。 最小吞吐量和存储值以及规模增量为所有客户（从小型容器到大型容器）提供全面的价格和弹性范围。 对于以 100 RU/s 为单位预配的、最少为 400 RU/s 的吞吐量和以 GB 消耗的存储，每个数据库或容器按小时计费。 不同于预配的吞吐量，存储按消耗进行计费。 也就是说，无需提前预留任何存储。 仅为所使用的存储付费。

有关详细信息，请参阅 [Azure Cosmos DB 定价页](https://www.azure.cn/pricing/details/cosmos-db/)。

<!--Not Available on [Understanding your Azure Cosmos DB bill](understand-your-bill.md)-->

<!-- Not Available on Pricing model in Azure Cosmos DB-->
## <a name="try-azure-cosmos-db-for-free"></a>免费试用 Azure Cosmos DB 

Azure Cosmos DB 免费为开发人员提供多个选项。 这些选项包括：

* **Azure 试用帐户**：Azure 提供了一个[免费层](https://www.azure.cn/pricing/1rmb-trial/)，它在前 30 天为你提供了价值人民币 1500 元的 Azure 额度。
<!--Not Available on For more information [Azure trial account](../billing/billing-avoid-charges-free-account.md)-->
<!--Not Available on * **Try Azure Cosmos DB for free**-->

* **Azure Cosmos DB 模拟器**：为方便进行开发，Azure Cosmos DB 模拟器提供了一个模拟 Azure Cosmos DB 服务的本地环境。 模拟器免费提供，并且具有对云服务的高保真度。 使用 Azure Cosmos DB 模拟器可在本地开发和测试应用程序，无需创建 Azure 订阅且不会产生任何费用。 投入生产之前，可以在本地使用模拟器开发应用程序。 如果对模拟器的应用程序功能感到满意，可切换到云中的“使用 Azure Cosmos DB 帐户”，从而大幅节省成本。 有关模拟器的详细信息，请参阅[使用 Azure Cosmos DB 进行开发和测试](local-emulator.md)一文。

<!-- Not Available on ## Pricing with reserved capacity-->
## <a name="next-steps"></a>后续步骤

可在以下文章中了解更多关于优化 Azure Cosmos DB 资源成本的信息：

* 了解[针对开发和测试进行优化](optimize-dev-test.md)
<!--Not Available on * Learn more about [Understanding your Azure Cosmos DB bill](understand-your-bill.md)-->
* 详细了解如何[优化吞吐量成本](optimize-cost-throughput.md)
* 详细了解如何[优化存储成本](optimize-cost-storage.md)
* 详细了解如何[优化读取和写入成本](optimize-cost-reads-writes.md)
* 详细了解如何[优化查询成本](optimize-cost-queries.md)
* 详细了解如何[优化多区域 Cosmos 帐户的成本](optimize-cost-regions.md)
<!--Not Available on * Learn about [Azure Cosmos DB reserved capacity](cosmos-db-reserved-capacity.md)-->
* 了解 [Azure Cosmos DB 模拟器](local-emulator.md)