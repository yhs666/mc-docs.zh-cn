---
title: 使用 Azure Cosmos DB 在多个区域分配数据 | Azure
description: 了解如何通过多区域分配式多模型数据库服务 Azure Cosmos DB，使用多区域数据库进行多区域范围的异地复制、多主数据库故障转移和数据恢复。
services: cosmos-db
author: rockboyfor
manager: digimobile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
origin.date: 10/26/2018
ms.date: 12/03/2018
ms.author: v-yeche
ms.openlocfilehash: 185568014670870630bc868b127fcb9d2863254f
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52675347"
---
<!-- Notice in meta: 全球分布 to 多个区域分布 -->
<!-- Notice in meta: 全球范围 to 多个数据中心范围 -->
# <a name="multiple-region-data-distribution-with-azure-cosmos-db"></a>使用 Azure Cosmos DB 多区域分配数据

如今的应用程序需要具备高响应能力并始终联机。 若要实现低延迟和高可用性，需要在与用户关系密切的数据中心内部署这些应用程序的实例。 这些应用程序通常部署在多个数据中心，称为多区域分布式应用程序。 多区域分布式应用程序需要在中国境内都可以以透明方式复制数据的多区域分布式数据库，以确保应用程序能在与用户关系密切的数据副本上执行操作。 Azure Cosmos DB 是一个多区域分布式数据库服务，旨在提供低延迟、吞吐量弹性缩放和妥善定义的语义，以实现数据一致性和高可用性。 简单而言，如果应用程序需要保证在中国任何位置都能提供快速响应、始终联机并需要无限且可弹性缩放的吞吐量和存储，则你应该考虑使用 Azure Cosmos DB 生成应用程序。

<!-- Notice: 全球 to 多个区域分布 --> Azure Cosmos DB 是一个基础 Azure 服务，默认可在所有 Azure 区域中使用。 Azure 在中国的 4 个区域运营 Azure 数据中心，并且区域数目在持续增加，以满足不断增长的客户需求。 创建 Azure Cosmos 帐户时，需确定要将其部署在哪些区域。 我们全天候运转 Azure Cosmos DB 服务，让你将工作重心放在自己的应用程序上。

<!--Not Available on [Azure regions](https://www.azure.cn/global-infrastructure/regions/)--> 可将数据库配置为多区域分布，并使其可在任何 Azure 中国区域中使用。 为了降低延迟，应将数据定位在靠近用户的位置。 选择所需的区域数目取决于应用程序的多区域覆盖范围以及用户所处的位置。 Azure Cosmos DB 以透明方式将帐户中的所有数据复制到与帐户关联的所有区域。 它提供多区域分布式 Azure Cosmos 数据库和容器的单个系统映像，使应用程序能够在本地读取和写入。 使用 Azure Cosmos DB 可以随时添加或删除与帐户关联的区域。 无需暂停或重新部署应用程序即可添加或删除区域。 得益于该服务提供的多宿主功能，它始终都能保持高可用性。

<!-- Notice: Azure China Regions -->
## <a name="key-benefits-of-multiple-region-distribution"></a>多区域分布的主要优点

**生成多区域主动-主动应用**：借助多主数据库功能，每个区域都是写入区域（此外还可读）。 多主数据库功能在整个中国保证无限弹性写入可伸缩性、99.999% 的读写可用性，并保证在 99% 的时间内，以小于 10 毫秒的延迟为快速读写提供服务  

使用 Azure Cosmos DB 的多宿主 API，应用程序始终知道最靠近的区域并将请求发送到该区域。 无需进行任何配置更改就能识别最靠近的区域。 在 Azure Cosmos DB 帐户中添加和删除区域时，无需重新部署应用程序，它会持续保持高可用性。

**生成高响应能力的应用**：可以轻松将应用程序设计为在针对数据库选择的所有区域中，以小于 10 毫秒的延迟执行近实时读取和写入。  Azure Cosmos DB 在内部处理区域之间的数据复制，并保证针对 Azure Cosmos 帐户选择的一致性级别。

许多应用程序将受益于多区域（本地）写入功能所带来的性能增强。 某些需要非常一致性的应用程序偏向于将所有写入汇集到单个区域。 为了支持这些应用程序，Azure Cosmos DB 支持单区域和多区域配置。

**生成高度可用的应用**：在多个区域中运行数据库可以提高数据库的可用性。 如果一个区域不可用，其他区域会自动处理应用程序请求。 Azure Cosmos DB 为多区域数据库提供 99.999% 的读取和写入可用性。

**发生区域性服务中断时保持业务连续性**：Azure Cosmos DB 支持区域性服务中断期间的[自动故障转移](how-to-manage-database-account.md#automatic-failover)。 此外，在发生区域性服务中断期间，Azure Cosmos DB 会继续维持其延迟、可用性、一致性和吞吐量方面的 SLA。 为帮助确保整个应用程序高度可用，Azure Cosmos DB 提供手动故障转移 API 来模拟区域性服务中断。 使用此 API 可以执行常规的业务连续性演练。

**多重读取和写入可伸缩性**：使用多主数据库功能，可以在整个中国弹性缩放读取和写入吞吐量。 多主数据库功能保证应用程序针对 Azure Cosmos DB 数据库或容器配置的吞吐量可在所有区域中实现，并受到[有经济保障的 SLA](https://www.azure.cn/support/sla/documentdb/) 的保护。

**多个妥善定义的一致性模型**：Azure Cosmos DB 的复制协议可提供五个妥善定义的、实用且直观的一致性模型。 每个模型在一致性与性能之间提供折衷方案。 使用这些一致性模型可以轻松生成多区域分布式应用程序。

## <a name="Next Steps"></a>后续步骤

阅读以下文章详细了解多区域分布：

* [多区域分布 - 揭秘](global-dist-under-the-hood.md)
* [如何配置多宿主客户端](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [如何在 Azure Cosmos 帐户中添加/删除区域](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [如何为 SQL API 帐户创建自定义冲突解决策略](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)

<!--Update_Description: update meta properties, wording update, update link -->