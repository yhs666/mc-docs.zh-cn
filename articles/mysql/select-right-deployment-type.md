---
title: 为 Azure Database for MySQL 选择适当的部署类型
description: 本文介绍将 Azure Database for MySQL 部署为基础结构即服务 (IaaS) 或平台即服务 (PaaS) 之前应考虑的因素。
author: WenJason
ms.author: v-jay
ms.service: mysql
ms.topic: conceptual
origin.date: 08/05/2019
ms.date: 11/04/2019
ms.openlocfilehash: 62fe91412db4df1daceca160509c42f243678e6e
ms.sourcegitcommit: cb2caa72ec0e0922a57f2fa1056c25e32c61b570
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142189"
---
# <a name="choose-the-right-mysql-server-option-in-azure"></a>在 Azure 中选择适当的 MySQL Server 选项

> [!NOTE]
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql-database-on-azure/)。

在 Azure 中，MySQL 服务器工作负荷可以在托管的虚拟机基础结构即服务 (IaaS) 中运行，或者作为托管的平台即服务 (PaaS) 运行。 PaaS 提供多个部署选项，每个部署选项中有多个服务层级。 在 IaaS 和 PaaS 之间选择时，必须决定是要管理数据库、应用修补程序并进行备份，还是要将这些操作委托给 Azure。

做出决策时，请考虑以下选项：

- **Azure Database for MySQL**。 此选项是基于稳定 MySQL 社区版的完全托管式 MySQL 数据库引擎。 此关系数据库即服务 (DBaaS) 托管在 Azure 云平台中，属于 PaaS 行业类别。

  借助 Azure 上的 MySQL 托管实例，可以使用内置的功能，否则，如果 MySQL 服务器位于本地或 Azure VM 中，需要进行大量的配置。

  将 MySQL 用作服务时，需要预先支付纵向或横向扩展选项的费用才能在不中断服务的情况下获得更高的控制度。 与独立的 MySQL 服务器不同，Azure Database for MySQL 提供内置高可用性、智能和管理等附加功能。

- **Azure VM 上的 MySQL**。 此选项属于 IaaS 行业类别。 使用此服务可以在 Azure 云平台上的完全托管式虚拟机中运行 MySQL 服务器。 所有最新版本的 MySQL 都可以安装在 IaaS 虚拟机上。

  与 Azure Database for MySQL 相比，Azure VM 上的 MySQL 的最重要差别在于提供对数据库引擎的控制。 但是，获得这种控制的代价是需要自行管理 VM 和许多数据库管理 (DBA) 任务。 这些任务包括维护和修补数据库服务器、数据库恢复和高可用性设计。

下表列出了这些选项之间的主要差别：

