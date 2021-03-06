---
title: “超大规模”常见问题解答
description: 对客户关于“超大规模”服务层级中的 Azure SQL 数据库（通常称为超大规模数据库）提出的常见问题的回答。
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: ''
origin.date: 10/12/2019
ms.date: 12/16/2019
ms.openlocfilehash: 03452a218f763cd2d4b74d2af19ad47f168d4016
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336223"
---
# <a name="azure-sql-database-hyperscale-faq"></a>Azure SQL 数据库“超大规模”常见问题解答

本文就客户对 Azure SQL 数据库“超大规模”服务层级中的数据库（在本常见问题解答的剩余部分简称为“超大规模”）提出的常见问题做出回答。 本文介绍“超大规模”支持的方案，以及与“超大规模”兼容的功能。

- 本常见问题解答适用于对“超大规模”服务层级有基本了解并希望其具体问题得到解答的读者。
- 本常见问题解答不是指南，不解答关于如何使用“超大规模”数据库的问题。 有关“超大规模”的简介，建议参考 [Azure SQL 数据库“超大规模”](sql-database-service-tier-hyperscale.md)文档。

## <a name="general-questions"></a>一般问题

### <a name="what-is-a-hyperscale-database"></a>什么是“超大规模”数据库

超大规模数据库是“超大规模”服务层级中的 Azure SQL 数据库，由超大规模横向扩展存储技术提供支持。 一个“超大规模”数据库支持最多 100 TB 的数据，提供高吞吐量和高性能，可以快速缩放以适应工作负荷要求。 缩放对应用程序透明 - 连接、查询处理等等，都与其他任何 Azure SQL 数据库一样工作。

### <a name="what-resource-types-and-purchasing-models-support-hyperscale"></a>哪些资源类型和购买模型支持“超大规模”

“超大规模”服务层级仅适用于在 Azure SQL 数据库中使用基于 vCore 的购买模型的单一数据库。  

### <a name="how-does-the-hyperscale-service-tier-differ-from-the-general-purpose-and-business-critical-service-tiers"></a>“超大规模”服务层级与“常规用途”和“业务关键”服务层级有何区别

根据下表中所述的数据库可用性、存储类型性能和最大大小区分基于 vCore 的服务层级。

| | 资源类型 | 常规用途 |  超大规模 | 业务关键 |
|:---:|:---:|:---:|:---:|:---:|
| **最适用于** |全部|提供以预算导向的、均衡的计算和存储选项。|大多数业务工作负荷。 自动缩放存储大小，最大可达 100 TB，快速的垂直和水平计算缩放，快速数据库还原。|事务率较高、IO 延迟较低的 OLTP 应用程序。 使用多个同步更新的副本提供最高故障复原能力和快速故障转移。|
|  **资源类型** ||单一数据库/弹性池/托管实例 | 单一数据库 | 单一数据库/弹性池/托管实例 |
| **计算大小**|单一数据库/弹性池* | 1 - 80 个 vCore | 1 - 80 个 vCore* | 1 - 80 个 vCore |
| |托管实例 | 8、16、24、32、40、64、80 个 vCore | 不适用 | 8、16、24、32、40、64、80 个 vCore |
| **存储类型** | 全部 |高级远程存储（每个实例） | 具有本地 SSD 缓存的分离的存储（每个实例） | 超快的本地 SSD 存储（每个实例） |
| **存储大小** | 单一数据库/弹性池 | 5 GB - 4 TB | 最多 100 TB | 5 GB - 4 TB |
| | 托管实例  | 32 GB - 8 TB | 不适用 | 32 GB - 4 TB |
| **IOPS** | 单一数据库** | 每个 vCore 提供 500 IOPS，最大 7000 IOPS | 超大规模是具有多个级别缓存的多层体系结构。 有效 IOPS 将取决于工作负荷。 | 5000 IOPS，最大 200,000 IOPS|
| | 托管实例 | 取决于文件大小 | 不适用 | 1375 IOPS/vCore |
|**可用性**|全部|1 个副本，无读取扩展，无本地缓存 | 多个副本，最多 4 个读取扩展，部分本地缓存 | 3 个副本，1 个读取扩展，完整的本地存储 |
|**备份**|全部|RA-GRS，7-35 天保留期（默认为 7 天）| RA-GRS，7 天保留期，恒定的时间时点恢复 (PITR) | RA-GRS，7-35 天保留期（默认为 7 天） |

