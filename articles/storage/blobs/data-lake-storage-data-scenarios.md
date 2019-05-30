---
title: 涉及 Azure Data Lake Storage Gen2 的数据方案 | Microsoft Docs
description: 了解可在 Data Lake Storage Gen2（以前称为 Azure Data Lake Store）中进行数据引入、处理、下载和可视化的不同方案和工具
services: storage
author: WenJason
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
origin.date: 02/12/2019
ms.date: 05/27/2019
ms.author: v-jay
ms.openlocfilehash: 92bef4cc3ab2af1410fd9f59b06795c36a12b492
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004563"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>使用 Azure Data Lake Storage Gen2 满足大数据需求

大数据处理的四个主要阶段：

> [!div class="checklist"]
> * 实时或批量引入大量数据到数据存储
> * 处理数据
> * 下载数据
> * 可视化数据

首先，请创建存储器帐户和文件系统。 然后，授予对数据的访问权限。 本文的前几个部分介绍如何完成这些任务。 剩下部分将重点介绍每个处理阶段的选项和工具。

## <a name="create-a-data-lake-storage-gen2-account"></a>创建 Data Lake Storage Gen2 帐户

Data Lake Storage Gen2 帐户是一个存储帐户，其命名空间具有层次结构。 

若要创建一个，请参阅[快速入门：创建 Azure Data Lake Storage Gen2 存储帐户](/storage/blobs/data-lake-storage-quickstart-create-account?toc=%2fstorage%2fblobs%2ftoc.json)。

## <a name="create-a-file-system"></a>创建文件系统

文件系统  是一个包含文件夹和文件的容器。 必须至少有其中一个，然后才能将数据引入存储帐户中。  下面是可以用来创建它们的工具的列表。