|            | Azure Database for MySQL | Azure VM 上的 MySQL    |
|:-------------------|:-----------------------------|:--------------------|
| 服务级别协议 (SLA)                | 提供 99.99% 可用性 SLA| 同一可用性集中的两个或更多个实例的可用性高达 99.95%。<br/><br/>使用高级存储的单一实例 VM 的可用性为 99.9%。<br/><br/>请参阅[虚拟机 SLA](https://www.azure.cn/zh-cn/support/sla/virtual-machines/)。 |
| 操作系统修补        | 自动  | 由客户管理 |
| MySQL 修补     | 自动  | 由客户管理 |
| 高可用性 | 高可用性 (HA) 模型以节点级中断发生时的内置故障转移机制为依据。 在这种情况下，服务将自动创建一个新实例，并将存储附加到此实例。 | 客户建构、实施、测试和维护高可用性。 功能可能包括不中断的故障转移群集、不中断的组复制、日志传送或事务复制。|
| 混合场景 | 使用[数据传入复制](/mysql/concepts-data-in-replication)可将数据从外部 MySQL 服务器同步到 Azure Database for MySQL 服务中。 外部服务器可以处于本地、虚拟机中或是其他云提供商托管的数据库服务。<br/><br/> 使用[只读副本](/postgresql/concepts-read-replicas)功能可将 Azure Database for MySQL 主服务器中的数据复制到最多五个只读副本服务器。 副本位于同一个 Azure 区域中，或者跨不同的区域。 使用 binlog 复制技术异步更新只读副本。<br/><br/>跨区域读取复制目前为公共预览版。| 由客户管理
| 备份和还原 | 自动创建[服务器备份](/mysql/concepts-backup#backups)并将其存储在用户配置的本地冗余或异地冗余存储中。 服务将创建完整备份、差异备份和事务日志备份 | 由客户管理 |
| 监视数据库操作 | 可让客户针对数据库操作[设置警报](/mysql/concepts-monitoring)，并在即将达到阈值时采取措施。 | 由客户管理 |
| 灾难恢复 | 将自动创建的备份存储在用户配置的[本地冗余存储或异地冗余存储](/mysql/howto-restore-server-portal)中。 备份还可以将服务器还原到某个时间点。 保留期为 7 到 35 天。 还原是使用 Azure 门户完成的。 | 完全由客户管理。 责任包括但不限于计划、测试、存档、存储和保留。 另一个选项是使用 Azure 恢复服务保管库备份 Azure VM 和 VM 上的数据库。 此选项目前为预览版。 |

## <a name="business-motivations-for-choosing-paas-or-iaas"></a>选择 PaaS 或 IaaS 的业务动机

有多个因素可能会影响你决定选择 PaaS 或 IaaS 来托管 MySQL 数据库。

### <a name="cost"></a>成本

资金限制通常是确定数据库最佳托管解决方案的首要考虑因素。 无论你是现金不足的创业公司，或是在预算严格受限的情况下运作现有公司的团队，都存在这种情况。 本部分介绍 Azure 中适用于 Azure Database for MySQL 和 Azure VM 上的 MySQL 的计费与许可基础知识。

#### <a name="billing"></a>计费

Azure Database for MySQL 目前在多个层级中以服务的形式提供，它资源价格各不相同。 所有资源都按固定费率按小时计费。 有关目前支持的服务层级、计算大小和存储量的最新信息，请参阅[基于 vCore 的购买模型](/mysql/concepts-pricing-tiers)。 可以动态调整服务层级和计算大小，以满足应用程序的不同吞吐量需求。 你需要按一般的[数据传输费率](https://azure.cn/pricing/details/data-transfer/)支付 Internet 流量传出费用。

Azure 使用 Azure Database for MySQL 自动配置、修补和升级数据库软件。 这些自动化操作可以降低管理成本。 此外，Azure Database for MySQL 提供[内置备份](/mysql/concepts-backup)功能。 这些功能可帮助你大幅节省成本，尤其是存在大量的数据库时。 相比之下，对于 Azure VM 上的 MySQL，可以选择并运行任何 MySQL 版本。 无论使用何种 MySQL 版本，都需要为预配的 VM 以及使用的特定 MySQL 许可证类型付费。

Azure Database for MySQL 针对任何类型的节点级中断提供内置高可用性，同时仍可为服务维护 99.99% 的 SLA 保证。 但是，对于 VM 中的数据库高可用性，客户应使用可对 MySQL 数据库使用的高可用性选项，例如 [MySQL 复制](https://dev.mysql.com/doc/refman/8.0/en/replication.html)。 使用支持的高可用性选项不会提供额外的 SLA。 但是，它可以让你凭借额外的成本和管理开销实现 99.99% 以上的数据库可用性。

有关定价的详细信息，请参阅以下文章：
* [Azure Database for MySQL 定价](https://azure.cn/pricing/details/mysql/)
* [虚拟机定价](https://azure.cn/pricing/details/virtual-machines/)
* [Azure 定价计算器](https://azure.cn/pricing/calculator/)

### <a name="administration"></a>管理

对许多企业来说，决定过渡到到云服务的关键在于降低管理复杂度，因为这涉及到成本。 借助 IaaS 和 PaaS，Azure 可以：

- 管理底层基础结构。
- 自动复制所有数据以提供灾难恢复。
- 配置和升级数据库软件。
- 管理负载均衡。
- 发生服务器故障时执行透明的故障转移。

以下列表描述了每个选项的管理注意事项：

* 使用 Azure Database for MySQL 可以持续管理数据库。 但是，不再需要管理数据库引擎、操作系统或硬件。 可以持续管理的项的示例包括：

  - 数据库
  - 登录
  - 索引优化
  - 查询优化
  - 审核
  - 安全性

  此外，在另一个数据中心配置高可用性只需极少量的配置或管理，或者根本无需配置或管理。

* 使用 Azure VM 上的 MySQL，可以完全掌控操作系统和 MySQL 服务器实例配置。 在 VM 上，可以决定何时更新或升级操作系统与数据库软件。 还可以决定何时安装任何其他软件，例如防病毒应用程序。 提供的某些自动化功能可以大大简化修补、备份和高可用性。 可以控制 VM 的大小、磁盘数目及其存储配置。 有关详细信息，请参阅 [Azure 的虚拟机和云服务大小](/virtual-machines/windows/sizes)。

### <a name="time-to-move-to-azure"></a>迁移到 Azure 的时机

* 当开发人员工作效率和新解决方案的快速面市时间至关重要时，Azure Database for MySQL 是面向云的应用程序的适当解决方案。 该服务提供类似于 DBA 的编程功能，非常适合云架构师和开发人员，因为它能降低管理底层操作系统和数据库的需求。

* 如果你想要避免购置新本地硬件所要花费的时间和费用，Azure VM 上的 MySQL 是适用于需要 MySQL 数据库或者要访问 Windows 或 Linux 中的 MySQL 功能的应用程序的适当解决方案。 如果 Azure Database for MySQL 不合适，则此解决方案也很适合将现有的本地应用程序和数据库按原样迁移到 Azure。

  由于无需更改呈现层、应用层和数据层，重新架构现有解决方案时可以节省时间和预算。 你可以专注于将所有解决方案迁移到 Azure，并执行 Azure 平台可能需要的某些性能优化。

## <a name="next-steps"></a>后续步骤

* 参阅 [Azure Database for MySQL 定价](https://azure.cn/pricing/details/MySQL/)。
* 从[创建第一个服务器](/MySQL/quickstart-create-MySQL-server-database-using-azure-portal)开始。
