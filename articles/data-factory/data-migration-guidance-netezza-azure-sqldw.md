---
title: 使用 Azure 数据工厂将数据从本地 Netezza 服务器迁移到 Azure | Microsoft Docs
description: 使用 Azure 数据工厂将数据从本地 Netezza 服务器迁移到 Azure。
services: data-factory
documentationcenter: ''
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 9/03/2019
ms.date: 10/14/2019
ms.openlocfilehash: 68bcd5dfb3081ae87db11c1ee84f133f58ddc5be
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275902"
---
# <a name="use-azure-data-factory-to-migrate-data-from-on-premises-netezza-server-to-azure"></a>使用 Azure 数据工厂将数据从本地 Netezza 服务器迁移到 Azure 

Azure 数据工厂提供高性能、稳健且经济高效的机制用于将数据从本地 Netezza 服务器大规模迁移到 Azure 存储或 Azure SQL 数据仓库。 本文提供面向数据工程师和开发人员的以下信息：

> [!div class="checklist"]
> * 性能 
> * 复制复原能力
> * 网络安全性
> * 高级解决方案体系结构 
> * 有关实现的最佳做法  

## <a name="performance"></a>性能

Azure 数据工厂提供一个可在不同级别实现并行度的无服务器体系结构，使开发人员能够生成管道，以充分利用网络带宽和数据库带宽将环境的数据移动吞吐量最大化。

![性能](media/data-migration-guidance-netezza-azure-sqldw/performance.png)

