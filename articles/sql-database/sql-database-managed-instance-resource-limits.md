---
title: Azure SQL 数据库资源限制 - 托管实例 | Microsoft Docs
description: 本文概述托管实例的 Azure SQL 数据库资源限制。
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab, jovanpop, sachinp
manager: digimobile
origin.date: 10/17/2018
ms.date: 12/03/2018
ms.openlocfilehash: 6029f84315643d10bccc7447ea4cc7fc7a785ba8
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52673149"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Azure SQL 数据库托管实例资源限制概述

本文提供 Azure SQL 数据库托管实例资源限制的概述，并介绍如何创建请求来提高默认区域订阅限制。 

> [!NOTE]
> 有关其他托管实例限制，请参阅[基于 vCore 的购买模型](sql-database-managed-instance.md#vcore-based-purchasing-model)和[托管实例服务层](sql-database-managed-instance.md#managed-instance-service-tiers)。 有关支持的功能和 T-SQL 语句的差异，请参阅[功能差异](sql-database-features.md)和 [T-SQL 语句支持](sql-database-managed-instance-transact-sql-information.md)。

## <a name="instance-level-resource-limits"></a>实例级资源限制

托管实例的某些特征和资源限制取决于底层基础结构和体系结构。 限制取决于硬件代次和服务层。

### <a name="hardware-generation-characteristics"></a>硬件代次特征

Azure SQL 数据库托管实例可部署在两个硬件代次（Gen4 和 Gen5）上。 硬件代次具有不同的特征，如下表中所述：

|   | **第 4 代** | **第 5 代** |
| --- | --- | --- |
| 硬件 | Intel E5-2673 v3 (Haswell) 2.4-GHz 处理器、附加的 SSD vCore = 1 PP（物理核心） | Intel E5-2673 v4 (Broadwell) 2.3-GHz 处理器、快速 eNVM SSD、vCore=1 LP（超线程） |
| 计算 | 8、16、24 个 vCore | 8、16、24、32、40、64、80 个 vCore |
| 内存 | 每个 vCore 7 GB | 每个 vCore 5.1 GB |
| 最大存储空间（业务关键） | 1 TB | 1 TB、2 TB 或 4 TB，具体取决于核心数 |

### <a name="service-tier-characteristics"></a>服务层特征

托管实例有两个服务层 -“常规用途”和“业务关键”（公共预览版）。 这些层提供不同的功能，如下表中所述：

| **功能** | **常规用途** | **业务关键（预览版）** |
| --- | --- | --- |
| vCore 数目\* | Gen4：8、16、24<br/>Gen5：8、16、24、32、40、64、80 | Gen4：8、16、24、32 <br/> Gen5：8、16、24、32、40、64、80 |
| 内存 | Gen4：56GB-156GB<br/>Gen5：44GB-440GB<br/>\*与 vCore 数成正比 | Gen4：56GB-156GB <br/> Gen5：44GB-440GB<br/>\*与 vCore 数成正比 |
| 最大存储大小 | 8 TB | Gen 4：1 TB <br/> 第 5 代： <br/>- 1 TB（适用于 8、16 个 vCore）<br/>- 2 TB（适用于 24 个 vCore）<br/>- 4 TB（适用于 32、40、64、80 个 vCore） |
| 每个数据库的最大存储 | 由每个实例的最大存储大小决定 | 由每个实例的最大存储大小决定 |
| 每个实例的数据库数目上限 | 100 | 100 |
| 每个实例的数据库文件数目上限 | 最多 280 个 | 无限制 |
| 预期的最大存储 IOPS | 500-5000（[取决于数据文件大小](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)）。 | 取决于底层 SSD 速度。 |

## <a name="supported-subscription-types"></a>支持的订阅类型

目前，托管实例仅支持以下订阅类型中的部署：

- [提前支付](https://www.azure.cn/zh-cn/offers/ms-mc-arz-33p/ )

## <a name="regional-resource-limitations"></a>区域资源限制

支持的订阅类型可以包含每个区域的有限数量的资源。 托管实例根据订阅类型对每个 Azure 区域实施两种默认限制：

- **子网限制**：在单个区域中部署托管实例的子网数目上限。
- **实例数目限制**：可在单个区域中部署的实例数目上限。

下表显示了受支持订阅的默认区域限制：

|订阅类型| 托管实例子网数目上限 | 实例数目上限 |GP 托管实例数目上限*|BC 托管实例数目上限*|
| :---| :--- | :--- |:--- |:--- |
|提前支付|1*|4*|4*|1*|

\* 可在一个子网中部署 1 个 BC 或 4 个 GP 实例，使子网中的“实例单元”总数永远不会超过 4 个。

** 如果另一个服务层中没有任何实例，则适用一个服务层中的实例数目上限。 如果你打算在同一子网内混用 GP 和 BC 实例，请参考以下部分了解允许的组合方式。 简单的规则是，子网总数不能超过 3 个，且实例单元的总数不能超过 12 个。

> [!IMPORTANT]
> 规划部署时，请考虑到业务关键 (BC) 实例（由于增加了冗余性）通常使用比常规用途 (GP) 实例多 4 倍的容量。 因此，在计算中，1 个 GP 实例 = 1 个实例单元，1 个 BC 实例 = 4 个实例单元。 若要根据默认限制简化消耗量分析，请汇总区域中部署了托管实例的所有子网内的实例单元，并将其结果与订阅类型的实例单元限制进行比较。

> [!Note] 
> [提前支付](https://www.azure.cn/zh-cn/offers/ms-mc-arz-33p/)订阅类型可以包含一个业务关键实例，或最多 4 个常规用途实例。

以下示例演示了使用非空子网并混用 GP 与 BC 服务层的部署案例。

|子网数|子网 1|子网 2|子网 3|
|:---|:---|:---|:---|
|1|1 个 BC 和最多 8 个 GP<br>2 个 BC 和最多 4 个 GP|不适用| 不适用|
|2|0 个BC，最多 4 个 GP|1 个BC，最多 4 个 GP<br>2 个 BC，0 个 GP|不适用|
|2|1 个 BC，0 个 GP|0 个BC，最多 8 个 GP<br>1 个BC，最多 4 个 GP|不适用|
|2|2 个 BC，0 个 GP|0 个BC，最多 4 个 GP|不适用|
|3|1 个 BC，0 个 GP|1 个 BC，0 个 GP|0 个BC，最多 4 个 GP|
|3|1 个 BC，0 个 GP|0 个BC，最多 4 个 GP|0 个BC，最多 4 个 GP|

## <a name="next-steps"></a>后续步骤

- 有关托管实例的详细信息，请参阅[什么是托管实例？](sql-database-managed-instance.md)。 
- 有关定价信息，请参阅 [SQL 数据库托管实例定价](https://azure.cn/pricing/details/sql-database)。
- 若要了解如何创建第一个托管实例，请参阅[快速入门指南](sql-database-managed-instance-get-started.md)。