|工具 | 指南 |
|---|--|
|Azure 存储资源管理器 | [使用存储资源管理器创建文件系统](/storage/blobs/data-lake-storage-explorer#create-a-filesystem) |
|AzCopy | [使用 AzCopyV10 创建 Blob 容器或文件共享](/storage/common/storage-use-azcopy-v10?toc=%2fstorage%2fblobs%2ftoc.json#create-a-blob-container-or-file-share)|
|将 Hadoop 文件系统 (HDFS) 命令行界面 (CLI) 与 HDInsight 配合使用 |[通过将 HDFS 与 HDInsight 配合使用来创建文件系统](/storage/blobs/data-lake-storage-use-hdfs-data-lake-storage?toc=%2fstorage%2fblobs%2ftoc.json#create-a-file-system) |

使用存储资源管理器或 AzCopy 创建文件系统是最容易的方法。 使用 HDInsight 创建文件系统需要的工作会多一些。 但是，如果已打算使用 HDInsight 群集来处理数据，则可先创建群集，然后使用 HDFS CLI 来创建文件系统。  

## <a name="grant-access-to-the-data"></a>授予对数据的访问权限

在开始引入数据之前，先设置相应的访问权限，以便访问帐户以及帐户中的数据。

可通过三种方法授予访问权限：

* 将下述角色之一分配给用户、组、用户托管标识或服务主体：

  [存储 Blob 数据读者](/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)

  [存储 Blob 数据参与者](/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)

  [存储 Blob 数据所有者](/role-based-access-control/built-in-roles#storage-blob-data-owner-preview)

* 使用共享访问签名 (SAS) 令牌。

* 使用存储帐户密钥。

下表介绍如何为每个 Azure 服务或工具授予访问权限。

|工具 | 授予访问权限 | 指南 |
|---|--|---|
|存储资源管理器| 向用户和组分配角色 | [为具有 Azure Active Directory 的用户分配管理员和非管理员角色](/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) |
|AzCopy| 向用户和组分配角色 <br>**or**<br> 使用 SAS 令牌| [为具有 Azure Active Directory 的用户分配管理员和非管理员角色](/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)<br><br>[轻松创建从 Azure 存储下载文件所需的 SAS - 使用 Azure 存储资源管理器](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/)|
|Apache DistCp | 为用户分配的托管标识分配角色 | [创建包含 Data Lake Storage Gen2 的 HDInsight 群集](/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure HDInsight| 为用户分配的托管标识分配角色 | [创建包含 Data Lake Storage Gen2 的 HDInsight 群集](/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)|

若要授予对特定文件和文件夹的访问权限，请参阅以下文章。

* [使用 Azure 存储资源管理器和 Azure Data Lake Storage Gen2 设置文件和目录级别权限](/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

* [文件和目录上的访问控制列表](/storage/blobs/data-lake-storage-access-control#access-control-lists-on-files-and-directories)

若要了解如何设置安全性的其他方面，请参阅 [Azure Data Lake Storage Gen2 安全指南](/storage/common/storage-data-lake-storage-security-guide?toc=%2fstorage%2fblobs%2ftoc.json)。

## <a name="ingest-the-data"></a>引入数据

本部分重点介绍不同的数据源和将数据引入 Data Lake Storage Gen2 帐户的不同方式。

### <a name="ad-hoc-data"></a>临时数据

这表示可用于形成大数据应用程序原型的较小数据集。 存在数种不同的引入临时数据的方式，具体取决于数据源。 

下面是可以用来引入临时数据的工具的列表。

| 数据源 | 引入方式 |
| --- | --- |
| 本地计算机 |[存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)<br><br>[AzCopy 工具](../common/storage-use-azcopy-v10.md)|
| Azure 存储 Blob |[AzCopy 工具](../common/storage-use-azcopy-v10.md)<br><br>[HDInsight 群集上运行的 DistCp](data-lake-storage-use-distcp.md)|

### <a name="streamed-data"></a>流数据

这表示可由应用程序、设备、传感器等多种源生成的数据。可通过各种工具将此数据引入 Data Lake Storage Gen2。 这些工具通常实时逐事件捕获和处理数据，并随后批量将事件写入 Data Lake Storage Gen2，以便这些事件可以得到进一步处理。

下面是一个列表，其中包含可以用来引入流式处理的数据的工具。

|工具 | 指南 |
|---|--|
|Azure HDInsight Storm | [从 Apache Storm on HDInsight 写入到 Apache Hadoop HDFS](/hdinsight/storm/apache-storm-write-data-lake-store) |

### <a name="data-associated-with-azure-hdinsight-clusters"></a>与 Azure HDInsight 群集关联的数据

大多数 HDInsight 群集类型（Hadoop、HBase、Storm）支持将 Data Lake Storage Gen2 用作数据存储库。 HDInsight 群集从 Azure 存储 Blob (WASB)访问数据。 要提高性能，可从 WASB 将数据复制到与此群集关联的 Data Lake Storage Gen2 帐户。 可使用以下工具复制数据。

下面是一个列表，其中包含可以用来引入与 HDInsight 群集关联的数据的工具。

|工具 | 指南 |
|---|--|
|Apache DistCp | [使用 DistCp 在 Azure 存储 Blob 与 Data Lake Storage Gen2 之间复制数据](/storage/blobs/data-lake-storage-use-distcp) |
|AzCopy 工具 | [使用 AzCopy 传输数据](/storage/common/storage-use-azcopy-v10) |

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>存储在本地或 IaaS Hadoop 群集的数据

使用 HDFS 可在本地计算机上的现有 Hadoop 群集中存储大量数据。 Hadoop 群集可能在本地部署或 Azure 上的 IaaS 群集中。 通过一次性方法或重复方式将此类数据复制到 Azure Data Lake Storage Gen2 可能存在一些要求。 实现此目的有多种可用选项。 以下是替代项和相关折衷方案的列表。

| 方法 | 详细信息 | 优点 | 注意事项 |
| --- | --- | --- | --- |
| 使用 Distcp 将数据从 Hadoop 复制到 Azure 存储。 然后使用适当机制将数据从 Azure 存储复制到 Data Lake Storage Gen2。 |可使用以下工具将数据从 Azure 存储复制到 Data Lake Storage Gen2： <ul><li>[AzCopy 工具](../common/storage-use-azcopy-v10.md)</li><li>[HDInsight 群集上运行的 Apache DistCp](data-lake-storage-use-distcp.md)</li></ul> |可使用开放源代码工具。 |涉及多种技术的多步处理 |

### <a name="really-large-datasets"></a>大型数据集

对于上传兆兆字节范围内的数据集，使用上述方法可能有时速度慢且成本高。 在这种情况下，可以使用 Azure ExpressRoute。  

Azure ExpressRoute 允许在 Azure 数据中心与本地中的基础结构之间创建专有连接。 这对传输大量数据提供了可靠的选项。 若要了解详细信息，请参阅 [Azure ExpressRoute 文档](../../expressroute/expressroute-introduction.md)。

## <a name="process-the-data"></a>处理数据

数据在 Data Lake Storage Gen2 中可用后，可使用支持的大数据应用程序对此数据运行分析。 

下面是一个列表，其中包含的工具可以用来对存储在 Data Lake Storage Gen2 中的数据运行数据分析作业。

|工具 | 指南 |
|---|--|
|Azure HDInsight | [将 Azure Data Lake Storage Gen2 用于 Azure HDInsight 群集](/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |

## <a name="download-the-data"></a>下载数据

用户可能还希望为一些方案从 Azure Data Lake Storage Gen2 下载或移动数据，例如：

* 将数据移动到其他存储库以便连接现有数据处理管道。 例如，用户可能希望从 Data Lake Storage Gen2 将数据移动到 Azure SQL 数据库或本地 SQL 服务器。

* 构建应用程序原型时，下载数据到本地计算机以在 IDE 中进行处理。

下面是一个列表，其中包含的工具可以用来从 Data Lake Storage Gen2 下载数据。

|工具 | 指南 |
|---|--|
|Apache DistCop | [使用 DistCp 在 Azure 存储 Blob 与 Data Lake Storage Gen2 之间复制数据](/storage/blobs/data-lake-storage-use-distcp) |