- 单个复制活动可以利用可缩放的计算资源：使用 Azure Integration Runtime 时，能够以无服务器方式为每个复制活动指定[最多 256 个 DIU](/data-factory/copy-activity-performance#data-integration-units)；使用自承载集成运行时时，可以手动纵向扩展计算机或横向扩展为多个计算机（[最多 4 个节点](/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)），单个复制活动将在所有节点之间分布其分区。 
- 单个复制活动使用多个线程读取和写入数据存储。 
- Azure 数据工厂控制流可以同时启动多个复制活动（例如，使用 [For Each 循环](/data-factory/control-flow-for-each-activity)）。 

可以在[复制活动性能指南](/data-factory/copy-activity-performance)中获取更多详细信息

## <a name="resilience"></a>复原能力

在单个复制活动运行中，Azure 数据工厂具有内置的重试机制，因此，它可以处理数据存储或底层网络中特定级别的暂时性故障。

在数据源与接收器数据存储之间复制数据时，还可通过 Azure 数据工厂复制活动提供的两种方式处理不兼容的行。 遇到不兼容的数据时，可以中止复制活动并使其失败，或者，可以通过跳过不兼容的数据行来继续复制剩余的数据。 此外，还可以在 Azure Blob 存储中记录不兼容的行，以了解失败的原因，修复数据源中的数据，并重试复制活动。

## <a name="network-security"></a>网络安全性 

默认情况下，Azure 数据工厂通过 HTTPS 协议使用加密的连接将数据从本地 Netezza 服务器传输到 Azure 存储或 Azure SQL 数据仓库。 它提供传输中数据加密，并可防止窃听和中间人攻击。

如果你不希望通过公共 Internet 传输数据，可以通过 Azure Express Route 使用专用对等互连链路传输数据，以此实现更高的安全性。 请参阅下面的解决方案体系结构来了解如何实现此目的。

## <a name="solution-architecture"></a>解决方案体系结构

通过公共 Internet 迁移数据：

![solution-architecture-public-network](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-public-network.png)

- 在此体系结构中，将通过公共 Internet 使用 HTTPS 安全传输数据。
- 需要在企业防火墙后面的 Windows 计算机上安装 Azure 数据工厂自承载集成运行时才能实现此体系结构。 确保 Windows 计算机上的 Azure 数据工厂自承载集成运行时可以直接访问你的 Netezza 服务器。 可以手动纵向扩展计算机或横向扩展为多个计算机，以充分利用网络带宽和数据存储带宽来复制数据。
- 可以使用此体系结构实现初始快照数据迁移和增量数据迁移。

通过专用链路迁移数据： 

![solution-architecture-private-network](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-private-network.png)

- 在此体系结构中，数据迁移是通过 Azure Express Route 使用专用对等互连链路完成的，因此，数据永远不会遍历公共 Internet。 
- 需要在 Azure 虚拟网络中的 Windows VM 上安装 Azure 数据工厂自承载集成运行时才能实现此体系结构。 可以手动纵向扩展 VM 或横向扩展为多个 VM，以充分利用网络带宽和数据存储带宽来复制数据。
- 可以使用此体系结构实现初始快照数据迁移和增量数据迁移。

## <a name="implementation-best-practices"></a>有关实现的最佳做法 

### <a name="authentication-and-credential-management"></a>身份验证和凭据管理 

- 若要对 Netezza 进行身份验证，可以使用[通过连接字符串进行的 ODBC 身份验证](/data-factory/connector-netezza#linked-service-properties)。 
- 支持使用多种身份验证类型连接到 Azure Blob 存储。  强烈建议使用 [Azure 资源的托管标识](/data-factory/connector-azure-blob-storage#managed-identity)：托管标识构建在 Azure AD 中自动管理的 Azure 数据工厂标识基础之上，使你无需在链接服务定义中提供凭据，即可配置管道。  或者，可以使用[服务主体](/data-factory/connector-azure-blob-storage#service-principal-authentication)、[共享访问签名](/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)或[存储帐户密钥](/data-factory/connector-azure-blob-storage#account-key-authentication)对 Azure Blob 存储进行身份验证。 
- 也支持使用多种身份验证类型连接到 Azure Data Lake Storage Gen2。  强烈建议使用 [Azure 资源的托管标识](/data-factory/connector-azure-data-lake-storage#managed-identity)，不过，也可以使用[服务主体](/data-factory/connector-azure-data-lake-storage#service-principal-authentication)或[存储帐户密钥](/data-factory/connector-azure-data-lake-storage#account-key-authentication)。 
- 也支持使用多种身份验证类型连接到 Azure SQL 数据仓库。 强烈建议使用 [Azure 资源的托管标识](/data-factory/connector-azure-sql-data-warehouse#managed-identity)，不过，也可以使用[服务主体](/data-factory/connector-azure-sql-data-warehouse#service-principal-authentication)或 [SQL 身份验证](/data-factory/connector-azure-sql-data-warehouse#sql-authentication)。
- 如果不使用 Azure 资源的托管标识，我们强烈建议[在 Azure Key Vault 中存储凭据](/data-factory/store-credentials-in-key-vault)，以便更轻松地集中管理和轮换密钥，而无需修改 Azure 数据工厂链接服务。

### <a name="initial-snapshot-data-migration"></a>初始快照数据迁移 

对于卷大小小于 100 GB 的小型表，或者可以在 2 小时内迁移到 Azure 的表，可使每个复制作业加载每个表的数据。 可以运行多个 Azure 数据工厂复制作业来同时加载不同的表，以获得更好的吞吐量。 

在每个复制作业中，还可以结合数据分区选项使用 [parallelCopies 设置](/data-factory/copy-activity-performance#parallel-copy)来运行并行查询并按分区复制数据，从而达到一定的并行度。 下面是可供选择的数据分区选项及其详细说明。
- 建议从数据切片开始，因为这更有效。  确保 parallelCopies 设置中的并行度数字小于 Netezza 服务器上的表中的数据切片分区总数。  
- 如果每个数据切片分区的卷大小仍然很大（例如，大于 10 GB），我们建议切换到动态范围分区，以便定义分区数目，并按分区列定义每个分区的卷大小（上限和下限）。

如果大型表的卷大小大于 100 GB，或者在 2 小时内无法迁移到 Azure，则我们建议按自定义查询将数据分区，然后使每个复制作业每次复制一个分区。 可以同时运行多个 Azure 数据工厂复制作业，以获得更好的吞吐量。 请注意，要使每个复制作业目标按自定义查询加载一个分区，仍可以通过数据切片或动态范围启用并行度，以提高吞吐量。 

如果网络或数据存储的暂时性问题导致任何复制作业失败，你可以重新运行失败的复制作业，以再次从表中加载特定的分区。 加载其他分区的所有其他复制作业不受影响。

将数据加载到 Azure SQL 数据仓库时，我们建议在复制作业中启用 Polybase，并使用 Azure Blob 存储作为暂存存储。

### <a name="delta-data-migration"></a>增量数据迁移 

可以使用架构中的时间戳列或递增键来标识表中的新行或更新的行，然后将最新的值作为高水印存储在外部表中，下次加载数据时，可以使用此外部表来筛选增量数据。 

不同的表可以使用不同的水印列来标识新行或更新的行。 建议创建一个外部控制表，其中的每行代表 Netezza 服务器上的一个表，并具有自身特定的水印列名称和高水印值。 

### <a name="self-hosted-integration-runtime-configuration-on-azure-vm-or-machine"></a>Azure VM 或计算机上的自承载集成运行时配置

假设你要将数据从 Netezza 服务器迁移到 Azure，无论 Netezza 服务器是在企业防火墙后的本地位置还是在 VNET 环境中，都需要在 Windows 计算机或 VM 上安装自承载集成运行时，因为它是移动数据的引擎。

- 每个计算机或 VM 的初始建议配置为 32 个 vCPU 和 128 GB 内存。 可以在数据迁移过程中持续监视 IR 计算机的 CPU 和内存利用率，以确定是否需要进一步扩展计算机来提高性能，或缩减计算机来节省成本。
- 还可以通过将最多 4 个节点关联到一个自承载 IR 进行横向扩展。 针对自承载 IR 运行的单个复制作业将利用所有 VM 节点来同时复制数据。 为实现高可用性，我们建议从 2 个 VM 节点着手，以避免在数据迁移过程中出现单一故障点。

### <a name="rate-limiting"></a>速率限制

最佳做法是使用有代表性的示例数据集执行性能 POC，以便能够确定每个复制活动的适当分区大小。 建议在 2 小时内将每个分区加载到 Azure。  

首先在一个自承载 IR 计算机上使用单个复制活动来复制表。 根据表中的数据切片分区数逐渐增大 parallelCopies 设置，然后看看是否根据复制作业的吞吐量，在 2 小时内将整个表加载到 Azure。 

如果无法做到这一点，并且未充分利用自承载 IR 节点和数据存储的容量，请逐渐增大并发复制活动的数目，直到达到数据存储的网络或带宽限制。 

持续监视自承载 IR 计算机上的 CPU 和内存利用率，如果看到 CPU 和内存已充分利用，请准备好横向扩展计算机或横向扩展为多个计算机。 

遇到 Azure 数据工厂复制活动报告的限制错误时，请在 Azure 数据工厂中减小并发性或 parallelCopies 设置，或考虑提高网络和数据存储的带宽/IOPS 限制。 


### <a name="estimating-price"></a>估算价格 

考虑构建以下管道用于将数据从本地 Netezza 服务器迁移到 Azure SQL 数据仓库：

![pricing-pipeline](media/data-migration-guidance-netezza-azure-sqldw/pricing-pipeline.png)

假设条件如下： 

- 总数据量为 50 TB。 
- 使用第一个解决方案体系结构迁移数据（Netezza 服务器位于防火墙后的本地位置）
- 50 TB 划分为 500 个分区，每个复制活动移动一个分区。
- 为每个复制活动配置了一个针对 4 台计算机的自承载 IR，可实现 20-MBps 的吞吐量。 （在复制活动中，parallelCopies 设置为 4，用于从表中加载数据的每个线程可实现 5-MBps 的吞吐量）
- ForEach 并发性设置为 3，聚合吞吐量为 60 MBps。
- 完成迁移总共需要花费 243 小时。

下面是根据上述假设的估算出的价格： 

![pricing-table](media/data-migration-guidance-netezza-azure-sqldw/pricing-table.png)

> [!NOTE]
> 这是一个虚构的定价示例。 实际价格取决于环境中的实际吞吐量。 不包括 Windows 计算机（装有自承载集成运行时）的价格。 

### <a name="additional-references"></a>其他参考 
- [使用 Azure 数据工厂将数据从本地关系数据仓库迁移到 Azure](https://azure.microsoft.com/mediahandler/files/resourcefiles/data-migration-from-on-premise-relational-data-warehouse-to-azure-data-lake-using-azure-data-factory/Data_migration_from_on-prem_RDW_to_ADLS_using_ADF.pdf)
- [Netezza 连接器](/data-factory/connector-netezza)
- [ODBC 连接器](/data-factory/connector-odbc)
- [Azure Blob 存储连接器](/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage Gen2 连接器](/data-factory/connector-azure-data-lake-storage)
- [Azure SQL 数据仓库连接器](/data-factory/connector-azure-sql-data-warehouse)
- [复制活动性能和优化指南](/data-factory/copy-activity-performance)
- [创建和配置自承载集成运行时](/data-factory/create-self-hosted-integration-runtime)
- [自承载集成运行时的高可用性和可伸缩性](/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [数据移动安全注意事项](/data-factory/data-movement-security-considerations)
- [在 Azure Key Vault 中存储凭据](/data-factory/store-credentials-in-key-vault)
- [以增量方式从一个表复制数据](/data-factory/tutorial-incremental-copy-portal)
- [以增量方式从多个表复制数据](/data-factory/tutorial-incremental-copy-multiple-tables-portal)
- [Azure 数据工厂定价页](https://azure.cn/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>后续步骤

- [使用 Azure 数据工厂复制多个容器中的文件](solution-template-copy-files-multiple-containers.md)