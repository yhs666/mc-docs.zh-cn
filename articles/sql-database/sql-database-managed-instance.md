---
title: Azure SQL 数据库托管实例概述 | Microsoft Docs
description: 本主题介绍 Azure SQL 数据库托管实例，解释其工作原理，并说明它与 Azure SQL 数据库中的单一数据库的差别。
services: sql-database
author: WenJason
ms.reviewer: carlrab
manager: digimobile
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: DBs & servers
ms.topic: conceptual
origin.date: 09/14/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.openlocfilehash: ee40dafff2dbe3b60ba91312341a5af732e11841
ms.sourcegitcommit: d8b4e1fbda8720bb92cc28631c314fa56fa374ed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913816"
---
# <a name="what-is-a-managed-instance-preview"></a>什么是托管实例（预览版）？

Azure SQL 数据库托管实例（预览版）是 Azure SQL 数据库的一个新部署模型，几乎与最新的 SQL Server 本地 (Enterprise Edition) 数据库引擎完全兼容。它提供一个本机[虚拟网络 (VNet)](../virtual-network/virtual-networks-overview.md) 实现来解决常见的安全问题，并提供本地 SQL Server 客户惯用的[业务模型](https://azure.cn/pricing/details/sql-database/)。 托管实例允许现有 SQL Server 客户将其本地应用程序即时转移到云中，而只需对应用程序和数据库做出极少量的更改。 同时，托管实例保留了所有 PaaS 功能（自动修补和版本更新、[自动备份](sql-database-automated-backups.md)、[高可用性](sql-database-high-availability.md)），可大幅降低管理开销和总拥有成本。

> [!IMPORTANT]
> 有关目前支持托管实例的区域列表，请参阅[使用 Azure SQL 数据库托管实例将数据库迁移到完全托管的服务](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/)。
 
下图概括描绘了托管实例的主要功能：

![主要功能](./media/sql-database-managed-instance/key-features.png)

在正式版推出之前，托管实例旨在通过分阶段的发布计划，实现外围应用与最新本地 SQL Server 版本的近乎 100% 的兼容性。 

若要在 Azure SQL 数据库单一数据库和虚拟机中托管的 SQL Server IaaS 之间作出抉择，请参阅[如何在 Azure 云中选择合适的 SQL Server 版本](sql-database-paas-vs-sql-server-iaas.md)。

## <a name="vcore-based-purchasing-model"></a>基于 vCore 的购买模型

[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)提供了灵活性、控制力和透明性，并且还提供了一种简单明了的方法来将本地工作负荷要求转换到云。 此模型允许根据工作负荷需求来缩放计算、内存和存储资源。 此外，借助[面向 SQL Server 的 Azure 混合使用权益](../virtual-machines/windows/hybrid-use-benefit-licensing.md)，vCore 模型能够节省高达 30% 的费用。

虚拟核心表示逻辑 CPU，提供不同代的硬件供客户选择。
- 第 4 代逻辑 CPU 基于 Intel E5-2673 v3 (Haswell) 2.4 GHz 处理器。
- 第 5 代逻辑 CPU 基于 Intel E5-2673 v4 (Broadwell) 2.3 GHz 处理器。

下表可帮助你了解如何选择计算、内存、存储和 I/O 资源的最佳配置。

||第 4 代|第 5 代|
|----|------|-----|
|硬件|Intel E5-2673 v3 (Haswell) 2.4 GHz 处理器、附加的 SSD vCore = 1 PP（物理核心）|Intel E5-2673 v4 (Broadwell) 2.3 GHz 处理器、快速 eNVM SSD、vCore=1 LP（超线程）|
|计算大小|8、16、24 个 vCore|8、16、24、32、40、64、80 个 vCore|
|内存|每个 vCore 7 GB|每个 vCore 5.5 GB|
||||

## <a name="managed-instance-service-tiers"></a>托管实例服务层

托管实例可在两个服务层中提供：
- **常规用途**：适用于具有典型性能和 IO 延迟要求的应用程序。
- **业务关键**：适用于具有低 IO 延迟要求，对工作负荷中基础维护操作影响最低的应用程序。

这两个服务层保证 99.99% 的可用性，可让你独立选择存储大小和计算容量。 有关 Azure SQL 数据库高可用性体系结构的详细信息，请参阅[高可用性和 Azure SQL 数据库](sql-database-high-availability.md)。

> [!IMPORTANT]
> 在公共预览版中，不支持在常规用途和业务关键服务层之间切换。 如果想要将数据库迁移到不同服务层中的实例，可以创建新实例，从原始实例对数据库进行时间点还原，然后删除不再需要的原始实例。 

### <a name="general-purpose-service-tier"></a>常规用途服务层

以下列表描述了常规用途服务层的主要特征： 

- 适用于具有典型性能要求的大多数业务应用程序 
- 高性能 Azure 高级存储 (8 TB) 
- 每个实例 100 个数据库 

以下列表概述了常规用途服务层的主要特征：

|功能 | 说明|
|---|---|
| vCore 数目* | 8、16、24（第 4 代）<br>8、16、24、32、40、64、80（第 5 代）|
| SQL Server 版本/内部版本 | SQL Server 数据库引擎（最新稳定版） |
| 最小存储大小 | 32 GB |
| 最大存储大小 | 8 TB |
| 每个数据库的最大存储 | 由每个实例的最大存储大小决定 |
| 预期的存储 IOPS | 每个数据文件 500-7500 IOPS（取决于数据文件）。 请参阅[高级存储](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) |
| 每个数据库的数据文件 (ROWS) 数目 | 多个 | 
| 每个数据库的日志文件 (LOG) 数目 | 1 | 
| 受管理的自动备份 | 是 |
| 高可用性 | 存储在 Azure 存储和 [Azure Service Fabric ](../service-fabric/service-fabric-overview.md) 中的数据 |
| 内置的实例和数据库监视与指标 | 是 |
| 自动软件修补 | 是 |
| VNet - Azure 资源管理器部署 | 是 |
| VNet - 经典部署模型 | 否 |
| 门户支持 | 是|
|||

\* 虚拟核心表示逻辑 CPU，提供不同代的硬件供客户选择。 第 4 代逻辑 CPU 基于 Intel E5-2673 v3 (Haswell) 2.4 GHz 处理器，第 5 代逻辑 CPU 基于 Intel E5-2673 v4 (Broadwell) 2.3 GHz 处理器。 

有关详细信息，请参阅 [Azure SQL 数据库中的标准/常规用途可用性和体系结构](sql-database-high-availability.md#standardgeneral-purpose-availability)。

### <a name="business-critical-service-tier"></a>“业务关键”服务层

业务关键服务层适用于具有高 IO 要求的应用程序。 它使用多个 Always On 独立副本，提供最高级别的故障恢复能力。 

以下列表概述了业务关键服务层的主要特征： 
-   为具有最严苛性能和 HA 要求的商业应用程序设计 
-   附带超高速 SSD 存储（第 4 代最多 1 TB，第 5 代最多 4 TB）
-   每个实例最多支持 100 个数据库 
- 内置附加的只读实例，可用于报告和其他只读工作负荷
- [内存中 OLTP](sql-database-in-memory.md)，可用于具有高性能要求的工作负荷  

|功能 | 说明|
|---|---|
| vCore 数目* | 8、16、24、32（第 4 代）<br>8、16、24、32、40、64、80（第 5 代）|
| SQL Server 版本/内部版本 | SQL Server（最新可用版本） |
| 其他功能 | [In-Memory OLTP](sql-database-in-memory.md)<br> 1 个额外的只读副本（[读取扩展](sql-database-read-scale-out.md)）
| 最小存储大小 | 32 GB |
| 最大存储大小 | 第 4 代：1 TB（所有 vCore 大小）<br> 第 5 代：<ul><li>8、16 个 Vcore 1 TB</li><li>24 个 Vcore 2 TB</li><li>32、40、64、80 个 Vcore 4 TB</ul>|
| 每个数据库的最大存储 | 由每个实例的最大存储大小决定 |
| 每个数据库的数据文件 (ROWS) 数目 | 多个 | 
| 每个数据库的日志文件 (LOG) 数目 | 1 | 
| 受管理的自动备份 | 是 |
| 高可用性 | 数据存储在本地 SSD 中并使用 [Always On 可用性组](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server)和 [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| 内置的实例和数据库监视与指标 | 是 |
| 自动软件修补 | 是 |
| VNet - Azure 资源管理器部署 | 是 |
| VNet - 经典部署模型 | 否 |
| 门户支持 | 是|
|||

有关详细信息，请参阅 [Azure SQL 数据库中的高级/业务关键可用性和体系结构](sql-database-high-availability.md#premiumbusiness-critical-availability)。

## <a name="advanced-security-and-compliance"></a>高级安全性和符合性 

### <a name="managed-instance-security-isolation-in-azure-china-cloud"></a>Azure 中国云中的托管实例安全隔离 

使用托管实例可以进一步实现与 Azure 云中其他租户的安全隔离。 安全隔离包括： 

- 使用 Azure Express Route 或 VPN 网关[实现本机虚拟网络](sql-database-managed-instance-vnet-configuration.md)并连接到本地环境 
- 仅通过专用 IP 地址公开 SQL 终结点，以便从专用 Azure 或混合网络建立安全连接
- 具有专用底层基础结构（计算、存储）的单一租户

下图概述了应用程序的各种连接选项： 

![高可用性](./media/sql-database-managed-instance/application-deployment-topologies.png)  

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory 集成和多重身份验证 

通过 SQL 数据库，可使用 [Azure Active Directory 集成](sql-database-aad-authentication.md)集中管理数据库用户和其他 Microsoft 服务的身份。 此功能简化了权限管理，增强了安全性。 Azure Active Directory 支持[多重身份验证](sql-database-ssms-mfa-authentication-configure.md) (MFA)，以便在支持单一登录进程的同时提高数据和应用程序安全性。 

### <a name="authentication"></a>身份验证 
SQL 数据库身份验证是指用户连接到数据库时如何证明其身份。 SQL 数据库支持两种类型的身份验证：  

- SQL 身份验证，使用用户名和密码。
- Azure Active Directory 身份验证，使用 Azure Active Directory 管理的标识，支持托管域和集成域。 

### <a name="authorization"></a>授权

授权是指用户可以在 Azure SQL 数据库中执行哪些操作，由用户帐户的数据库角色成员身份和对象级权限控制。 托管实例的授权功能与 SQL Server 2017 相同。 

## <a name="database-migration"></a>数据库迁移 

托管实例面向需要从本地或 IaaS 数据库实施项目迁移大量数据库的用户方案。 托管实例支持多个数据库迁移选项： 

### <a name="backup-and-restore"></a>备份和还原  

迁移方法利用从 SQL 到 Azure Blob 存储的备份。 可将 Azure 存储 Blob 中存储的备份直接还原到托管实例。 若要将现有 SQL 数据库还原到托管实例，可以：

- 使用 [T-SQL RESTORE 命令](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql)。 
  - 有关介绍如何还原 Wide World Importers - 标准数据库备份文件的教程，请参阅[将备份文件还原到托管实例](sql-database-managed-instance-restore-from-backup-tutorial.md)。 本教程介绍如何将备份文件上传到 Azure 博客存储并使用共享访问签名 (SAS) 密钥进行保护。
  - 有关从 URL 还原的信息，请参阅[从 URL 本机还原](sql-database-managed-instance-migrate.md#native-restore-from-url)。
  
### <a name="data-migration-service"></a>数据迁移服务

Azure 数据库迁移服务是一项完全托管的服务，旨在实现从多个数据库源到 Azure 数据平台的无缝迁移，并且最小化停机时间。 此服务简化了将现有第三方和 SQL Server 数据库移到 Azure 所要执行的任务。 公共预览版中的部署选项包括 Azure SQL 数据库、托管实例和 Azure VM 中的 SQL Server。

## <a name="sql-features-supported"></a>支持的 SQL 功能 

在服务正式版推出之前，托管实例旨在通过分阶段的计划，实现外围应用与本地 SQL Server 的近乎 100% 的兼容性。 有关功能和比较列表，请参阅 [SQL 数据库功能比较](sql-database-features.md)。
 
托管实例支持与 SQL 2008 数据库的向后兼容。 支持从 SQL 2005 数据库服务器直接迁移，迁移后的 SQL 2005 数据库的兼容级别将更新为 SQL 2008。 
 
下图概括描绘了托管实例中外围应用的兼容性：  

![迁移](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>本地 SQL Server 与托管实例之间的主要差异 

托管实例受益于云中的一贯最新状态，这意味着，本地 SQL Server 中的某些功能可能会过时、被弃用或被取代。 在某些情况下，当工具需要识别特定的功能是否以略微不同的方式工作或者服务是否不在某个环境中运行时，你无法完全控制这一点： 

- 高可用性是内置的，且是预先配置的。 Always On 高可用性功能的公开方式不同于 SQL IaaS 实施项目的类似功能 
- 自动备份和时间点还原。 客户可以启动 `copy-only` 备份，而不会干扰自动备份链。 
- 托管实例不允许指定完整的物理路径，因此，必须以不同的方式支持所有相应方案：RESTORE DB 不支持 WITH MOVE，CREATE DB 不允许物理路径，BULK INSERT 仅适用于 Azure Blob，等等。 
- 托管实例支持使用 [Azure AD 身份验证](sql-database-aad-authentication.md)作为 Windows 身份验证的云替代方法。 
- 对于包含内存中 OLTP 对象的数据库，托管实例会自动管理 XTP 文件组和文件

### <a name="managed-instance-administration-features"></a>托管实例管理功能  

托管实例可让系统管理员专注于业务中最重要的事务。 许多系统管理员/DBA 活动都是不需要的，或者很简单。 例如，OS/RDBMS 安装和修补、动态实例大小调整和配置、备份、数据库复制（包括系统数据库）、高可用性配置，以及运行状况和性能监视数据流的配置。 

> [!IMPORTANT]
> 有关支持、部分支持和不支持的功能列表，请参阅 [SQL 数据库功能](sql-database-features.md)。 有关托管实例与 SQL Server 的 T-SQL 差异列表，请参阅[托管实例与 SQL Server 的 T-SQL 差异](sql-database-managed-instance-transact-sql-information.md)

## <a name="next-steps"></a>后续步骤

- 有关功能和比较列表，请参阅 [SQL 常用功能](sql-database-features.md)。
- 有关 VNet 配置的详细信息，请参阅[托管实例 VNet 配置](sql-database-managed-instance-vnet-configuration.md)。
