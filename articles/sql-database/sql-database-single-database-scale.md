---
title: 缩放单一数据库资源 - Azure SQL 数据库 | Microsoft Docs
description: 本文介绍如何在 Azure SQL 数据库中缩放适用于单一数据库的计算和存储资源。
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 09/28/2018
ms.date: 10/29/2018
ms.openlocfilehash: b21483aa66d7868f7faca1f846951a2789af776f
ms.sourcegitcommit: b8f95f5d6058b1ac1ce28aafea3f82b9a1e9ae24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "50135859"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>在 Azure SQL 数据库中缩放单一数据库资源

本文介绍如何在 Azure SQL 数据库中缩放适用于单一数据库的计算和存储资源。 

## <a name="vcore-based-purchasing-model-change-storage-size"></a>基于 vCore 的购买模型：更改存储大小

- 可以使用 1GB 作为增量，将存储预配到最大大小限制。 最小可配置数据存储为 5GB 
- 可通过 [Azure 门户](https://portal.azure.cn)、[Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1)、[PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase)、[Azure CLI](/cli/sql/db#az-sql-db-update) 或 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update) 为单一数据库增加或减少大小上限，以预配存储。
- SQL 数据库会自动为日志文件额外分配 30% 的存储，并为 TempDB 的每个 vCore 分配 32GB，但不会超过 384GB。 TempDB 位于所有服务层中的附加 SSD 上。
- 单一数据库的存储价格等于数据存储与日志存储量之和乘以服务层的存储单价。 vCore 价格已包括 TempDB 费用。 有关额外存储价格的详细信息，请参阅 [SQL 数据库定价](https://azure.cn/pricing/details/sql-database/)。

> [!IMPORTANT]
> 在某些情况下，可能需要收缩数据库来回收未使用的空间。 有关详细信息，请参阅[管理 Azure SQL 数据库中的文件空间](sql-database-file-space-management.md)。

## <a name="vcore-based-purchasing-model-change-compute-resources"></a>基于 vCore 的购买模型：更改计算资源

最初选择 vCore 数量后，可以使用 [Azure 门户](sql-database-single-databases-manage.md#manage-an-existing-sql-server)、[Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1)、 [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase)、[Azure CLI](/cli/sql/db#az-sql-db-update) 或 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update)，根据实际体验动态扩展或缩减单一数据库。 

更改数据库的服务层和/或计算大小将以新的计算大小创建原始数据库的副本，并将连接切换到副本。 当我们切换到副本时，在此过程中不会丢失任何数据，但在短暂的瞬间，将禁用与数据库的连接，因此可能回滚某些处于进行状态的事务。 用于切换的时间长度因情况而异，但通常为 4 秒以下，并且 99% 的情况下少于 30 秒。 如果在禁用连接的那一刻有大量的事务正在进行，则用于切换的时间长度可能会更长。 

整个扩展过程的持续时间同时取决于更改前后数据库的大小和服务层。 例如，一个正在更改到标准服务层、从“常规用途”服务层更改或在标准服务层内更改的 250 GB 的数据库应在六小时内完成。 如果数据库与正在“业务关键”服务层内更改计算大小的大小相同，应在三小时内完成扩展。

> [!TIP]
> 若要监视正在进行的操作，请参阅：[使用 SQL REST API 管理操作](https://docs.microsoft.com/rest/api/sql/Operations/List)、[使用 CLI 管理操作](/cli/sql/db/op)、[使用 T-SQL 监视操作](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database)和以下两个 PowerShell 命令：[Get-AzureRmSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) 和 [Stop-AzureRmSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity)。

* 如果要升级到更高的服务层或计算大小，除非显式指定了更大的大小（最大），否则，最大数据库大小不会增大。
* 若要对数据库进行降级，数据库所用空间必须小于目标服务层和计算大小允许的最大大小。 
* 在启用了[异地复制](sql-database-geo-replication-portal.md)的情况下升级数据库时，请先将其辅助数据库升级到所需的服务层和计算大小，然后再升级主数据库（用于实现最佳性能的常规指南）。 在升级到另一版本时，必须首先升级辅助数据库。
* 在启用了[异地复制](sql-database-geo-replication-portal.md)的情况下降级数据库时，请先将其主数据库降级到所需的服务层和计算大小，然后再降级辅助数据库（用于实现最佳性能的常规指南）。 在降级到另一版本时，必须首先降级主数据库。
* 更改完成前不会应用数据库的新属性。

## <a name="dtu-based-purchasing-model-change-storage-size"></a>基于 DTU 的购买模型：更改存储大小

- 单一数据库的 DTU 价格附送了一定容量的存储，无需额外费用。 超出附送的量后，可花费额外的费用预配额外的存储，但不能超过存储上限，不超过 1 TB 时，以 250 GB 为增量进行预配，超出 1 TB 时，以 256 GB 为增量进行预配。 有关包括的存储量和大小上限，请参阅[单一数据库：存储大小和计算大小](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)。
- 可通过 Azure 门户、[Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1)、[PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase)、[Azure CLI](/cli/sql/db#az-sql-db-update) 或 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update) 为单一数据库增加大小上限，以预配额外存储。
- 单一数据库的额外存储价格等于额外存储量乘以服务层的额外存储单价。 有关额外存储价格的详细信息，请参阅 [SQL 数据库定价](https://azure.cn/pricing/details/sql-database/)。

> [!IMPORTANT]
> 在某些情况下，可能需要收缩数据库来回收未使用的空间。 有关详细信息，请参阅[管理 Azure SQL 数据库中的文件空间](sql-database-file-space-management.md)。

## <a name="dtu-based-purchasing-model-change-compute-resources-dtus"></a>基于 DTU 的购买模型：更改计算资源 (DTU)

首先选择服务层、计算大小和存储量，然后使用 Azure 门户、[Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1)、[PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase)、[Azure CLI](/cli/sql/db#az-sql-db-update) 或 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update)，根据实际体验动态扩展或缩减单一数据库。 

下面的视频演示了如何动态更改服务层和计算大小以增加单一数据库的可用 DTU。

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

更改数据库的服务层和/或计算大小将以新的计算大小创建原始数据库的副本，并将连接切换到副本。 当我们切换到副本时，在此过程中不会丢失任何数据，但在短暂的瞬间，将禁用与数据库的连接，因此可能回滚某些处于进行状态的事务。 用于切换的时间长度因情况而异，但 99% 的情况下少于 30 秒。 如果在禁用连接的那一刻有大量的事务正在进行，则用于切换的时间长度可能会更长。 

整个扩展过程的持续时间同时取决于更改前后数据库的大小和服务层。 例如，一个正在更改到标准服务层、从标准服务层更改或在标准服务层内更改的 250 GB 的数据库应在六小时内完成。 如果数据库与正在高级服务层内更改计算大小的大小相同，应在三小时内完成扩展。

> [!TIP]
> 若要监视正在进行的操作，请参阅：[使用 SQL REST API 管理操作](https://docs.microsoft.com/rest/api/sql/Operations/List)、[使用 CLI 管理操作](/cli/sql/db/op)、[使用 T-SQL 监视操作](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database)和以下两个 PowerShell 命令：[Get-AzureRmSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) 和 [Stop-AzureRmSqlDatabaseActivity](https://docs.microsoft.com/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity)。

* 如果要升级到更高的服务层或计算大小，除非显式指定了更大的大小（最大），否则，最大数据库大小不会增大。
* 若要对数据库进行降级，数据库所用空间必须小于目标服务层和计算大小允许的最大大小。 
* 从高级层降级至标准层时，如果同时满足 (1) 目标计算大小支持该数据库的最大大小，(2) 最大大小超出目标计算大小包括的存储量，那么将产生额外存储费用。 例如，如果将最大大小为 500 GB 的 P1 数据库缩小至 S3，那么将产生额外的存储费用，因为 S3 支持的最大大小为 500 GB，而它的附送存储量仅为 250 GB。 因此，额外的存储量为 500 GB - 250 GB = 250 GB。 有关额外存储定价的信息，请参阅 [SQL 数据库定价](https://azure.cn/pricing/details/sql-database/)。 如果实际使用的空间量小于附送的存储量，只要将数据库最大大小减少到附送的量，就能避免此项额外费用。 
* 在启用了[异地复制](sql-database-geo-replication-portal.md)的情况下升级数据库时，请先将其辅助数据库升级到所需的服务层和计算大小，然后再升级主数据库（用于实现最佳性能的常规指南）。 在升级到另一版本时，必须首先升级辅助数据库。
* 在启用了[异地复制](sql-database-geo-replication-portal.md)的情况下降级数据库时，请先将其主数据库降级到所需的服务层和计算大小，然后再降级辅助数据库（用于实现最佳性能的常规指南）。 在降级到另一版本时，必须首先降级主数据库。
* 各服务层的还原服务不同。 如果要降级到基本层，则备份保留期也将减少 - 请参阅 [Azure SQL 数据库备份](sql-database-automated-backups.md)。
* 更改完成前不会应用数据库的新属性。

## <a name="next-steps"></a>后续步骤

有关总体资源限制，请参阅 [SQL 数据库基于 vCore 的资源限制 - 单一数据库](sql-database-vcore-resource-limits-single-databases.md)和 [SQL 数据库基于 DTU 的资源限制 - 弹性池](sql-database-dtu-resource-limits-single-databases.md)。
