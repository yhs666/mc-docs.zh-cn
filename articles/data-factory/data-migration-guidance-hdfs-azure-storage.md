---
title: 使用 Azure 数据工厂将数据从本地 Hadoop 群集迁移到 Azure 存储 | Microsoft Docs
description: 使用 Azure 数据工厂将数据从本地 Hadoop 群集迁移到 Azure 存储。
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
origin.date: 8/30/2019
ms.date: 10/14/2019
ms.openlocfilehash: 56e71bdfaeeec2c148b198ddb96bf567303bc286
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275905"
---
# <a name="use-azure-data-factory-to-migrate-data-from-on-premises-hadoop-cluster-to-azure-storage"></a>使用 Azure 数据工厂将数据从本地 Hadoop 群集迁移到 Azure 存储 

Azure 数据工厂提供高性能、稳健且经济高效的机制用于将数据从本地 HDFS 大规模迁移到 Azure Blob 存储或 Azure Data Lake Storage Gen2。 简单而言，Azure 数据工厂提供两种方法用于将数据从本地 HDFS 迁移到 Azure。 你可以根据自己的情况选择所需的方法。 

- ADF DistCp 模式。 ADF 支持使用 [DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) 将文件按原样复制到 Azure Blob（包括[分阶段复制](/data-factory/copy-activity-performance#staged-copy)）或 Azure Data Lake Store，在这种情况下，它可以充分利用群集的功能，而不必依赖于 ADF 自承载集成运行时 (IR) 而运行。 它将提供更高的复制吞吐量，尤其是当群集非常强大时。 根据 Azure 数据工厂中的配置，复制活动会自动构造 DistCp 命令，提交到 Hadoop 群集，以及监视复制状态。 使用与 DistCp 集成的 ADF，客户不仅可以利用现有的强大群集来实现最佳复制吞吐量，而且还能受益于 ADF 提供的灵活计划功能和统一的监视体验。 默认建议使用 ADF DistCp 模式将数据从本地 Hadoop 群集迁移到 Azure。
- ADF 本机 IR 模式。 在某些情况下，DistCp 无法适应你的情况（例如，在 VNET 环境中，DistCp 工具不支持 Express Route 专用对等互连 + Azure 存储 VNet 终结点）。 此外，你有时不希望使用现有的 Hadoop 群集作为引擎来迁移数据，因为这会给群集带来繁重的负载，从而可能影响现有 ETL 作业的性能。 如果存在这种情况，你可以使用 ADF 本机功能，在其中，ADF 集成运行时 (IR) 可以充当一个引擎，用于将数据从本地 HDFS 复制到 Azure。

本文为数据工程师和开发人员提供上述两种方法的以下信息：
> [!div class="checklist"]
> * 性能 
> * 复制复原能力
> * 网络安全性
> * 高级解决方案体系结构 
> * 有关实现的最佳做法  

## <a name="performance"></a>性能

在 ADF DistCp 模式下，吞吐量与单独使用 DistCp 工具时相同，将利用现有 Hadoop 群集的容量。 DistCp（分布式复制）是用于在群集之间/内部进行大规模复制的工具。 它使用 MapReduce 来影响数据分发、错误处理和恢复以及报告。 它将文件和目录列表扩展成映射任务的输入，其中每个任务将复制源列表中指定的文件分区。 使用与 DistCp 集成的 ADF，可以生成管道来充分利用网络带宽以及存储 IOPS 和带宽，以最大化环境的数据移动吞吐量。  

在 ADF 本机 IR 模式下，它还允许不同级别的并行度，这样，就可以通过手动纵向扩展自承载 IR 计算机或横向扩展到多个自承载 IR 计算机，充分利用网络带宽以及存储 IOPS 和带宽来最大化数据移动吞吐量。

- 单个复制活动可以利用可缩放的计算资源。 使用自承载集成运行时可以手动纵向扩展计算机或横向扩展到多个计算机（[最多 4 个节点](/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)），单个复制活动将在所有节点之间对其文件集进行分区。 
- 单个复制活动使用多个线程读取和写入数据存储。 
- ADF 控制流可以同时启动多个复制活动（例如，使用 [For Each 循环](/data-factory/control-flow-for-each-activity)）。 

可以在[复制活动性能指南](/data-factory/copy-activity-performance)中获取更多详细信息

## <a name="resilience"></a>复原能力

在 ADF DistCp 模式下，可以利用不同的 DistCp 命令行参数（例如，-i 表示忽略失败；-update 表示当源文件和目标文件的大小不同时写入数据）来实现不同级别的复原能力。

在 ADF 本机 IR 模式下，在单个复制活动运行中，ADF 具有内置的重试机制，因此，它可以处理数据存储或底层网络中特定级别的暂时性故障。 执行从本地 HDFS 复制到 Blob 以及从本地 HDFS 到 ADLS Gen2 的二元复制时，ADF 会大范围地自动执行检查点设置。 如果某个复制活动运行失败或超时，在后续重试时（请确保将重试计数设置为 > 1），复制将从上一个失败点继续，而不是从头开始。

## <a name="network-security"></a>网络安全性 

ADF 默认通过 HTTPS 协议使用加密的连接将数据从本地 HDFS 传输到 Azure Blob 存储或 Azure Data Lake Storage Gen2。  HTTPS 提供传输中数据加密，并可防止窃听和中间人攻击。 

如果你不希望通过公共 Internet 传输数据，可以通过 Azure Express Route 使用专用对等互连链路传输数据，以此实现更高的安全性。 请参阅下面的解决方案体系结构来了解如何实现此目的。

## <a name="solution-architecture"></a>解决方案体系结构

通过公共 Internet 迁移数据：

![solution-architecture-public-network](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-public-network.png)

- 在此体系结构中，将通过公共 Internet 使用 HTTPS 安全传输数据。 
- 建议在公用网络环境中使用 ADF DistCp 模式。 这样，不仅可以利用现有的强大群集来实现最佳复制吞吐量，而且还能受益于 ADF 提供的灵活计划功能和统一的监视体验。
- 需要在企业防火墙后的 Windows 计算机上安装 ADF 自承载集成运行时，以便将 DistCp 命令提交到 Hadoop 群集并监视复制状态。 由于此计算机不是用于移动数据的引擎（仅用于控制），因此，此计算机的容量不会影响数据移动吞吐量。
- 支持 DistCp 命令的现有参数。


通过专用链路迁移数据： 

![solution-architecture-private-network](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-private-network.png)

- 在此体系结构中，数据迁移是通过 Azure Express Route 使用专用对等互连链路完成的，因此，数据永远不会遍历公共 Internet。
- DistCp 工具不支持 Express Route 专用对等互连 + Azure 存储 VNet 终结点，因此，我们建议通过集成运行时使用 ADF 本机功能来迁移数据。
- 需要在 Azure 虚拟网络中的 Windows VM 上安装 ADF 自承载集成运行时才能实现此体系结构。 可以手动纵向扩展 VM 或横向扩展到多个 VM，以充分利用网络和存储 IOPS/带宽。
- 建议先对每个 Azure VM （装有 ADF 自承载集成运行时）使用以下配置：Standard_D32s_v3 大小，32 个 vCPU，128 GB 内存。 可以在数据迁移过程中持续监视 VM 的 CPU 和内存利用率，以确定是否需要进一步扩展 VM 来提高性能，或缩减 VM 来节省成本。
- 还可以通过将最多 4 个 VM 节点关联到一个自承载 IR 进行横向扩展。 针对自承载 IR 运行的单个复制作业将自动为文件集分区，并利用所有 VM 节点来并行复制文件。 为实现高可用性，我们建议从两个 VM 节点着手，以避免在数据迁移过程中出现单一故障点。
- 可以使用此体系结构实现初始快照数据迁移和增量数据迁移。


## <a name="implementation-best-practices"></a>有关实现的最佳做法 

### <a name="authentication-and-credential-management"></a>身份验证和凭据管理 

- 若要对 HDFS 进行身份验证，可以使用 [Windows (Kerberos) 或“匿名”](/data-factory/connector-hdfs#linked-service-properties)。 
- 支持使用多种身份验证类型连接到 Azure Blob 存储。  强烈建议使用 [Azure 资源的托管标识](/data-factory/connector-azure-blob-storage#managed-identity)：托管标识构建在 Azure AD 中自动管理的 ADF 标识基础之上，使你无需在链接服务定义中提供凭据，即可配置管道。  或者，可以使用[服务主体](/data-factory/connector-azure-blob-storage#service-principal-authentication)、[共享访问签名](/data-factory/connector-azure-blob-storage#shared-access-signature-authentication)或[存储帐户密钥](/data-factory/connector-azure-blob-storage#account-key-authentication)对 Azure Blob 存储进行身份验证。 
- 也支持使用多种身份验证类型连接到 Azure Data Lake Storage Gen2。  强烈建议使用 [Azure 资源的托管标识](/data-factory/connector-azure-data-lake-storage#managed-identity)，不过，也可以使用[服务主体](/data-factory/connector-azure-data-lake-storage#service-principal-authentication)或[存储帐户密钥](/data-factory/connector-azure-data-lake-storage#account-key-authentication)。 
- 如果不使用 Azure 资源的托管标识，我们强烈建议[在 Azure Key Vault 中存储凭据](/data-factory/store-credentials-in-key-vault)，以便更轻松地集中管理和轮换密钥，而无需修改 ADF 链接服务。

### <a name="initial-snapshot-data-migration"></a>初始快照数据迁移 

在 ADF DistCp 模式下，可以创建一个复制活动来提交带有不同参数的 DistCp 命令，以控制初始的数据迁移行为。 

在 ADF 本机 IR 模式下，建议使用数据分区，尤其是在迁移 10 TB 以上的数据时。 若要将数据分区，请利用 HDFS 中的文件夹名称，然后，每个 ADF 复制作业一次可以复制一个文件夹分区。 可以并行运行多个 ADF 复制作业，以获得更好的吞吐量。
如果网络或数据存储的暂时性问题导致任何复制作业失败，你可以重新运行失败的复制作业，以再次从 HDFS 中加载特定的分区。 加载其他分区的所有其他复制作业不受影响。

### <a name="delta-data-migration"></a>增量数据迁移 

在 ADF DistCp 模式下，可以利用 DistCp 命令行参数“-update”（表示当源文件和目标文件的大小不同时写入数据）来实现增量数据迁移。

在 ADF 本机 IR 模式下，识别 HDFS 中的新文件或已更改文件的最高效方法是使用时间分区命名约定 - 如果 HDFS 中的数据经过时间分区，并且文件或文件夹名称中包含时间切片信息（例如 /yyyy/mm/dd/file.csv），则管道可以轻松识别要增量复制的文件/文件夹。
或者，如果 HDFS 中的数据未经过时间分区，则 ADF 可按文件的 LastModifiedDate 来识别新文件或更改的文件。 ADF 的工作原理是扫描 HDFS 中的所有文件，仅复制上次修改时间戳大于特定值的新文件和更新文件。 请注意，如果 HDFS 中有大量文件，则初始文件扫描可能需要很长时间，而不管有多少个文件与筛选条件相匹配。 在这种情况下，我们建议先将数据分区，并使用同一分区进行初始快照迁移，以便可以并行执行文件扫描。

### <a name="estimating-price"></a>估算价格 

假设构造了以下管道用于将数据从 HDFS 迁移到 Azure Blob 存储： 

![pricing-pipeline](media/data-migration-guidance-hdfs-to-azure-storage/pricing-pipeline.png)

假设条件如下： 

- 总数据量为 1 PB
- 使用第二种解决方案体系结构（ADF 本机 IR 模式）迁移数据
- 1 PB 划分为 1000 个分区，每个复制操作移动一个分区。
- 为每个复制活动配置了一个关联到 4 个计算机的自承载 IR，可实现 500 MBps 的吞吐量。
- ForEach 并发性设置为 4，聚合吞吐量为 2 GBps。
- 完成迁移总共需要花费 146 小时。


下面是根据上述假设的估算出的价格： 

![pricing-table](media/data-migration-guidance-hdfs-to-azure-storage/pricing-table.png)

> [!NOTE]
> 这是一个虚构的定价示例。  实际价格取决于环境中的实际吞吐量。
> 不包括 Azure Windows VM（装有自承载集成运行时）的价格。

### <a name="additional-references"></a>其他参考 
- [HDFS 连接器](/data-factory/connector-hdfs)
- [Azure Blob 存储连接器](/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage Gen2 连接器](/data-factory/connector-azure-data-lake-storage)
- [复制活动性能和优化指南](/data-factory/copy-activity-performance)
- [创建和配置自承载集成运行时](/data-factory/create-self-hosted-integration-runtime)
- [自承载集成运行时的高可用性和可伸缩性](/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [数据移动安全注意事项](/data-factory/data-movement-security-considerations)
- [在 Azure Key Vault 中存储凭据](/data-factory/store-credentials-in-key-vault)
- [基于时间分区文件名增量复制文件](/data-factory/tutorial-incremental-copy-partitioned-file-name-copy-data-tool)
- [基于 LastModifiedDate 复制新文件和更改的文件](/data-factory/tutorial-incremental-copy-lastmodified-copy-data-tool)
- [ADF 定价页](https://azure.cn/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>后续步骤

- [使用 Azure 数据工厂复制多个容器中的文件](solution-template-copy-files-multiple-containers.md)