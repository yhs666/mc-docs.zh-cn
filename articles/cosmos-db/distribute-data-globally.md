---
title: 使用 Azure Cosmos DB 在多个区域分配数据
description: 了解如何通过多区域分配式多模型数据库服务 Azure Cosmos DB，使用多区域数据库进行多区域范围的异地复制、多主数据库故障转移和数据恢复。
services: cosmos-db
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 10/26/2018
ms.date: 01/07/2019
ms.openlocfilehash: f8e180e317f128cf1752365e5bcdef79b7a6cb82
ms.sourcegitcommit: ce4b37e31d0965e78b82335c9a0537f26e7d54cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2019
ms.locfileid: "54026800"
---
<!-- Notice in meta: 全球分布 to 多个区域分布 -->
<!-- Notice in meta: 全球范围 to 多个数据中心范围 -->
# <a name="multiple-region-data-distribution-with-azure-cosmos-db"></a>使用 Azure Cosmos DB 多区域分配数据

如今的应用程序需要具备高响应能力并始终联机。 若要实现低延迟和高可用性，需要在靠近用户的数据中心部署这些应用程序的实例。 这些应用程序通常部署在多个数据中心，称为多区域分布式应用程序。 多区域分布式应用程序需要在中国境内都可以以透明方式复制数据的多区域分布式数据库，以确保应用程序能在与用户关系密切的数据副本上执行操作。 

<!-- Notice: 全球 to 多个区域分布 --> Azure Cosmos DB 是一个多区域分布式数据库服务，旨在提供低延迟、吞吐量弹性缩放和妥善定义的语义，以实现数据一致性和高可用性。 简单而言，如果应用程序需要保证在中国任何位置都能提供快速响应、始终联机并需要无限且可弹性缩放的吞吐量和存储，请考虑使用 Azure Cosmos DB 来生成应用程序。

<!--Not Available on [Azure regions](https://www.azure.cn/global-infrastructure/regions/)--> 可将数据库配置为多区域分布，并使其可在任何 Azure 中国区域中使用。 为了降低延迟，请将数据放置在更靠近用户的位置。 选择所需的区域数目取决于应用程序的多区域覆盖范围以及用户所处的位置。 Azure Cosmos DB 以透明方式将帐户中的所有数据复制到与帐户关联的所有区域。 它提供多区域分布式 Azure Cosmos 数据库和容器的单个系统映像，使应用程序能够在本地读取和写入。 

使用 Azure Cosmos DB 可以随时添加或删除与帐户关联的区域。 无需暂停或重新部署应用程序即可添加或删除区域。 得益于该服务提供的多宿主功能，它始终都能保持高可用性。

<!-- Notice: Azure China Regions -->
## <a name="key-benefits-of-multiple-region-distribution"></a>多区域分布的主要优点

**构建多区域主动-主动应用。** 使用多主数据库功能，每个区域都是写入区域。 该区域也是可读的。 多主数据库功能还可以保证：

- 无限弹性写入可伸缩性。 
- 在全中国实现 99.999% 的读写可用性。
- 在 99% 的时间内，在 10 毫秒内为读写提供服务。

通过使用 Azure Cosmos DB 多宿主 API，应用程序可知晓最靠近的区域。 然后可以向该区域发送请求。 无需进行任何配置更改就能识别最靠近的区域。 在 Azure Cosmos DB 帐户中添加和删除区域时，无需重新部署应用程序。 该应用程序仍然高度可用。

**生成高响应能力的应用。** 可以轻松将应用程序设计为执行近乎实时的读写。 它可在为数据库选择的所有区域中实现个位数的毫秒级延迟。 Azure Cosmos DB 在内部处理区域之间的数据复制。 因此，可保证为 Azure Cosmos DB 帐户选择的一致性级别。

许多应用程序将受益于多区域（本地）写入功能所带来的性能增强。 某些需要非常一致性的应用程序偏向于将所有写入汇集到单个区域。 对于这些应用程序，Azure Cosmos DB 支持单区域和多区域配置。

**生成高度可用的应用。** 在多个区域中运行数据库可以提高数据库的可用性。 如果一个区域不可用，其他区域可自动处理应用程序请求。 Azure Cosmos DB 为多区域数据库提供 99.999% 的读取和写入可用性。

**在区域性中断期间保持业务连续性。** Azure Cosmos DB 支持在区域性中断期间进行[自动故障转移](how-to-manage-database-account.md#automatic-failover)。 在区域性中断期间，Azure Cosmos DB 会继续维持其延迟、可用性、一致性和吞吐量方面的 SLA。 为帮助确保整个应用程序高度可用，Azure Cosmos DB 提供手动故障转移 API 来模拟区域性中断。 使用此 API 可以执行常规业务连续性演练。

**在中国缩放读写吞吐量。** 使用多主数据库功能，可以在中国各地弹性缩放读写吞吐量。 多主数据库功能保证应用程序针对 Azure Cosmos DB 数据库或容器配置的吞吐量可在所有区域中实现。 吞吐量还受[有资金支持的 SLA](https://www.azure.cn/support/sla/documentdb/) 的保护。

**从多个明确定义的一致性模型中进行选择。** Azure Cosmos 数据库复制协议提供了五种明确定义、实用且直观的一致性模型。 每个模型在一致性与性能之间进行了权衡。 使用这些一致性模型可以轻松生成多区域分布式应用程序。

## <a name="Next Steps"></a>后续步骤

阅读以下文章详细了解多区域分布：

* [多区域分布 - 揭秘](global-dist-under-the-hood.md)
* [配置多宿主客户端](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [在 Azure Cosmos DB 帐户中添加或删除区域](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [为 SQL API 帐户创建自定义冲突解决策略](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)

<!--Update_Description: update meta properties, wording update -->