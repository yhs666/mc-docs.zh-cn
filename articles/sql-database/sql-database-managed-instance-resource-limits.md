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
origin.date: 02/27/2019
ms.date: 04/08/2019
ms.openlocfilehash: 0d1725ac1c10c6c16aeff50ce3e6385b6ef2d84f
ms.sourcegitcommit: 0777b062c70f5b4b613044804706af5a8f00ee5d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "59003521"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Azure SQL 数据库托管实例资源限制概述

本文提供 Azure SQL 数据库托管实例资源限制的概述，并介绍如何创建请求来提高默认区域订阅限制。

> [!NOTE]
> 有关其他托管实例限制，请参阅[基于 vCore 的购买模型](sql-database-managed-instance.md#vcore-based-purchasing-model)和[托管实例服务层](sql-database-managed-instance.md#managed-instance-service-tiers)。 有关支持的功能和 T-SQL 语句的差异，请参阅[功能差异](sql-database-features.md)和 [T-SQL 语句支持](sql-database-managed-instance-transact-sql-information.md)。

## <a name="instance-level-resource-limits"></a>实例级资源限制

托管实例的某些特征和资源限制取决于底层基础结构和体系结构。 限制取决于硬件代次和服务层。

### <a name="hardware-generation-characteristics"></a>硬件代次特征

Azure SQL 数据库托管实例可部署在两个硬件代次（Gen4 和 Gen5）上。 硬件代次具有不同的特征，如下表中所述：

|   | **Gen4** | **Gen5** |
| --- | --- | --- |
| 硬件 | Intel E5-2673 v3 (Haswell) 2.4-GHz 处理器、附加的 SSD vCore = 1 PP（物理核心） | Intel E5-2673 v4 (Broadwell) 2.3-GHz 处理器、快速 NVMe SSD、vCore=1 LP（超线程） |
| 计算 | 8、16、24 个 vCore | 8、16、24、32、40、64、80 个 vCore |
| 内存 | 每个 vCore 7 GB | 每个 vCore 5.1 GB |
| 内存中 OLTP 内存 | 每个 vCore 3 GB | 每个 vCore 2.6 GB |
| 最大存储（常规用途） |  8 TB | 8 TB |
| 最大存储空间（业务关键） | 1 TB | 1 TB、2 TB 或 4 TB，具体取决于核心数 |

### <a name="service-tier-characteristics"></a>服务层特征

托管实例具有两个服务层：常规用途和业务关键。 这些层提供不同的功能，如下表中所述：

| **功能** | **常规用途** | **业务关键** |
| --- | --- | --- |
| vCore 数目\* | 第 4 代：8、16、24<br/>第 5 代：8、16、24、32、40、64、80 | 第 4 代：8、16、24、32 <br/> 第 5 代：8、16、24、32、40、64、80 |
| 内存 | Gen4：56 GB - 168 GB<br/>Gen5：40.8 GB - 408 GB<br/>\*与 vCore 数成正比 | Gen4：56 GB - 168 GB <br/> Gen5：40.8 GB - 408 GB<br/>\*与 vCore 数成正比 |
| 最大存储大小 | 8 TB | Gen4：1 TB <br/> Gen5： <br/>- 1 TB（适用于 8、16 个 vCore）<br/>- 2 TB（适用于 24 个 vCore）<br/>- 4 TB（适用于 32、40、64、80 个 vCore） |
| 每个数据库的最大存储 | 由每个实例的最大存储大小决定 | 由每个实例的最大存储大小决定 |
| 每个实例的数据库数目上限 | 100 | 100 |
| 每个实例的数据库文件数目上限 | 最多 280 个 | 每个数据库 32,767 个文件 |
| 数据/日志 IOPS（近似值） | 500 - 7,500（每个文件）<br/>\*[取决于文件大小](/virtual-machines)| 11 K - 110 K（每个 vCore 为 1,375） |
| 日志吞吐量 | 22 MB/s（每个实例） | 3 MB/s（每个 vCore）<br/>最大为 48 MB/秒（每个实例）|
| 数据吞吐量（近似值） | 每个文件 100-250 MB/秒<br/>\*[取决于文件大小](/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | 每个 vCore 24-48 MB/秒 |
| IO 延迟（近似） | 5-10 毫秒 | 1-2 毫秒 |
| 最大 tempDB 大小 | 192 - 1,920 GB（每个 vCore 为 24 GB） | 无约束 - 受最大实例存储大小限制 |

**注释**：

- 与最大存储大小限制进行比较的实例存储大小同时包括用户数据库和系统数据库中的数据和日志文件大小。 可以使用 <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys.master_files</a> 系统视图来确定数据库使用的空间总量。 错误日志不会持久保存，不包括在大小中。 备份不包括在存储大小中。
- 吞吐量和 IOPS 还取决于不受托管实例显式限制的页面大小。

## <a name="supported-subscription-types"></a>支持的订阅类型

目前，托管实例仅支持以下订阅类型中的部署：

- [提前支付](https://www.azure.cn/zh-cn/offers/ms-mc-arz-33p/ )

## <a name="regional-resource-limitations"></a>区域资源限制

支持的订阅类型可以包含每个区域的有限数量的资源。 托管实例根据订阅类型对每个 Azure 区域实施两种默认限制：

- **子网限制**：在单一区域中部署托管实例的子网数上限。
- **实例数目限制**：可在单一区域中部署的实例数上限。

下表显示了受支持订阅的默认区域限制：

|订阅类型| 托管实例子网数目上限 | 实例数目上限 |GP 托管实例数目上限*|BC 托管实例数目上限*|
| :---| :--- | :--- |:--- |:--- |
|提前支付|1*|4*|4*|1*|

\* 可在一个子网中部署 1 个 BC 或 4 个 GP 实例，使子网中的“实例单元”总数永远不会超过 4 个。

** 如果另一个服务层中没有任何实例，则适用一个服务层中的实例数目上限。 如果你打算在同一子网内混用 GP 和 BC 实例，请参考以下部分了解允许的组合方式。 简单的规则是，子网总数不能超过 3 个，实例单位总数不能超过 12 个。

如果在当前区域中需要更多托管实例，可以[在 Azure 门户中创建特殊支持请求](#obtaining-a-larger-quota-for-sql-managed-instance)，以提高这些限制。 或者，可以在另一个 Azure 区域中创建新的托管实例，而不需要发送支持请求。

> [!IMPORTANT]
> 在规划部署时，请考虑到业务关键 (BC) 实例（由于增加了冗余）通常都会使用比常规用途 (GP) 实例多 4 倍的容量。 因此，在计算中，1 个 GP 实例 = 1 个实例单元，1 个 BC 实例 = 4 个实例单元。 若要根据默认限制简化消耗量分析，请汇总区域中部署了托管实例的所有子网内的实例单元，并将其结果与订阅类型的实例单元限制进行比较。

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
|3|1 个 BC，0 个 GP|0 个BC，最多 4 个 GP|0 个 BC，最多 4 个 GP|

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>获取更大的 SQL 托管实例配额

如果在当前区域中需要更多托管实例，可以使用 Azure 门户发送扩展配额的支持请求。
若要启动获取更大配额的过程，请执行以下操作：

1. 打开“帮助 + 支持”，单击“新建支持请求”。

   ![帮助和支持](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. 在新支持请求的“基本信息”选项卡上：
   - 对于“问题类型”，选择“服务和订阅限制(配额)”。
   - 对于“订阅”，请选择自己的订阅。
   - 对于“配额类型”，选择“SQL 数据库托管实例”。
   - 对于“支持计划”，选择自己的支持计划。

     ![问题类型配额](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. 单击“下一步”。
4. 在新支持请求的“问题”选项卡上：
   - 对于“严重性”，选择问题的严重性级别。
   - 对于“详细信息”，提供有关问题的其他信息，包括错误消息。
   - 对于“文件上传”，附加包含详细信息的文件（最多 4 MB）。

     ![问题详细信息](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > 有效的请求应包括：
     > - 需要提高订阅限制的区域
     > - 每个服务层在配额增加后在现有的子网中所需的实例数目（如果需要扩展任何现有的子网）
     > - 所需的新子网数目，以及每个服务层在新子网内的实例总数（如果需要在新子网中部署托管实例）。

5. 单击“下一步”。
6. 在新支持请求的“联系人信息”选项卡上，输入首选联系方式（电子邮件或电话）和联系人详细信息。
7. 单击**创建**。

## <a name="next-steps"></a>后续步骤

- 有关托管实例的详细信息，请参阅[什么是托管实例？](sql-database-managed-instance.md)。
- 有关定价信息，请参阅 [SQL 数据库托管实例定价](https://azure.cn/pricing/details/sql-database)。
- 若要了解如何创建第一个托管实例，请参阅[快速入门指南](sql-database-managed-instance-get-started.md)。
