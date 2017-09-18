---
title: "Azure HDInsight 上的 Hadoop 组件发行说明 | Azure"
description: "Azure HDInsight 的 Hadoop 组件的最新发行说明和版本。 获取 Spark、R Server、Hive 和更多工具的开发技巧和详细信息。"
services: hdinsight
documentationcenter: 
editor: cgronlun
manager: jhubbard
author: nitinme
tags: azure-portal
ms.assetid: a363e5f6-dd75-476a-87fa-46beb480c1fe
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/04/2017
ms.date: 09/18/2017
ms.author: v-haiqya
ms.openlocfilehash: fc5a133b23438d1b9669c1b5d764e1b3aa31fb98
ms.sourcegitcommit: c2a877dfd2f322f513298306882c7388a91c6226
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Azure HDInsight 上的 Hadoop 组件发行说明

[!INCLUDE [hdinsight-linux-acn-version.md](../../includes/hdinsight-linux-acn-version.md)]

本文提供有关**最新** Azure HDInsight 版本更新的信息。 有关较早版本的信息，请参阅 [HDInsight 发行说明存档](hdinsight-release-notes-archive.md)。

> [!IMPORTANT]
> Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 版本控制文章](hdinsight-component-versioning.md)。

## <a name="notes-for-08012017-release-of-hdinsight"></a>HDInsight 08/01/2017 版发行说明

