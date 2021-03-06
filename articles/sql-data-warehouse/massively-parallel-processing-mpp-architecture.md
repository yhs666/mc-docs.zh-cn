---
title: Azure Synapse Analytics（以前称为 SQL DW）体系结构
description: 了解 Azure Synapse Analytics（以前称为 SQL DW）如何将大规模并行处理 (MPP) 与 Azure 存储结合，实现高性能和可伸缩性。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
origin.date: 11/04/2019
ms.date: 12/09/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 920a81e70cf84f5d461074749be9dd7ebc09f2c8
ms.sourcegitcommit: 369038a7d7ee9bbfd26337c07272779c23d0a507
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2019
ms.locfileid: "74807650"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-architecture"></a>Azure Synapse Analytics（以前称为 SQL DW）体系结构 

Azure Synapse 是一种无限制的分析服务，它将企业数据仓库和大数据分析结合在一起。 借助它可以使用无服务器的按需资源或预配资源，任意执行自己定义的大规模数据查询。 Azure Synapse 将这两个领域结合在一起，提供统一的体验来引入、准备、管理和处理数据，以满足即时 BI 需求。

 Azure Synapse 包含四个组件：
- SQL Analytics：基于 T-SQL 的完整分析 
    - SQL 池（按预配的 DWU 付费）- 已公开发布
    - SQL 随选（按处理的 TB 付费）-（预览）
- Spark：深度集成的 Apache Spark（预览） 
- 数据集成：混合数据集成（预览）
- Studio：统一的用户体验。  （预览版）
## <a name="sql-analytics-mpp-architecture-components"></a>SQL Analytics MPP 体系结构组件

[SQL Analytics](sql-data-warehouse-overview-what-is.md#sql-analytics-and-sql-pool-in-azure-synapse) 利用向外扩展体系结构在多个节点间分布数据的计算处理。 缩放单位是计算能力（称为[数据仓库单位](what-is-a-data-warehouse-unit-dwu-cdwu.md)）的抽象概念。 计算独立于存储，使用户能够独立于系统中的数据来缩放计算。

![SQL Analytics 体系结构](media/massively-parallel-processing-mpp-architecture/massively-parallel-processing-mpp-architecture.png)

SQL Analytics 使用基于节点的体系结构。 应用程序将 T-SQL 命令连接到、发布给控制节点，该节点是 SQL Analytics 的单一入口点。 控制节点运行用于优化并行处理查询的 MPP 引擎，然后将操作传递给计算节点以实现并行工作。 

计算节点将所有用户数据存储在 Azure 存储中并运行并行查询。 数据移动服务 (DMS) 是一项系统级内部服务，它根据需要在节点间移动数据以并行运行查询和返回准确的结果。 

使用脱偶联的存储和计算，用户可以在使用 SQL Analytics 时执行以下操作：

* 无论存储需求如何，都可独立计算大小。
* 无需移动数据，即可在 SQL 池（数据仓库）中增加或减少计算能力。
* 在保持数据不受影响的情况下暂停计算容量，因此只需为存储付费。
* 在操作期间恢复计算容量。

### <a name="azure-storage"></a>Azure 存储

SQL Analytics 利用 Azure 存储保护用户数据。  由于数据通过 Azure 存储进行存储和管理，因此会对存储消耗单独收费。 将数据本身分片到“分布区”中来优化系统性能  。 可选择在定义表时用于分布数据的分片模式。 支持以下分片模式：

* 哈希
* 轮循机制
* 复制

### <a name="control-node"></a>控制节点

控制节点是体系结构的核心。 它是与所有应用程序和连接进行交互的前端。 MPP 引擎在控制节点上运行以优化和协调并行查询。 将 T-SQL 查询提交到 SQL Analytics 时，控制节点会将其转换为可针对每个分布区并行运行的查询。

### <a name="compute-nodes"></a>计算节点

计算节点提供计算能力。 分布区映射到计算节点以进行处理。 如果支付更多计算资源费用，SQL Analytics 会将分布区重新映射到可用的计算节点。 计算节点数的范围是 1 到 60，它由 SQL Analytics 的服务级别确定。

每个计算节点均有一个节点 ID，该 ID 会显示在系统视图中。 在名称以 sys.pdw_nodes 开头的系统视图中找到 node_id 列即可查看计算节点 ID。 有关这些系统视图的列表，请参阅 [MPP 系统视图](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views?view=aps-pdw-2016-au7)。

### <a name="data-movement-service"></a>数据移动服务
数据移动服务 (DMS) 是一项数据传输技术，它可协调计算节点间的数据移动。 某些查询需要移动数据以确保并行查询返回准确的结果。 需要移动数据时，DMS 可确保正确的数据到达正确的位置。 

## <a name="distributions"></a>分发

分布区是存储和处理针对分布式数据运行的并行查询的基本单位。 SQL Analytics 运行查询时，工作会被分割成 60 个并行运行的小型查询。 

每个小型查询各在一个数据分布区上运行。 每个计算节点管理其中一个或多个分布区。 具有最多计算资源的 SQL 池的每个分布区占 1 个计算节点。 具有最小计算资源的 SQL 池的所有分布区占 1 个计算节点。  

## <a name="hash-distributed-tables"></a>哈希分布表
哈希分布表可为大型表上的联接和聚合提供最高查询性能。 

为了将数据分片到哈希分布式表中，SQL Analytics 使用哈希函数明确将 1 个行分配到 1 个分布区。 在表定义中，可以将一个列指定为分布列。 哈希函数使用分布列中的值将 1 个行分配到 1 个分布区。

下图说明了如何将完整的非分布式表存储为哈希分布表。 

![分布式表](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "分布式表")  

* 一个行属于一个分布区。  
* 通过确定性哈希算法将一个行分配到一个分布区。  
* 不同大小的表显示，每个分布区的表行的数目各不相同。

选择分布列时需考虑到性能，例如特异性、数据倾斜，以及在系统上运行的查询类型。

## <a name="round-robin-distributed-tables"></a>轮循分布表
轮循机制表是最简单的表，在被用作负载临时表时，它可创造和提供高速性能。

轮循机制分布表在表中均匀分布数据，但不会进行进一步优化。 首先随机选择一个分布区，然后将行的缓冲区按顺序分配给分布区。 将数据加载到轮循机制表速度很快，但就查询性能而言，哈希分布式表的性能更佳。 轮循机制表上的联接要求重新安排数据，因此这需要花费更多时间。


## <a name="replicated-tables"></a>复制表
复制表为小型表提供最快查询性能。

复制表在每个计算节点上缓存表的完整副本。 因此复制表以后，无需在执行联接或聚合前在计算节点中间传输数据。 复制表尤为适用于小型表。 它需要额外存储并且在写入数据时会产生额外负载，因此不适用于大型表。  

下图显示了在每个计算节点的第一个分布区上缓存的复制表。  

![复制表](media/sql-data-warehouse-distributed-data/replicated-table.png "复制表") 

## <a name="next-steps"></a>后续步骤
对 Azure Synapse 有了初步的认识后，请学习如何快速[创建 SQL 池][create a SQL pool]和[加载示例数据][load sample data]。 如果不熟悉 Azure，遇到新术语时，[Azure 词汇表][Azure glossary] 可以提供帮助。 或者，查看以下一些其他 Azure Synapse 资源。  

* [功能请求]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL pool]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[功能请求]: https://support.windowsazure.cn/support/support-azure

[SLA for Azure Synapse]: https://www.azure.cn/support/sla/sql-data-warehouse/
[Service Level Agreements]: https://www.azure.cn/support/legal/sla/