\*“超大规模”服务层级不支持弹性池

### <a name="who-should-use-the-hyperscale-service-tier"></a>哪些群体应使用“超大规模”服务层级

“超大规模”服务层级面向拥有大型本地 SQL Server 数据库并希望通过迁移到云来实现应用程序现代化的客户，或已使用 Azure SQL 数据库并且想大大拓展数据库增长可能性的客户。 “超大规模”服务层级也适用于那些寻求高性能和高可伸缩性的客户。 使用“超大规模”服务层级，可以：

- 数据库大小最大为 100 TB
- 快速进行数据库备份，无需考虑数据库大小（备份基于存储快照）
- 快速进行数据库还原，无需考虑数据库大小（从存储快照还原）
- 无论数据库大小和 vCore 数目如何，都可提高日志吞吐量
- 使用一个或多个只读副本的读取扩展，用于卸载读取工作负荷，并用作热备用服务器。
- 在恒定时间内快速纵向扩展计算，以便提高适应繁重工作负荷的能力；然后在恒定时间内减少。 例如，这与在 P6 和 P11 之间来回缩放类似，但速度更快，因为这不是一种数据大小操作。

### <a name="what-regions-currently-support-hyperscale"></a>哪些区域当前支持“超大规模”

“超大规模”服务层级目前已在 [Azure SQL 数据库“超大规模”概述](sql-database-service-tier-hyperscale.md#regions)下面列出的区域中推出。

### <a name="can-i-create-multiple-hyperscale-databases-per-logical-server"></a>能否为每个逻辑服务器创建多个“超大规模”数据库

是的。 有关每个逻辑服务器的“超大规模”数据库数量的详细信息和限制，请参阅[逻辑服务器上单一和共用数据库的 SQL 数据库资源限制](sql-database-resource-limits-logical-server.md)。

### <a name="what-are-the-performance-characteristics-of-a-hyperscale-database"></a>“超大规模”数据库的性能特征有哪些

“超大规模”体系结构提供高性能和吞吐量，同时支持大型数据库。 

### <a name="what-is-the-scalability-of-a-hyperscale-database"></a>什么是“超大规模”数据库的可伸缩性

“超大规模”根据工作负荷需求，提供快速的可伸缩性。

- **增加/减少**

  使用“超大规模”数据库，可以纵向扩展 CPU、内存等资源的主要计算大小，然后在恒定的时间内将其减少。 因为是共享存储，所以纵向扩展和缩减不是数据大小操作。  
- **缩小/扩大**

  借助“超大规模”数据库，还能够预配一个或多个额外的计算副本，用于为读取请求提供服务。 这意味着，可以将这些额外的计算副本用作只读副本，从主计算卸载读取工作负荷。 这些副本除用作只读副本外，在主计算发生故障转移时，还充当热备用节点。

  对每个额外的计算副本的预配可在恒定时间内完成，并且是联机操作。 将连接字符串的 `ApplicationIntent` 参数设置为 `ReadOnly`，即可连接到这些额外的只读计算副本。 具有 `ReadOnly` 应用程序意向的任何连接均自动路由到某个额外的只读计算副本。

## <a name="deep-dive-questions"></a>深入的问题

### <a name="can-i-mix-hyperscale-and-single-databases-in-a-single-logical-server"></a>是否可以在单个逻辑服务器中混合使用超大规模数据库和单一数据库

可以。

### <a name="does-hyperscale-require-my-application-programming-model-to-change"></a>“超大规模”数据库需要更改应用程序编程模型吗

不，应用程序编程模型保持原样。 可以照常使用连接字符串和其他常规方式，以与“超大规模”数据库进行交互。

### <a name="what-transaction-isolation-level-is-the-default-in-a-hyperscale-database"></a>“超大规模”数据库中的默认事务隔离级别是什么

在主要副本上，默认的事务隔离级别为 RCSI（已提交读快照隔离）。 在读取扩展次要副本上，默认隔离级别为“快照”。

### <a name="can-i-bring-my-on-premises-or-iaas-sql-server-license-to-hyperscale"></a>能否将我的本地或 IaaS SQL Server 许可证引入“超大规模”

能，[Azure 混合权益](https://azure.cn/pricing/hybrid-benefit/)适用于“超大规模”。 每个 SQL Server Standard 核心都可以映射到 1 个超大规模 vCore。 每个 SQL Server Enterprise 核心都可以映射到 4 个“超大规模”vCore。 对于次要副本，不需要 SQL 许可证。 Azure 混合权益价格会自动应用到读取扩展（次要）副本。

### <a name="what-kind-of-workloads-is-hyperscale-designed-for"></a>哪种工作负荷专为“超大规模”设计

“超大规模”支持所有 SQL Server 工作负荷，但它主要针对 OLTP 进行了优化。 还可以引入混合 (HTAP) 和分析（数据市场）工作负荷。

### <a name="how-can-i-choose-between-azure-sql-data-warehouse-and-azure-sql-database-hyperscale"></a>如何在 Azure SQL 数据仓库和 Azure SQL 数据库“超大规模”之间做出选择

如果目前运行的是使用 SQL Server 作为数据仓库的交互式分析查询，“超大规模”是很好的选择，因为能以较低费用托管中小型数据仓库（例如几 TB 到 100 TB），并且只需对 T-SQL 代码进行极少量的更改，即可将 SQL Server 数据仓库工作负荷迁移到“超大规模”。

如果大规模运行包含复杂查询且持续引入速率超过 100 MB/秒的数据分析，并使用并行数据仓库 (PDW)、Teradata 或其他大规模并行处理 (MPP) 数据仓库，则 SQL 数据仓库可能是最佳选择。
  
## <a name="hyperscale-compute-questions"></a>“超大规模”计算问题

### <a name="can-i-pause-my-compute-at-any-time"></a>能不能随时暂停计算

目前不能，但可以缩减计算和副本数量，以降低非高峰时段的成本。

### <a name="can-i-provision-a-compute-replica-with-extra-ram-for-my-memory-intensive-workload"></a>能不能为内存密集型工作负荷预配包含额外 RAM 的计算副本

否。 要获取更多 RAM，需要升级到更大的计算大小。 有关详细信息，请参阅[超大规模存储和计算大小](sql-database-vcore-resource-limits-single-databases.md#hyperscale---provisioned-compute---gen5)。

### <a name="can-i-provision-multiple-compute-replicas-of-different-sizes"></a>能不能预配大小不同的多个计算副本

否。

### <a name="how-many-read-scale-out-replicas-are-supported"></a>支持多少个读取扩展副本

默认情况下，超大规模数据库是使用一个读取扩展副本（包括主要副本共有 2 个副本）创建的。 可以使用 [Azure 门户](https://portal.azure.cn)或 [REST API](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) 将只读副本的数目缩放为 0 到 4 个。

### <a name="for-high-availability-do-i-need-to-provision-additional-compute-replicas"></a>若要实现高可用性，是否需要预配额外的计算副本

在超大规模数据库中，数据复原能力是在存储级别提供的。 只需一个副本即可提供复原能力。 计算副本关闭后，系统自动创建一个新副本，且不会丢失数据。

但是，如何只有一个副本，故障转移后可能需要一些时间才能在新副本中生成本地缓存。 在缓存重新生成阶段，数据库直接从页面服务器中提取数据，导致存储延迟提高，查询性能下降。

对于需要高可用性且只能对故障排除造成轻微影响的任务关键应用，应该预配至少 2 个计算副本（包括主要计算副本）。 这是默认配置。 这样，就有一个充当故障转移目标的热备用副本可用。

## <a name="data-size-and-storage-questions"></a>数据大小和存储问题

### <a name="what-is-the-maximum-database-size-supported-with-hyperscale"></a>“超大规模”支持的最大数据库大小是多少

100 TB。

### <a name="what-is-the-size-of-the-transaction-log-with-hyperscale"></a>“超大规模”数据库事务日志的大小

“超大规模”数据库的事务日志几乎是无穷大的。 不需要担心在具有高日志吞吐量的系统上耗尽日志空间。 但是，可能为连续主动写入工作负荷限制日志生成速率。 峰值持续日志生成速率为 100 MB/秒。

### <a name="does-my-tempdb-scale-as-my-database-grows"></a>`tempdb` 是否会随着数据库增长而缩放

`tempdb` 数据库位于本地 SSD 存储，是根据预配的计算大小配置的。 为提供最大性能优势，对 `tempdb` 进行了优化。 `tempdb` 大小不可配置，它由你管理。

### <a name="does-my-database-size-automatically-grow-or-do-i-have-to-manage-the-size-of-data-files"></a>数据库大小是否自动增长，我是否需要管理数据文件的大小

你的数据库会随着你插入/引入更多数据自动增长。

### <a name="what-is-the-smallest-database-size-that-hyperscale-supports-or-starts-with"></a>“超大规模”支持或最初使用的最小数据库大小是多少

10 GB。

### <a name="in-what-increments-does-my-database-size-grow"></a>数据库的大小按多少增量增长

每个数据文件以 10 GB 增量增长。 多个数据文件可以同时增长。

### <a name="is-the-storage-in-hyperscale-local-or-remote"></a>“超大规模”中的存储是本地存储还是远程存储

在“超大规模”数据库中，数据文件存储在 Azure 标准存储中。 数据完全缓存在本地 SSD 存储中靠近计算副本的页面服务器上。 此外，计算副本在本地 SSD 和内存中均有数据缓存，以便降低从远程页面服务器提取数据的频率。

### <a name="can-i-manage-or-define-files-or-filegroups-with-hyperscale"></a>能否使用“超大规模”数据库管理或定义文件或文件组

否。 自动添加数据文件。 创建其他文件组的常见原因在“超大规模”存储体系结构中不适用。

### <a name="can-i-provision-a-hard-cap-on-the-data-growth-for-my-database"></a>能否为我的数据库的数据增长预配硬上限

否。

### <a name="how-are-data-files-laid-out-with-hyperscale"></a>如何使用“超大规模”排列数据文件

数据文件由页面服务器控制，每个数据文件有一个页面服务器。 随着数据大小的增长添加数据文件和关联的页面服务器。

### <a name="is-database-shrink-supported"></a>是否支持数据库收缩

否。

### <a name="is-data-compression-supported"></a>是否支持数据压缩

是，包括行、页和列存储压缩。

### <a name="if-i-have-a-huge-table-does-my-table-data-get-spread-out-across-multiple-data-files"></a>如果表格巨大，表格数据是否会分布在多个数据文件中

是的。 与给定表格关联的数据页可在多个数据文件中出现，它们均属于相同文件组。 SQL Server 使用[按比例填充策略](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups#file-and-filegroup-fill-strategy)在数据文件间分布数据。

## <a name="data-migration-questions"></a>数据迁移问题

### <a name="can-i-move-my-existing-azure-sql-databases-to-the-hyperscale-service-tier"></a>能否将现有 Azure SQL 数据库迁移到“超大规模”服务层级

是的。 可以将现有 Azure SQL 数据库迁移到“超大规模”服务层级。 这是一种单向迁移。 无法将数据库从“超大规模”层级移到另一个服务层级。 对于概念证明 (POC)，我们建议创建数据库的副本，并将副本迁移到“超大规模”。
  
### <a name="can-i-move-my-hyperscale-databases-to-other-service-tiers"></a>能否将“超大规模”数据库迁移到其他服务层级

否。 目前，无法将超大规模数据库迁移到其他服务层级。

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>迁移到“超大规模”服务层级后，是否会丢失一些功能

是的。 目前，部分 Azure SQL 数据库功能不受支持，包括但不限于长期备份保留。 将数据库迁移到“超大规模”服务层级后，这些功能将停止运行。  我们预期这些限制是暂时性的。

### <a name="can-i-move-my-on-premises-sql-server-database-or-my-sql-server-database-in-a-cloud-virtual-machine-to-hyperscale"></a>能否将我的本地 SQL Server 数据库或云虚拟机中的 SQL Server 数据库迁移到“超大规模”

是的。 可以使用所有现有的迁移技术迁移到“超大规模”，包括事务复制，以及任何其他数据移动技术（批量复制、Azure 数据工厂、SSIS）。 另请参阅支持许多迁移方案的 [Azure 数据库迁移服务](../dms/dms-overview.md)。

### <a name="what-is-my-downtime-during-migration-from-an-on-premises-or-virtual-machine-environment-to-hyperscale-and-how-can-i-minimize-it"></a>从本地或虚拟机环境迁移到“超大规模”期间，我的停机时间有多长，如何尽量减少停机时间

迁移到“超大规模”造成的停机时间与将数据库迁移到其他 Azure SQL 数据库服务层级造成的停机时间相同。 可以使用[事务复制](replication-to-sql-database.md#data-migration-scenario
)尽量减少大小最多几 TB 的数据库的故障时间迁移。 对于非常大的数据库（10 TB 以上），可以考虑使用 ADF、Spark 或其他数据移动技术迁移数据。

### <a name="how-much-time-would-it-take-to-bring-in-x-amount-of-data-to-hyperscale"></a>向“超大规模”引入 X 数据量需要多少时间

“超大规模”每秒能够使用 100 MB 的新数据/更改的数据，但将数据移入 Azure SQL 数据库所需的时间也会受到可用网络吞吐量、源读取速度和目标数据库服务级别目标的影响。

### <a name="can-i-read-data-from-blob-storage-and-do-fast-load-like-polybase-in-sql-data-warehouse"></a>能否从 blob 存储读取数据并执行快速加载（如 SQL 数据仓库中的 Polybase）

可让客户端应用程序从 Azure 存储中读取数据并将数据加载到“超大规模”数据库（就像对任何其他 Azure SQL 数据库执行的操作一样）。 Azure SQL 数据库当前不支持 Polybase。 作为提供快速加载的替代方法，可以使用 [Azure 数据工厂](/data-factory/)。 SQL 的 Spark 连接器支持批量插入。

还可以使用 BULK INSERT 或 OPENROWSET 从 Azure Blob 存储批量读取数据：[批量访问 Azure Blob 存储中的数据的示例](https://docs.microsoft.com/sql/relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage?view=sql-server-2017#accessing-data-in-a-csv-file-referencing-an-azure-blob-storage-location)。

“超大规模”数据库中不支持简单恢复或批量日志记录模式。 提供高可用性和时点恢复需要完整恢复模式。 但是，相比于其他 Azure SQL 数据库服务层级而言，“超大规模”日志体系结构提供更佳的数据引入速率。

### <a name="does-hyperscale-allow-provisioning-multiple-nodes-for-parallel-ingesting-of-large-amounts-of-data"></a>“超大规模”是否允许预配多个节点，用于并行引入大量数据

否。 “超大规模”是对称多处理 (SMP) 体系结构，而不是大规模并行处理 (MPP) 或多主数据库体系结构。 只能创建多个副本来横向扩展只读工作负荷。

### <a name="what-is-the-oldest-sql-server-version-supported-for-migration-to-hyperscale"></a>支持迁移到“超大规模”的 SQL Server 最早版本是什么

SQL Server 2005。 有关详细信息，请参阅[迁移到单一数据库或共用数据库](sql-database-single-database-migrate.md#migrate-to-a-single-database-or-a-pooled-database)。 对于兼容性问题，请参阅[解决数据库迁移的兼容性问题](sql-database-single-database-migrate.md#resolving-database-migration-compatibility-issues)。

### <a name="does-hyperscale-support-migration-from-other-data-sources-such-as-amazon-aurora-mysql-postgresql-oracle-db2-and-other-database-platforms"></a>“超大规模”是否支持从其他数据源（如 Amazon Aurora、MySQL、PostgreSQL、Oracle、DB2）和其他数据库平台进行迁移

是的。 [Azure 数据库迁移服务](../dms/dms-overview.md)支持许多迁移方案。

## <a name="business-continuity-and-disaster-recovery-questions"></a>业务连续性和灾难恢复问题

### <a name="what-slas-are-provided-for-a-hyperscale-database"></a>对“超大规模”数据库提供的 SLA 是什么

请参阅 [Azure SQL 数据库的 SLA](https://www.azure.cn/zh-cn/support/sla/sql-data/)。 更多的次要计算副本可提高可用性，对于包含两个或更多个次要计算副本的数据库，SLA 高达 99.99%。

### <a name="are-the-database-backups-managed-for-me-by-the-azure-sql-database-service"></a>Azure SQL 数据库服务是否托管我的数据库备份

是的。

### <a name="how-often-are-the-database-backups-taken"></a>创建数据库备份的频率

“超大规模”数据库没有传统的完整、差异和日志备份。 但有数据文件的定期存储快照。 生成的日志只是按配置的保留期按原样保留，因此可以还原到保留期内的任意时间点。

### <a name="does-hyperscale-support-point-in-time-restore"></a>“超大规模”是否支持时间点还原

是的。

### <a name="what-is-the-recovery-point-objective-rporecovery-time-objective-rto-for-database-restore-in-hyperscale"></a>“超大规模”中的数据库还原的恢复点目标 (RPO)/恢复时间目标 (RTO) 是什么

无论数据库大小，RPO 最少为 0，RTO 目标不超过 10 分钟。 

### <a name="do-backups-of-large-databases-affect-compute-performance-on-my-primary"></a>大型数据库的备份是否影响主计算节点的计算性能

否。 备份由存储子系统管理，利用存储快照。 它们不会影响主计算节点上的用户工作负荷。

### <a name="can-i-perform-geo-restore-with-a-hyperscale-database"></a>能否对“超大规模”数据库执行异地还原

是的。  完全支持异地还原。

### <a name="can-i-set-up-geo-replication-with-hyperscale-database"></a>能否对“超大规模”数据库设置异地复制

目前没有。

### <a name="can-i-take-a-hyperscale-database-backup-and-restore-it-to-my-on-premises-server-or-on-sql-server-in-a-vm"></a>能否备份“超大规模”数据库，并还原到我的本地服务器或 VM 中的 SQL Server

否。 “超大规模”数据库的存储格式不同于任何 SQL Server 发行版，你不控制备份且无权访问备份。 若要将数据移出“超大规模”数据库，可以使用任何数据移动技术（例如 Azure 数据工厂、SSIS 等）提取数据。

## <a name="cross-feature-questions"></a>跨功能问题

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>迁移到“超大规模”服务层级后，是否会丢失一些功能

是的。 部分 Azure SQL 数据库功能不受支持，包括但不限于长期备份保留。 将数据库迁移到“超大规模”服务层级后，这些功能将停止运行。

### <a name="will-polybase-work-with-hyperscale"></a>Polybase 是否适用于“超大规模”

否。 Azure SQL 数据库不支持 Polybase。

### <a name="does-hyperscale-have-support-for-r-and-python"></a>“超大规模”是否支持 R 和 Python

否。 Azure SQL 数据库中不支持 R 和 Python。

### <a name="are-compute-nodes-containerized"></a>计算节点是否是容器化的

否。 “超大规模”进程在 [Service Fabric](/service-fabric/) 节点 (VM) 上运行，而不是在容器中运行。

## <a name="performance-questions"></a>性能问题

### <a name="how-much-write-throughput-can-i-push-in-a-hyperscale-database"></a>可以在“超大规模”数据库中推送多少写入吞吐量

对于任何“超大规模”计算大小，事务日志吞吐量限制设置为 100 MB/秒。 实现此速率的能力取决于多种因素，包括但不限于工作负荷类型、客户端配置，以及在主要计算副本上提供足够的计算容量来以此速率生成日志。

### <a name="how-many-iops-do-i-get-on-the-largest-compute"></a>在最大的计算节点上可以获得多少 IOPS

IOPS 和 IO 延迟根据工作负荷模式而异。 如果访问的数据缓存在计算副本上，则它的 IO 性能与使用本地 SSD 时相同。

### <a name="does-my-throughput-get-affected-by-backups"></a>吞吐量是否受备份影响

否。 计算节点与存储层分离。 这消除了备份对性能的影响。

### <a name="does-my-throughput-get-affected-as-i-provision-additional-compute-replicas"></a>当我预配其他计算副本时，吞吐量是否会受到影响

从技术上讲，由于存储已共享，并且主要计算副本和次要计算副本之间没有发生直接物理复制，添加次要副本不会影响主要副本上的吞吐量。 但我们可以限制连续主动写入工作负荷，从而允许日志应用到次要副本和页面服务器上，并避免次要副本上读取性能不佳。

## <a name="scalability-questions"></a>可伸缩性问题

### <a name="how-long-would-it-take-to-scale-up-and-down-a-compute-replica"></a>纵向扩展和减少计算副本需要多长时间

无论数据大小如何，纵向扩展或缩减计算节点都需要 5 到 10 分钟时间。

### <a name="is-my-database-offline-while-the-scaling-updown-operation-is-in-progress"></a>进行纵向扩展/缩减操作时，数据库是否处于脱机状态

否。 纵向扩展/缩减操作为联机操作。

### <a name="should-i-expect-connection-drop-when-the-scaling-operations-are-in-progress"></a>缩放操作过程中，连接是否会断开

如果在缩放操作结束时发生故障转移，纵向扩展或缩减操作会导致现有连接断开。 添加次要副本不会导致连接断开。

### <a name="is-the-scaling-up-and-down-of-compute-replicas-automatic-or-end-user-triggered-operation"></a>纵向扩展和缩减计算副本是自动的操作还是由最终用户触发的操作

是由最终用户触发的。 不是自动的。  

### <a name="does-the-size-of-my-tempdb-database-also-grow-as-the-compute-is-scaled-up"></a>计算纵向扩展时，`tempdb` 数据库大小是否也会随之增长

是的。 `tempdb` 数据库会随着计算自动纵向扩展。  

### <a name="can-i-provision-multiple-primary-compute-replicas-such-as-a-multi-master-system-where-multiple-primary-compute-heads-can-drive-a-higher-level-of-concurrency"></a>能否预配多个主要计算副本（例如多主数据库系统，其中多个主要计算标头可以驱动更高的并发级别）？

否。 只有主要计算副本接受读/写请求。 次要计算副本只接受只读请求。

## <a name="read-scale-out-questions"></a>读取扩展问题

### <a name="how-many-secondary-compute-replicas-can-i-provision"></a>可以预配多少个次要计算副本

默认情况下，我们将为“超大规模”数据库创建一个辅助副本。 若要调整副本数目，可以使用 [Azure 门户](https://portal.azure.cn)或 [REST API](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)。

### <a name="how-do-i-connect-to-these-secondary-compute-replicas"></a>如何连接到这些次要计算副本

将连接字符串的 `ApplicationIntent` 参数设置为 `ReadOnly`，即可连接到这些额外的只读计算副本。 任何标记为 `ReadOnly` 的连接均自动路由到某个额外的只读计算副本。  

### <a name="how-do-i-validate-if-i-have-successfully-connected-to-secondary-compute-replica-using-ssms-or-other-client-tools"></a>如何使用 SSMS 或其他客户端工具验证是否已成功连接到辅助计算副本？

可执行以下 T-SQL 查询：`SELECT DATABASEPROPERTYEX ('<database_name>', 'Updateability')`。
如果已连接到只读辅助副本，则结果为 `READ_ONLY`；如果已连接到主要副本，则结果为 `READ_WRITE`。 请注意，必须将数据库上下文设置为“超大规模”数据库的名称，而不能设置为 `master` 数据库的名称。

### <a name="can-i-create-a-dedicated-endpoint-for-a-read-scale-out-replica"></a>能否为读取扩展副本创建专用终结点

否。 只能通过指定 `ApplicationIntent=ReadOnly` 连接到读取扩展副本。

### <a name="does-the-system-do-intelligent-load-balancing-of-the-read-workload"></a>系统是否对读取工作负荷进行智能负载均衡

否。 具有只读意向的连接将重定向到任意读取扩展副本。

### <a name="can-i-scale-updown-the-secondary-compute-replicas-independently-of-the-primary-replica"></a>能否独立于主要副本纵向扩展/缩减次要计算副本

否。 辅助计算副本也用作高可用性故障转移目标，因此它们的配置需要与主要副本相同，才能在故障转移后提供预期的性能。

### <a name="do-i-get-different-tempdb-sizing-for-my-primary-compute-and-my-additional-secondary-compute-replicas"></a>对于我的主要计算副本和额外的次要计算副本，能否获得不同的 `tempdb` 大小

否。 `tempdb` 数据库根据计算大小预配进行配置，辅助计算副本的大小与主要计算副本相同。

### <a name="can-i-add-indexes-and-views-on-my-secondary-compute-replicas"></a>能否对次要计算副本添加索引和视图

否。 “超大规模”数据库有共享存储，这表示所有计算副本会看到相同的表格、索引和视图。 如果要使次要副本上的额外索引得到读取优化，必须在主要副本上添加索引。

### <a name="how-much-delay-is-there-going-to-be-between-the-primary-and-secondary-compute-replicas"></a>主要和次要计算副本之间的延迟是多少

从主要副本上提交事务的时间起，具体取决于当前日志生成速率，可能是瞬时的，也可能为低延迟（毫秒计）。

## <a name="next-steps"></a>后续步骤

有关“超大规模”服务层级的详细信息，请参阅[“超大规模”服务层级](sql-database-service-tier-hyperscale.md)。