| 标题 | 说明 | 受影响的区域  | 群集类型  | 
| --- | --- | --- | --- | --- |
| 发布 HDInsight 上的 Microsoft R Server 9.1 |HDInsight 现在支持在 HDInsight 上设置 R Server 9.1 群集。 有关 Microsoft R Server 9.1 版本的详细信息，请参阅[此博客](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/introducing-microsoft-r-server-9-1-release/)。 |服务 |R Server |
| HDInsight 3.6 现在包含较新版本的 Hadoop 堆栈|<ul><li>有关更新版本的详细列表，请参阅 [HDInsight 中可用的 Hadoop 组件版本](hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions)。</li><li>有关在 Hadoop 堆栈最新版本中修复的 bug 的列表，请参阅 [Apache 修补程序信息](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/patch_parent.html)。</li><li>有关 HDP 2.6.1（现在可在 HDInsight 3.6 中使用）之间的重大更改的列表，请参阅 [https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/behavior_changes.html)。</li><li>有关 HDP 2.6.1 中的已知问题的列表，请参阅[已知问题](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_release-notes/content/known_issues.html)。</li></ul> |服务 |全部 |不适用 |
| Interactive Hive（预览版）群集的更新 |<ul><li><b>功能改进。</b> 实现了缓存元存储，它可通过缓存元数据来减少后端 SQL 上的负载降低并为所有元数据操作提高性能。  此改进现在是所有 Interactive Hive 群集上的默认设置。 有关详细信息，请参阅 [https://issues.apache.org/jira/browse/HIVE-16520](https://issues.apache.org/jira/browse/HIVE-16520)。</li><li><b>功能改进。</b> 动态分区加载进行了优化。 有关详细信息，请参阅 [https://issues.apache.org/jira/browse/HIVE-14204](https://issues.apache.org/jira/browse/HIVE-14204)。</li><li><b>功能改进。</b> Linux 上的 HDInsight 的配置优化。</li><li><b>Bug 修复。</b> `CredentialProviderFactory$getProviders` 不是线程安全的。 此问题现在已修复。 有关详细信息，请参阅 [https://issues.apache.org/jira/browse/HADOOP-14195](https://issues.apache.org/jira/browse/HADOOP-14195)。</li><li><b>Bug 修复。</b> WASB 驱动程序 `liststatus` API 的高 CPU 使用率导致 ATS 性能低下。 此问题现在已修复。 有关详细信息，请参阅 [https://github.com/Azure/azure-storage-java/pull/154](https://github.com/Azure/azure-storage-java/pull/154)。</li></ul> |服务 |交互式 Hive（预览版） |
| Hadoop 群集的更新 |Templeton 作业操作可靠性得到提高。 有关详细信息，请参阅 [https://issues.apache.org/jira/browse/HIVE-15947](https://issues.apache.org/jira/browse/HIVE-15947) |服务 |Hadoop |
| YARN 更新 | HDInsight 现在创建 250 GB Ambari 数据库（而不增加成本），这会形成更好的客户体验。 此更改应防止填满 ATS，并且可能具有更好的性能。 |服务 |全部 |
| Spark 更新 | 发布 Spark 2.1.1。 有关详细信息，请参阅 [Spark 版本 2.1.1](https://spark.apache.org/releases/spark-release-2-1-1.html)。 | 服务 | Spark |

## <a name="general-availability-of-hdinsight-36"></a>HDInsight 3.6 公开上市

* 在此次发布中，Azure HDInsight 添加了基于 HDP 2.6 的 3.6 版。 [此处](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html)提供 HDP 2.6 发行说明；[此处](hdinsight-component-versioning.md)提供有关 HDInsight 版本的详细信息。 HDInsight 3.6 可用于以下工作负荷：

    * Hadoop v2.7.3
    * HBase v1.1.2
    * Storm v1.1.0
    * Spark v2.1.0
    * Interactive Hive v2.1.0

* **Hive View 2.0 支持**。 这应该能够改善 Interactive Hive 的用户体验。 有关详细信息，请参阅 [Hortonworks 文档](http://docs.hortonworks.com/HDPDocuments/Ambari-2.5.0.3/bk_ambari-views/content/ch_using_hive_view.html)。

* **Hive LLAP 性能增强**。 有关详细信息，请参阅 [Hortonworks 文档](https://hortonworks.com/blog/top-5-performance-boosters-with-apache-hive-llap/)。

* **Hive 中的新增功能**。 请参阅 [Hortonworks 文档](https://hortonworks.com/apache/hive/#section_4)。

* **Hive CLI 弃用**：已弃用 Hive CLI，建议客户改为使用 Beeline。 有关详细信息，请参阅 [Apache 文档](https://cwiki.apache.org/confluence/display/Hive/Replacing+the+Implementation+of+Hive+CLI+Using+Beeline)。 有关如何将 Beeline 与 HDInsight 配合使用的说明，请参阅[将 Beeline 与 HDInsight Hadoop 群集配合使用](hdinsight-hadoop-use-hive-beeline.md)。

* **Apache Phoenix 和 HBase 中的新增功能**。
    * 存储配额支持：常用于多租户环境，可按表和按命名空间提供有限的存储空间。
    * Phoenix 索引改进：增量创建索引以及从之前失败的场景中重新生成/恢复索引。
    * Phoenix 数据完整性工具：支持验证架构、索引和其他元数据。

* **HBase 问题**：运行 CSV 批量上传 MapReduce 作业时，以下语法可能会导致错误。

        HADOOP_CLASSPATH=$(hbase mapredcp):/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv

    请改用以下语法：

        HADOOP_CLASSPATH=/path/to/hbase-protocol.jar:/path/to/hbase/conf hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /data/example.csv

## <a name="release-of-spark-21-on-hdinsight-36-preview"></a>发布 Spark 2.1 on HDInsight 3.6（预览版）
* [Spark 2.1](http://spark.apache.org/releases/spark-release-2-1-0.html) 纠正了旧版中的许多稳定性和可用性问题。 它还带来了针对所有 Spark 工作负荷（如 Spark Core、SQL、ML 和流式处理）的新功能。
* 结构化流式处理通过支持事件时间水印和 Kafka 0.10 连接器提高了可伸缩性。
* 现在使用新的可缩放分区处理机制处理 Spark SQL 分区。 有关如何升级，请参阅 [此处](http://spark.apache.org/releases/spark-release-2-1-0.html) 的更多详细信息。
* Azure HDInsight 3.6 预览版中的 Spark 2.1 目前不支持使用 ODBC 驱动程序的 BI 工具连接。

## <a name="release-of-spark-201-on-hdinsight-35"></a>发布 Spark 2.0.1 on HDInsight 3.5
Spark 2.0.1 现已在 Spark 群集（HDInsight 版本 3.5）上发行。

## <a name="release-of-r-server-90-on-hdinsight-35-spark-20"></a>发布 R Server 9.0 on HDInsight 3.5 (Spark 2.0)
*   R Server 群集现在包括适用于两个版本的选项：R Server 9.0 on HDI 3.5 (Spark 2.0) 和 R Server 8.0 on HDI 3.4 (Spark 1.6)。
*   R Server 9.0 on HDI 3.5 (Spark 2.0) 是基于 R 3.3.2 构建的，包括新 ScaleR 数据源函数（称为 RxHiveData 和 RxParquetData），用于将数据直接从 Hive 和 Parquet 加载到 Spark DataFrames，由 ScaleR 分析。 有关详细信息，请通过使用 ?RxHiveData 和 ?RxParquetData 命令在 R 中查看关于这些函数的内联帮助。
*   现在，作为预配流的一部分，RStudio Server 社区版（通过选择退出选项）默认安装在“群集预配”边栏选项卡上。

## <a name="--release-of-spark-20-on-hdinsight"></a>- 发布 Spark 2.0 on HDInsight
* HDInsight 3.5 上的 Spark 2.0 群集现支持 Livy 和 Jupyter 服务。

## <a name="--release-of-r-server-on-hdinsight"></a>- 发布 R Server on HDInsight
* 进行边缘节点访问的 URI 已更改为 **clustername**-ed-ssh.azurehdinsight.cn
* R Server on HDInsight 群集预配已简化。
* R Server on HDInsight 现已作为常用的 HDInsight“R Server”群集类型推出，不再作为单独的 HDInsight 应用程序安装。 边缘节点和 R Server 二进制文件现在会在 R Server 群集部署过程中进行预配。 这会提高预配的速度和可靠性。 R Server 的定价模型会相应地进行更新。

## <a name="release-of-r-server-on-hdinsight"></a>发布 R Server on HDInsight
随此版本一起部署的基于 Linux 的 HDInsight 群集的所有版本号：

| HDI | HDI 群集版本 | HDP | HDP 内部版本 | Ambari 内部版本 |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8268980 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8268980 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8269383 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

随此版本一起部署的基于 Windows 的 HDInsight 群集的所有版本号：

| HDI | HDI 群集版本 | HDP | HDP 内部版本 |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1033.2559206 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1033.2559206 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1033.2559206 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1033.2559206 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1033.2559206 |2.3 |2.3.3.1-25 |

<!--Update_Description: wording update-->